# Step 3: Extracting information from GEE


>First thing to do is to import the shapefile with the lake sampling points as an asset. You have done this in a previous practical, but if [you forgot how to do this, click here](https://liesjacobs.github.io/worldfoodecosystems2023/practical2/intro.html)

Import the data in your script (much like you did [here](https://liesjacobs.github.io/worldfoodecosystems2023/practical2/intro.html)), rename the variable and give it a logical name (e.g. 'lakes') and add the layer to the map. 

In my code, the line that visualizes the data looks like this: 
```javascript
Map.addLayer(lakes,{'color':'purple','opacity':0.5}, "lakes");
```

Now, let's extract the mean water deficit for the first epoch, and the biome classification, this for each point. We have done this before, so below are some clues for the code which - based on your work last week - you should be able to adjust to make it work. 

```javascript
// load the image collection of TERRACLIMATE, and filter the correct dates
var dataset = ee.ImageCollection('IDAHO_EPSCOR/TERRACLIMATE')
                  .filter(ee.Filter.date('1980-01-01', '1990-01-01'));
// from the image collection, select the band containing the deficit information.
var deficit = ?.select(?);
print(deficit); //this is still an image collection, so we'll need to reduce it and take the mean (so that we have one mean layer instead of all the monthly layers in the collection
//let's calculate the monthly mean deficit over the first epoch
var meandeficit = ?.mean();
var deficitVis = {
  min: 0,
  max: 1500.0,
  palette: ['blue', "green", 'red'],
};
// plot the mean deficit map:
Map.addLayer(?, ?, 'deficit');

/// now we extract the values for deficit for each of the points. This is very similar to what we did in practical 5

var lakes_deficit80s = meandeficit.reduceRegions(lakes, ee.Reducer.mean( ),4638.3); // the 4638 is the resolution of the layer of terraclimate (see also in the catalogue)
Export.table.toDrive({
  collection: lakes_deficit80s,
  description:'lakes_deficit80s',
  fileFormat: 'CSV'
}); //we write the elevation values per camera location to a csv --> download this csv file

```
>Now, do the same for the Biome map: extract the values of the biome class for each point. 

This is similar to the water deficit, with the exception that the OpenLand Biome map is already an image and that you don't have to take the mean, but the mode (the most common pixel value) and that the resolution of the openland biome map is 1000m. Also export this csv file. 

### You now have two csv files in your google drive: one containing the information on the deficit in the 80s, one containing information on the biomes can be imported in R

We now go back to R to do the [data analysis](https://liesjacobs.github.io/worldfoodecosystems2023/practical3/Analysis.html)



<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/exploring.html">Practical 1: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/understandinggradients.html">Practical 1: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/intro.html">Practical 5: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/QGIS.html">Practical 5: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical2/Rstudio.html">Practical 5: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/intro.html">Practical 6: Problem description</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/API.html">Practical 6: Exercise 1</a></li>
    <li><strong>Practical 6: Exercise 2</strong></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical3/Analysis.html">Practical 6: Exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>
