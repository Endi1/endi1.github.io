# Using Python schema to validate scraped objects

Web scraping describes the various techniques employed in extracting data from websites. More often than not, this is done programmatically using tiny programs called spiders. However, web scraping offers insidious challenges and obstacles that might not be immediately obvious. One of them is issues with **data validity.** 

Usually the final objective of web scraping is extracting structured data from semi-structured or unstructured content. The structured data often comes with a prerequisite of data validity. In other words, for the structured data that we need to scrape, certain (or all) data points needs to satisfy some requirements in order to be considered valid. For example, let’s assume that we have to scrape property data from a collection of websites that list new properties for sale. We need to scrape the following data (among others):

- price (in USD)
- surface
- number of rooms
- location

For all of them we need to make sure that they are found in each object in the output, and that they are of the correct type and value. For example, *price, number of rooms, and surface* all need to be numbers. *location* must be a string. Furthermore, *number of rooms* must not be too big (there’s not that many houses with over 100 rooms on the market, if any). If any of these conditions are not met, then it means that there is a problem with scraping and parsing one (or more) of the pages and we risk sending faulty data downstream.

Most large-scale web crawling solutions will have some sort of pipeline in their workflow that is dedicated to validating and processing the scraped data. One way to do validation is by using ad-hoc logic that makes sure that the data points are of the type and value required. Often this involves complex multi-step checks and algorithms and it’s hard to scale and update as new data types are scraped or if old ones have updated requirements.

To avoid this, I like using a Python library called [schema](https://github.com/keleshev/schema). It allows you to validate JSON or YAML data using Python data types and logic. To continue the example above, for the property data we scrape, we can define the following schema:

    from schema import Schema, And, Use, Optional, SchemaError

    schema = Schema([{
    	'price': int,
    	'surface':  int,
    	'number_of_rooms': And(int, lambda n: 0 < n and n <= 100),
        'location': str
    }])

Finally in the pipeline we can simply do

    schema.validate(data)


If the data is valid then it will return the data, otherwise a `SchemaError` exception will be thrown.

Doing this we can add a clear validation layer that is separate from any processing layer which is easy to read, update, and scale as needed.