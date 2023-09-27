# Step 3: Mapping and geo-processing


>Let's start off easy, and build a map of the raccoon sightings in Belgium: 
>>Load all the data in QGIS: the video below shows how you can make a map of the vector data you collected:


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/132331875-35103196-e929-4c60-99fe-9b05740ba3c8.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>

If your scalebar shows weird (low) numbers: this is probably because the units are still in the units of the project (so degrees): if you click on the scalebar, on the right you'll see a dropdown menu where you can change the desired units to e.g. km. 


>Next step in QGIS is to use the grids we made earlier and summarize the data per grid. Each of these things can be achieved through a different function in QGIS
>>How many raccoon sightings? --> *Count points per polygon*

>>Average topographic diversity? -->*Zonal statistics*


>>total river length per polygon grid cell? -->*Sum of line length*

The videos below show you how you can implement these different functions: 

### Count points per polygon


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/138072650-241cfcec-7a5a-4b1f-9bd6-3a84c9efabfe.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>






### Average topographic diversity 


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/138072707-08825837-3a32-48b7-a6e3-ad8c3852daf2.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>







### Sum of line length


<video style="width:100%" controls>
  <source src="https://user-images.githubusercontent.com/89069805/138072762-877dae88-0f75-4312-ab85-234c1380426d.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>






***



Note that this last video also demonstrates how to export the data as a csv. Note that for filenaming, it is a good idea to avoid points and spaces within the filename. This we do to make sure we can read the results of our processing in Rstudio, where we will do the [final analysis](https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/Analysis.html)

<iframe width="640px" height= "480px" src= "https://forms.office.com/Pages/ResponsePage.aspx?id=zcrxoIxhA0S5RXb7PWh05Vl3_L7XnVBBlpWSqA8whj9UQ0o0Q1dNVEFRQUlZUk1ZSDJZR0NRRFE2TC4u&embed=true" frameborder= "0" marginwidth= "0" marginheight= "0" style= "border: none; max-width:100%; max-height:100vh" allowfullscreen webkitallowfullscreen mozallowfullscreen msallowfullscreen> </iframe>


<nav>
  <ul>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/intro.html">Practical 1: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/exploring.html">Practical 1: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical1/understandinggradients.html">Practical 1: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/intro.html">Practical 2: exercise 1</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/QGIS.html">Practical 2: exercise 2</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical2/Rstudio.html">Practical 2: exercise 3</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical3/intro.html">Practical 6: Problem description</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical3/API.html">Practical 6: Data Collection</a></li>
    <li><strong>Practical 6: Mapping and spatial processing</strong></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/practical3/Analysis.html">Practical 6: Analysis</a></li>
    <li><a href="https://liesjacobs.github.io/World-Food-and-Ecosystems/"><b>Back to Overview Page</b></a></li>
  </ul>
</nav>
