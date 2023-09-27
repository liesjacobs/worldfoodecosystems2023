## Step 3: exploring the functionalities
In this practical we will be exploring the datasets and functionalities of Google Earth Engine. If all is well, you have created an account on earth engine by following the instructions on [the starting page](https://liesjacobs.github.io/World_Food-and_Ecosystems/)


### Exercise 1: loading the biome map of the world and exploring its values

Now that we know more or less how it looks, let's use it. 
If at any point your lost on the basic JavaScript commands, go [back](https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/intro.html)

> Starting to build the script: exploring the Biome map 

Copy this piece of code in the 'script' panel of the earth engine GUI. 

```javascript

// first we retrieve the Image (the raster dataset) from the server and declare the variable
var biomes = ee.Image("OpenLandMap/PNV/PNV_BIOME-TYPE_BIOME00K_C/v01");

// but what have we done exactly? Let's see what it is in this 'Image'. 
print(biomes);

```

Save the file (e.g. name it 'BiomePlotting') and run it. 


<iframe width="640px" height= "480px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9UNEw1SkZCMDNKOUpINlBUMTFOUUU3OEM1WS4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>


***


> Exercise 2: plotting the biome map on the baselayer map and discovering its values


Now that we know (more or less) which Map we have loaded (for more info, you can always search the searchbar on top of the GUI), let's plot the map: 


```javascript
// First we need to define how we want to handle the visualisation: we need to *declare a variable* that describes how/what we want to plot. 
var visualization = {
  bands: ['biome_type'],
  min: 1.0,
  max: 32.0,
  palette: [
    "1c5510","659208","ae7d20","000065","bbcb35","009a18",
    "caffca","55eb49","65b2ff","0020ca","8ea228","ff9adf",
    "baff35","ffba9a","ffba35","f7ffca","e7e718","798649",
    "65ff9a","d29e96",
  ]
};


//after we have set the parameters for the visualisation (declared through the variable 'visualization'we can actually plot: 
Map.setCenter(4,53,7);
Map.addLayer(biomes, visualization, "Potential distribution of biomes");

```



<iframe width="640px" height= "480px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9UQVZPTzZROU1CTVFXUzVVREdKUlNUNlgzOS4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>









 

#### TIP: for each of the functions, there is a documentation provided by earth-engine: https://developers.google.com/earth-engine/apidocs





Now that you know the basic functionalities of the GUI, let's try an actual [application](https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/understandinggradients.html). 


<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><strong>Practical 1: exercise 2</strong></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/understandinggradients.html">Practical 1: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>

