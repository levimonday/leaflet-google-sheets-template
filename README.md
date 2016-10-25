
### Summary
1. A Google Docs / Drive Spreadsheet is used as a data source
2. A PHP script reads the published Google spreadsheet in real-time and converts that to a [GeoJSON format](http://geojson.org/geojson-spec.html)
3. A [Leaflet](http://leafletjs.com/) Javascript-based map points at the GeoJSON output and magically renders [Wi-Fi markers into a map](https://joinopenworks.com/wifi) that works in any modern web browser.

Google spreadsheet allows for anybody to potentially edit/maintain the data.

Additions or changes to the spreadsheet appear on the map somewhere between immediately or as much as a few minutes later. 

### Publishing the Source Google Sheet in CSV Format

This code requires the spreadsheet to be published to the web and therefore doesn't use the Google Sheets API to get the data (which is another way).


#### To create your sourceGoogle spreadsheet
* First, make a copy of the [base spreadsheet template](https://docs.google.com/spreadsheets/d/10eNXFh6mzFtii7B2PW90jmHtrQLJlRCrf3kkHU0HIH8/edit?usp=sharing)
* Now add real title and GSP data

#### To publish your sourceGoogle spreadsheet
* Go to (File -> Publish to Web) and do the following
* "Entire Document" (unless you only want to publish one tab, in which case select the tab)
* "Comma-seperated values (.csv)
* Click Publish
* Save the URL provided. It will look like https://docs.google.com/spreadsheets/d/{key - bunch or random numbers and letters}/pub?output=csv

The "CSV data source URL" to be used in the google-spreadsheet-to-geojson.php PHP script looks like
``https://docs.google.com/spreadsheet/pub?key={key - bunch or random numbers and letters}&single=true&gid={tab id}&output=csv``

If you want the first tab then use gid=0 above. If you want another tab in the Google Sheet then open that tab in you browser and look in the URL for gid=########## and use that value.

### Using PHP to Convert Google Sheets CSV to a GeoJSON File
Take the "CSV data source URL" you just constructed and insert it in the $spreadsheet_url varaible near the top of google-spreadsheet-to-geojson.php

The column values are used in this example to generate the GeoJSON are hard-coded to include 3 fields: title, longitude, and latitude.
If you're using more than these three fields in your GeoJSON output then it will be necessary to  update the field names in the $spreadsheetData array structure of google-spreadsheet-to-geojson.php.

### Getting Latitude and Longitude

If you know how to do custom functions in Google Sheets then you can [convert an address into latitude and longitude](https://ctrlq.org/code/19992-google-maps-functions-for-google-script) with some customization.

Non-programming / manual ways to get latitude and longitude numbers
* Go to MapQuest, zoom in and center the position you want in the middle of the map and right click on the spot you want. The pop-up will show the lat and long.
* In Google Maps the URL for the map will contain the longitude,latitude (in that order) ex: 34.8509174,-82.3987371

### Render a Leaflet Map Showing the GeoJSON Data
The index.html file loads the GeoJSON file into a local Javascript variable. Point this at your GeoJSON file and Leaflet will 
render the GeoJSON data. For example, you'll need to change this line to point at your PHP script that renders the JSON

var geoJsonData = JSON.parse(readJSON('http://example.com/my-map/google-spreadsheet-to-geojson.php'));

Leaflet JS is using open MapQuest tiles. [As of July 2016](http://devblog.mapquest.com/2016/06/15/modernization-of-mapquest-results-in-changes-to-open-tile-access/),
it's necessary to [register an account with MapQuest. Up to 15,000 views a month is free](https://developer.mapquest.com/plans).

### Registering for Map Tiles
If you register with CloudMade you can use their map tiles instead, as described by Leaflet 
example http://leafletjs.com/examples/geojson.html

If using Mapquest then after registering for an account do the following:
* [Login to your developer account](https://developer.mapquest.com/user/login)
* Go to Keys and Reporting
* Register a new Application. Give it a name. The callback may not be necessary. Save the application
* Click on the "Application" you created and copy the "Consumer Key", NOT the secret.
* You'll use that key value in the index.html in the MapQuest <script> tag where it says =PASTEYOURMAPQUESTKEYHERE

Leaflet has a bunch of other plug-ins and options, so the maps can be tweaked in all sorts of ways.

Documentation for MapQuest and Leaflet begins at
https://developer.mapquest.com/documentation/leaflet-plugins/maps/

* Created during OpenData Day 2014 in Greenville SC https://github.com/OpenUpstate/OpenDataDay2014
