# US Cities Weather Map App

This app will help visualize and tell the weather conditions of major US cities. It will use data from http://openweathermap.org.

The technology you will need for build this project:

1. AngularJS
  * Ajax
2. Google Maps API
  * google.maps.Map
  * google.maps.Marker (with custom icons)
  * google.maps.InfoWindow

## Project Overview

In part 1, we will build a map overview that allows you to see what the current weather all over the country is like. In part 2, we will add the ability to drill down to an individual city and get 5-day forecast information.

## API Key

You may use the instructor's Open Weather Map Application Key: eac2948bfca65b78a8c5564ecf91d00e.

## List of Cities

The Open Weather Map service provides a large list of city IDs. I have created a list of large US cities. You may use this list:

var cityIds = [
  4180439,
  5128638,
  4560349,
  4726206,
  4671654,
  5809844,
  5368361,
  5391811,
  5308655,
  4684888,
  4887398,
  5391959,
  5392171,
  4164138,
  4273837,
  5746545,
  4699066,
  5419384,
  4990729
];

You may choose to add or remove cities as you see fit. Read API details on how to do so: http://openweathermap.org/current.

## APIs

This is the API documentation for getting the current weather conditions for a city. We will use the "Call for several city IDs" API, i.e. http://api.openweathermap.org/data/2.5/group, for part 1.

For part 2, we will use the forecast API to get a 5-day forecast. http://openweathermap.org/forecast5

Play with the API using Postman or curl. Here is an example request with curl:

curl "api.openweathermap.org/data/2.5/weather?id=4671654&APPID=eac2948bfca65b78a8c5564ecf91d00e"

Here is how to request for multiple cities using the "group" API:

curl "api.openweathermap.org/data/2.5/group?id=4671654,4560349,5391959&APPID=eac2948bfca65b78a8c5564ecf91d00e"

## Weather Icons

Given a result object from the API, which contains the properties: coord, sys, weather, base, main, wind, clouds, id, and name. You can get the icon URL by grabbing result.weather[0].icon, prepending the base URL http://openweathermap.org/img/w/ in front, and then appending .png. For example, to render a image tag in AngularJS would look like

<img ng-src="http://openweathermap.org/img/w/{{result.weather[0].icon}}.png">

For more information: http://openweathermap.org/weather-conditions

## Part 1

### Setup

1. Create a new index.html file.
2. Get the angular.js file.
3. Create an .css file for the project.
5. Link the Google Maps JS API <script src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY"></script> in the head section. Replace with your API key.
6. Link the .js and .css files in the index.html file.
7. Copy n paste the cityIds array in the "List of Cities" section above into your .js file.
8. Create an AngularJS module for your app.
9. Create an AngularJS controller for your app.
10. Link the module and the controller in your HTML file. Make sure it is working by putting something on the $scope object and then displaying it on the page.

### Make a Map

1. Create a div element with an id of "map".
2. Initialize a new Google map on that element. You will need to style the div with a height and width in order to see the map show up.
3. Center the map somewhere in Kansas: latitude: 39.099727, longitude: -94.578567.
4. Tweak the zoom level so that you see the entire United States.
5. Using $http, make an API request to get the weather conditions for the list of cities in cityIds. Use the "Call for several city IDs" API: http://openweathermap.org/current. Print the received data to the console and inspect the data you got.
6. For each result you got, create a marker in the coordinate for that city and plot it on the map. See if you can do this without writing a loop.

### Fancy Map Icons

1. Use custom icons for the markers. See the section above named "Weather Icons" for information on how to get the URL for the icon.
2. Adjust the center of the icon - since the weather icons are not meant to be used as pins. Center the weather icon directly on top of the city. You will pass an object for the "icon" property to the google.maps.Marker constructor. See this example for how its done: https://developers.google.com/maps/documentation/javascript/examples/icon-complex. The images are 50x50 pixels in size. These are the value that have worked for me:

    {
      url: 'http://openweathermap.org/img/w/' + result.weather[0].icon + '.png',
      size: new google.maps.Size(50, 50),
      origin: new google.maps.Point(0, 0),
      anchor: new google.maps.Point(25, 25)
    };

### Making the sidebar

1. Create a sidebar div to complement the map. Layout them out side by side with the map taking up most of the width. I recommend using absolute layout with percentages to layout the map next to the sidebar and have them stretch out to the full width and height of the window.
2. Add the weather results to the sidebar. For each returned result, display the city name, the current temperature. If the sidebar grows too long, use the CSS property: `overflow: auto;` to make it adhere to the window height and allow scrolling through the entire list within the sidebar.
3. Add the weather icon to the list display as well. Again, see the above "Weather Icons" section.

### Info Window

1. Make an info window pop up when a marker is clicked. The info window should display
  * the city name
  * current temperature
  * hi temperature
  * lo temperature
  * pressure
  * humidity
  * wind
2. You may find the info window is positioned too high. To fix this, you will need to adjust the "anchorPoint" property that's passed to the google.maps.Marker constructor. See https://developers.google.com/maps/documentation/javascript/reference#MarkerOptions. It takes a google.maps.Point object which you will instantiate by new google.maps.Point(x, y)
3. Make it so that there is only one info window open at any one time. I suggest reusing the same info window for any marker - simply open that info window on a the next marker that is clicked on, rather than creating a separate info window for each marker.
4. Make it so that clicking on a city on the the sidebar will also cause the info window to pop up on top of the marker. The simplest way I have found to do this is to attach a marker to a result object, i.e. result.marker = marker. Then you can extract a function which opens up the info window for a result, which can be called in both these cases:
  * when a marker is clicked
  * when a result on the sidebar is clicked
