# Exercise 2: opening and analyzing the data in QGIS

Now that we have all the data in our folder, we can open QGIS and load the vector (shapefile) of the watersheds, and the two rasterfiles (the biodiversity tif and the NDVI tif files).

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131482487-7562b9c7-afd5-4512-8ad1-7754e3b532fb.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>





Now we get to the core objective of the exercise: what is the link between biodiversity and available vegetation? 


### To answer this question, we'll need to decide and simplify (see course 1)
* Geographic scale – spatial unit 
* Temporal scale 
* Boundary conditions/assumptions
* Dimensions (which processes and structures will you account for, which not)
* Dimension descriptions (how do you approximate/describe the dimension)

Of course, for the purpose of this exercise, these decisions have already been taken, and are summarized here: 


| building block  |  decision |
|---|---|
| Geographic scale |  Watersheds |
| temporal scale |  similar timespans need to be covered and aggregated over sufficiently large timespan |
| Assumption | more primary producers = more available energy, resulting into a higher biodiversity |
| Dimensions | We'll consider vegetation cover and mammal biodiversity |
| Dimension description | MODIS NDVI (as proxy for vegetation) and mammal species richness by biodiversity.org |



### Now that we have simplified we can take the average species richness and average NDVI per watershed

* we'll use the function *zonal statistics* in the QGIS toolbox to calculate this
* because both are averages over a spatial unit, the size of the spatial unit is not explicitally corrected for here
* In principle, it is good practice to have all your files in the same projection system (see also, courses of Digital Earth). In our case, the shapefile is in geographic coordinates (WGS84) while the raster file on species richness is in World Eckert IV. The Zonal Statistics tool we use in this class apparently is capable of dealing with this difference. However, if ever a tool does not work (properly) remember that this could be one of the reasons. If you would like to reproject a vector layer, [here's a video on how to do so](https://user-images.githubusercontent.com/89069805/136777324-94c57539-329e-4a20-8d4e-bc9a0c0af8e3.mp4). 



<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131485589-8a7ddc91-cf03-4f07-a731-8052861a1f7b.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>




Our vector file now has two extra attribute columns: mean NDVI and mean mammal richness. 

### Can we now visualize this relationship? 

* we are particularly interested in how mammal richness depends on NDVI, or Richness~f(NDVI)
* QGIS (and many others) are typically used for visualization and analysis of geographic (spatial) data
* But, some basic analytic figures can be made as well, e.g. a Scatterplot: 


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131486341-0bc9bec4-4229-4959-b910-5bd0cefebecd.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>





**
We now have a scatterplot, but the statistical tools to analyse these data in QGIS are limited. So, let's try to import this data into Rstudio and build a simple  [statistical regression](https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/Rstudio.html)

<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/exploring.html">Practical 1: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/understandinggradients.html">Practical 1: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/intro.html">Practical 2: exercise 1</a></li>
    <li><strong>Practical 2: exercise 2</strong></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/Rstudio.html">Practical 2: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>


