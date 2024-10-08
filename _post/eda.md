---
layout: post
title: "A Peek Through the Clouds"
date: 2022-11-14
author: Drew Millane 
introduction: Exploratory data analysis on weather in Provo
read_time: true
image: 
  path: https://miro.medium.com/max/800/1*swBI4_YaHnMIwOvz3rGuFQ.jpeg
  thumbnail: https://miro.medium.com/max/800/1*swBI4_YaHnMIwOvz3rGuFQ.jpeg
  caption: "Photo from [Pexels](https://www.pexels.com)"

---
 
# Introduction 
John Tukey once said, “The greatest value of a picture is when it forces us to notice what we never expected to see”. There is something beautiful about taking a hodgepodge of data and transforming it into a story. What we could not see is now unveiled for us to discover and ask more questions. In other words, exploratory data analysis propels curiosity and discovery. Excitedly, this power can be harnessed with a few lines of code. 

As discussed in my previous blog post, weather patterns have a major impact on all we do, especially in this time of drought. With the scraped data we get to split the clouds and explore! In this post, I have shared five graphics that help display patterns in our dataset, each with a question in mind. I'll leave a link to my repository below. Let’s begin! 

## How Much Precipitation Did Provo Get This Year? 

The graph below displays the amount of precipitation Provo received in inches starting at the beginning of this year. From what we can see at first glance, it hasn’t rained very much here in Provo. The highest is 0.6 inches per hour, which is considered heavy rainfall. What is surprising is that this rainfall occurred in June not in April, which is the month that I would have assumed to have the heaviest rainfall. It appears that rainfall occurs most often in March-April and September-October, ranging from light to moderate rainfall. It would be fascinating to see this graph compared to a similar graph of rainfall from a wetter state, like Oregon. 

<p align="center">
<img src="https://github.com/amillane/stat386-projects/raw/main/assets/images/rain.png" width = "700" height='400'>
</p>

## What Is The Trend of Temperature For This Year?

This graph is another line plot that displays the temperature over time. As expected, the overall temperature  increases we get closer to summer and decreases as we approach winter. What is interesting is the variability of temperature from January to June. It varies a lot between these two months, but the variability starts to decrease after June and stays consistent. Why is that? I’m curious to see if this pattern of variability is consistent in years past. 

<p align="center">
<img src="https://github.com/amillane/stat386-projects/raw/main/assets/images/temp.png" width = "700" height='400'>
</p>

## Is There A Relationship Between Cloud Cover and Precipitation? 

To answer this question, I created a scatter plot to see whether there is a relationship between the amount of cloud cover and precipitation. My intuition led me to suspect that more cloud cover brings more rain. However, the graph seems to show only a weak positive relationship. Higher rainfalls are occurring more with higher cloud cover, but not enough to say there is a strong relationship. 

<p align="center">
<img src="https://github.com/amillane/stat386-projects/raw/main/assets/images/cloudcover.png" width = "700" height='400'>
</p>

## Is There a Relationship Between Windspeed and Temperature Difference?

For this scatterplot, I created a new variable that took the difference between the actual temperature and what the temperature felt like outside. This was done so that I could see whether windspeed increased the difference between the two. From this plot, there appears to be a weak positive relationship. I’m curious to see if this correlation would change if we had more data. 

<p align="center">
<img src="https://github.com/amillane/stat386-projects/raw/main/assets/images/feellike.png" width = "700" height='400'>
</p>

## Overall, What Are The Relationships Between Variables?

To get an overall idea of the correlation of the variables in the dataset, I selected the ones I found most important and created a heatmap. There seems to be both positive and negative relationships, but most are fairly weak. 


<p align="center">
<img src="https://github.com/amillane/stat386-projects/raw/main/assets/images/matrix.png" width = "500" height='350'>
</p>


# Conclusion 

After having gone through these five graphics, I hope you have gotten a sweet taste of exploratory data analysis as I have! All in all, the most interesting thing I found was a strange variability in temperature late in the year. What does this mean compared to past years? Is this a trend of climate change or just a natural occurence? These are the questions that we can answer to create a story with the data we have!

What did you find most interesting? Let me know in the comments below!

[Link to Repository](https://github.com/amillane/Provo_Weather-)
