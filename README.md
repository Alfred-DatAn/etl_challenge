# ETL project: Covid-19 pandemic

### __EXTRACT -----__

Throught the __“coronavirus-monitor.p.rapidapi.com” API__ we extracted information related to the current Covid pandemic. This information is arranged by country and each country has information such as cases confirmed, deaths, patients recovered, among other variables up to the date the request was performed. The resulting request was a dictionary containing a list, and inside that list, a dictionary by country.

### __TRANSFORM -----__

After we requested the data, __we decided to gather all the countries' info into groups__ by continent, G7 and G20. For this we ran two approaches:

1.	Got the list of the country members by continent from another table hosted on internet, then matched that list with the countries retrieved by the API request.

2.	For the smallest groups (G7, G20), we looked directly into the indexes of those countries in the request.
By the time we built the groups, we did some “country_name” changes so both names in the list and the request matched. Eg. From “United Kingdom” to “UK”.

Once the all the groups were done, we had to transform all the numeric variables into integer values, since all the data was string datatype. For this we created a function __replace(region_list)__ to do the task.

### __LOAD -----__

Two methods were proposed to load the data to Mongo after the transformation was made:

1. The first one consisted of creating a single dictionary that contains each of the regions and groups. Then, the connection to mongo was established and the database was created. It should be mentioned, that inside of this database, __only a single collection was created__, and by using the update() method from pymongo, the dictionary with all the regions and groups was loaded. It must be said that with this method the way to extract the data from the Mongo database is by using the find() method from pymongo, and then iterating on this element.

2. The second method proposed was to __create a collection for each region and group inside the database__. Then, by using the method  insert_one() from pymongo, an iteration was made on each list to load the data. Unlike from the first method, the way to query documents, in this case, is by using directly pymongo query and projection operators.
