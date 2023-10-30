## Exercise 3: Understanding the Gradient : how process influences form

In this exercise we will actually *use* data from earth engine to gain insight in a real-life question: 
How can we explain the remarkable gradient in land cover (and related agricultural practices) along the West-Coast of the USA (see video below)


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/135052084-665539ec-8de3-4457-8cc4-f67af8c24812.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>


Our working hypothesis, is that the strong difference we see in vegetation on this video is somehow due to the elevation - temperature - precipitation gradients. But can we actually quantify these gradients? This is the topic of the code below. 

>WARNING: the codes below can look scary, but no worries: the goal here is to understand what GEE can do, and how to broad strokes of the code below works. 



### > step 1: make a new Script in Earth engine, and give it an appropriate name (e.g. transects): the first time you make a script you'll be asked to enter your username and repository name




<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131994092-5ed947ec-1aa9-4277-bfa1-db014ac2458f.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>



Now we can think about the building blocks of the analysis (see slide 45, course 1): 
- Which dimensions will we take into account (which processes will we consider)
- How will we describe the dimensions
- what Geographic scale – spatial unit is appropriate
- what is the temporal scale we will work on
- What are the boundary conditions or assumptions

Note that these are exactly the first questions you also need to answer for your own final report. More-over, in these practicals we will typically already provide a completed version of the table you are expected to be able to fill in yourself for your own project, so that you have plenty of examples. The further we go ahead, the more '?' we introduce into the table: to fill them, you'll need to apply the principles you have learned thus far to address these gaps. 



| building block  |  decision |
|---|---|
| Geographic scale |  Transect (line) |
| temporal scale |  all data input should span same timespan |
| Assumption | The vegetation gradient we observe on such a short transect are due to either a steep temperature gradient, or a steep precipitation gradient, or both: either can be induced by the topographic barrier |
| Dimensions | We'll consider the elevation, air temperature and precipitation
| Dimension description | elevation: the SRTM DEM at 90m resolution, temperature: long term averages extracted from worldclim at ca. 900m, precipitation: ?  |

>TIP: If you are looking for data, or data types, you could use the search bar above to find datasets and even examples of code to import/plot/...

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131996471-05d9d1b8-0a0f-4e1d-82da-d9cdc1ba0bd2.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

Or you can browse the entire catalog [here](https://developers.google.com/earth-engine/datasets/catalog). 

| Dataset      | Type | Source     |Access point     |
| :---        |    :---    |          :---  |         :---  |
| SRTM DEM 90 meters  | Raster       | ? |https://developers.google.com/earth-engine/datasets/catalog/CGIAR_SRTM90_V4|
| WORLDCLIM   | Raster        | ?      | https://developers.google.com/earth-engine/datasets/catalog/WORLDCLIM_V1_MONTHLY?hl=en     |


Use the links above to find out if you can use the WORLDCLIM climate data can be used for precipitation as well, and what the source is of the SRTM DEM: 

<iframe width="640px" height="480px" src="https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9UN0pRTEE0TkpJT0dDVFgwMllNVEZKQlpDTS4u&embed=true" frameborder="0" marginwidth="0" marginheight="0" style="border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>


***

### > step 2: Define the spatial unit, and import the relevant layers



The spatial unit in our case is a 'Line': we can define this using the function ee.Geometry.LineString, which just needs the beginning and end coordinates as (LONG,LAT) pairs. 

Next, we can plot the transect on the map. 

```javascript
// defining the variable 'transect' as the gradient we want to investigate:  
var endpoint = [-120, 44.65];  
var startpoint = [-124, 44.65];
Map.setCenter(-122, 44.65, 8); 
var transect = ee.Geometry.LineString([endpoint, startpoint]);
Map.addLayer(transect, {color: 'FF0000'}, 'transect'); //Map is the name GEE gives to the lower panel that shows 'the map':-)
```

Now, eventually, for points along the transect we want to see how elevation, temperature and precipitation changes when going from left to right on the transect. This means: we are working towards a plot where we have longitude on the X-axis and elevation/precipitation/temperature on the y-axis. 

One of the easiest ways to do this, is to first create an Image with several bands: one for longitude (or latitude, if you want to investigate a vertical gradient), one for elevation, one band for yearly temperature, and one band for yearly precipitation. 

Once we have this image, we can collect the various variables (longitude, elevation, temperature...) along the transect with the ee.Reducer.toList() function that is explained on [this page](https://developers.google.com/earth-engine/guides/charts_array).

But first things first, let's make this Image with the different bands. 
```javascript
//To create a raster with longitudes and latitudes, we can use the following function: 
var latLonImg = ee.Image.pixelLonLat();
//you can check this by printing the newly created dataset to the console: 
print(latLonImg); 
//check in the console here on the right: the image has two bands: which ones?
```

<iframe width="640px" height="480px" src="https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9URTlUNTExMVhBOFBRRk9JRFI3MU5LS05GQy4u&embed=true" frameborder="0" marginwidth="0" marginheight="0" style="border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>

If you want to know exactly what a certain function does, you can either google it, or consult it on the platform itself this way: 


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/179742254-e5a09908-89c3-426c-9c7c-9d79af16d7a1.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>


```javascript
//let's Map one of those two: 
var vizParams = {
  bands: ['longitude'],
  min: -180,
  max: 180,
  palette: ['00FFFF', '0000FF'],
  opacity: 0.5
};

Map.addLayer(latLonImg,vizParams, 'longitude');

```

<iframe width="640px" height="480px" src="https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9UQUdGVzVXUUtKQVZVNkRXWDFRWUw3NzJCWS4u&embed=true" frameborder="0" marginwidth="0" marginheight="0" style="border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>



Now that we know what is inside of this new image, we can continue: we will import a thematic layer (e.g. the elevation) and add the longitude and latitude bands to that image: this allows us to have, for each long-lat combination a band value (e.g. for temperature or elevation or ...)

```javascript
// Import a digital surface model
var elevImg =ee.Image("CGIAR/SRTM90_V4");
//What is in the image (which are the bands, how many bands are there?)-->
print(elevImg);

```
hmmm, we only need the elevation, which band is that? Again, to find information about the datasets, you can use the search bar above: 

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/179746069-76558d59-a5bd-44a4-95ff-83070baced21.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

```javascript
// let's now select the band we want and add the latlon bands to it: 
var elevation = elevImg.select('elevation');
var elevationlatlon = elevation.addBands(latLonImg);
// does it now have the bands you are interested in? 
print(elevationlatlon); // how many bands are there now?

//let's also add this image to the map below: let's plot the band  elevation
var vizParams2 = {
  bands: ['elevation'],
  min: 0,
  max: 1800,
  palette: ['00FFFF', '0000FF'],
  opacity: 0.5
};
Map.addLayer(elevationlatlon,vizParams2, 'elevation');
```

GREAT! now we have an image which already has the coordinates as pixel values and the elevation, meaning we can try to attempt to use ee.Reducer.to.List and plot the elevation changes according to longitude changes along the profile: 

```javascript
// Reduce elevation and coordinate bands by transect line; get a dictionary with
// band names as keys, pixel values as lists.
var elevTransect = elevationlatlon.reduceRegion({
  reducer: ee.Reducer.toList(),
  geometry: transect,
  scale: 1000,  //we use a scale (or a resolution) of 1000m, this seems weird when we have a DEM of 90m, but we do this because of the resolution of the other datasets (e.g. temperature) we will also use. 
});
print(elevTransect); //Check what is inside this object: their seems to be 3 lists, which ones, and does this make sense to you? 
     
```


***


### > step 3: prepare the data for plotting on a graph

Now that we have an object that contains a list with the longitudes and the elevations, we can plot the elevations and the longitudes on one plot: 



```javascript
// Get longitude and elevation value lists from the reduction dictionary.
var lon = ee.List(elevTransect.get('longitude'));
var elev = ee.List(elevTransect.get('elevation'));

// Sort the longitude and elevation values by ascending longitude.
var lonSort = lon.sort(lon);
var elevSort = elev.sort(lon);

// Define the chart and print it to the console.
var chart = ui.Chart.array.values({array: elevSort, axis: 0, xLabels: lonSort})
                .setOptions({
                  title: 'Elevation Profile Across Longitude',
                  hAxis: {
                    title: 'Longitude',
                    viewWindow: {min: -124.50, max: -120}, 
                    titleTextStyle: {italic: false, bold: true}
                  },
                  vAxis: {
                    title: 'elevation(m)',
                    titleTextStyle: {italic: false, bold: true}
                  },
                  colors: ['1d6b99'],
                  lineSize: 5,
                  pointSize: 0,
                  legend: {position: 'none'}
                });
print(chart);

```

*Note that you can click on the small arrow on the right upper corner of your graph to maximize the window and even download the data!






<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/179748619-4537869a-90f1-476b-a46c-23dd07a9500e.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>


***
GREAT! Now that we have done this for an image (the image with the elevation band), let's try to do this for an ImageCollection: An image collection is a series of images bundles together, for example because they contain the same type of data but at different timesteps. An image collection for precipitation could for example consist of an image for each day, whereby each image contains the daily precipitation per pixel. Before you can analyze this, you will thus have to convert the collection to a single image (e.g. by summing all the values to get the total precipitation, or by averaging to get e.g. the average temperature). 

This is exactly what we will do next. After the reduction of the ImageCollection to the image, the exercise is very similar to the one of elevation vs. longitude. 


```javascript
// we will use this dataset for the temperature (and perhaps also for precipitation?)
var climatevar = ee.ImageCollection('WORLDCLIM/V1/MONTHLY');
print(climatevar); // Ok this dataset contains several images with multiple bands representing monthly means: we'll need to select the right band and average them to get an annual mean: 
var temperature = climatevar.select('tavg');
print(temperature); // is there now only one band per image? YES
// However, there are still 12 features (for the 12 months): select the first one and plot it on the map. Like this: 
var tempjan = temperature.first();
print(tempjan);
var vizParams = {
  min: -100,
  max : 250,
  palette: ['00FFFF', '0000FF'],
  opacity: 0.5
};
Map.addLayer(tempjan, vizParams, 'mean temperature januari'); 
// Now click on Amsterdam on the map: what is the average monthly value for this city as depicted in the inspector? can this be correct?
// Why is this value so high? (tip: search in the searchbar for the worldclim temperature dataset and look in the band description )
// now we can take the average temperature over all the months: 
var average_T = temperature.reduce('mean').multiply(0.1); //why do we multiply with 0.1? (again, search in the searchbar for the worldclim temperature dataset)

print(average_T);
     
```
Right, now we have an actual image with the band we want: the exercise is now very similar to the one with elevation: 

```javascript
// Ok: now we can repeat the same exercise to get the values along the transect
var average_T_withlatlon = average_T.addBands(latLonImg);
print(average_T_withlatlon);

// Reduce elevation and coordinate bands by transect line; get a dictionary with
// band names as keys, pixel values as lists.
var tempTransect = average_T_withlatlon.reduceRegion({
  reducer: ee.Reducer.toList(),
  geometry: transect,
  scale: 1000, //why 1000? (tip: search the openlandmap dataset in the search bar above)
});
print(tempTransect); 


// Get longitude and temperature value lists from the reduction dictionary.
var lon = ee.List(tempTransect.get('longitude'));
var temp = ee.List(tempTransect.get('tavg_mean'));

// Sort the longitude and elevation values by ascending longitude.
var lonSort = lon.sort(lon);
var tempSort = temp.sort(lon);

// Define the chart and print it to the console.
var chart = ui.Chart.array.values({array: tempSort, axis: 0, xLabels: lonSort})
                .setOptions({
                  title: 'Temperature Profile Across Longitude',
                  hAxis: {
                    title: 'Longitude',
                    viewWindow: {min: -124.50, max: -120},
                    titleTextStyle: {italic: false, bold: true}
                  },
                  vAxis: {
                    title: 'temperature (C)',
                    titleTextStyle: {italic: false, bold: true}
                  },
                  colors: ['1d6b99'],
                  lineSize: 5,
                  pointSize: 0,
                  legend: {position: 'none'}
                });
print(chart);

```

## **Congrats: you are now able to reduce an image collection to an image and make nice plots: can you do the same for precipitation? **



<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/practical1/exploring.html">Practical 1: exercise 3</a></li>
      <li><strong>Practical 1: exercise 3</strong></li>
    <li><a href="https://liesjacobs.github.io/worldfoodecosystems2023/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>
