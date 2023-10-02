# Step 2: Data Exploration  

The first thing to do is to understand the data you are working with. 

In the folder you downloaded on the previous page there are 3 types of files: 
- "Lakes_Reservoirs_database.csv": this is the original lakes database: each line represents a measurement of EC at a given time and location. Each location has an X and Y and a unique location name. You'll see that for a lot of points we have multiple measurements, spanning over multiple years. In our case, we want to compare data from 1980-1990 to those measured in the timespan of 2005-2015. We only want to consider those points who have data for at least 5y in both epochs. If we filter only these points, and then average all the measurements for these points we end up with the pre-processed csv file: 
- "lakesaverage.csv": this file thus contains only those points which have sufficient measurements in both epochs. First, we calculated for each station the average EC in each year: those stations which have at least 5 years of measurements in both epochs are considered: for those stations, the average over the two epochs was calculated: do you see in which columns these values now appear? What is the DIFF column? 
- "lakes.cpg,dbf,prj,shp,shx": this is the shapefile that basically contains the information of lakesaverage.csv but now in a spatial data object. 



>Unzip the folder you just [downloaded](https://canvas.uva.nl/courses/32040/modules/items/1502508) and open both csv files in R and start a new R script. If you are not sure on how to do that: consult the previous practical [here](https://liesjacobs.github.io/worldfoodecosystems2023/practical2/Rstudio.html)

The original database might take a while to load (why?). The lakesaverage.csv should load quickly. 

>Step1: exploring the data: 

```r
#let's check out this 'DIFF' column
difference<- boxplot(lakesaverage$DIFF, outline = F)

```


***

Now, let's do a first check: how well are the EC values in the second epoch associated with those in the first epoch?  

```r
plot(?, ?) #give the axis correct names

#theoretically, if no change occurs, they should more or less lie on the 1:1 line (the diagonal): let's plot this: 
abline(a=0, b=1,col="red")
legend(x = 2, y=1000, legend = c("diagonal line"), col=c("red"), lty=1)



```
Because of the two outliers, it's a bit difficult to see what goes on with the majority of the points... 
### Build the same plot, but now limit the x and y axis to '2500', give the axis appropriate names and adjust the legend so that it still falls within the plot. Personalize this graph by adding a title with your name and load this graph to the canvas quiz.

If you don't know how to do this by heart, google is your best friend. I found e.g. [this resource](https://statisticsglobe.com/set-axis-limits-in-r)


Visually, do you conclude that most points have increased or decreased in their EC content? Does this confirm what you saw in the boxplot? 





**OK, now on to the GEE processing: let's extract data on water deficit and the biomes for each point [in the next exercise](https://liesjacobs.github.io/worldfoodecosystems2023/practical3/Mapping.html)


<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/exploring.html">Practical 1: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/understandinggradients.html">Practical 1: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/intro.html">Practical 5: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/QGIS.html">Practical 5: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/Rstudio.html">Practical 5: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/intro.html">Practical 6: Problem description</a></li>
    <li><strong>Practical 6: Data Exploration</strong></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/Mapping.html">Practical 6: Exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/Analysis.html">Practical 6: Exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>
