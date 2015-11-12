# Instructions for adding the railway data to our DuBois map.

## Using GIT
1. if you want this file and the railway.geoson on your computer, you can synch this branch and it will pull them down for you. You don't have to do this for the exercise, but FYI.

## Adding Shape Files to CartoDB from public data sets
1. Switch to your dataset view
2. click on "search" in the top left
  - Search for "United States"
  - scroll until you find the "USA States" select it
  - at the top is an option to "connect datasets"
3. repeat step 2, but search for "USA Counties" and connect the dataset '
cb_2013_us_county_500k'

4. CartoDB should now synch those datasets into your account (it may take a moment)
  - by default they are set to update one a month, if someone makes changes to the file. you can turn this off if you want. its not terribly necessary for this project, but if you were working with frequently updated data, could be handy!

## Filtering, clipping and visualizing just the things we want
1. open the "ne_50m_admin_1_states" dataset as a map
2. We want to make Georgia pop-out **Let CartoDB do some work for you**
  - use the WIZARD to create a "CATEGORY" map using "NAME" as the field
  - it doesn't matter that it randomly assigns states colors, we just want the code started for us
  - switch over to the cartoCSS editor
  - set one of the selectors to Georgia like this:
` #ne_50m_admin_1_states[name="Georgia"] {
  - delete the rest of the selectors and apply changes
  - now we can play with how Gergia stands out from the rest of the map. Maybe making it a different color? Changing its border? Adding a dashed border even? With this CSS: ` line-dasharray: 4, 4;

3. Now for the tricky part: add the county layer and use the same ideas from above to get only the Gerogia counties AND display Dougherty county as seperate from the other Georgia Counties.
 - We will need a little SQL to make our lives easier by removing all of the non-Georgia counties. We are actually going to make a new dataset!
 - in the "cb_2013_us_county_500k" dataset let's select only the Georgia counties. 
 - **Use the FILTER** button to select only the rows that have a "STATEFP" of 13
   - FP is the Federal Processing code, its a unique number for each state. We can find it on the map. You'll note this dataset is inconvienent because it doesn't have state/county names!!
   - switch from the filter button to the SQL button to see how CartoDB has selected those rows for you
 - above your map/data there should be a green bar that says "create dataset from Query" clikc on that and CartoDB will make you a new dataset that is just the Georgia Counties. At this point you could delete or unsync cb_2013_us_county_500k if you needed to (you can also leave it)
 - **rename** your new dataset "georgia_counties" so you know what it is!
 - Now that you have the Georgia counties, its up to you to add them to your map and visualize only Dougherty County.

## Adding the Railroads
1. In your data view add a new dataset
2. In the bottom-right-ish corner of the add dataset window is a place to paste a URL
  - Use this one: ** https://github.com/hurc305/mapping_dubois/blob/gh-pages/railways_tutorial/table_10m_railroads.geojson **
  - NOTE: this is a geojson file being pulled from our github repo! You can actually preview it on github if you like.

## SOME SQL TO BE EDITED:
SELECT georgia.cartodb_id, ST_railroads.the_geom, railroads.the_geom_webmercator  FROM georgia, railroads WHERE ST_Intersects(georgia.the_geom, railroads.the_geom)
