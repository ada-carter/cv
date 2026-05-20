# Getting Started

This guide will help you set up your environment and understand the tools we'll be using throughout the course.

## Required Tools and Accounts

### Core Tools
- **Python**: We'll be using Python 3.8 or higher
- **Anaconda/Miniconda**: For managing Python environments and packages
- **Git**: For version control and accessing course materials
- **VSCode** (recommended) or PyCharm: For code editing and development
- **Image Annotation Software**: For creating training datasets
- **Roboflow** (Easiest and most intuitive, but not a great long-term solution): This web-based annotation platform was used in the pilot class associated with our course.
    - Create a free account at [roboflow.com](https://roboflow.com)
    - Supports multiple annotation types (bounding boxes, polygons, keypoints)
    - Easy export to common formats (YOLO, COCO, VOC)
    - Includes data augmentation features
- **BIIGLE**: Open-source annotation tool designed with marine use in mind.
    - Create a free account at [biigle.de](https://biigle.de/)
    - Supports multiple annotation types
    - Can be self-hosted
    - Ideal for annotating marine imagery
    - Great community
  - **Alternative Open Source Options**:
    - [LabelImg](https://github.com/tzutalin/labelImg): Simple, desktop-based tool for bounding boxes
    - [CVAT](https://github.com/opencv/cvat): Powerful open-source annotation tool
      - Can be run locally or deployed as a server
      - Supports multiple annotation types
      - Good for team collaboration
    - [VGG Image Annotator (VIA)](https://www.robots.ox.ac.uk/~vgg/software/via/): Browser-based tool
      - No installation required
      - Works completely offline
      - Supports points, lines, polygons, and bounding boxes
    - [Label Studio](https://github.com/heartexlabs/label-studio): Open source data labeling tool
      - Supports multiple data types beyond just images
      - Can be self-hosted
      - Has both community and enterprise versions

### Required Accounts
- **Google Account**: For accessing Google Colab
- **Hugging Face Account**: For accessing and sharing models
  - Join the [OceanCV organization](https://huggingface.co/OceanCV) on Hugging Face to access our models and contribute your own
  - Request to join by visiting the organization page and clicking "Request to join"
  - This will allow you to collaborate on models and datasets with other course participants
- **GitHub Account**: For accessing course materials and contributing

## Working Environments

### Option 1: Google Colab (Recommended for Beginners)
Google Colab provides a free, cloud-based Jupyter notebook environment with GPU support. To use, this is the simplist way to work in the website oceancv.org, especially when displaying resources to large classes:
1. Navigate to any `.ipynb` file in the course
2. Click the "Open in Colab" button at the top of the notebook
3. Save a copy to your Google Drive to preserve your work

### Option 2: Local Setup
To run the course materials locally:

1. Clone the repository:
```bash
git clone https://github.com/atticus-carter/cv.git
cd cv
```

2. Create and activate a conda environment:
```bash
conda env create -f environment.yml
conda activate cv
```

3. Launch Jupyter Lab:
```bash
jupyter lab
```

### Option 3: Local Book Build
To build and view the complete book locally:

1. Install the required dependencies:
```bash
pip install -r requirements.txt
```

2. Build the book:
```bash
jupyter-book build .
```

3. View the book by opening `_build/html/index.html` in your browser

## Self-Teaching Guide

If you're using this book for self-study rather than as part of a formal class, here's how to make the most of it:

### Lesson Structure

Most lessons in this book follow a consistent structure:

1. **Overview**: A brief introduction to the topic and its relevance to marine computer vision
2. **Learning Objectives**: Clear statements of what you should be able to do after completing the lesson
3. **Content**: Core concepts and explanations, often with visual examples
4. **Code Examples**: Practical implementations demonstrating the concepts
5. **Exercises**: Mini challenges to test your understanding
6. **Additional Resources**: Links to further reading and related topics

### Types of Lessons

The book contains several types of content:

1. **Markdown (.md) files**: Conceptual lessons focused on explaining principles and ideas
2. **Jupyter Notebooks (.ipynb)**: Interactive lessons with executable code that you can run and modify
3. **Project Files**: Complete workflows demonstrating real-world applications

### Progressive Learning Path

The book is organized into chapters that build upon each other:
- **Chapter 1**: Introduces marine imaging and AI fundamentals
- **Chapter 2**: Covers computer vision basics like image annotation and augmentation
- **Chapter 3**: Dives into specific CV techniques (classification, object detection, etc.)
- **Chapter 4**: Guides you through a synthesis project applying what you've learned

### Working Through Mini Exercises

Each lesson includes mini exercises that reinforce learning. For most effective self-teaching:

1. **Read through the lesson**: Understand the concepts before attempting exercises
2. **Try exercises independently**: Attempt to solve problems before looking at solutions
3. **Experiment with code**: Modify examples to deepen understanding
4. **Complete end-of-lesson challenges**: These consolidate what you've learned

### Implementation and Practice

For most beneficial self-learning:

1. **Maintain a personal project**: Apply techniques to your own marine imagery dataset
2. **Create implementation notebooks**: Document your experiments and findings
3. **Build incrementally**: Start with simpler techniques before attempting advanced ones
4. **Review and troubleshoot**: Debug issues to build problem-solving skills

### Using External Resources

Supplement your learning with:
1. **Documentation**: Refer to official libraries documentation (PyTorch, Tensorflow, etc.)
2. **Academic Papers**: Many lessons reference original research papers
3. **Community Forums**: Sites like Stack Overflow for troubleshooting
4. **Video Tutorials**: For visual explanations of complex concepts

Remember that computer vision is a hands-on field - the more you practice implementing these techniques, the better you'll understand them.

## Development Tools

### Hugging Face Integration
We'll be using Hugging Face for model sharing and deployment. Key components:
- **Transformers Library**: For accessing and using pre-trained models
- **Datasets Library**: For accessing and sharing datasets
- **Model Hub**: For sharing trained models with the community

### Jupyter Notebooks
- Most lessons are in Jupyter notebook format (`.ipynb`)
- Can be run in Colab, locally, or viewed in the book
- Include both explanatory text and executable code cells

## How to Get Help

1. **Course Issues**: Use the GitHub repository's Issues page
2. **Direct Contact**: Email attcart@uw.edu for any help or questions related to the course
3. **GitHub Discussions**: Ask questions on the GitHub repository's Discussions tab for community support
4. **Technical Problems**: 
   - Check the Tools Troubleshooting section in the book
   - Ask questions in the GitHub Discussions
   - Open an issue with detailed error information

## Contributing

We welcome contributions to improve the course materials! Here's how:

1. **Report Issues**
   - Use the GitHub Issues page
   - Include detailed description and steps to reproduce
   - Add relevant labels (bug, enhancement, documentation, etc.)

2. **Suggest Improvements**
   - Open a discussion on GitHub
   - Describe your proposed changes
   - Link to relevant issues or materials

3. **Submit Changes**
   - Fork the repository
   - Create a new branch for your changes
   - Make your changes following our style guidelines
   - Submit a Pull Request with a clear description
   - Link relevant issues