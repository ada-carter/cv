# Computer Vision Across the Marine Sciences

[![Jupyter Book Badge](https://jupyterbook.org/badge.svg)](https://oceancv.org)
[![NSF-Funded](https://img.shields.io/badge/NSF-Funded-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2307504)
[![UW Course](https://img.shields.io/badge/UW-OCEAN_462--C-purple.svg)](https://www.washington.edu)

## Overview

This repository contains the source code and content for the "Computer Vision Across the Marine Sciences" Jupyter Book, a comprehensive educational resource designed to bridge the gap between marine science and computer vision. The book serves as the foundation for the University of Washington course OCEAN 462-C and is publicly available at [oceancv.org](https://oceancv.org).

## About the Book

The book teaches how to apply computer vision techniques to marine science challenges through:

- **Interactive Tutorials**: Hands-on Python notebooks covering data preparation, model training, and evaluation
- **Case Studies**: Real-world applications in marine biology, fisheries, and oceanography
- **Integration with Modern Tools**: Examples using YOLO, PyTorch, TensorFlow, and Hugging Face

Topics covered include:
- Introduction to marine imagery and AI fundamentals
- Image annotation, augmentation, and preprocessing
- Classification, object detection, segmentation, and tracking
- Model deployment with Streamlit
- Dataset discovery and selection
- Model sharing and documentation

## Usage

### Online Access
Visit [oceancv.org](https://oceancv.org) to access the interactive web version of the book.

### Local Setup
To build and explore the book locally:

1. Clone this repository:
   ```bash
   git clone https://github.com/ada-carter/cv.git
   cd cv
   ```

2. Create a conda environment:
   ```bash
   conda env create -f environment.yml
   conda activate cv
   ```

3. Build the book:
   ```bash
   jupyter-book build .
   ```

4. View the book:
   ```bash
   # Open _build/html/index.html in your browser
   ```

## Contributing

We welcome contributions! Please see our [contributing guidelines](CONTRIBUTING.md) for more information on how to report issues, submit feature requests, or contribute code.

## Authors and Maintainers

- **Ada Carter & Katie Bigham** - Primary Authors
- **Sasha Seroy** - Supervisor
- **Mikelle Nuwer** - Supervisor

## License
## License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE file for details.


## Acknowledgments

This book was developed with the support of the National Science Foundation under Grant No. OCE-2307504. Any opinions, findings, and conclusions expressed in this material are those of the authors and do not necessarily reflect the views of the National Science Foundation.

