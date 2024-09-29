---
layout: post
title: "The Monument Project"
date: 2024-08-16
author: Drew Millane
introduction: Working alongside the Lord Mayor of London, City, University of London, and Imetrum, we began uncovering answers to questions about the Great Fire of London monument created by scientist Robert Hooke.
read_time: true
image: 
  path: https://github.com/amillane/amillane.github.io/blob/main/public/test.jpeg?raw=true
---



# Introduction 

In 1666, a massive fire known as The Great Fire of London devastated a significant portion of the city, reducing much of it to ashes. Despite the destruction, London and its people persevered. To commemorate their resilience, a monument was erected in 1677 called The Monument.

The Monument was designed by the English scientist Robert Hooke, in collaboration with architect Christopher Wren. Standing approximately 202 feet tall with 313 steps, it is located near the original source of the fire in the heart of London. Beyond serving as a memorial, The Monument also functioned as a giant zenith telescope, reflecting Hooke's expertise in physics and astronomy. He intended it to be a place for conducting experiments and observing the stars.

Hooke aimed to prove the heliocentric theory by measuring the stellar parallax of the star Gamma Draconis. However, according to him, the vibrations from the bustling city interfered with the accuracy of his observations, making The Monument an unsuitable instrument for this purpose.

This summer, I had the opportunity to work with Imetrum, a company specializing in non-contact measurement systems. One of the projects I contributed to focused on The Monument. Using their product, Video Gauge (VG), we monitored The Monument to evaluate its structural integrity, gathering insights that bridge the 17th century with modern analysis. This project was conducted in collaboration with the Lord Mayor of London and the University of London. In this post, I provide an overview of the measurement approach, the enabling software, and present the results, challenges, and potential directions for further research.

# Video Gauge and Digital Image Correlation #

Digital Image Correlation (DIC) is a displacement measurement technique that the tracks the movement of a point of interest on a surface. It is carried out by taking photographs at different stages and comparing them. It accomplishes this by searching for matched points between the first and second photo, usually characterized by unique light intensity or an artificial pattern on the object.

To match points between two different images, DIC utilizes pixel values and minimizes a selected correlation function to identify the best match. Various correlation functions can be chosen to achieve the desired results. Imetrum has their own patented algorithm that we used for this project. 

A key advantage of DIC is that certain software techniques enable sub-pixel resolution, allowing for high-precision measurements. This capability allows for accurate displacement measurements within a fraction of a pixel.

The VG software allows for measurements of any object under a micron of resolution. Furthermore, it has the added benefit of being in video format with measurements in real-time. Using the proper cameras and lenses VG will be able to detect minuscule movement The Monument is making, fraction of a milimeter.

# Equipment
The equipment that was utilized for the monument measurement was an Imetrum DMS system that incorporates Video Gauge (VG), two cameras, 4 lenses, and a surveyor tripod. The cameras used had a sampling of 120 Hz, which means that the DMS stored 250 MB of data every second the camera was recording. 

# Data Collection 
Data was collected on eight separate occasions at different locations and times around The Monument. The goal was to collect data before, during, and after the lunch break to capture the transition of people being in the monument. As shown in this image from Google Maps, there were three sites where the structure was recorded.

![This map gives an idea of where we recorded data around The Monument](https://github.com/amillane/DataTrailblazing/blob/master/assets/images/monument/Monument%20Project%20Map.png?raw=true "Site Locations")

## Site Descriptions

- **Site 1**: Located on the north face, directly under the structure. Videos taken underneath the top square base.
- **Site 2**: Positioned 100 meters away from the east face. Videos taken of the column portion of the structure.
- **Site 3**: Relatively close to Site 2, at a different viewpoint southwest of Site 2. Videos taken of the columns of the structure.

## Recording Information

| Site | Time  | Rec Length (sec) | Target Distance (m) |
|------|-------|------------------|---------------------|
| 1    | 12:05 | 100              | 62                  |
| 1    | 12:10 | 100              | 62                  |
| 1    | 12:19 | 137              | 62                  |
| 1    | 12:34 | 603              | 62                  |
| 2    | 13:56 | 201              | 118                 |
| 2    | 14:02 | 136              | 118                 |
| 2    | 14:21 | 215              | 118                 |
| 3    | 14:43 | 1092             | 114                 |


# Exploratory Data Analysis 
Data acquired from Site 1 gathered displacement on both the x and y axis over time. Site 2 and 3 measured the rotation of the column portion of the monument from north to south.

![Measuring the base of The Monument ](https://github.com/amillane/DataTrailblazing/blob/master/assets/images/monument/Capture.PNG?raw=true)

Figure 3 shows the rotation measurements taken at Site 3. Between 0 and 1000 seconds, there is a consistent signal that fluctuates between -0.01 and 0 pixels. Although this consistent signal appears throughout the measurement, it is intermittently interrupted by noise, such as the disturbance observed at 600 seconds.

This phenomenon was present in all the samples taken on that day. The goal is to remove these noise-affected areas to enable a more accurate analysis of the monument's movement.

!Rotation measurements from Site 3
*Figure 3: Rotation measurements from Site 3*

# FFT

To undertake this, the Fast Fourier Transform (FFT) was implemented. FFT is a measurement method that converts a signal in the time domain to the frequency domain. In other words, this allows for individual frequencies to be detected throughout the signal. 

In the case of the Monument, the goal of this project is to be able to measure what frequency the structure is moving. With FFT's we can remove the noise frequencies from wind, foot traffic, and other inhibitors that may affected the camera. The graph below shows the frequencies of Site 3 in the frequency domain. 


# Next Steps

This analysis only scratches the surface of the full story. The next steps in the project involve exploring why the Monument consistently resonates at 0.75 Hz. Could it be due to the vibrations from the London Underground? Or perhaps the metal railings inside the structure? If this is the true natural frequency of the Monument, why did Hooke abandon his attempt to prove stellar parallax? Although I wasn't in England long enough to see the project through, the team continues to explore these questions and uncover new ones.

It was a great pleasure to collaborate with Imetrum and witness their impressive technology in action. I'm grateful for the opportunity to contribute my statistical and data analysis skills to such an exciting project. Go visit their [website](https://www.imetrum.com/) and check out their innovative technology!
