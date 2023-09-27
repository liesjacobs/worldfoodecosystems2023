# Exercise 3: exporting the attribute data and importing in Rstudio

So... we want to export the attribute data of our shapefile to a file that can be imported in Rstudio, e.g. a .csv file. 

Luckily, that's quite straightforward: 

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131507742-01689c6d-726c-4cfa-9853-ea126a24a054.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>







### Great: so now we can shift to Rstudio

Open Rstudio. If all goes well, it should look something like this: 

![Rstudio_Home](https://user-images.githubusercontent.com/89069805/131488428-fe3591d5-2cd0-4107-8dd1-84b4aafe883b.png)

With on the left the console (where you execute single commands), on the top right a panel where you can see the variables in the 'environment'. In the bottom right you can see the plots you make, but also e.g. the packages that are installed, or the help function. 

We now have to do two things: 

1. Import the dataset
2. Build an R script


### Importing the .csv file we just made

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131488949-4c653f75-fd4a-4cb1-b619-feae62826521.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>



Depending on the settings of your PC, you might need to adjust the decimal notation, or the seperator. 
If all is well, you now see the attribute information in the environment. 



### Building a script

Now we can start analyzing the data. The first thing you need to do is build an empty R file where you will later put the commands you'll use for analysis. 

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131489386-bf1b4aee-c1bc-42d3-a1fa-afc8397c0b7e.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>




We can add simple code, as illustrated in the video below 

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131489891-e0210044-50ad-4361-9fea-1b8e095dbbc7.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

***
Note: In this exercise we are not considering the fact that the samples (watersheds) are spatially correlated: nearby watersheds will resemble each other more than watersheds that are further apart from each other. This is called "spatial autocorrelation". Ignoring spatial autocorrelation (as we do here) could pose a problem in the sense that the observations (waterhsed) are not fully independent of one another, violating the assumption of our statistical test. Dealing with spatial autocorrleation is topic of courses at the master level. 
***

The plot is a bit ugly, so we can pimp it, and also plot the regression line. 
Copy paste the code below and execute it yourself: 

```r
# this is the file where we will list the commands needed for the analysis
# using a hashtag = comments 

# now we can build simple code, to do some analysis

#cleaning data
attributesNDVI_Richness<-na.omit(attributesNDVI_Richness)
attach(attributesNDVI_Richness)

# first, let's plot
library(scales) # if you get an error here, first install package 'scales' using install.packages('scales')
plot(NDVImean, richnessme, xlab = "NDVI", ylab = "Mammal richness", col=alpha("green",0.3), pch = 16)

# then build a regression: 
regression <- lm(richnessme~NDVImean, data = attributesNDVI_Richness)
summary(regression)


# plot regression line on scatterplot
abline(regression, col="black", lwd=2)

text(1000,150, paste("r squared is ", round(summary(regression)$r.squared,2)))
text(1000,140, paste("p-value of NDVI is ", summary(regression)$coefficients[8]))

```

<iframe width="640px" height= "480px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9URFJGOVZBWE5YTURMWVNWV1A4VU9FWkFPMS4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>


After the execution, you should get something like this: 

![example](https://user-images.githubusercontent.com/89069805/131496702-b8d0af27-b702-4175-8525-1ce44975cc2b.png)

<iframe width="640px" height= "480px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9UNkY5REtCVFdJOFJUUE5UWUdOVUNXUzIzNS4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>

<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/exploring.html">Practical 1: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/understandinggradients.html">Practical 1: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/intro.html">Practical 2: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/QGIS.html">Practical 2: exercise 2</a></li>
    <li><strong>Practical 2: exercise 3</strong></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>

