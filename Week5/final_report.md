# Final Report

## Introduction/Business Problem

Many people having to move to another city seek a similar neighborhood to live in as they are used to. Whereas some criteria for neighborhoods being similar are subjective in nature, e.g. the feel/vibe or liking for other people living there, there are also objective measures: the venues defining the face of the neighborhood, like parks, sights, restaurants etc.

To help those people choose a neighborhood in their new city to move to, comparing the venues to their old city's neighborhood provides a valuable assistance in their decision. To be precise, clustering neighborhoods of both cities based on venue distribution enables the stakeholders to take neighborhoods in the same cluster as their old one into closer selection. This significantly narrows down the neighborhoods they have to subjectively inspect to reach a decision, saving them valuable time.

We use the cities of Toronto and Manhattan.

## Data Acquisition

First we need a list of neighborhoods in both cities and add geospatial data to it. With these lists we query the Foursquare API using the geospatial data in them to get venues and their categories for every neighborhood. From there we can clean, wrangle and cluster the data.

We parse the [wikipedia page](https://en.wikipedia.org/w/index.php?title=List_of_postal_codes_of_Canada:_M&oldid=1012118802) that lists all neighborhoods of Toronto to get the list of Toronto neighborhoods. We use the `read_html` function of the pandas library. Thereby we have to resort to an older version of the wikipedia page, since `read_html` cannot handle the new layout. We get a pandas dataframe which looks like this:


||Postal Code|Borough|Neighborhood|
---|---|---|---
|0|M1B|Scarborough|Malvern, Rouge|
|1|M1C|Scarborough|Rouge Hill, Port Union, Highland Creek|
|2|M1E|Scarborough|Guildwood, Morningside, West Hill|
|3|M1G|Scarborough|Woburn|
|4|M1H|Scarborough|Cedarbrae|

To add the geospatial coordinates, we merge a CSV-file provided by the course creators on the postal code. The resulting dataframe's head is:

||Postal Code|Borough|Neighborhood|Latitude|Longitude|
---|---|---|---|---|---
|0|M1B|Scarborough|Malvern, Rouge|43.806686|-79.194353|
|1|M1C|Scarborough|Rouge Hill, Port Union, Highland Creek|43.784535|-79.160497|
|2|M1E|Scarborough|Guildwood, Morningside, West Hill|43.763573|-79.188711|
|3|M1G|Scarborough|Woburn|43.770992|-79.216917|
|4|M1H|Scarborough|Cedarbrae|43.773136|-79.239476|

The analogous dataframe for Manhattan we extract from a geojson-file that the course creators provide


||Borough|Neighborhood|Latitude|Longitude|
---|---|---|---|---
|0|Manhattan|Marble Hill|40.876551|-73.910660|
|1|Manhattan|Chinatown|40.715618|-73.994279|
|2|Manhattan|Washington Heights|40.851903|-73.936900|
|3|Manhattan|Inwood|40.867684|-73.921210|
|4|Manhattan|Hamilton Heights|40.823604|-73.949688|

For both cities we query venue data from the Foursquare API with a radius of 500m and a limit of 100 venues per neighborhood. The resulting dataframe looks like this:

||Neighborhood |Neighborhood Latitude |Neighborhood Longitude |Venue |Venue Latitude |Venue Longitude |Venue Category|
---|---|---|---|---|---|---|---
0|Malvern, Rouge |43.806686 |-79.194353 |Wendy’s |43.807448 |-79.199056 |Fast Food Restaurant|
1|Rouge Hill, Port Union, Highland Creek |43.784535 |-79.160497 |Great Shine Window Cleaning |43.783145 |-79.157431 |Home Service|
2|Rouge Hill, Port Union, Highland Creek |43.784535 |-79.160497 |Royal Canadian Legion |43.782533 |-79.163085 |Bar|
3|Guildwood, Morningside, West Hill |43.763573 |-79.188711 |RBC Royal Bank |43.766790 |-79.191151 |Bank |
4|Guildwood, Morningside, West Hill |43.763573 |-79.188711 |G & G Electronics |43.765309 |-79.191537 |Electronics Store |

## Methodology

We skipped the exploratory data analysis part, since they were done in prior workbooks for Toronto and Manhattan individually: There, we listed the neighborhood data to get a feel for the quality and shape of the data and visualized the neighborhoods by their geospational data with folium. Here, we only combined those data, so we would have repeated ourselves.

To get a numerical representation of the venue categories and their frequency, we used one-hot-encoding and taking the mean.

The main machine learning algorithm that we used for clustering is k-means, as in previous workbooks. Different cluster counts were tried, since the algorithm yielded almost no clusters with neighborhoods from both cities. We settled on a count of 5, since more resulted in no significant greater overlap between the cities and only produced more outliers.

Since we worked with non-numerical data, there was not much use for inferential statistical analysis or other visualization techniques to get an intuition about numerical data. Printing out the neighborhood data in dataframes and visualizing them in a map sufficed.

## Results

The results are not satisfactory. As noted in the methodology section, we could not find a way to get more overlap between neighborhoods of both cities by changing the cluster count. The overlap we got would not help a person looking to find a similar neighborhood as their old one in their new city.

## Discussions

The weak overlap in the clustering results may come from the categorical labels that Foursquare uses for venues in different cities. Since Foursquare has users contributing venue data, they may have a bias in their selection of category of a venue. As explored in the notebook, Toronto has 366 unique categories and Manhattan 272. They have 211 categories in common. This seems like a high enough commonality to yield overlapping clustering results, but it did not. The observation that Toronto has more outliers and more clusters can be explained by its higher number of unique categories.

Since the venue data seems sufficient to yield a satisfactory result, further work could attempt to use different clustering algorithms. A more detailed analysis about the frequency of venue categories that exist only for one of both cities could show that the venue data for both cities does not have a sufficiently high commonality. Which could mean we need better data, maybe from other data sources.

## Conclusion

Our aim was to give people a map of common neighborhoods in the cities of Toronto and Manhattan. The naive approach we took to just combine the data from previous notebooks did not yield useable clusters. Better data or other clustering algorithms could improve the results.
