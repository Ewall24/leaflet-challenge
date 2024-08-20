# leaflet-challenge      

<u>link to website:</u> 
https://ewall24.github.io/leaflet-challenge/

Instructions

### Part 1: Create the Earthquake Visualization 

## Your first task is to visualize an earthquake dataset. Complete the following steps:

    Get your dataset. To do so, follow these steps:
        The USGS provides earthquake data in a number of different formats, updated every 5 minutes. Visit the USGS GeoJSON Feed 
###// Store our API endpoint as queryURL
var queryURL = "https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson";
// Perform a GET request to the query URL
d3.json(queryURL).then(function(data) {
    // Once we get a response, send the data.features object to the createFeatures function.
    createFeatures(data.features);
});

Function to create features from the earthquake data
function createFeatures(earthquakeData) {



Links to an external site. page and choose a dataset to visualize. The following image is an example screenshot of what appears when you visit this link:

![3-Data](https://github.com/user-attachments/assets/026672da-1c2e-4aa1-a806-58569c3f2cd5) 




When you click a dataset (such as "All Earthquakes from the Past 7 Days"), you will be given a JSON representation of that data. Use the URL of this JSON to pull in the data for the visualization. The following image is a sampling of earthquake data in JSON format: 

![4-JSON](https://github.com/user-attachments/assets/ed76d417-5578-481b-b520-2e05b1c7b533)

### Create a GeoJSON layer containing the features array on the Earthquake Data object
    function createCircleMarker(feature, latlng) {
        let options = {
            radius: feature.properties.mag * 5, // Adjust size based on magnitude
            fillColor: chooseColor(feature.geometry.coordinates[2]), // Use depth to choose color
            color: chooseColor(feature.geometry.coordinates[2]), // Use depth to choose color
            weight: 1,
            opacity: 0.8,
            fillOpacity: 0.35
        };
        return L.circleMarker(latlng, options);
    }

## Part 2 Import and visualize the data by doing the following:

    ### Using Leaflet, create a map that plots all the earthquakes from your dataset based on their longitude and latitude.

### Create a variable for earthquakes to house latlng, each feature for popup, and circle radius/color/weight/opacity
    let earthquakes = L.geoJSON(earthquakeData, {
        onEachFeature: onEachFeature,
        pointToLayer: createCircleMarker
    });


        Your data markers should reflect the magnitude of the earthquake by their size and the depth of the earthquake by color. Earthquakes with higher magnitudes should appear larger, and earthquakes with greater depth should appear darker in color.




        
    Include popups that provide additional information about the earthquake when its associated marker is clicked.

### Give each feature a popup describing the place and time of the earthquakes
    function onEachFeature(feature, layer) {
        layer.bindPopup(`<strong>Location: </strong>${feature.properties.place}
        <p><strong>Time: </strong>${new Date(feature.properties.time)}</p>
        <p><strong>Magnitude: </strong>${feature.properties.mag}</p>
        <p><strong>Depth: </strong>${feature.geometry.coordinates[2]} km</p>
        `);
    } 



Hint: The depth of the earth can be found as the third coordinate for each earthquake.
   
    
## Create a legend that will provide context for your map data.

let legend = L.control({ position: 'bottomleft' });
legend.onAdd = function() {
    var div = L.DomUtil.create('div', 'info legend');
    // Apply inline styles directly to the legend
    div.style.backgroundColor = 'white'; // White background
    div.style.border = '2px solid lightgray'; // Light black border (dark gray)
    div.style.borderRadius = '5px'; // Rounded corners
    div.style.padding = '10px'; // Padding inside the box
    div.style.fontSize = '14px'; // Font size
    div.style.boxShadow = '2px 2px 5px rgba(0, 0, 0, 0.6)'; // Shadow effect
    var grades = [-10, 10, 30, 50, 70, 90];
    var labels = [];
    var legendInfo = "<h4>Depth (km)</h4>";
    div.innerHTML = legendInfo;
    // Go through each depth item to label and color the legend
    for (var i = 0; i < grades.length; i++) {
        // Add inline styles for each list item
        labels.push('<li style="margin: 0; padding: 5px 0; font-size: 12px;">' +
                    '<span style="display: inline-block; width: 80px; height: 18px; background-color:' +
                    chooseColor(grades[i] + 1) + '; border-radius: 4px;"></span> ' +
                    grades[i] + (grades[i + 1] ? '&ndash;' + grades[i + 1] : '+') + '</li>');
    }



### Your visualization should look something like the preceding map.







