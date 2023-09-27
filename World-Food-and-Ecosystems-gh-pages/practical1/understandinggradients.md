### Exercise 3: Understanding the Gradient : how process influences form

In this exercise we will actually *use* data from earth engine to gain insight in a real-life question: 
How can we explain the remarkable gradient in land cover (and related agricultural practices) along the West-Coast of the USA (see video below)


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/135052084-665539ec-8de3-4457-8cc4-f67af8c24812.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>




>WARNING: the codes below can look scary, but no worries: it is not the intention to be able to reproduce this: the goal here is to understand what GEE can do, and how to broad strokes of the code below works. 



> step 1: make a new Script in Earth engine, and give it an appropriate name (e.g. transects): the first time you make a script you'll be asked to enter your username and repository name




<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131994092-5ed947ec-1aa9-4277-bfa1-db014ac2458f.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>



Now we can think about the building blocks of the analysis (see slide 45, course 1): 
- what Geographic scale – spatial unit is appropriate
- what is the temporal scale we will work on
- What are the boundary conditions or assumptions
- Which dimensions will we take into account (which processes will we consider)
- How will we describe the dimensions


| building block  |  decision |
|---|---|
| Geographic scale |  Transect (line) |
| temporal scale |  all data input should span same timespan |
| Assumption | The different landcovers on such a short transect are due to either a steep temperature gradient, or a steep precipitation gradient, either can be induced by the topographic barrier |
| Dimensions | We'll use winter and summer temperature and yearly precipitation |
| Dimension description | ERA5 for temperature, CHIRPS for precipitation, a local DEM for topography |


>TIP: If you are looking for data, or data types, you could use the search bar above to find datasets and even examples of code to import/plot/...

<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131996471-05d9d1b8-0a0f-4e1d-82da-d9cdc1ba0bd2.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>






***

> step 2: Define the spatial unit, and import the relevant layers



The spatial unit in our case is a 'Line': we can define this using the function ee.Geometry.LineString, which just needs the beginning and end coordinates as (LONG,LAT) pairs. 

Next, we can plot the transect on the map. 

```javascript
// defining the variable 'transect' as the gradient we want to investigate:  
var endpoint = [-118.821944, 46.527222];  
var startpoint = [-123.416667, 46.783333];
var transect = ee.Geometry.LineString([endpoint, startpoint]);
Map.addLayer(transect, {color: 'FF0000'}, 'transect'); //Map is the name GEE gives to the lower panel that shows 'the map':-)
```



Now, we can import the Image(collections) we need and filter them

```javascript
// Daily mean 2m air temperature
var era5_2mt = ee.ImageCollection('ECMWF/ERA5/DAILY')
                   .select('mean_2m_air_temperature')
                   .filter(ee.Filter.date('2013-12-21', '2014-09-23'));
print(era5_2mt);
    
//import elevation:
var elevation = ee.Image('USGS/NED');  // elevation is here simply one image (so no image collection) because it only contains 1 acquisition (unlike landsat imagery)

// let's add precipitation ass well (mm/pentad, or mm/5days), similar as temperature
var chirps = ee.ImageCollection("UCSB-CHG/CHIRPS/PENTAD"); //rainfall (from the chirps dataset) is available every 5 days: so it is an image collection
var chirpsprecip = chirps.filterBounds(transect)  //again, we filter for location: only the transect
    .select(['precipitation'], ['previp'])   //again, we select only the band of interest (precipitation) and we give it our own name
    .set('system:time_start', chirps.get('system:time_start')); //this is a line that defines the startdate of the image collection
    
```


***


> step 3: prepare the data for plotting on a graph

Now that we have all the image(collections), we can 'reduce' the data, so that we have the winter and summer temperature along the transect, the total precipitation and the topography, and we can combine all this data into one *new image*. 

```javascript
//selecting a year worth of data for CHIRPS and sum all the data (so total yearly precipitation)
var chirpsprecipselect = chirpsprecip.filterDate('2013-12-21', '2014-12-21') //we filter on a range of dates
                          .reduce(ee.Reducer.sum()) //we reduce, by taking the sum
                          .select([0], ['yearprec']); //we select the relevant band (the first band only, or that with index 0) and we give it a name
                          
// Calculate bands for seasonal temperatures and elevations; 
// Calculate bands for seasonal temperatures and elevations; 
var summer = era5_2mt.filterDate('2014-06-21', '2014-09-23')
    .reduce(ee.Reducer.mean())
    .select([0], ['summer']);
var winter = era5_2mt.filterDate('2013-12-21', '2014-03-20')
    .reduce(ee.Reducer.mean())
    .select([0], ['winter']);

//join all the data into one new image: 
// we first make a list describing the distance from the startingpoint:
var startingPoint = ee.FeatureCollection(ee.Geometry.Point(startpoint));
var distance = startingPoint.distance(450000);
//now we add all the bands to the distance list:
var image = distance.addBands(elevation).addBands(winter).addBands(summer).addBands(chirpsprecipselect); //addBands is here a function, that takes the bands defined above as input

```



OK: now we have all ingredients to sort the information and plot it all in one chart 

```javascript
// Extract band values along the transect line: convert the image information into an array (list).
var array = image.reduceRegion(ee.Reducer.toList(), transect, 1000)
                 .toArray(image.bandNames());

// Sort points along the transect by their distance from the starting point.
var distances = array.slice(0, 0, 1);
array = array.sort(distances);

// Create arrays for plotting in a figure.
var elevationAndTempAndPrecip = array.slice(0, 1);  // For the Y axis.
// Project distance slice to create a 1-D array for x-axis values.
var distance = array.slice(0, 0, 1).project([1]);


```
> step 3: prepare the data for plotting on a graph  (note: you do not need to be able to code this yourself)


```javascript
// Generate and style the chart.
var chart = ui.Chart.array.values(elevationAndTempAndPrecip, 1, distance)
    .setChartType('LineChart')
    .setSeriesNames(['Elevation', 'Winter 2014', 'Summer 2014', 'Precipitation 2014'])
    .setOptions({
      title: 'Elevation, temperatures and precipitation along transect',
      vAxes: {
        0: {
          title: 'Average seasonal temperature '
        },
        1: {
          title: 'Elevation (meters, blue) or precipitation (mm, Green)',
          baselineColor: 'transparent'
        }
      },
      hAxis: {
        title: 'Distance from starting points (m)'
      },
      interpolateNulls: true,
      pointSize: 0,
      lineWidth: 1,
      // Our chart has two Y axes: one for temperature and one for elevation/precip.
      // Y axis, which we do here:
      series: {
        0: {targetAxisIndex: 1},
        1: {targetAxisIndex: 0},
        2: {targetAxisIndex: 0}, 
        3: {targetAxisIndex: 1},
      }
    });

print(chart);
Map.setCenter(-121, 46, 7);
Map.addLayer(elevation, {min: 4000, max: 0});
Map.addLayer(transect, {color: 'FF0000'});

```



*Note that you can click on the small arrow on the right upper corner of your graph to maximize the window




<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/131997467-23ba1c44-9af8-487b-a8f7-6c4409bdab17.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>


## **Congrats: you can now analyze multiple layers at the same time on the same graph: is the result what you expected**? 
<iframe width="640px" height= "480px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9UNUFRSFhMUjFINzJHR0NLRjlBSktRRlE2Vy4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>


<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/exploring.html">Practical 1: exercise 3</a></li>
      <li><strong>Practical 1: exercise 3</strong></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>
