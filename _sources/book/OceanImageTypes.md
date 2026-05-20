# Ocean Image Data

## Overview

In this lesson, we will explore **marine photographic and videographic data**. We will also discuss the tools used to capture these images and videos, such as **underwater cameras**, **drones**, and **satellite imagery**. Understanding the types of imagery available is the first step toward understanding what questions can be answered with **Marine Imagery** and **Computer Vision (CV)** techniques.

### Learning Objectives

By the end of this section, you will:
- Understand the different types of ocean image data.
- Identify the tools and techniques used to capture marine imagery.

---

## Tools for Capturing Ocean Image Data

Ocean image data comes from a variety of sources, each with unique strengths and limitations. Let's explore different types of ocean image data and the tools used to capture them.

### Underwater Imagery

Underwater imagery capture is nearly as diverse as the environments that make up the oceans. Each capture type has different advantages and disadvantages, understanding these are key to knowing what questions can be answered by a particular set of images. Some studies utilize multiple image capture types and imagery can be captured to complement other sampling types.

Underwater imagery capture falls into several broad categories including **diver-held cameras**, **simple camera platforms**,  and **underwater vehicles**.

#### Diver-Held Cameras
In shallow waters underwater imagery collection can be as simple as outfitting a diver with a camera. These divers can be free swimming or towed by a small craft. 

:::{figure} images/SF9.1A-Papahanaumokuakea.jpg
:name: diver_camera
Marine biologists use photoquadrats to record species diversity and percent coral cover underwater at the Papahānaumokuākea Marine National Monument in Hawai‘i. Credit: National Oceanic and Atmospheric Administration (NOAA)
:::

**Advantages of Diver-Held Cameras**:
- Highly responsive - human operator can adjust the image quality, field of view, positioning and lighting on the fly in response to water conditions and observations.

**Drawbacks of Diver-Held Cameras**:
- Limited to depths accessible by human divers.
- Limited to time a human diver can stay in the water.

#### Simple Camera Platforms

##### Drop Cameras
Drop cameras are used to collect images of the seafloor at a point location. They were used to collect some of the earliest images of the seafloor. The cameras can collect video and digital still imagery. They are commonly used in habitat mapping as a method of ground truthing either acoustically collected data or model predictions.  

:::{figure} images/Drop-camera.JPG
:name: drop_camera
Drop camera used by the Commonwealth Scientific and Industrial Research Organisation (CSIRO) being put overboard. Credit: CSIRO
:::

**Advantages of Drop Cameras**:
- Low cost.

**Drawbacks of Drop Cameras**:
- Limited to specific, small areas.

##### Towed Camera Systems

Towed camera systems can capture images or video transects at the seafloor or in the midwater. They are towed behind a ship and control of the platform comes from a winch and wire. Depending on the system they may or may not stream live data back to the ship via their wire. They are generally considered to be "simple mobile platforms" in contrast to the higher mobility of underwater vehicles. Towed cameras can be part of their own sled or integrated into other sampling equipment such as epibenthic sledges, plankton nets, and coring systems. 

:::{figure} images/DTIS_NIWA.jpg
:name: towed_camera
The Deep Towed Imaging System (DTIS) being deployed from the RV <em>Tangaroa</em> in the Ross Sea, Antarctica. DTIS is owned and operated by the National Institute of Water and Atmospheric Research (NIWA) in New Zealand. Credit: Dave Bowden, NIWA
:::

**Advantages of Towed Camera Systems**:
- Much lower cost in comparison to HOVs, ROVs, and AUVs
- High-definition video and still imagery.

**Drawbacks of Towed Camera Systems**:
- Less responsive then HOVs and ROVs to real-time information
- Limited in mission capability compared to HOVs and ROVs

##### Fixed Underwater Cameras

There is a lot of variety in fixed underwater camera platforms. 

Generally, these cameras are positioned on the seafloor or on a mooring at a fixed depth for long-term monitoring. They can be placed by an underwater vehicle, lowered into position via a wire, or allowed to free fall from a ship. Fixed cameras on the seafloor are often deployed on tripods or benthic landers. These systems can be used to monitor the natural environment or focused on an experimental setup, such as settlement plates and whale bone colonization (Segal et al., 2012; Smith et al., 2024). 

A common type of fixed camera are baited remote underwater video systems (BRUVS). These are used to sample fish or other fauna assemblages, gather data on relative abundances, and body size structure. They are considered to be a cost-effective and non-invasive method. Further information on BRUVS can be found in Harvey et al., 2013, Whitmarsh et al., 2017, and Langlois et al., 2020. 

Fixed camera systems can be cabled and uncabled. Uncabled camera systems are limited by their battery life and storage capacity. In contrast, cabled camera systems can send data back in real-time and their sampling routine can be changed to respond to changing conditions. An example of cabled camera systems include the three digital still cameras and one video camera on the **Ocean Observatories Initiative's Regional Cabled Array** which provide live video feeds from deep-sea environments.


:::{figure} images/image.png
:name: underwater-camera
A digital still camera on the OOI Regional Cabled Array capturing methane seep activity. Credit: UW/NSF-OOI/WHOI
:::

**Advantages of Fixed Cameras**:
- Ideal for long-term ecological studies.
- Useful for capturing rare or unpredictable events
- High-definition video and still imagery.

**Drawbacks of Fixed Cameras**:
- Limited to specific, small areas.

#### Underwater Vehicles
##### HOVs (Human Operated Vehicles)
HOVs are crewed submersibles, they typically carry one pilot, a copilot, and one or two scientists. They are highly flexible in the operations they can undertake including collecting real-time video and still imagery and physical sampling. In 2013, there were ten large manned submersibles used by scientific institutions (Smith and Ruhmohr 2013). Notable examples are:

:::{figure} images/Alvin.jpg
:name: HOV_Alvin
Human-occupied vehicle Alvin is recovered onboard after completing its final dive of the 2018 DEEP SEARCH expedition. Credit: DEEP SEARCH 2018 - BOEM, USGS, NOAA.
:::

- **Alvin**: 3-person research submersible operated by the National Deep Submergence Facility (NDSF) at WHOI. Depth rated to 6,500 meters, includes multiple 4K ultra-high definition cameras, able to support custom sensors and samplers.  
- **Shinkai 6500**: 3-person research submersible operated by the Japan Agency for Marine Earth Science and Technology (JAMSTEC). Depth rated to 6,500 meters, includes multiple HD video cameras and manipulators.

**Advantages of HOVs**:
- Real-time control and response similar to that of scuba divers. 
- Deep-water capabilities beyond the reach of divers.

**Drawbacks of HOVs**:
- Diving time restricted by battery and oxygen reserves.
- Risk to human occupants.
- High cost - including significant maintenance infrastructure and trained personnel.

##### ROVs (Remotely Operated Vehicles)

ROVs are uncrewed submersibles controlled from the surface. They are connected to a surface vessel via an umbilical or tether. The tether provides control signals, power, and live feedback from video and other sensors. Due to their connection to the ship, there are no limitations on the length of time an ROV can stay in the water. They provide real-time video and still imagery, often equipped with sampling tools. ROVs can range in size from small to very large and are used at depths between 30 and 6500 m. Notable examples are:

- **Jason**: Capable of deep-sea exploration, reaching depths of up to 6,500 meters. Operated by the National Deep Submergence Facility (NDSF) at WHOI, it includes high-definition video and robotic arms for sample collection.
- **ROPOS**: A Canadian ROV with similar capabilities as Jason, rated to depths of 5,000 meters. Operated by the Canadian Scientific Submersible Facility (CSSF), it includes high-definition video and robotic arms for sample collection.

:::{figure} images/image-1.png
:name: rov-jason
The ROV Jason Equipped with 4k video cameras. Credit: M. Elend, University of Washington
:::

**Advantages of ROVs**:
- Real-time control for targeted exploration.
- Long dive times compared to HOVs and AUVs.
- Highly flexible, adaptable to many mission types.

**Drawbacks of ROVs**:
- Limited mobility due to tethers.
- High cost - including significant maintenance infrastructure and trained personnel.

##### AUVs (Autonomous Underwater Vehicles)

AUVs are pre-programmed to follow missions autonomously, capturing imagery without human intervention. A prime example is **Sentry**, which collects data at depths of up to 6,000 meters. AUVs deployments are only limited by the size of their onboard batteries which now commonly can operate for up to 24 hours at time. 

:::{figure} images/image-3.png
:name: auv-sentry
AUV Sentry at the surface after a deep dive. Credit: WHOI
:::

**Advantages of AUVs**:
- Autonomous, covering large areas efficiently.
- Can be fitted with multiple sensors.
- Stability in water column.
- Long deployment times compared to HOVs.

**Drawbacks of AUVs**:
- Limited real-time control.
- Short deployment times compared to ROVs

```{seealso}
If you are interested in learning more about methods of underwater marine imaging there are several good reviews on the topic including {cite:t}`Jamieson2013`, {cite:t}`Mallet2014`, and {cite:t}`Durden2016`.
```

### Satellite Imagery

Satellites offer broad coverage for observing large-scale oceanographic phenomena, such as **phytoplankton blooms** and **sea surface temperature**.

:::{figure} images/image-2.png
:name: satellite-imagery
Satellite imagery showing a phytoplankton bloom. Credit: NASA Goddard Space Flight Center, Ocean Ecology Laboratory
:::

**Advantages of Satellite Imagery**:
- Global coverage and ability to monitor seasonal changes.
  
**Drawbacks of Satellite Imagery**:
- Lower resolution compared to underwater systems, making it less ideal for studying small-scale features.

### Aerial Imagery

Aerially mounted cameras on drones, planes and helicopters can produce high quality (often georeferenced) imagery of nearshore sites or areas that are hard to get to by other means of transport, like sea ice.

:::{figure} images/image-4.png
:name: seal-population
Seals on Arctic sea ice captured with an aerial camera, annotated with a CV model. Credit: A. Carter, NOAA National Marine Mammal Laboratory
:::

**Advantages of Aerial Imagery**:
- Large georeferenced imagery can be obtained in relatively quick transects.
- Drone captured imagery can be very inexpensive 
  
**Drawbacks of Aerial Imagery**:
- Aerial imagery doesn't capture underwater information, and can be very expensive to operate.
