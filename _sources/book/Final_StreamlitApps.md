# Creating Streamlit Applications for YOLOv11 Models

Streamlit is an open-source Python library that makes it easy to create interactive web applications. Below is an example of how to load YOLOv11 weights, run inference, and display results.

## Installation
```bash
pip install streamlit ultralytics
```

## Development Environment Setup

### Setting Up Your Editor
For the best development experience with Streamlit, use a modern code editor:

1. **VSCode Setup:**
   * Install VSCode from [code.visualstudio.com](https://code.visualstudio.com/)
   * Install the Python extension
   * Create a new project folder and open it in VSCode
   * Use the integrated terminal to install streamlit: `pip install streamlit ultralytics`

2. **JetBrains PyCharm Setup:**
   * Install PyCharm from [jetbrains.com/pycharm](https://www.jetbrains.com/pycharm/)
   * Create a new Python project
   * Configure a virtual environment
   * Install required packages via terminal or the PyCharm package manager

### Project Structure
A basic Streamlit project structure might look like:
```
my_yolo_app/
├── app.py              # Main Streamlit application
├── requirements.txt    # Dependencies
├── models/             # Store your YOLOv11 model weights
│   └── yolov11n.pt
└── examples/           # Example images for testing
    └── example1.jpg
```

### Creating Requirements.txt
Create a `requirements.txt` file containing:
```
streamlit>=1.22.0
ultralytics>=8.0.0
Pillow>=9.0.0
numpy>=1.22.0
```

## Basic Inference App

```python
import streamlit as st
from PIL import Image
from ultralytics import YOLO

def main():
    st.title("YOLOv11 Object Detection App")

    # ...existing code for UI elements...
    uploaded_file = st.file_uploader("Upload an image:", type=["jpg", "jpeg", "png"])
    if uploaded_file is not None:
        image = Image.open(uploaded_file)
        st.image(image, caption="Uploaded Image", use_column_width=True)
        if st.button("Run Detection"):
            # Load a YOLOv11 model (shows how to upload custom weights)
            model = YOLO("yolov11n.pt")  # Replace with your own weights if needed

            # Run inference
            results = model([image], stream=True)
            for result in results:
                # Show bounding boxes, masks, or other outputs in the console
                st.write("Boxes:", result.boxes)
                if result.masks:
                    st.write("Masks available")
                if result.keypoints:
                    st.write("Keypoints available")
                if result.probs is not None:
                    st.write("Classification probabilities:", result.probs)
                if result.obb is not None:
                    st.write("Oriented bounding boxes:", result.obb)

                # Display results
                result.show()
                result.save("result.jpg")  # Saves to disk
                st.image("result.jpg", caption="Inference Result")

if __name__ == "__main__":
    main()
```

## Advanced Streamlit Features

### Customizing the Sidebar

The sidebar provides a convenient place for controls and settings:

```python
def sidebar_controls():
    st.sidebar.title("Model Settings")
    
    # Model selection
    model_type = st.sidebar.selectbox(
        "Select Model Type", 
        ["YOLOv11n", "YOLOv11s", "YOLOv11m", "YOLOv11l"]
    )
    
    # Confidence threshold slider
    conf_threshold = st.sidebar.slider(
        "Confidence Threshold", 
        min_value=0.0, 
        max_value=1.0, 
        value=0.25, 
        step=0.05
    )
    
    # IoU threshold slider
    iou_threshold = st.sidebar.slider(
        "IoU Threshold", 
        min_value=0.0, 
        max_value=1.0, 
        value=0.45, 
        step=0.05
    )
    
    return model_type, conf_threshold, iou_threshold

# In your main function:
model_type, conf_threshold, iou_threshold = sidebar_controls()
```

### Handling Multiple Input Types

Allow users to input data in multiple ways:

```python
def get_input_data():
    st.header("Input Options")
    
    input_method = st.radio(
        "Select input method",
        ["Upload Image", "Paste URL", "Use Webcam"]
    )
    
    image = None
    
    if input_method == "Upload Image":
        uploaded_file = st.file_uploader("Upload an image", type=["jpg", "jpeg", "png"])
        if uploaded_file is not None:
            image = Image.open(uploaded_file)
            
    elif input_method == "Paste URL":
        url = st.text_input("Enter image URL")
        if url and st.button("Fetch Image"):
            try:
                import requests
                from io import BytesIO
                response = requests.get(url)
                image = Image.open(BytesIO(response.content))
            except Exception as e:
                st.error(f"Error fetching image: {e}")
                
    elif input_method == "Use Webcam":
        if st.button("Capture from Webcam"):
            picture = st.camera_input("Take a picture")
            if picture:
                image = Image.open(picture)
    
    return image
```

### Progress and Status Indicators

Show progress during long operations:

```python
def process_images(images):
    # Create a progress bar
    progress_bar = st.progress(0)
    status_text = st.empty()
    
    results = []
    for i, img in enumerate(images):
        # Update progress
        progress = (i + 1) / len(images)
        progress_bar.progress(progress)
        status_text.text(f"Processing image {i+1}/{len(images)}")
        
        # Process image
        result = model(img)
        results.append(result)
        
    status_text.text("Processing complete!")
    return results
```

### Displaying Results with Tabs

Organize results using tabs:

```python
def display_results(results):
    tab1, tab2, tab3 = st.tabs(["Visualizations", "Statistics", "Raw Data"])
    
    with tab1:
        st.header("Detection Results")
        for i, result in enumerate(results):
            st.subheader(f"Image {i+1}")
            result.save(f"result_{i}.jpg")
            st.image(f"result_{i}.jpg")
    
    with tab2:
        st.header("Detection Statistics")
        for i, result in enumerate(results):
            st.subheader(f"Image {i+1}")
            # Count detections by class
            boxes = result.boxes
            if len(boxes) > 0:
                labels = boxes.cls
                unique_labels, counts = np.unique(labels.cpu().numpy(), return_counts=True)
                st.write("Detected objects:")
                for label, count in zip(unique_labels, counts):
                    st.write(f"- {model.names[int(label)]}: {count}")
            else:
                st.write("No objects detected")
    
    with tab3:
        st.header("Raw Detection Data")
        for i, result in enumerate(results):
            st.subheader(f"Image {i+1}")
            st.json(result.tojson())
```

## Deploying to Streamlit Cloud

To deploy your app to [Streamlit Cloud](https://streamlit.io/cloud):

1. **Push your code to GitHub:**
   ```bash
   git init
   git add .
   git commit -m "Initial commit of Streamlit YOLO app"
   git branch -M main
   git remote add origin https://github.com/yourusername/your-repo.git
   git push -u origin main
   ```

2. **Sign up for Streamlit Cloud** and connect your GitHub account

3. **Deploy your app:**
   - Click "New app" in the Streamlit Cloud dashboard
   - Select your repository, branch, and main file path (e.g., app.py)
   - Click "Deploy"

4. **Configure advanced settings:**
   - If your model requires GPU, select the appropriate compute resources
   - Set environment variables if needed
   - Configure authentication if you want to restrict access

5. **Handling model weights:**
   - For small models (<200MB), you can include them in your repo
   - For larger models:
     - Use Hugging Face Hub: `model = YOLO("OceanCV/your-model-name")`
     - Or use a file storage service and download at startup

## Running the App
```bash
# Run locally
streamlit run app.py

# With specific server options
streamlit run app.py --server.port 8501 --server.address localhost
```

## Additional Features

### File Management
Managing user-uploaded files and outputs:

```python
import os
import uuid
import tempfile

def save_uploaded_file(uploaded_file):
    # Create a temporary directory
    temp_dir = tempfile.mkdtemp()
    
    # Generate a unique filename
    filename = f"{uuid.uuid4().hex}_{uploaded_file.name}"
    filepath = os.path.join(temp_dir, filename)
    
    # Save the file
    with open(filepath, "wb") as f:
        f.write(uploaded_file.getbuffer())
        
    return filepath
```

### Session State
Persist data between reruns of your app:

```python
# Initialize session state variables
if "detection_history" not in st.session_state:
    st.session_state.detection_history = []
    
# Add to history
def add_to_history(result, image_name):
    st.session_state.detection_history.append({
        "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
        "image": image_name,
        "detections": len(result.boxes),
        "classes": [model.names[int(cls)] for cls in result.boxes.cls.cpu().numpy()]
    })
    
# Display history
def show_history():
    st.subheader("Detection History")
    for i, item in enumerate(st.session_state.detection_history):
        st.write(f"**{i+1}. {item['timestamp']}** - {item['image']}")
        st.write(f"Found {item['detections']} objects: {', '.join(item['classes'])}")
        st.divider()
```

### Batch Processing
Process multiple images at once:

```python
def batch_processing():
    st.subheader("Batch Processing")
    
    uploaded_files = st.file_uploader(
        "Upload multiple images", 
        type=["jpg", "jpeg", "png"], 
        accept_multiple_files=True
    )
    
    if uploaded_files and st.button("Process All"):
        images = []
        for uploaded_file in uploaded_files:
            image = Image.open(uploaded_file)
            images.append(image)
            
        with st.spinner("Processing images..."):
            results = process_images(images)
            
        st.success(f"Processed {len(results)} images")
        display_results(results)
```

By combining these features, you can create sophisticated Streamlit applications for computer vision tasks that are both functional and user-friendly.
