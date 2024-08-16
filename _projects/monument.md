---
layout: post
title: "The Monument Project"
date: 2024-08-16
author: Drew Millane
introduction: Working alongside the Lord Mayor of London, City, University of London, and Imetrum, we began uncovering answers to questions about the Great Fire of London monument created by scientist Robert Hooke.
read_time: true
image: 
  path: https://github.com/amillane/amillane.github.io/blob/main/public/test.jpeg?raw=true
  thumbnail: https://github.com/amillane/amillane.github.io/blob/main/public/test.jpeg?raw=true
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

# Data Collection 

# Exploratory Data Analysis 

## FFT

# Next Steps

