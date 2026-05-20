# Imagery Fundamentals 

## Overview

In this lesson, we will explore some fundamentals of imagery. Understanding these basics will help with understanding **Marine Imagery** and **Computer Vision (CV)** techniques.

### Learning Objectives

By the end of this section, you will:
- Understand what an image is
- Identify fundamental image types used in marine imaging

---

## What is an image?

The dictionary definition of an image as given by Merriam-Webster is: 

A) a visual representation of something: such as
1) a likeness of an object produced on a photographic material
2) a picture produced on an electronic display (such as a television or computer screen)

B) the optical counterpart of an object produced by an optical device (such as a lens or mirror) or an electronic device 

Images are composed of pixels. The word pixel comes from **pic**ture **el**ments and to the computer each pixel is simply a number. 

:::{figure} images/pixel_lincoln.png
:name: pixel_example
Image of Abraham Lincoln as a matrix of pixel values. Example of how images are displayed for human users versus how the image appears to the computer. Credit: {cite}`Wevers2019`
:::

## Fundamental image types

### Video and still images

Video captures moving objects via a series of images known as frames while still photography captures freezes a moment in a single frame.

For marine science studies video and still images capture different types of information. Video or time-lapse still imagery allows for observation of behavior, interactions between fauna and their habitat, and changes over time. Individual still images are often used for spatial studies and typically have a higher resolution than images from video {cite}`Jamieson2013`, though improvements in technology are narrowing this gap. 

### Monocular, stereo, and omnidirectional photography

Both video and still imagery can be captured by monocular, stereo, and omnidirectional systems. **Monocular**, **stereo**, and **omnidirectional** refer to the number of cameras that make up the capture system. Monocular of single camera systems are most commonly used in marine studies, but stereo systems provide additional information and thus are popular in fisheries science for the determination of fish size as well as abundance {cite}`Durden2016`.

:::{figure} images/mono_stereo.jpg
:name: mono_stereo_example
(a) Monocular and (b) stereo camera, courtesy of ELP, from {cite}`Valikhanli2023`
:::
 
### Color and monochrome photography

As with video vs still imagery and monocular vs stereo camera systems, the capture of imagery in color or black and white (monochrome) is dependent on the needs of the study. Monochrome and greyscale images may have better resolution than color imagery and may be useful in turbid or low light conditions due to the reduction in scatter light. While color images may be necessary for studies with taxonomic identification goals {cite}`Smith2013`. 

### Non-conventional photography

There are many non-conventional photographic systems that are used in the marine environments such as

- Multispectral fluorescence imaging to observe bioluminescence 
- 3-dimensional laser holography to quantify and identify plankton and measure their geometry. 
- Shadowgraph illumination and other systems that use dark-field illumination to image plankton and other particles.
- Light-field cameras which allow for the focus in images to be changed after capture, and allow for imaging of the seafloor and objects above it at the same time. 