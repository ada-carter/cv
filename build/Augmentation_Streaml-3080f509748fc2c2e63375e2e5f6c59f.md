# 2.6 Augmentation Streamlit App

## Introduction

Training a robust computer vision model on marine imagery requires a well-designed augmentation strategy. Underwater images are affected by conditions that rarely appear in standard benchmark datasets: backscatter particles that introduce blur and noise, color shifts toward green or blue caused by water absorption of red wavelengths, uneven illumination from artificial lights, and motion blur from moving animals or a drifting camera rig. Augmentation policies that handle these artifacts can dramatically improve a model's generalization from controlled dive footage to open-ocean or turbid-estuary conditions.

The problem is that choosing augmentation parameters is not straightforward. Too much brightness adjustment washes out texture detail. Too little rotation fails to account for a camera mounted at an angle. Domain experts, including marine biologists who annotate the data, often have strong intuitions about what a plausible underwater image looks like but no convenient way to test those intuitions against code.

This lesson solves that problem by building a Streamlit web application. Streamlit turns a plain Python script into an interactive browser-based interface with almost no boilerplate. The result is a tool that both developers and non-coding collaborators can use to explore augmentation effects on real marine images before those augmentations are locked into a training pipeline.

## App Structure

The application has two sections. The sidebar contains sliders and toggle controls that set augmentation parameters. The main panel shows the original image and the augmented image side by side, updating instantly whenever any parameter changes.

The controls cover the augmentations most relevant to marine imagery:

- **Brightness** (slider, 0.5 to 2.0): simulates changes in ambient light depth or artificial light distance. A value of 1.0 leaves the image unchanged.
- **Contrast** (slider, 0.5 to 2.0): sharpens or flattens the tonal range. Useful for modeling murky versus clear water.
- **Blur radius** (slider, 0 to 10): applies a Gaussian blur that mimics backscatter or camera defocus.
- **Noise level** (slider, 0 to 50): adds random pixel-level noise, modeling sensor noise in low-light conditions.
- **Horizontal flip** (checkbox): flips the image left-to-right. Valid for most marine subjects because the ocean has no inherent horizontal directionality, unlike road scenes where lane markings have a fixed orientation.

## Code Walkthrough

Install the required packages:

```bash
pip install streamlit pillow numpy
```

Save the following as `app.py`:

```python
import streamlit as st
from PIL import Image, ImageEnhance, ImageFilter
import numpy as np

st.title('Marine Image Augmentation Explorer')

uploaded = st.file_uploader('Upload a marine image', type=['jpg', 'png', 'jpeg'])
if uploaded:
    img = Image.open(uploaded).convert('RGB')

    brightness = st.sidebar.slider('Brightness', 0.5, 2.0, 1.0)
    contrast = st.sidebar.slider('Contrast', 0.5, 2.0, 1.0)
    blur = st.sidebar.slider('Blur radius', 0, 10, 0)
    flip = st.sidebar.checkbox('Horizontal flip')

    aug = ImageEnhance.Brightness(img).enhance(brightness)
    aug = ImageEnhance.Contrast(aug).enhance(contrast)
    if blur > 0:
        aug = aug.filter(ImageFilter.GaussianBlur(blur))
    if flip:
        aug = aug.transpose(Image.FLIP_LEFT_RIGHT)

    col1, col2 = st.columns(2)
    col1.header('Original')
    col1.image(img)
    col2.header('Augmented')
    col2.image(aug)
```

Each augmentation is applied sequentially using Pillow's `ImageEnhance` and `ImageFilter` modules, which are fast and require no additional dependencies. `ImageEnhance.Brightness` and `ImageEnhance.Contrast` each accept a float factor: values below 1.0 reduce the effect, values above 1.0 amplify it. `ImageFilter.GaussianBlur` takes a radius in pixels; a radius of 0 is a no-op, so the `if blur > 0` guard avoids a redundant filter call. `Image.FLIP_LEFT_RIGHT` is a lossless transpose that does not re-encode the image.

The `st.columns(2)` call splits the main panel into two equal columns. Each column receives an `image()` call that renders a PIL Image object directly. Streamlit handles the conversion to a browser-displayable format automatically.

## Running the App

Run the app locally with:

```bash
streamlit run app.py
```

Streamlit opens a browser tab at `http://localhost:8501`. Every time you move a slider or toggle a checkbox, the script re-runs from top to bottom and the augmented image updates in place.

In Google Colab, you cannot open `localhost` directly from your browser. Use localtunnel to expose the port publicly:

```bash
!streamlit run app.py &
!npx localtunnel --port 8501
```

The `&` sends the Streamlit process to the background. `localtunnel` prints a public URL that you open in a new tab. The tunnel connection may be slow, but it is sufficient for interactive exploration during a class session.

## Extending the App

The Pillow-based augmentations in the walkthrough above cover the most common cases. For a production-grade augmentation explorer, consider switching to Albumentations, which provides transforms that Pillow does not:

- `ElasticTransform`: warps the image as if it were printed on a rubber sheet. Useful for simulating the optical distortion introduced by water near a dome port lens.
- `GridDistortion`: applies a grid-based deformation that can mimic lens barrel distortion from wide-angle underwater housings.
- `CoarseDropout` (formerly Cutout): randomly blacks out rectangular patches, a regularization technique that forces the model to use context rather than relying on a single discriminative region.

Add a download button so users can save the augmented image for annotation or pipeline testing:

```python
from io import BytesIO

buf = BytesIO()
aug.save(buf, format='PNG')
st.sidebar.download_button(
    label='Download augmented image',
    data=buf.getvalue(),
    file_name='augmented.png',
    mime='image/png'
)
```

`BytesIO` holds the encoded PNG in memory without writing a file to disk. `st.sidebar.download_button` places the download link in the sidebar alongside the controls.

## Marine Science Application

Before finalizing an augmentation policy for a training run, bring the Streamlit app to a session with the biologists or field scientists who collected the data. Ask them to upload representative images from each deployment site and move the sliders to the extremes. If a marine biologist says "that blur level is unrealistic for the ROV footage from this site," the slider provides immediate feedback that is far more intuitive than reading a set of float parameters in a YAML config file.

This validation step catches two common mistakes. First, augmentations that are physically implausible for the specific imaging context, for example, flipping a fish school image horizontally is fine, but flipping an image of a seafloor transect with a visible compass rose or scaling reference bar would corrupt the label geometry. Second, augmentations that are too aggressive and destroy the signal that the model needs, for example, a blur radius of 8 on 640x640 images may be appropriate for modeling severe backscatter in turbid water, but it will obscure the fine texture that distinguishes soft coral from sponge colonies.

Domain expert review of augmentation parameters, enabled by an interactive tool like this one, is a low-cost step that pays dividends at evaluation time.
