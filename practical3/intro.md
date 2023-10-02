# Welcome to the last practical. 


Congratulations, you have come a very long way...

In the previous two sessions, we have focessed a lot on implementing command-line based data retrieval and analysis, and tried to move away from a point-and-click approach. 

You will probably have noticed that the level has gone up, with more and more pieces of code left open for you to fill in.



Make sure you found your way trough the [first practical](https://liesjacobs.github.io/worldfoodecosystems2023/practical1/intro.html) as well as the [fifth practical](https://liesjacobs.github.io/worldfoodecosystems2023/practical2/intro.html) as this last practical builds upon this. 



## Step 1: The case-study

The case we are investigating today is the salinization of fresh water lakes. Recently, a global dataset of surface water salinity - with measurements between 1980 and 2019 - was published [here](https://www.nature.com/articles/s41467-021-24281-8). The paper reports on the dataset and how it was established. In this practical we will analyze the dataset to answer following questions: 
- has salinity - as measured by the electrical conductivity (EC) increased or descreased in global freshwater lakes? 
- Is salinity of the water linked to rainfall deficits? 
- Are increases/decreases in salinity different across different biomes? 
- Which local drivers can influence salinity trends? 

### Problem simplification

Much like all problems, we'll need to simplify and define this one as well: 


| Building block  |  Decision |
|---|---|
| Geographic scale |  Points: measurement points of salinity in the database. Each seperate point is considered to be a location (regardless if two points are taken in the same water body) |
| temporal scale |  We will compare averages over 1980-1990 with averages over 2005-2015: only stations that have >5y of measurement in both epochs are considered|
| Assumption | We assume that water deficits (low precipitation with high evaporation) [for example here](https://www.sciencedirect.com/science/article/pii/S0048969721054802) is linked to higher salinity  |
| Dimensions | we focus on (i) a quanitified rainfall deficit and (ii) the biome map|
| Dimension description | The Terraclimate dataset (Climate water deficit band) and the OpenLand Biome map |


### Data description

Now that we have described the problem, we can describe the data we'll use

| Dataset      | Type | Source     |Access point     |
| :---        |    :---    |          :---  |         :---  |
| EC sampling points    | Vector:points       | Thorslund et al. 2020  | [here](https://doi.pangaea.de/10.1594/PANGAEA.913939?format=html#download)  |
| TerraClimate, water deficit  | Raster        | derived from the TerraClimate Collection   |Google Earth Engine Catalogue    |
| Biome map| Raster       | OpenLand potential Biomes     |Google Earth Engine Catalogue|

***

The datasets by Thorslund et al. are very large and need to be pre-processed as we want to compare data from 1980-1990 to that from 2005-2015 for those stations for which an average can be reliably calculated. 

To make this exercise feasible, this preprocessing has already been done: the aggregated csv file and its conversion into a shapefile as well as the original file can be found [here](https://canvas.uva.nl/courses/32040/modules/items/1502508). 

**Now we are ready for the [next step](https://liesjacobs.github.io/worldfoodecosystems2023/practical3/API.html): we'll first explore the original and the pre-processed data in R**

<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/exploring.html">Practical 1: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/understandinggradients.html">Practical 1: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/intro.html">Practical 5: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/QGIS.html">Practical 5: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/Rstudio.html">Practical 5: exercise 3</a></li>
    <li><strong>Practical 6: Describing the problem</strong></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/API.html">Practical 6: Exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/Mapping.html">Practical 6: Exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/Analysis.html">Practical 6: Exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>

