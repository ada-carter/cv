# Marine Imaging

## Overview
This section provides a non-comprehensive review of the field of marine imaging. We will explore the history of marine imagery, the types of research that utilize imagery in marine environments, and some challenges inherent to using imaging techniques underwater.

### Learning Objectives

By the end of this section, you will:

- Understand the historical development of marine imaging techniques and their scientific relevance.
- Identify various types of marine research that rely on imagery, including ecological surveys, habitat mapping, and behavior studies.
- Recognize the primary challenges associated with capturing and analyzing marine imagery, including environmental and technical hurdles.

---

## Introduction
The use of imagery in marine science has a long tradition, both for scientific inquiry and for engaging the public in the wonders of the ocean. Underwater photography began as early as the mid-19th century and has evolved into one of the most critical tools for studying the ocean's depths. The non-destructive nature of imaging makes it invaluable for visualizing marine flora, fauna, geologic formations, human-made infrastructure, and even marine litter across vast spatial scales. As shown by {cite}`Kocak2005current`, advancements in underwater imaging technology have significantly improved our ability to document anthropogenic structures such as shipwrecks and submerged infrastructure, as well as track and quantify marine litter. These developments not only enhance scientific research but also support conservation efforts by providing detailed visual evidence of human impact on marine environments.

The first documented underwater photographs were taken in 1856 using a pole-mounted system {cite}`Baker1997`. These early images laid the foundation for modern marine imaging, which now encompasses an array of advanced platforms equipped with high-resolution cameras capable of capturing data in various formats, from still images to video and hyperspectral data. Technological advancements in cameras, lighting, and data storage have enabled researchers to gather terabytes of imagery data from environments ranging from shallow coastal waters to the deep sea (see [Ocean Image Data](book/OceanImageTypes.md).)

As imaging technology continues to evolve, the volume of imagery data has grown exponentially. This creates both opportunities and challenges. On the one hand, researchers can access unprecedented visual data to study marine ecosystems. On the other hand, processing and analyzing such vast datasets is a significant bottleneck. Currently, manual annotation by trained specialists is the primary method for extracting meaningful data from images. However, as noted by {cite}`Schoening2016`, this traditional approach is becoming increasingly impractical due to the sheer scale of modern image collections.

---

## Types of studies that use marine images

Marine imagery is utilized across a wide range of scientific disciplines, either as a primary data source or in combination with other data types. According to {cite}`Kocak2005current`, different imaging technologies serve specific purposes in marine research, from high-resolution cameras used for benthic surveys to multi-spectral and hyperspectral imaging systems that capture detailed environmental data. Kocak and Caimi categorize these technologies based on their applications, such as habitat mapping, behavioral studies, and pollution monitoring, reviewing the versatility and evolving capabilities of underwater imaging systems in addressing diverse scientific questions. These studies vary in scope from exploratory surveys to hypothesis-driven research, and they can encompass both spatial and temporal analyses. Below are some of the key types of studies that rely on marine imagery:

### Ecological Surveys

Marine imagery is widely used in ecological surveys to assess the distribution, abundance, and diversity of marine organisms. Images can capture snapshots of marine life in its natural habitat, providing valuable data for biodiversity assessments and monitoring changes in marine ecosystems over time. For example, time-series image data collected from fixed cameras on the seafloor can reveal seasonal patterns and long-term trends in benthic communities. Traditionally, this process is carried out through manual annotations by humans, a method that remains in use among some scientists. However, the advent of automated identification systems has started to transform this practice by streamlining workflows and improving the accuracy of data extraction. As shown by {cite}`piechaud2019automated`, advancements in computer vision have made it possible to automate the identification of benthic epifauna, significantly reducing the time and effort required for manual annotation while increasing the accuracy and consistency of species identification. 

### Habitat Mapping

Imagery is essential for mapping marine habitats, particularly in areas that are difficult to access using traditional sampling methods. As discussed in {cite}`ventura2018mapping`, the use of Unmanned Aerial Vehicles (UAVs) combined with Object-Based Image Analysis (OBIA) has proven highly effective in mapping ecologically sensitive marine habitats. Ventura et al. (2018) demonstrated that UAVs equipped with high-resolution cameras can provide detailed imagery of coastal environments, including seagrass meadows and biogenic reefs, at a fraction of the cost and effort required by traditional methods. Their study highlighted the capability of OBIA to classify habitat types based on texture, color, and spatial relationships within the images, allowing for accurate and efficient habitat mapping. The research further showed that this approach could identify changes in habitat structure over time, making it a valuable tool for monitoring and conservation efforts. By utilizing UAV imagery and advanced analytical techniques, researchers can achieve high spatial accuracy in habitat classification, even in areas where fieldwork is challenging or impractical. Similarly, AUVs and ROVs can produce high-resolution images that can be used to create detailed maps of interesting seafloor features as demonstrated by {cite}`katie2016`. Creating large-area photomosaics using high-resolution imagery allows researchers to map expansive regions of the seafloor accurately. These mosaics can then be imported into Geographic Information Systems (GIS) for further spatial analysis and visualization. By aligning and stitching individual images into coherent photomosaics, researchers can achieve a continuous view of the underwater environment, essential for identifying habitat boundaries and assessing spatial distribution of benthic fauna. 

### Animal Behavior Studies

Marine imagery allows researchers to observe and document animal behavior in situ without disturbing the organisms. Video footage from remotely operated vehicles (ROVs) or stationary cameras can capture feeding behaviors, mating rituals, predator-prey interactions, and other activities that provide insights into the lives of marine species. As shown by {cite}`burns2024situ`, recent advancements in in situ digital synthesis strategies have greatly enhanced the accuracy and depth of behavioral studies in the marine environment. Burns et al. introduced a multi-faceted approach that integrates real-time video recording, 3D laser imaging, and genetic analysis to document intricate behaviors of fragile mid-water fauna like gelatinous zooplankton.

Their study showcases how high-resolution imaging systems can capture detailed behavioral interactions, such as predation and reproduction, in situ, preserving the organisms' natural responses. This approach is particularly beneficial for species that are difficult to study in laboratory settings due to their sensitivity to handling and environmental changes. The incorporation of automated tools for behavioral tracking has also improved the efficiency of analyzing large datasets, allowing for more comprehensive behavioral assessments across different temporal and spatial scales.

Furthermore, Burns et al. demonstrated the use of imaging platforms like the RAD-2 system to hold specimens in their natural state, minimizing stress-induced behavioral changes during capture. This breakthrough ensures that researchers can observe authentic behaviors without the issues commonly introduced by traditional collection methods. Such technologies represent a significant step forward in marine behavioral studies, providing insights into the dynamic interactions of marine life and contributing to broader ecological understanding.

### Geologic and Physical Oceanography

Marine imagery plays a crucial role in both geologic and physical oceanography by enabling researchers to map underwater features and monitor dynamic ocean processes like sediment transport and surface currents. High-resolution images from satellites and underwater vehicles provide insights into seafloor structures, volcanic activity, and ocean circulation patterns, informing everything from current movement to the presence or absence of geologic activity.

A notable method used in physical oceanography is computing coastal surface currents from satellite imagery through the Maximum Cross-Correlation (MCC) technique. According to {cite}`liu2017computing`, this approach tracks distinct features in sequential satellite images, such as temperature gradients and color differences, to measure the speed and direction of surface currents. By merging data from thermal infrared (IR) and ocean color (OC) sensors like MODIS and VIIRS, researchers can achieve broader spatial coverage and improved accuracy in current detection.

Similarly, geologic studies benefit from marine imagery by allowing researchers to map seabed sediments and characterize seafloor habitats. As shown by {cite}`diesing2014mapping`, various techniques such as geostatistical modeling and machine learning, can be applied to classify seabed types from both multibeam echosounder data and imagery. These methods can enhance our understanding of sediment distribution and geological processes, informing marine spatial planning, habitat protection, and resource management.

### Pollution and Marine Litter Studies

Marine plastic pollution is a growing concern due to its widespread ecological and socio-economic impacts. Recent advancements in remote sensing and machine learning have revolutionized the ability to detect and monitor marine litter. Hyperspectral imaging, combined with deep learning algorithms, has proven effective for high-resolution identification of floating plastic debris.

One promising approach is hyperspectral sensing from aerial platforms, as demonstrated by {cite}`balsi2021high`. This study utilized drones equipped with hyperspectral cameras to capture detailed images of marine environments. Hyperspectral sensors, which can capture hundreds of spectral bands across visible and near-infrared wavelengths, are capable of distinguishing plastic materials from other floating objects based on their unique spectral signatures. By applying machine learning models to these spectral data, Balsi et al. achieved high accuracy in detecting marine plastic litter, even in challenging conditions where debris is partially submerged or mixed with organic material.

Similarly, {cite}`bhanumathi2022marine` explored deep learning techniques for marine plastic detection. The study employed convolutional neural networks (CNNs) to classify marine debris in images collected from aerial and satellite platforms. Their approach demonstrated that deep learning models could be trained to recognize various types of plastic waste, including bottles, bags, and fishing nets, with a high degree of accuracy. The integration of CNNs with remote sensing data provides a scalable solution for monitoring marine pollution across large geographic areas.

Both studies highlight the importance of developing automated detection systems to address the growing problem of marine litter. High-resolution imagery and advanced analytics enable researchers to track the distribution and movement of plastic debris over time, providing critical data for policy-making and cleanup efforts. The combination of hyperspectral imaging and machine learning represents a powerful tool for tackling marine plastic pollution, offering the potential for continuous, large-scale monitoring of marine environments.

Later in this course, we will build on these methods by developing an object detection model specifically trained to identify common types of plastics that could be floating on the ocean. Our dataset will stay normally-spectraled as we do not have multispectral money.

## Challenges for imaging in the marine environment

Despite its many advantages, marine imaging presents several inherent challenges. These can be broadly categorized into optical challenges and environmental challenges:

### Optical Challenges

Underwater imaging is affected by the unique optical properties of water. Light behaves differently underwater than it does in air, which can result in reduced visibility, color distortion, and scattering of light. These issues make it challenging to capture clear, high-resolution images in marine environments. Factors that contribute to optical challenges include:

- **Light Attenuation**: Water absorbs light, particularly in the red and yellow wavelengths, which limits the distance that light can travel underwater. According to {cite}`Kocak2005current`, the current art of underwater imaging has evolved significantly over time, with advancements in lighting systems, camera platforms, and image processing that have enhanced marine research capabilities. Addressing these limitations requires advanced imaging technologies such as specialized lighting systems and wavelength-specific filters designed to maximize visibility in underwater environments. Their work highlights innovations like LED-based lighting and multi-spectral cameras, which help mitigate the impacts of light absorption and enhance image clarity at greater depths. This affects the clarity and color accuracy of underwater images.

- **Turbidity**: Suspended particles in the water can scatter light and reduce visibility, making it difficult to capture clear images in areas with high sediment loads or plankton blooms.

- **Backscatter**: Particles in the water can cause backscatter, a phenomenon where light reflects off particles and returns to the camera, resulting in image noise.

### Environmental Challenges

In addition to optical challenges, environmental factors can also affect the quality and usability of marine images. These include:

- **Pressure and Depth**: Cameras and other imaging equipment must be designed to withstand the extreme pressures found at great depths. Failure to do so can result in equipment damage and data loss.

- **Temperature Variability**: Marine environments can have wide temperature ranges, from near-freezing temperatures in the deep sea to warm tropical waters. Imaging equipment must be able to operate reliably across these temperature extremes.

- **Biofouling**: Over time, marine organisms such as algae, bryozoans, barnacles, and biofilms can accumulate on cameras and other imaging equipment, reducing image quality. Regular maintenance and specialized materials are required to prevent and mitigate biofouling.

- **Dynamic Conditions**: Ocean currents, waves, and tides can affect the stability of imaging platforms, making it challenging to capture steady, high-quality images.

## Solutions to Imaging Challenges

Researchers have developed various strategies to address these challenges, including:

- **Lighting Systems**: Using specialized underwater lighting systems to enhance visibility and reduce the effects of light attenuation.

- **Image Processing Techniques**: Applying image correction algorithms to address color distortion and backscatter in underwater images.

- **Robust Equipment Design**: Developing cameras and imaging platforms that can withstand harsh marine conditions, including pressure-resistant housings and biofouling-resistant coatings.

As shown by {cite}`marouchos2018challenges`, developing imaging systems for extreme marine environments requires addressing engineering challenges such as pressure tolerance, lighting, and data processing. Modern systems must be capable of capturing high-quality images under low-light and high-pressure conditions while ensuring accurate color reproduction and minimizing backscatter. Stereo imaging systems are particularly valuable for size estimation of marine organisms and require careful calibration to maintain accuracy. The development of automated processing pipelines for these large datasets is essential for making marine imagery more accessible to researchers.

Further advancements in underwater image color correction have been reviewed extensively by Vlachos and Skarlatos {cite}`vlachos2021extensive`. Their work highlights the importance of addressing color distortion caused by light absorption and scattering in water. They discuss various correction methods, including machine learning approaches, to improve the accuracy and clarity of underwater images. These advancements are critical for quantitative analyses and for creating realistic visual representations of underwater environments. I highly recommend checking out either of these papers to learn more about the technical side of camera calibration and the best ways to correct for challenging environments.

---
## FAIR Data Principles in Marine Imaging

The push to make marine image data FAIR (Findable, Accessible, Interoperable, Reusable) is crucial in addressing the challenges posed by increasing data volumes and technical heterogeneity in marine imaging. According to {cite}`schoening2022making`, marine research institutions are increasingly adopting the FAIR principles to ensure sustainable and efficient data management.

Marine images are unique in that they often require large datasets due to the need for high-resolution images to capture detailed observations of underwater environments. The FAIR principles aim to reduce the management overhead of these large datasets by promoting standardized formats, persistent identifiers, and interoperable data-sharing systems.

One of the primary innovations proposed is the concept of Image FAIR Digital Objects (iFDOs). These digital objects standardize the metadata and documentation associated with marine images, ensuring that they can be easily found, accessed, and reused by researchers worldwide. iFDOs also address the issue of missing semantic structure in image data by providing a structured format for annotations and metadata, which can be utilized by machine learning algorithms for automated image analysis.

The development of FAIR-compliant infrastructure environments, as outlined by {cite}`schoening2022making`, is another key component in advancing marine imaging. This infrastructure includes data portals and media asset management systems that enable researchers to share and access marine image data efficiently. By integrating iFDOs with existing research tools such as QGIS and BIIGLE, the FAIR infrastructure environment enhances the interoperability and reusability of marine image data.

In summary, the ongoing efforts to make marine image data FAIR represent a significant step forward in addressing the challenges associated with managing and utilizing large-scale marine imagery datasets. These efforts not only facilitate more efficient data sharing and analysis but also contribute to the broader goal of advancing oceanographic research and promoting sustainable use of marine resources.



