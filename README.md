# APIs - Cumulative Lab


## Introduction 

We've seen how the Yelp API works and how to create basic visualizations using Folium. It's time to put those skills to work in order to create a working map! Taking things a step further, you'll also independently explore how to perform pagination in order to retrieve a full results set from the Yelp API.

## Objectives

You will be able to: 

* Practice using functions to organize your code
* Use pagination to retrieve all results from an API query
* Practice parsing data returned from an API query
* Practice interpreting visualizations of a dataset
* Create maps using Folium

## Your Task: Query Yelp for All Businesses in a Category and Analyze the Results

![restaurant counter with pizza](images/restaurant_counter.jpg)

Photo by <a href="https://unsplash.com/@jordanmadrid?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jordan Madrid</a> on <a href="/s/photos/pizza-restaurant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

### Overview

You've now worked with some API calls, but we have yet to see how to retrieve a more complete dataset in a programmatic manner. In this lab, you will write a query of businesses on Yelp, then use *pagination* to retrieve all possible results for that query. Then you will create a summary of your findings, including a Folium map of the geographic locations of those businesses.

### Technical Details

Returning to the Yelp API, the [documentation](https://www.yelp.com/developers/documentation/v3/business_search) also provides us details regarding the **API limits**. These often include details about the number of requests a user is allowed to make within a specified time limit and the maximum number of results to be returned. In this case, we are told that any request has a **maximum of 50 results per request** and defaults to 20. Furthermore, any search will be limited to a **total of 1000 results**. To retrieve all 1000 of these results, we would have to page through the results piece by piece, retrieving 50 at a time. Processes such as these are often referred to as pagination.

Also, be mindful of the **API** ***rate*** **limits**. You can only make **5000 requests per day** and can make requests too fast. Start prototyping small before running a loop that could be faulty. You can also use `time.sleep(n)` to add delays. For more details see https://www.yelp.com/developers/documentation/v3/rate_limiting.

In this lab, you will define a search and then paginate over the results to retrieve all of the results. You'll then parse these responses as a list of dictionaries (for further exploration) and create a map using Folium to visualize the results geographically.

### Requirements

#### 1. Make the Initial Request

Start by filling in your API key to make the initial request to the business search API. Investigate the structure of the response you get back and start figuring out how you will extract the relevant information.

#### 2. Add Pagination

Using loops and functions, collect the maximum number of results for your query from the API.

#### 3. Perform Exploratory Analysis

Interpret visualizations related to the price range, average rating, and number of reviews for all query results.

#### 4. Create a Folium Map

Using latitude and longitude data, plot the query results on an interactive map.

## 1. Make the Initial Request

### Querying

Start by making an initial request to the Yelp API. Your search must include at least 2 parameters: **term** and **location**. For example, you might search for pizza restaurants in NYC. The term and location is up to you but make the request below.

Use the `requests` library ([documentation here](https://requests.readthedocs.io/en/master/user/quickstart/#make-a-request)).

You'll also need an API key from Yelp. If you haven't done this already, go to the Yelp [Manage App page](https://www.yelp.com/developers/v3/manage_app) and create a new app (after making an account if you haven't already).


```python
# Replace None with appropriate code

# Import the requests library
None

# Get this from the "Manage App" page. Make sure you set them
# back to None before pushing this to GitHub, since otherwise
# your credentials will be compromised
api_key = None

# These can be whatever you want! But the solution uses "pizza"
# and "New York NY" if you want to compare your work directly
term = None
location = None

# Set up params for request
url = "https://api.yelp.com/v3/businesses/search"
headers = {
    "Authorization": "Bearer {}".format(api_key)
}
url_params = {
    "term": term.replace(" ", "+"),
    "location": location.replace(" ", "+")
}

# Make the request using requests.get, passing in
# url, headers=headers, and params=url_params
response = None

# Confirm we got a 200 response
response
```


```python
# __SOLUTION__

# IMPORTANT: THIS SOLUTION WILL NOT RUN UNLESSS YOU FILL IN THE API KEY!

# Import the requests library
import requests

# Get this from the "Manage App" page. Make sure you set them
# back to None before pushing this to GitHub, since otherwise
# your credentials will be compromised
api_key = None

# These can be whatever you want! But the solution uses "pizza"
# and "New York NY" if you want to compare your work directly
term = "pizza"
location = "New York NY"

# Set up params for request
url = "https://api.yelp.com/v3/businesses/search"
headers = {
    "Authorization": "Bearer {}".format(api_key)
}
url_params = {
    "term": term.replace(" ", "+"),
    "location": location.replace(" ", "+")
}

# Make the request using requests.get, passing in
# url, headers=headers, and params=url_params
response = requests.get(url, headers=headers, params=url_params)

# Confirm we got a 200 response
response
```




    <Response [200]>




```python
# Run this cell without changes

# Get the response body in JSON format
response_json = response.json()
# View the keys
response_json.keys()
```


```python
# __SOLUTION__

# Get the response body in JSON format
response_json = response.json()
# View the keys
response_json.keys()
```




    dict_keys(['businesses', 'total', 'region'])



### Extracting Data

Now, retrieve the value associated with the `'businesses'` key, and inspect its contents.


```python
# Replace None with appropriate code

# Retrieve the value from response_json
businesses = None

# View the first 2 records
businesses[:2]
```


```python
# __SOLUTION__

# Retrieve the value from response_json
businesses = response_json["businesses"]

# View the first 2 records
businesses[:2]
```




    [{'id': 'ysqgdbSrezXgVwER2kQWKA',
      'alias': 'julianas-pizza-brooklyn-5',
      'name': "Juliana's Pizza",
      'image_url': 'https://s3-media2.fl.yelpcdn.com/bphoto/clscwgOF9_Ecq-Rwsq7jyQ/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/julianas-pizza-brooklyn-5?adjust_creative=fnLNGidUiBCfkbeVwRKDHA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=fnLNGidUiBCfkbeVwRKDHA',
      'review_count': 2273,
      'categories': [{'alias': 'pizza', 'title': 'Pizza'}],
      'rating': 4.5,
      'coordinates': {'latitude': 40.70274718768062,
       'longitude': -73.99343490196397},
      'transactions': ['delivery'],
      'price': '$$',
      'location': {'address1': '19 Old Fulton St',
       'address2': '',
       'address3': '',
       'city': 'Brooklyn',
       'zip_code': '11201',
       'country': 'US',
       'state': 'NY',
       'display_address': ['19 Old Fulton St', 'Brooklyn, NY 11201']},
      'phone': '+17185966700',
      'display_phone': '(718) 596-6700',
      'distance': 308.56984360837544},
     {'id': 'zj8Lq1T8KIC5zwFief15jg',
      'alias': 'prince-street-pizza-new-york-2',
      'name': 'Prince Street Pizza',
      'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/ZAukOyv530w4KjOHC5YY1w/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/prince-street-pizza-new-york-2?adjust_creative=fnLNGidUiBCfkbeVwRKDHA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=fnLNGidUiBCfkbeVwRKDHA',
      'review_count': 3770,
      'categories': [{'alias': 'pizza', 'title': 'Pizza'},
       {'alias': 'italian', 'title': 'Italian'}],
      'rating': 4.5,
      'coordinates': {'latitude': 40.72308755605564,
       'longitude': -73.99453001177575},
      'transactions': ['delivery', 'pickup'],
      'price': '$',
      'location': {'address1': '27 Prince St',
       'address2': None,
       'address3': '',
       'city': 'New York',
       'zip_code': '10012',
       'country': 'US',
       'state': 'NY',
       'display_address': ['27 Prince St', 'New York, NY 10012']},
      'phone': '+12129664100',
      'display_phone': '(212) 966-4100',
      'distance': 1961.8771417367063}]



### Preparing Data

Write a function `prepare_data` that takes in a list of dictionaries like `businesses` and returns a copy that has been prepared for analysis:

1. The `coordinates` key-value pair has been converted into two separate key-value pairs, `latitude` and `longitude`
2. All other key-value pairs except for `name`, `review_count`, `rating`, and `price` have been dropped
3. All dictionaries missing one of the relevant keys or containing null values have been dropped

In other words, the final keys for each dictionary should be `name`, `review_count`, `rating`, `price`, `latitude`, and `longitude`.

Complete the function in the cell below:


```python
# Replace None with appropriate code

def prepare_data(data_list):
    """
    This function takes in a list of dictionaries and prepares it
    for analysis
    """
    
    # Make a new list to hold results
    results = []
    
    for business_data in data_list:
    
        # Make a new dictionary to hold prepared data for this business
        prepared_data = {}
        
        # Extract name, review_count, rating, and price key-value pairs
        # from business_data and add to prepared_data
        # If a key is not present in business_data, add it to prepared_data
        # with an associated value of None
        None
    
        # Parse and add latitude and longitude columns
        None
        
        # Add to list if all values are present
        if all(prepared_data.values()):
            results.append(prepared_data)
    
    return results
    
# Test out function
prepared_businesses = prepare_data(businesses)
prepared_businesses[:5]
```


```python
# __SOLUTION__

def prepare_data(data_list):
    """
    This function takes in a list of dictionaries and prepares it
    for analysis
    """
    
    # Make a new list to hold results
    results = []
    
    for business_data in data_list:
    
        # Make a new dictionary to hold prepared data for this business
        prepared_data = {}
        
        # Extract name, review_count, rating, and price key-value pairs
        # from business_data and add to prepared_data
        # If a key is not present in business_data, add it to prepared_data
        # with an associated value of None
        for key in ("name", "review_count", "rating", "price"):
            prepared_data[key] = business_data.get(key, None)
    
        # Parse and add latitude and longitude columns
        coordinates = business_data["coordinates"]
        prepared_data["latitude"] = coordinates["latitude"]
        prepared_data["longitude"] = coordinates["longitude"]
        
        # Add to list if all values are present
        if all(prepared_data.values()):
            results.append(prepared_data)
    
    return results
    
# Test out function
prepared_businesses = prepare_data(businesses)
prepared_businesses[:5]
```




    [{'name': "Juliana's Pizza",
      'review_count': 2273,
      'rating': 4.5,
      'price': '$$',
      'latitude': 40.70274718768062,
      'longitude': -73.99343490196397},
     {'name': 'Prince Street Pizza',
      'review_count': 3770,
      'rating': 4.5,
      'price': '$',
      'latitude': 40.72308755605564,
      'longitude': -73.99453001177575},
     {'name': "Lombardi's Pizza",
      'review_count': 6137,
      'rating': 4.0,
      'price': '$$',
      'latitude': 40.7215934960083,
      'longitude': -73.9955956044561},
     {'name': 'Rubirosa',
      'review_count': 2334,
      'rating': 4.5,
      'price': '$$',
      'latitude': 40.722766,
      'longitude': -73.996233},
     {'name': "Grimaldi's Pizzeria",
      'review_count': 4635,
      'rating': 3.5,
      'price': '$$',
      'latitude': 40.702583,
      'longitude': -73.9932413}]



Check that your function created the correct keys:


```python
# Run this cell without changes

assert sorted(list(prepared_businesses[0].keys())) == ['latitude', 'longitude', 'name', 'price', 'rating', 'review_count']
```


```python
# __SOLUTION__

assert sorted(list(prepared_businesses[0].keys())) == ['latitude', 'longitude', 'name', 'price', 'rating', 'review_count']
```

The following code will differ depending on your query, but we expect there to be 20 businesses in the original list, and potentially fewer in the prepared list (if any of them were missing data):


```python
# Run this cell without changes
print("Original:", len(businesses))
print("Prepared:", len(prepared_businesses))
```


```python
# __SOLUTION__
print("Original:", len(businesses))
print("Prepared:", len(prepared_businesses))
```

    Original: 20
    Prepared: 19


Great! We will reuse this function once we have retrieved the full dataset.

## 2. Add Pagination

Now that you are able to extract information from one page of the response, let's figure out how to request as many pages as possible.

### Determining the Total

Depending on the number of total results for your query, you will either retrieve all of the results, or just the first 1000 (if there are more than 1000 total).

We can find the total number of results using the `"total"` key:


```python
# Run this cell without changes
response_json["total"]
```


```python
# __SOLUTION__
response_json["total"]
```




    11600



(This is specific to the implementation of the Yelp API. Some APIs will just tell you that there are more pages, or will tell you the number of pages total, rather than the total number of results. If you're not sure, always check the documentation.)

In the cell below, assign the variable `total` to either the value shown above (if it is less than 1000), or 1000.


```python
# Replace None with appropriate code
total = None
```


```python
# __SOLUTION__
# For "pizza" in NYC, there are well over 1000 results, so we assign
# total to 1000
total = 1000
```

### Calculating the Offsets

The documentation states in the parameters section:

> **Name**: `limit`, **Type:** int, **Description:** Optional. Number of business results to return. By default, it will return 20. Maximum is 50.

> **Name**: `offset`, **Type:** int, **Description:** Optional. Offset the list of returned business results by this amount.

So, to get the most results with the fewest API calls we want to set a limit of 50 every time. If, say, we wanted to get 210 total results, that would mean:

1. Offset of `0` (first 50 records)
2. Offset of `50` (second 50 records)
3. Offset of `100` (third 50 records)
4. Offset of `150` (fourth 50 records)
5. Offset of `200` (final 10 records)

In the cell below, create a function `get_offsets` that takes in a total and returns a list of offsets for that total. You can assume that there is a limit of 50 every time.

*Hint: you can use `range` ([documentation here](https://docs.python.org/3.3/library/stdtypes.html?highlight=range#range)) to do this in one line of code. Just make the returned result is a list.*


```python
# Replace None with appropriate code
def get_offsets(total):
    """
    Get a list of offsets needed to get all pages
    of data up until the total
    """
    None
```


```python
# __SOLUTION__
def get_offsets(total):
    """
    Get a list of offsets needed to get all pages
    of data up until the total
    """
    return list(range(0, total, 50))
```

Check that your function works below:


```python
# Run this cell without changes

assert get_offsets(200) == [0, 50, 100, 150]
assert get_offsets(210) == [0, 50, 100, 150, 200]
```


```python
# __SOLUTION__

assert get_offsets(200) == [0, 50, 100, 150]
assert get_offsets(210) == [0, 50, 100, 150, 200]
```

### Putting It All Together

Recall that the following variable has already been declared for you:


```python
# Run this cell without changes
url_params
```


```python
# __SOLUTION__
url_params
```




    {'term': 'pizza', 'location': 'New+York+NY'}



We'll go ahead and also specify that the limit should be 50 every time:


```python
# Run this cell without changes
url_params["limit"] = 50
```


```python
# __SOLUTION__
url_params["limit"] = 50
```

In order to modify the offset, you'll need to add it to `url_params` with the key `"offset"` and whatever value is needed.

In the cell below, write code that:

* Creates an empty list for the full prepared dataset
* Loops over all of the offsets from `get_offsets` and makes an API call each time with the specified offset
* Calls `prepare_data` to get a cleaned version of the result of each API call
* Extends the full prepared dataset list with each query's prepared dataset


```python
# Replace None with appropriate code

# Create an empty list for the full prepared dataset
full_dataset = None

for offset in get_offsets(total):
    # Add or update the "offset" key-value pair in url_params
    None
    
    # Make the query and get the response
    response = requests.get(url, headers=headers, params=url_params)
    
    # Get the response body in JSON format
    response_json = None
    
    # Get the list of businesses from the response_json
    businesses = None
    
    # Call the prepare_data function to get a list of processed data
    prepared_businesses = None
    
    # Extend full_dataset with this list (don't append, or you'll get
    # a list of lists instead of a flat list)
    None

# Check the length of the full dataset. It will be up to `total`,
# potentially less if there were missing values
len(full_dataset)
```


```python
# __SOLUTION__

# Create an empty list for the full prepared dataset
full_dataset = []

for offset in get_offsets(total):
    # Add or update the "offset" key-value pair in url_params
    url_params["offset"] = offset
    
    # Make the query and get the response
    response = requests.get(url, headers=headers, params=url_params)
    
    # Get the response body in JSON format
    response_json = response.json()
    
    # Get the list of businesses from the response_json
    businesses = response_json["businesses"]
    
    # Call the prepare_data function to get a list of processed data
    prepared_businesses = prepare_data(businesses)
    
    # Extend full_dataset with this list (don't append, or you'll get
    # a list of lists instead of a flat list)
    full_dataset.extend(prepared_businesses)

# Check the length of the full dataset. It will be up to `total`,
# potentially less if there were missing values
len(full_dataset)
```




    812



This code may take up to a few minutes to run.

If you get an error trying to get the response body in JSON format, try adding `time.sleep(1)` right after the `requests.get` line, so your code will sleep for 1 second between each API call.

## 3. Perform Exploratory Analysis

Take the businesses from the previous question and do an initial exploratory analysis. We have provided some plots for you to interpret:


```python
# Run this cell without changes
from collections import Counter
import matplotlib.pyplot as plt
%matplotlib inline

fig, (ax1, ax2, ax3) = plt.subplots(ncols=3, figsize=(16, 5))

# Plot distribution of number of reviews
all_review_counts = [x["review_count"] for x in full_dataset]
ax1.hist(all_review_counts)
ax1.set_title("Review Count Distribution")
ax1.set_xlabel("Number of Reviews")
ax1.set_ylabel("Number of Businesses")

# Plot rating distribution
all_ratings = [x["rating"] for x in full_dataset]
rating_counter = Counter(all_ratings)
rating_keys = sorted(rating_counter.keys())
ax2.bar(rating_keys, [rating_counter[key] for key in rating_keys])
ax2.set_title("Rating Distribution")
ax2.set_xlabel("Rating")
ax2.set_ylabel("Number of Businesses")

# Plot price distribution
all_prices = [x["price"].replace("$", r"\$") for x in full_dataset]
price_counter = Counter(all_prices)
price_keys = sorted(price_counter.keys())
ax3.bar(price_keys, [price_counter[key] for key in price_keys])
ax3.set_title("Price Distribution")
ax3.set_xlabel("Price Category")
ax3.set_ylabel("Number of Businesses");
```


```python
# __SOLUTION__
from collections import Counter
import matplotlib.pyplot as plt
%matplotlib inline

fig, (ax1, ax2, ax3) = plt.subplots(ncols=3, figsize=(16, 5))

# Plot distribution of number of reviews
all_review_counts = [x["review_count"] for x in full_dataset]
ax1.hist(all_review_counts)
ax1.set_title("Review Count Distribution")
ax1.set_xlabel("Number of Reviews")
ax1.set_ylabel("Number of Businesses")

# Plot rating distribution
all_ratings = [x["rating"] for x in full_dataset]
rating_counter = Counter(all_ratings)
rating_keys = sorted(rating_counter.keys())
ax2.bar(rating_keys, [rating_counter[key] for key in rating_keys])
ax2.set_title("Rating Distribution")
ax2.set_xlabel("Rating")
ax2.set_ylabel("Number of Businesses")

# Plot price distribution
all_prices = [x["price"].replace("$", r"\$") for x in full_dataset]
price_counter = Counter(all_prices)
price_keys = sorted(price_counter.keys())
ax3.bar(price_keys, [price_counter[key] for key in price_keys])
ax3.set_title("Price Distribution")
ax3.set_xlabel("Price Category")
ax3.set_ylabel("Number of Businesses");
```


    
![png](index_files/index_45_0.png)
    


Describe the distributions displayed above and interpret them in the context of your query. (Your answer may differ from the solution branch depending on your query.)


```python
# Replace None with appropriate text
"""
None
"""
```


```python
# __SOLUTION__
"""
The review count distribution is very skewed, with the majority of businesses
having fewer than 1000 reviews, but some businesses with as many as 6000. This
is a fairly typical "long tail problem" we see with ratings/reviews data, although
that is usually represented with flipped axes

The rating distribution centers and peaks around 4, meaning the typical rating is
around 4 out of 5. There are no businesses with a rating below 2 in this dataset.

The price distribution is also skewed, with almost all results falling into the
$ or $$ category. This makes sense since we searched for "pizza".
"""
```

In the cell below, we also plot the rating distributions by price. In this setup, a price of one dollar sign is "lower price" and everything else is "higher price".


```python
# Run this cell without changes

higher_price = []
lower_price = []
for row in full_dataset:
    if row["price"] == "$":
        lower_price.append(row["rating"])
    else:
        higher_price.append(row["rating"])
        
fig, ax = plt.subplots()

ax.hist([higher_price, lower_price], label=["higher price", "lower price"], density=True)

ax.legend();
```


```python
# __SOLUTION__
higher_price = []
lower_price = []
for row in full_dataset:
    if row["price"] == "$":
        lower_price.append(row["rating"])
    else:
        higher_price.append(row["rating"])
        
fig, ax = plt.subplots()

ax.hist([higher_price, lower_price], label=["higher price", "lower price"], density=True)

ax.legend();
```


    
![png](index_files/index_51_0.png)
    


Is a higher price associated with a higher rating? (No need for any additional math/statistics, just interpret what you see in the plot.)


```python
# Replace None with appropriate text
"""
None
"""
```


```python
# __SOLUTION__
"""
Yes, higher price seems to be associated with a higher rating.

The peaks of the blue bars (higher price) are higher for ratings
of 4 or higher, whereas the peaks of the orange bars (lower price)
are higher for ratings of 3.5 or lower.
"""
```

Finally, let's look at ratings vs. review counts:


```python
# Run this cell without changes
fig, ax = plt.subplots(figsize=(16,5))

ax.scatter(all_review_counts, all_ratings, alpha=0.2)
ax.set_xlabel("Number of Reviews")
ax.set_ylabel("Rating")
# "zoom in" to a subset of review counts
ax.set_xlim(left=0, right=1000);
```


```python
# __SOLUTION__
fig, ax = plt.subplots(figsize=(16,5))

ax.scatter(all_review_counts, all_ratings, alpha=0.2)
ax.set_xlabel("Number of Reviews")
ax.set_ylabel("Rating")
# "zoom in" to a subset of review counts
ax.set_xlim(left=0, right=1000);
```


    
![png](index_files/index_57_0.png)
    


Is a higher number of reviews associated with a higher rating?


```python
# Replace None with appropriate text
"""
None
"""
```


```python
# __SOLUTION__
"""
This answer is a bit more ambiguous. Businesses with ratings of 5
OR below 3 seem to have fewer than 400 reviews. But also there
are a lot more ratings of 3.5 for businesses with <100 reviews
than ratings of 3.0. We would need some more analysis and potentially
a clarification of the question to answer this more thoroughly.
"""
```

## 4. Create a Folium Map

Make a map using Folium of the businesses you retrieved. Be sure to also add popups to the markers giving some basic information such as name, rating and price.

You can center the map around the latitude and longitude of the first item in `full_dataset`.


```python
# Replace None with appropriate code

# Import the library
None

# Set up center latitude and longitude
center_lat = None
center_long = None

# Initialize map with center lat and long
yelp_map = None

# Adjust this limit to see more or fewer businesses
limit=100

for business in full_dataset[:limit]:
    # Extract information about business
    lat = None
    long = None
    name = None
    rating = None
    price = None
    details = "{}\nPrice: {} Rating:{}".format(name,price,rating)
    
    # Create popup with relevant details
    popup = None
    
    # Create marker with relevant lat/long and popup
    marker = None
    
    marker.add_to(yelp_map)
    
yelp_map
```


```python
# __SOLUTION__

# Import the library
import folium

# Set up center latitude and longitude
center_lat = full_dataset[0]["latitude"]
center_long = full_dataset[0]["longitude"]

# Initialize map with center lat and long
yelp_map = folium.Map([center_lat, center_long], zoom_start=12)

# Adjust this limit to see more or fewer businesses
limit=100

for business in full_dataset[:limit]:
    # Extract information about business
    lat = business["latitude"]
    long = business["longitude"]
    name = business["name"]
    rating = business["rating"]
    price = business["price"]
    details = "{}\nPrice: {}\nRating: {}".format(name,price,rating)
    
    # Create popup with relevant details
    popup = folium.Popup(details)
    
    # Create marker with relevant lat/long and popup
    marker = folium.Marker([lat, long], popup=popup)
    
    marker.add_to(yelp_map)

yelp_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMiB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyIiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFs0MC43MDI3NDcxODc2ODA2MiwgLTczLjk5MzQzNDkwMTk2Mzk3XSwKICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3LAogICAgICAgICAgICAgICAgICAgIHpvb206IDEyLAogICAgICAgICAgICAgICAgICAgIHpvb21Db250cm9sOiB0cnVlLAogICAgICAgICAgICAgICAgICAgIHByZWZlckNhbnZhczogZmFsc2UsCiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICk7CgogICAgICAgICAgICAKCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfMDExMGRlZDgwNDQ3NDA2Y2FmYTNhMTg3MGFhNDIwZmEgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICJodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZyIsCiAgICAgICAgICAgICAgICB7ImF0dHJpYnV0aW9uIjogIkRhdGEgYnkgXHUwMDI2Y29weTsgXHUwMDNjYSBocmVmPVwiaHR0cDovL29wZW5zdHJlZXRtYXAub3JnXCJcdTAwM2VPcGVuU3RyZWV0TWFwXHUwMDNjL2FcdTAwM2UsIHVuZGVyIFx1MDAzY2EgaHJlZj1cImh0dHA6Ly93d3cub3BlbnN0cmVldG1hcC5vcmcvY29weXJpZ2h0XCJcdTAwM2VPRGJMXHUwMDNjL2FcdTAwM2UuIiwgImRldGVjdFJldGluYSI6IGZhbHNlLCAibWF4TmF0aXZlWm9vbSI6IDE4LCAibWF4Wm9vbSI6IDE4LCAibWluWm9vbSI6IDAsICJub1dyYXAiOiBmYWxzZSwgIm9wYWNpdHkiOiAxLCAic3ViZG9tYWlucyI6ICJhYmMiLCAidG1zIjogZmFsc2V9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzMwNDZmOTNlMzYyMzQ1NmI4MmRjYTE1M2EwMGFhYjI2ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzAyNzQ3MTg3NjgwNjIsIC03My45OTM0MzQ5MDE5NjM5N10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfOGRmZWFjYzE3YzIwNGVmN2FhZGUwNTNhOTc3ZTVhMmYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzNmNTdhYmI1MGUwYTQ0NTQ5OTg2MTdmZjVhMzcwYTNjID0gJChgPGRpdiBpZD0iaHRtbF8zZjU3YWJiNTBlMGE0NDU0OTk4NjE3ZmY1YTM3MGEzYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SnVsaWFuYSdzIFBpenphIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF84ZGZlYWNjMTdjMjA0ZWY3YWFkZTA1M2E5NzdlNWEyZi5zZXRDb250ZW50KGh0bWxfM2Y1N2FiYjUwZTBhNDQ1NDk5ODYxN2ZmNWEzNzBhM2MpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMzA0NmY5M2UzNjIzNDU2YjgyZGNhMTUzYTAwYWFiMjYuYmluZFBvcHVwKHBvcHVwXzhkZmVhY2MxN2MyMDRlZjdhYWRlMDUzYTk3N2U1YTJmKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2JjN2NjOWFhYWM5ZDQ1OTQ5OWRlMDQyNGU1ZTRlYTg4ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzIzMDg3NTU2MDU1NjQsIC03My45OTQ1MzAwMTE3NzU3NV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMDZkMmQyMTU2MGExNGQyNDkzZDEyNmNlYTlmM2ZiNjMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2NiYTlhNzk5MmFjOTRlYTJiOTcxNjQ3NGRmMTgzOTMyID0gJChgPGRpdiBpZD0iaHRtbF9jYmE5YTc5OTJhYzk0ZWEyYjk3MTY0NzRkZjE4MzkzMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UHJpbmNlIFN0cmVldCBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8wNmQyZDIxNTYwYTE0ZDI0OTNkMTI2Y2VhOWYzZmI2My5zZXRDb250ZW50KGh0bWxfY2JhOWE3OTkyYWM5NGVhMmI5NzE2NDc0ZGYxODM5MzIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYmM3Y2M5YWFhYzlkNDU5NDk5ZGUwNDI0ZTVlNGVhODguYmluZFBvcHVwKHBvcHVwXzA2ZDJkMjE1NjBhMTRkMjQ5M2QxMjZjZWE5ZjNmYjYzKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzM3MDRjM2NhMDcwMTRiYTE5N2ZkYzYzOTIzM2QxYjliID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzIxNTkzNDk2MDA4MywgLTczLjk5NTU5NTYwNDQ1NjFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzNjYzFhMjQ1ODEwOTQyYjI4OTFjMTI1NjYwNTVjZjE2ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9mYzc5MjMxMjE5YjA0YTdhYmE1ZGQ2ZGY4OWIxN2FmYiA9ICQoYDxkaXYgaWQ9Imh0bWxfZmM3OTIzMTIxOWIwNGE3YWJhNWRkNmRmODliMTdhZmIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxvbWJhcmRpJ3MgUGl6emEgUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzNjYzFhMjQ1ODEwOTQyYjI4OTFjMTI1NjYwNTVjZjE2LnNldENvbnRlbnQoaHRtbF9mYzc5MjMxMjE5YjA0YTdhYmE1ZGQ2ZGY4OWIxN2FmYik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8zNzA0YzNjYTA3MDE0YmExOTdmZGM2MzkyMzNkMWI5Yi5iaW5kUG9wdXAocG9wdXBfM2NjMWEyNDU4MTA5NDJiMjg5MWMxMjU2NjA1NWNmMTYpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfOWY4Y2Y2NjdhODRhNDJhZmFlYjBlMWYyMzFkNGY2MjAgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MjI3NjYsIC03My45OTYyMzNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzUwN2ViZWQ1NjFjODQwNTVhNzU2OGE3MzE5YTM3ZDVjID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9lZGFhZmVkMTc0MjI0MGI3YTk4Y2I0ZWY3ZThlYmZmZSA9ICQoYDxkaXYgaWQ9Imh0bWxfZWRhYWZlZDE3NDIyNDBiN2E5OGNiNGVmN2U4ZWJmZmUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJ1Ymlyb3NhIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81MDdlYmVkNTYxYzg0MDU1YTc1NjhhNzMxOWEzN2Q1Yy5zZXRDb250ZW50KGh0bWxfZWRhYWZlZDE3NDIyNDBiN2E5OGNiNGVmN2U4ZWJmZmUpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfOWY4Y2Y2NjdhODRhNDJhZmFlYjBlMWYyMzFkNGY2MjAuYmluZFBvcHVwKHBvcHVwXzUwN2ViZWQ1NjFjODQwNTVhNzU2OGE3MzE5YTM3ZDVjKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzM1OTk2MzUwNTJhMjRlM2M5ZWFhMDg0MDBiYWRmODM3ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzAyNTgzLCAtNzMuOTkzMjQxM10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfN2FiOTE0YTUyOTI3NGYwNTgzZDg2MTI2MWQ1MDQ2Y2IgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2ZkNzZmZDMxNjA3ZTQyODg4MWVjMTI4Yzg2ZGJlNTUwID0gJChgPGRpdiBpZD0iaHRtbF9mZDc2ZmQzMTYwN2U0Mjg4ODFlYzEyOGM4NmRiZTU1MCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+R3JpbWFsZGkncyBQaXp6ZXJpYSBQcmljZTogJCQgUmF0aW5nOiAzLjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfN2FiOTE0YTUyOTI3NGYwNTgzZDg2MTI2MWQ1MDQ2Y2Iuc2V0Q29udGVudChodG1sX2ZkNzZmZDMxNjA3ZTQyODg4MWVjMTI4Yzg2ZGJlNTUwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzM1OTk2MzUwNTJhMjRlM2M5ZWFhMDg0MDBiYWRmODM3LmJpbmRQb3B1cChwb3B1cF83YWI5MTRhNTI5Mjc0ZjA1ODNkODYxMjYxZDUwNDZjYikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl80MTQzZjU0Mzk5M2Y0MjIxYmIyMzE0OGMyMDU2MjczOSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcyOTU0NiwgLTczLjk1ODU2OF0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZjBjZDJhNWQ0Y2Q3NDZkZGE4ZWQyNmNkMDQ0MTJhY2UgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2E4NTI1NWJmZDM4MDQyZDFhNjAyNTQ3NjBjNzg5NzUzID0gJChgPGRpdiBpZD0iaHRtbF9hODUyNTViZmQzODA0MmQxYTYwMjU0NzYwYzc4OTc1MyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UGF1bGllIEdlZSdzIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9mMGNkMmE1ZDRjZDc0NmRkYThlZDI2Y2QwNDQxMmFjZS5zZXRDb250ZW50KGh0bWxfYTg1MjU1YmZkMzgwNDJkMWE2MDI1NDc2MGM3ODk3NTMpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNDE0M2Y1NDM5OTNmNDIyMWJiMjMxNDhjMjA1NjI3MzkuYmluZFBvcHVwKHBvcHVwX2YwY2QyYTVkNGNkNzQ2ZGRhOGVkMjZjZDA0NDEyYWNlKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2MwNjM0NzIyMWQ5NzQyYTA5NWRkZDgxNTkzYjEwOTRiID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzE1MzQsIC03My45OTE0XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yMmZmNGNlOTQ5ZGQ0ZjZjYWNiMGEyYmZmNzdjYWJiNSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYmQxMzA3MjEwZGNmNGUyMDk0ZmFlMDVjMjA5M2I5ZmEgPSAkKGA8ZGl2IGlkPSJodG1sX2JkMTMwNzIxMGRjZjRlMjA5NGZhZTA1YzIwOTNiOWZhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TY2FycidzIFBpenphIFByaWNlOiAkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzIyZmY0Y2U5NDlkZDRmNmNhY2IwYTJiZmY3N2NhYmI1LnNldENvbnRlbnQoaHRtbF9iZDEzMDcyMTBkY2Y0ZTIwOTRmYWUwNWMyMDkzYjlmYSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9jMDYzNDcyMjFkOTc0MmEwOTVkZGQ4MTU5M2IxMDk0Yi5iaW5kUG9wdXAocG9wdXBfMjJmZjRjZTk0OWRkNGY2Y2FjYjBhMmJmZjc3Y2FiYjUpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMjI0MWQzMzlmN2Y2NGUyNTkzMmMyZTg0NWRiNmViNjUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MDQ5MywgLTczLjkzMzk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8wYWRiZmQ3ODBmOGQ0MzE5YTAwZTc0MTkyYTM4MWM3MCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYjhkMzk3ODNjZDNhNDg4YzgzOWFjOGE3MTVjN2Q3YTkgPSAkKGA8ZGl2IGlkPSJodG1sX2I4ZDM5NzgzY2QzYTQ4OGM4MzlhYzhhNzE1YzdkN2E5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Sb2JlcnRhJ3MgUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzBhZGJmZDc4MGY4ZDQzMTlhMDBlNzQxOTJhMzgxYzcwLnNldENvbnRlbnQoaHRtbF9iOGQzOTc4M2NkM2E0ODhjODM5YWM4YTcxNWM3ZDdhOSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8yMjQxZDMzOWY3ZjY0ZTI1OTMyYzJlODQ1ZGI2ZWI2NS5iaW5kUG9wdXAocG9wdXBfMGFkYmZkNzgwZjhkNDMxOWEwMGU3NDE5MmEzODFjNzApCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMzZhNzViNjgxNTNlNGYzNjg2NzllNDZjM2Y0OTE3YWYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MjgxOTksIC03My45ODUxODJdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzVmNTg4Y2I2NzFmOTRjMjk5MjM0NWE2MDE3ZTM0OTVlID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF81OTU3ZjE1MDRlZjE0MzA5YmNkYzgyMDVmZTIyMTQ5OCA9ICQoYDxkaXYgaWQ9Imh0bWxfNTk1N2YxNTA0ZWYxNDMwOWJjZGM4MjA1ZmUyMjE0OTgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkVhc3QgVmlsbGFnZSBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81ZjU4OGNiNjcxZjk0YzI5OTIzNDVhNjAxN2UzNDk1ZS5zZXRDb250ZW50KGh0bWxfNTk1N2YxNTA0ZWYxNDMwOWJjZGM4MjA1ZmUyMjE0OTgpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMzZhNzViNjgxNTNlNGYzNjg2NzllNDZjM2Y0OTE3YWYuYmluZFBvcHVwKHBvcHVwXzVmNTg4Y2I2NzFmOTRjMjk5MjM0NWE2MDE3ZTM0OTVlKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2ZiOTI2NWYzMjEwMTQ2ZTNhNDI2NmFlMjczNzQzMWRhID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjk1MDMwOSwgLTczLjk5NjEyNTJdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzgwYmQ1NmJmNjQ4NDRmMzNhNzZlZjU1MWEwNmRjMWY1ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9jNjU3ODAwNTU0Njc0ZjEwYWNhMmIyNjdjNTQzNTcxYyA9ICQoYDxkaXYgaWQ9Imh0bWxfYzY1NzgwMDU1NDY3NGYxMGFjYTJiMjY3YzU0MzU3MWMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRlbGxhcm9jY28ncyBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfODBiZDU2YmY2NDg0NGYzM2E3NmVmNTUxYTA2ZGMxZjUuc2V0Q29udGVudChodG1sX2M2NTc4MDA1NTQ2NzRmMTBhY2EyYjI2N2M1NDM1NzFjKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2ZiOTI2NWYzMjEwMTQ2ZTNhNDI2NmFlMjczNzQzMWRhLmJpbmRQb3B1cChwb3B1cF84MGJkNTZiZjY0ODQ0ZjMzYTc2ZWY1NTFhMDZkYzFmNSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8xZjk3ZTdjOTlkZmI0ODAzYWQ5YTg3MThiOGI3NDkzMSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjY4MTgsIC03NC4wMDAyNF0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMmZhYTY0NzNkODYyNDA2ZmExNmZlYTEwMmFlNDg0ZGMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzk1ZTBkYmVmMTJhNTQ4MzQ5MTkxZGI4YWRhOGY4M2MwID0gJChgPGRpdiBpZD0iaHRtbF85NWUwZGJlZjEyYTU0ODM0OTE5MWRiOGFkYThmODNjMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+THVjYWxpIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8yZmFhNjQ3M2Q4NjI0MDZmYTE2ZmVhMTAyYWU0ODRkYy5zZXRDb250ZW50KGh0bWxfOTVlMGRiZWYxMmE1NDgzNDkxOTFkYjhhZGE4ZjgzYzApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMWY5N2U3Yzk5ZGZiNDgwM2FkOWE4NzE4YjhiNzQ5MzEuYmluZFBvcHVwKHBvcHVwXzJmYWE2NDczZDg2MjQwNmZhMTZmZWExMDJhZTQ4NGRjKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2M2ZGFmZGQ4YWQ2MzRlZWY4MzgyZTZjMDVkZmM3OWQ0ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzMwNjAwNzYsIC03NC4wMDIxNTk5N10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfODJkM2FiNjA4MmU2NDZkMjlkMmQyZGYxYzRlZjUyYWEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzM3NDkwYzY0ODgxYjRlMWVhZmMzNDMyMTgzMzc2NTMwID0gJChgPGRpdiBpZD0iaHRtbF8zNzQ5MGM2NDg4MWI0ZTFlYWZjMzQzMjE4MzM3NjUzMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Sm9lJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfODJkM2FiNjA4MmU2NDZkMjlkMmQyZGYxYzRlZjUyYWEuc2V0Q29udGVudChodG1sXzM3NDkwYzY0ODgxYjRlMWVhZmMzNDMyMTgzMzc2NTMwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2M2ZGFmZGQ4YWQ2MzRlZWY4MzgyZTZjMDVkZmM3OWQ0LmJpbmRQb3B1cChwb3B1cF84MmQzYWI2MDgyZTY0NmQyOWQyZDJkZjFjNGVmNTJhYSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9mOGQ3NzAxMGMyODI0MDUxODYzODZiMGZjMWQyNDVmNSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcwMjQ0LCAtNzMuOTg5NDNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzRlYTlhMjk5MWQ2YTQzYTI4ZjZlNDdjNGVlZjMwZTE5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9iNTllMWNmMjRkNmM0Yzk5YTMzYWEzN2YwYzE0YzhhZiA9ICQoYDxkaXYgaWQ9Imh0bWxfYjU5ZTFjZjI0ZDZjNGM5OWEzM2FhMzdmMGMxNGM4YWYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkZyb250IFN0cmVldCBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF80ZWE5YTI5OTFkNmE0M2EyOGY2ZTQ3YzRlZWYzMGUxOS5zZXRDb250ZW50KGh0bWxfYjU5ZTFjZjI0ZDZjNGM5OWEzM2FhMzdmMGMxNGM4YWYpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZjhkNzcwMTBjMjgyNDA1MTg2Mzg2YjBmYzFkMjQ1ZjUuYmluZFBvcHVwKHBvcHVwXzRlYTlhMjk5MWQ2YTQzYTI4ZjZlNDdjNGVlZjMwZTE5KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzlmMDFkZDZiZDE2ODRiMGViNmNhMWZlNWZkYTQxY2QwID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjkxMjgzNDA3MDc0NiwgLTczLjk5MDYwMjkyMDEwNDJdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2E3ODY3NWUyZjU4ZDRjNWI5N2IwYTVmOTNmYzNjZjA5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8xMDEzYTZjNTY3YTE0YmRmYmE0ZTcyOTk3ODlmODk5MCA9ICQoYDxkaXYgaWQ9Imh0bWxfMTAxM2E2YzU2N2ExNGJkZmJhNGU3Mjk5Nzg5Zjg5OTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBpei16ZXR0YSBQcmljZTogJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9hNzg2NzVlMmY1OGQ0YzViOTdiMGE1ZjkzZmMzY2YwOS5zZXRDb250ZW50KGh0bWxfMTAxM2E2YzU2N2ExNGJkZmJhNGU3Mjk5Nzg5Zjg5OTApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfOWYwMWRkNmJkMTY4NGIwZWI2Y2ExZmU1ZmRhNDFjZDAuYmluZFBvcHVwKHBvcHVwX2E3ODY3NWUyZjU4ZDRjNWI5N2IwYTVmOTNmYzNjZjA5KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzlmNzY1YjcxMzkzNzQ1ZTg5YmIwZDM2NDZhMTg1ODVlID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjg5NywgLTczLjk2NTM2OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZjVlY2U1MTRiZTlmNDdmZDhkMmI2MWRhODkxNGY5NGIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2ZkNDdiMThkMWZmZjQyZmE4MGI4NWQxMzFkYTBkOTc5ID0gJChgPGRpdiBpZD0iaHRtbF9mZDQ3YjE4ZDFmZmY0MmZhODBiODVkMTMxZGEwZDk3OSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+THVpZ2kncyBQaXp6ZXJpYSBQcmljZTogJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9mNWVjZTUxNGJlOWY0N2ZkOGQyYjYxZGE4OTE0Zjk0Yi5zZXRDb250ZW50KGh0bWxfZmQ0N2IxOGQxZmZmNDJmYTgwYjg1ZDEzMWRhMGQ5NzkpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfOWY3NjViNzEzOTM3NDVlODliYjBkMzY0NmExODU4NWUuYmluZFBvcHVwKHBvcHVwX2Y1ZWNlNTE0YmU5ZjQ3ZmQ4ZDJiNjFkYTg5MTRmOTRiKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2YwOGM3MjgxNDQ0NTQ2NTg5YTIwZTY5MWRkYzliOTAzID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzMyMDYyNzAxNjUxMiwgLTc0LjAwMzY1NTIyNzAxMzddLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2EzOGQ5OWQ5YzcxNDQ3YTNhY2EwNGY1NTcyMmRlZTkwID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF83NDY4NjJiN2VhMDc0NzExODg0ZGVhZWNjODZlM2E1NCA9ICQoYDxkaXYgaWQ9Imh0bWxfNzQ2ODYyYjdlYTA3NDcxMTg4NGRlYWVjYzg2ZTNhNTQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJsZWVja2VyIFN0cmVldCBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9hMzhkOTlkOWM3MTQ0N2EzYWNhMDRmNTU3MjJkZWU5MC5zZXRDb250ZW50KGh0bWxfNzQ2ODYyYjdlYTA3NDcxMTg4NGRlYWVjYzg2ZTNhNTQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZjA4YzcyODE0NDQ1NDY1ODlhMjBlNjkxZGRjOWI5MDMuYmluZFBvcHVwKHBvcHVwX2EzOGQ5OWQ5YzcxNDQ3YTNhY2EwNGY1NTcyMmRlZTkwKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzc5YTQ0OGM3NDBkYzQ4ZWZhNWRlZTIzMmQwMDlhMjVhID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzExNjIsIC03My45NTc4M10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZTJhNzBlMDhkMWM3NDZiNGJjMDMxNTEzZjQxNWI5ODQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzZiYzdmMGJmZGFmYjQxMmRiZTM4Y2NiN2QxZWMyNzMwID0gJChgPGRpdiBpZD0iaHRtbF82YmM3ZjBiZmRhZmI0MTJkYmUzOGNjYjdkMWVjMjczMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TCdpbmR1c3RyaWUgUGl6emVyaWEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZTJhNzBlMDhkMWM3NDZiNGJjMDMxNTEzZjQxNWI5ODQuc2V0Q29udGVudChodG1sXzZiYzdmMGJmZGFmYjQxMmRiZTM4Y2NiN2QxZWMyNzMwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzc5YTQ0OGM3NDBkYzQ4ZWZhNWRlZTIzMmQwMDlhMjVhLmJpbmRQb3B1cChwb3B1cF9lMmE3MGUwOGQxYzc0NmI0YmMwMzE1MTNmNDE1Yjk4NCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl85ZTI3ZDJmNDRlNTc0NGIzYTIzMDNmODcyMzIwZWVlMSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjczMTU4LCAtNzQuMDAzMzJdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2QyMzQ3YmNmNmJkYTQzYWNhMmU2YmJhMzQ0NDBkZjM3ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9kMTYxMTlmYThjNGI0NmUxOWVjZTM0ZWIyMjU1NmM3NyA9ICQoYDxkaXYgaWQ9Imh0bWxfZDE2MTE5ZmE4YzRiNDZlMTllY2UzNGViMjI1NTZjNzciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkpvaG4ncyBvZiBCbGVlY2tlciBTdHJlZXQgUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2QyMzQ3YmNmNmJkYTQzYWNhMmU2YmJhMzQ0NDBkZjM3LnNldENvbnRlbnQoaHRtbF9kMTYxMTlmYThjNGI0NmUxOWVjZTM0ZWIyMjU1NmM3Nyk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl85ZTI3ZDJmNDRlNTc0NGIzYTIzMDNmODcyMzIwZWVlMS5iaW5kUG9wdXAocG9wdXBfZDIzNDdiY2Y2YmRhNDNhY2EyZTZiYmEzNDQ0MGRmMzcpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfM2RhNWQ0N2RhMzk5NGM2MzhlMzE3ZDFmZmJlMTUxZmUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MzgyOSwgLTc0LjAwNTRdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzk0NGM5YmE5MzZlOTQyZTM4YTRmMGZjNmQ0MzNlYWQ2ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF80ZmY0YmNiMzBkZTk0NDMzOGQ4ZGNmMDMyNmNhMzY0NCA9ICQoYDxkaXYgaWQ9Imh0bWxfNGZmNGJjYjMwZGU5NDQzMzhkOGRjZjAzMjZjYTM2NDQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJydW5ldHRpIFBpenphIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF85NDRjOWJhOTM2ZTk0MmUzOGE0ZjBmYzZkNDMzZWFkNi5zZXRDb250ZW50KGh0bWxfNGZmNGJjYjMwZGU5NDQzMzhkOGRjZjAzMjZjYTM2NDQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfM2RhNWQ0N2RhMzk5NGM2MzhlMzE3ZDFmZmJlMTUxZmUuYmluZFBvcHVwKHBvcHVwXzk0NGM5YmE5MzZlOTQyZTM4YTRmMGZjNmQ0MzNlYWQ2KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2Q4MjI4MjdjNDQ4MDRiZDFhMWI4ZjZiZTU3ZmE4OTEzID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjg4Mjg1LCAtNzMuOTg5MDA2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yYmZmZjE1YWM0NTA0ZjE2YTI3ZTExNzE3MTBhODU1OSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfODUwYjI5NTMwYTY1NDM4N2IwMjk4N2U0ZmM5YWE2YTkgPSAkKGA8ZGl2IGlkPSJodG1sXzg1MGIyOTUzMGE2NTQzODdiMDI5ODdlNGZjOWFhNmE5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Tb3R0b2Nhc2EgUGl6emVyaWEgUHJpY2U6ICQkIFJhdGluZzogNC41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzJiZmZmMTVhYzQ1MDRmMTZhMjdlMTE3MTcxMGE4NTU5LnNldENvbnRlbnQoaHRtbF84NTBiMjk1MzBhNjU0Mzg3YjAyOTg3ZTRmYzlhYTZhOSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9kODIyODI3YzQ0ODA0YmQxYTFiOGY2YmU1N2ZhODkxMy5iaW5kUG9wdXAocG9wdXBfMmJmZmYxNWFjNDUwNGYxNmEyN2UxMTcxNzEwYTg1NTkpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNmM5OWUyZDBhNmYyNGMyYmJiY2E0YWY2MDA3NTk0ZWUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43Mjc0OCwgLTczLjk4MjY4XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9jMDMyNTI5ZWUyYjU0OGI0YjdlM2VkNDJkNmE0OGU5MyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMDcwMDIzNzE2MDc1NGIzNGI2YjAyNWVmODk0ZjQ1ZTIgPSAkKGA8ZGl2IGlkPSJodG1sXzA3MDAyMzcxNjA3NTRiMzRiNmIwMjVlZjg5NGY0NWUyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5WaWxsYWdlIFNxdWFyZSBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jMDMyNTI5ZWUyYjU0OGI0YjdlM2VkNDJkNmE0OGU5My5zZXRDb250ZW50KGh0bWxfMDcwMDIzNzE2MDc1NGIzNGI2YjAyNWVmODk0ZjQ1ZTIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNmM5OWUyZDBhNmYyNGMyYmJiY2E0YWY2MDA3NTk0ZWUuYmluZFBvcHVwKHBvcHVwX2MwMzI1MjllZTJiNTQ4YjRiN2UzZWQ0MmQ2YTQ4ZTkzKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzU5NTkxMmI2NmQ5YjQzZDBiMGFhYWU0NWE1ODkwNGU2ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzIxNTM5MjQyMDc4NywgLTczLjk4NzYyNzU0MzgyODldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzNhZGFkMGE0Y2YzNjQ5MzViMzgwY2JjYjViNWViNjE4ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF84MGM0OWY5MDVlMzk0YzI4ODY3MmI0ZTA3ODdhMzA0ZSA9ICQoYDxkaXYgaWQ9Imh0bWxfODBjNDlmOTA1ZTM5NGMyODg2NzJiNGUwNzg3YTMwNGUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxhIE1hcmdhcml0YSBQaXp6ZXJpYSBQcmljZTogJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zYWRhZDBhNGNmMzY0OTM1YjM4MGNiY2I1YjVlYjYxOC5zZXRDb250ZW50KGh0bWxfODBjNDlmOTA1ZTM5NGMyODg2NzJiNGUwNzg3YTMwNGUpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNTk1OTEyYjY2ZDliNDNkMGIwYWFhZTQ1YTU4OTA0ZTYuYmluZFBvcHVwKHBvcHVwXzNhZGFkMGE0Y2YzNjQ5MzViMzgwY2JjYjViNWViNjE4KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzE1MGY0M2Q3NTgzZTRiYzU5MTk3ZGMyNjM1ZWNhMTE1ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuODAwODQsIC03My45Njc2Nl0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMjYxN2YzOGVmNTYxNDMxYWE1NWUyOWNkNDhhY2NiMWIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2IzZDM1N2RlYjNkZjQ2NjQ4M2MyMDZjNWY5OTg3NWQxID0gJChgPGRpdiBpZD0iaHRtbF9iM2QzNTdkZWIzZGY0NjY0ODNjMjA2YzVmOTk4NzVkMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TUFNQSdTIFRPTyEgUHJpY2U6ICQkIFJhdGluZzogMy41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzI2MTdmMzhlZjU2MTQzMWFhNTVlMjljZDQ4YWNjYjFiLnNldENvbnRlbnQoaHRtbF9iM2QzNTdkZWIzZGY0NjY0ODNjMjA2YzVmOTk4NzVkMSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8xNTBmNDNkNzU4M2U0YmM1OTE5N2RjMjYzNWVjYTExNS5iaW5kUG9wdXAocG9wdXBfMjYxN2YzOGVmNTYxNDMxYWE1NWUyOWNkNDhhY2NiMWIpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMGRmMzVhZWM4NThjNGJhYWIxNTZhMmNlZGQ2NTQ1NmEgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC42ODE4Njk1LCAtNzMuOTc2MTgxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9jYjhjYTYwZGI0ZDk0NGJhYmIxZDBlYjJlM2E5OTIzZiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNDRmYzAzMDQ4YzRmNDZhODg2M2QwODAxYzA4NzRhMTUgPSAkKGA8ZGl2IGlkPSJodG1sXzQ0ZmMwMzA0OGM0ZjQ2YTg4NjNkMDgwMWMwODc0YTE1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QYXRzeSdzIFBpenplcmlhIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jYjhjYTYwZGI0ZDk0NGJhYmIxZDBlYjJlM2E5OTIzZi5zZXRDb250ZW50KGh0bWxfNDRmYzAzMDQ4YzRmNDZhODg2M2QwODAxYzA4NzRhMTUpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMGRmMzVhZWM4NThjNGJhYWIxNTZhMmNlZGQ2NTQ1NmEuYmluZFBvcHVwKHBvcHVwX2NiOGNhNjBkYjRkOTQ0YmFiYjFkMGViMmUzYTk5MjNmKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzQ3YTA1ZmRmNzExZTQ3ZDQ4NGEzNGJlNzNlY2I2MGM4ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjkxMjE5MzI5ODM0LCAtNzMuOTk3MzQ0OTcwNzAzMV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMzM5NDg4OTUyOWQwNDM1ZjkzMTBmMjAzMDNjYzBjOTkgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2Y0ZTc5M2QyYjRkMDRjYjFiN2YzNzZkNDE4OTdhMzk2ID0gJChgPGRpdiBpZD0iaHRtbF9mNGU3OTNkMmI0ZDA0Y2IxYjdmMzc2ZDQxODk3YTM5NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+VGFibGUgODcgLSBCcm9va2x5biBIZWlnaHRzIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zMzk0ODg5NTI5ZDA0MzVmOTMxMGYyMDMwM2NjMGM5OS5zZXRDb250ZW50KGh0bWxfZjRlNzkzZDJiNGQwNGNiMWI3ZjM3NmQ0MTg5N2EzOTYpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNDdhMDVmZGY3MTFlNDdkNDg0YTM0YmU3M2VjYjYwYzguYmluZFBvcHVwKHBvcHVwXzMzOTQ4ODk1MjlkMDQzNWY5MzEwZjIwMzAzY2MwYzk5KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzkxNGYwM2JhMTFhNjRiYjBhODQzZTc0NDAyOTNjYjVjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzMzMzEsIC03My45ODc2M10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZmVmOTNmZWQyNTM2NDE0OGE3NzM1MDM1OWY3ZmM1YWIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzgwM2ExODA1YTAzZDRjZGViYTQ4ODNlMjlkNDRkYjk3ID0gJChgPGRpdiBpZD0iaHRtbF84MDNhMTgwNWEwM2Q0Y2RlYmE0ODgzZTI5ZDQ0ZGI5NyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Sm9lJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZmVmOTNmZWQyNTM2NDE0OGE3NzM1MDM1OWY3ZmM1YWIuc2V0Q29udGVudChodG1sXzgwM2ExODA1YTAzZDRjZGViYTQ4ODNlMjlkNDRkYjk3KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzkxNGYwM2JhMTFhNjRiYjBhODQzZTc0NDAyOTNjYjVjLmJpbmRQb3B1cChwb3B1cF9mZWY5M2ZlZDI1MzY0MTQ4YTc3MzUwMzU5ZjdmYzVhYikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl85MjM3MDViOTUwNzI0MzQzOWExNGZhNjUwZWRjN2I1ZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc1NDcsIC03My45ODY5Nl0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZTQ3NTJmNzQ0MmMwNGFjY2JhOTYwZWM3NTMyMTk4MjAgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzY5YTcxNmQwZDYyYTQ3ODc5NTFlZjVmYjU4NGJiNjUwID0gJChgPGRpdiBpZD0iaHRtbF82OWE3MTZkMGQ2MmE0Nzg3OTUxZWY1ZmI1ODRiYjY1MCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Sm9lJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZTQ3NTJmNzQ0MmMwNGFjY2JhOTYwZWM3NTMyMTk4MjAuc2V0Q29udGVudChodG1sXzY5YTcxNmQwZDYyYTQ3ODc5NTFlZjVmYjU4NGJiNjUwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzkyMzcwNWI5NTA3MjQzNDM5YTE0ZmE2NTBlZGM3YjVlLmJpbmRQb3B1cChwb3B1cF9lNDc1MmY3NDQyYzA0YWNjYmE5NjBlYzc1MzIxOTgyMCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl80NzYxMGE2OGZhZGM0NzA3ODE4YzcxZWM1OGU4ZGEwMiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxMDEyOTc3MjkwODMsIC03NC4wMDc3MjA2ODY0OTUzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8xMWJhM2YxNGMyNjI0ZWM1OWY3NWU5YTMxMmJlOWZhNiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfODAwYjM3MGQ1N2M1NDJjNjljYWQwNzA5MGFjZjc5MDAgPSAkKGA8ZGl2IGlkPSJodG1sXzgwMGIzNzBkNTdjNTQyYzY5Y2FkMDcwOTBhY2Y3OTAwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Kb2UncyBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8xMWJhM2YxNGMyNjI0ZWM1OWY3NWU5YTMxMmJlOWZhNi5zZXRDb250ZW50KGh0bWxfODAwYjM3MGQ1N2M1NDJjNjljYWQwNzA5MGFjZjc5MDApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNDc2MTBhNjhmYWRjNDcwNzgxOGM3MWVjNThlOGRhMDIuYmluZFBvcHVwKHBvcHVwXzExYmEzZjE0YzI2MjRlYzU5Zjc1ZTlhMzEyYmU5ZmE2KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2ZlMGVlOGMxYWNmMTQ4ZjhiNGUzOWZkZjMzOTkxYzYwID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzAzMTM3LCAtNzMuOTkzNjA4XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF84NGVmMDI4ZmI4YWY0ODZlYmY3NzdjMDcxNTQ4MTM3ZCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMWE2OTNlNWIwZGJlNDRhYWFjY2U5NzYxNmMwNGNjMjEgPSAkKGA8ZGl2IGlkPSJodG1sXzFhNjkzZTViMGRiZTQ0YWFhY2NlOTc2MTZjMDRjYzIxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5JZ25hemlvJ3MgUHJpY2U6ICQkIFJhdGluZzogMy41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzg0ZWYwMjhmYjhhZjQ4NmViZjc3N2MwNzE1NDgxMzdkLnNldENvbnRlbnQoaHRtbF8xYTY5M2U1YjBkYmU0NGFhYWNjZTk3NjE2YzA0Y2MyMSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9mZTBlZThjMWFjZjE0OGY4YjRlMzlmZGYzMzk5MWM2MC5iaW5kUG9wdXAocG9wdXBfODRlZjAyOGZiOGFmNDg2ZWJmNzc3YzA3MTU0ODEzN2QpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZjdjOGNkMjNiZTUxNGM2Yjg3M2YyMTc4MjAwY2JmMDcgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MjgzNiwgLTczLjk1NzJdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzQ0ZTg2Nzg2OTJhMzQ1M2U5Yjk2MWRjZTM0ODQ0ZjFhID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF81ODFlYTg4ZTg2M2E0YzU4OWVkZTM5MTIzYjE5Nzc1OCA9ICQoYDxkaXYgaWQ9Imh0bWxfNTgxZWE4OGU4NjNhNGM1ODllZGUzOTEyM2IxOTc3NTgiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBhdWxpZSBHZWUncyBTbGljZSBTaG9wIFByaWNlOiAkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzQ0ZTg2Nzg2OTJhMzQ1M2U5Yjk2MWRjZTM0ODQ0ZjFhLnNldENvbnRlbnQoaHRtbF81ODFlYTg4ZTg2M2E0YzU4OWVkZTM5MTIzYjE5Nzc1OCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9mN2M4Y2QyM2JlNTE0YzZiODczZjIxNzgyMDBjYmYwNy5iaW5kUG9wdXAocG9wdXBfNDRlODY3ODY5MmEzNDUzZTliOTYxZGNlMzQ4NDRmMWEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZjljNTI2NmNmNmU5NDFjZDhmNzI5N2ZkNmFhMThkNmMgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC42MjUwOTMsIC03My45NjE1MzFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzY2NjcyMjMxZTA2MzQ5OTg5NGU3NDU0NGM4OGEwMjViID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF83NWQyNDM1ZTdjM2I0YWU4YjZiZWExZGJhOGQ5OTUzMiA9ICQoYDxkaXYgaWQ9Imh0bWxfNzVkMjQzNWU3YzNiNGFlOGI2YmVhMWRiYThkOTk1MzIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRpIEZhcmEgUGl6emEgUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzY2NjcyMjMxZTA2MzQ5OTg5NGU3NDU0NGM4OGEwMjViLnNldENvbnRlbnQoaHRtbF83NWQyNDM1ZTdjM2I0YWU4YjZiZWExZGJhOGQ5OTUzMik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9mOWM1MjY2Y2Y2ZTk0MWNkOGY3Mjk3ZmQ2YWExOGQ2Yy5iaW5kUG9wdXAocG9wdXBfNjY2NzIyMzFlMDYzNDk5ODk0ZTc0NTQ0Yzg4YTAyNWIpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNDllMGJlZWQzMjUxNDJhZWI2YWNkNmYwMzVkMzE0ZmMgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC42OTc0NDI3LCAtNzMuOTY3NTE5N10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNjZlOTEzNDVlYzg5NDJhZjlhMWEwNzg1ZjAzM2RlNzMgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzNkOGM3NTcwNDQwYzRmY2M5OWQ0MGExZmIzMzdlZDhiID0gJChgPGRpdiBpZD0iaHRtbF8zZDhjNzU3MDQ0MGM0ZmNjOTlkNDBhMWZiMzM3ZWQ4YiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+SWwgUG9ydG8gUmlzdG9yYW50ZSBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNjZlOTEzNDVlYzg5NDJhZjlhMWEwNzg1ZjAzM2RlNzMuc2V0Q29udGVudChodG1sXzNkOGM3NTcwNDQwYzRmY2M5OWQ0MGExZmIzMzdlZDhiKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzQ5ZTBiZWVkMzI1MTQyYWViNmFjZDZmMDM1ZDMxNGZjLmJpbmRQb3B1cChwb3B1cF82NmU5MTM0NWVjODk0MmFmOWExYTA3ODVmMDMzZGU3MykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8xNDI5MzI1MGJhOWM0MDFkODY1ZTlkYTZjOGNhODk0ZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcyNDc0LCAtNzMuOTgxNjFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzgyMTkwZTFjNDU5MjQ3N2Q4ZDQ1Yzc0N2JiMTJiMGU5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF81OGFjMTJmYjUyMzI0Y2Q0OGM5MjFlNWYwM2YwMDdkNCA9ICQoYDxkaXYgaWQ9Imh0bWxfNThhYzEyZmI1MjMyNGNkNDhjOTIxZTVmMDNmMDA3ZDQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkdydXBwbyBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfODIxOTBlMWM0NTkyNDc3ZDhkNDVjNzQ3YmIxMmIwZTkuc2V0Q29udGVudChodG1sXzU4YWMxMmZiNTIzMjRjZDQ4YzkyMWU1ZjAzZjAwN2Q0KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzE0MjkzMjUwYmE5YzQwMWQ4NjVlOWRhNmM4Y2E4OTRlLmJpbmRQb3B1cChwb3B1cF84MjE5MGUxYzQ1OTI0NzdkOGQ0NWM3NDdiYjEyYjBlOSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl84YzQ2ZmM4MmZkNzc0ZDA4YTM1ZWU4NTRmMTI2ZGY1ZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc4Mzg0MSwgLTczLjk3NzU2OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNjE5M2IzMGNmNzI2NDY0ZGIyZTViZDg1ZWFlYzI3YzQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzM1NTZlODAxYWRhYTQ5OWY4YTc3YWI3NDg2NmE1NWYxID0gJChgPGRpdiBpZD0iaHRtbF8zNTU2ZTgwMWFkYWE0OTlmOGE3N2FiNzQ4NjZhNTVmMSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TWFkZSBJbiBOZXcgWW9yayBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF82MTkzYjMwY2Y3MjY0NjRkYjJlNWJkODVlYWVjMjdjNC5zZXRDb250ZW50KGh0bWxfMzU1NmU4MDFhZGFhNDk5ZjhhNzdhYjc0ODY2YTU1ZjEpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfOGM0NmZjODJmZDc3NGQwOGEzNWVlODU0ZjEyNmRmNWUuYmluZFBvcHVwKHBvcHVwXzYxOTNiMzBjZjcyNjQ2NGRiMmU1YmQ4NWVhZWMyN2M0KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2E4YjFkZWJlNjg0NDQwM2Q4MWQyZWJhZDAyOTViYTJhID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzEyMTY4LCAtNzMuOTU1NzA3XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF81ZWI3YTUxMDFlZjk0YTRhYThhNmYyNTlkNGQ0MmY3ZiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZjBlMTkzZDg1OTdhNDdjOWJmZjU1MWU0NGQyZmYyNjggPSAkKGA8ZGl2IGlkPSJodG1sX2YwZTE5M2Q4NTk3YTQ3YzliZmY1NTFlNDRkMmZmMjY4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5FbW15IFNxdWFyZWQgLSBCcm9va2x5biBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNWViN2E1MTAxZWY5NGE0YWE4YTZmMjU5ZDRkNDJmN2Yuc2V0Q29udGVudChodG1sX2YwZTE5M2Q4NTk3YTQ3YzliZmY1NTFlNDRkMmZmMjY4KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2E4YjFkZWJlNjg0NDQwM2Q4MWQyZWJhZDAyOTViYTJhLmJpbmRQb3B1cChwb3B1cF81ZWI3YTUxMDFlZjk0YTRhYThhNmYyNTlkNGQ0MmY3ZikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9mOTBjZjI2MTliYjc0OTFhYjVhZGQwYjMwODc5MGNmMiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxMzI0NTY3NTMxNSwgLTczLjk5ODY1MTEwNDQ5Nl0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMGJhNTJiYjAyZmVlNDk0MThmYjIxOTMxNTA5ZGUzMGQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzA2NDIzYzJhOTM0YTQ2OWQ4NDMzZTA5NjljNGFjYjU3ID0gJChgPGRpdiBpZD0iaHRtbF8wNjQyM2MyYTkzNGE0NjlkODQzM2UwOTY5YzRhY2I1NyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+THVuYSBQaXp6YSBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMGJhNTJiYjAyZmVlNDk0MThmYjIxOTMxNTA5ZGUzMGQuc2V0Q29udGVudChodG1sXzA2NDIzYzJhOTM0YTQ2OWQ4NDMzZTA5NjljNGFjYjU3KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2Y5MGNmMjYxOWJiNzQ5MWFiNWFkZDBiMzA4NzkwY2YyLmJpbmRQb3B1cChwb3B1cF8wYmE1MmJiMDJmZWU0OTQxOGZiMjE5MzE1MDlkZTMwZCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9iZTU1M2NiMmRmN2I0YzM0OGI0MjNkYWZhZDUzYzZiZCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcwNDMzNjIsIC03NC4wMTAxNzMzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF83ZjkwYWY2ZjQxMzQ0M2E1ODIyZDYzZWYzODIyMWY5MSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfODUzMGIzZDA3Mzc2NGU1N2FjMjY2NjQ1YjhjNTU4NTAgPSAkKGA8ZGl2IGlkPSJodG1sXzg1MzBiM2QwNzM3NjRlNTdhYzI2NjY0NWI4YzU1ODUwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5BZHJpZW5uZSdzIFBpenphYmFyIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF83ZjkwYWY2ZjQxMzQ0M2E1ODIyZDYzZWYzODIyMWY5MS5zZXRDb250ZW50KGh0bWxfODUzMGIzZDA3Mzc2NGU1N2FjMjY2NjQ1YjhjNTU4NTApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYmU1NTNjYjJkZjdiNGMzNDhiNDIzZGFmYWQ1M2M2YmQuYmluZFBvcHVwKHBvcHVwXzdmOTBhZjZmNDEzNDQzYTU4MjJkNjNlZjM4MjIxZjkxKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzVjOTczM2E5MjlmNzQ0MjVhY2RkODY4Zjc2YTM2MGVhID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzI1OTgsIC03My45ODY3Ml0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfN2U2ODYwODRhZjFjNDFhNGFkZGZiYzA2ZjJhMWViYWIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzA1YjFjZjQ2YTA5MjQyY2RiMGFmNDQ3OGRhMzY5NWEyID0gJChgPGRpdiBpZD0iaHRtbF8wNWIxY2Y0NmEwOTI0MmNkYjBhZjQ0NzhkYTM2OTVhMiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+RW1teSBTcXVhcmVkIC0gRWFzdCBWaWxsYWdlIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF83ZTY4NjA4NGFmMWM0MWE0YWRkZmJjMDZmMmExZWJhYi5zZXRDb250ZW50KGh0bWxfMDViMWNmNDZhMDkyNDJjZGIwYWY0NDc4ZGEzNjk1YTIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNWM5NzMzYTkyOWY3NDQyNWFjZGQ4NjhmNzZhMzYwZWEuYmluZFBvcHVwKHBvcHVwXzdlNjg2MDg0YWYxYzQxYTRhZGRmYmMwNmYyYTFlYmFiKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzA4MDAzYzFhN2QzZjQwMjI4YTNiMmNmNjFlNDc4YjQ3ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzE3OTUzLCAtNzMuOTYzNzAyN10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfY2NmY2FlMDVmYjljNDYzNjlmZmUxZjRiODI5YjQ3MTcgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2VmYzE4NDc4YmFjMDQzY2U5ZDEzYzEyN2E4YmExMWU4ID0gJChgPGRpdiBpZD0iaHRtbF9lZmMxODQ3OGJhYzA0M2NlOWQxM2MxMjdhOGJhMTFlOCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TGEgTm9ubmEgS3Jpc3B5IEtydXN0IFBpenphIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jY2ZjYWUwNWZiOWM0NjM2OWZmZTFmNGI4MjliNDcxNy5zZXRDb250ZW50KGh0bWxfZWZjMTg0NzhiYWMwNDNjZTlkMTNjMTI3YThiYTExZTgpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMDgwMDNjMWE3ZDNmNDAyMjhhM2IyY2Y2MWU0NzhiNDcuYmluZFBvcHVwKHBvcHVwX2NjZmNhZTA1ZmI5YzQ2MzY5ZmZlMWY0YjgyOWI0NzE3KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2E5ODExYThiN2YzNjQzOGRhZDI2YTdiNzI2ZWY5MmQ5ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNTk0NzE1LCAtNzMuOTgxMzE2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9mMDNiODg1ZjNkZTY0ZTNiYmQ0NTdjMzdlZDYyM2E1MSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNWZhMGI5YTQ3MzMzNDJhOWFlMDkyMmViMmQzMjQxMTcgPSAkKGA8ZGl2IGlkPSJodG1sXzVmYTBiOWE0NzMzMzQyYTlhZTA5MjJlYjJkMzI0MTE3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MICYgQiBTcHVtb25pIEdhcmRlbnMgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZjAzYjg4NWYzZGU2NGUzYmJkNDU3YzM3ZWQ2MjNhNTEuc2V0Q29udGVudChodG1sXzVmYTBiOWE0NzMzMzQyYTlhZTA5MjJlYjJkMzI0MTE3KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2E5ODExYThiN2YzNjQzOGRhZDI2YTdiNzI2ZWY5MmQ5LmJpbmRQb3B1cChwb3B1cF9mMDNiODg1ZjNkZTY0ZTNiYmQ0NTdjMzdlZDYyM2E1MSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9hZGZhNGE4YWIwMzA0N2IwYjcwMjc2ZjBmZDgzN2E5MCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcwOTMzLCAtNzMuOTU0MDddLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzY3NmZiZmU2NTZkNDQ2MGM5NzRlMDc0ODYzZDJjY2E5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF81YTNjOGM4MzI3Nzk0M2NhYWRhMjlmMmYxMmE5MzA2ZiA9ICQoYDxkaXYgaWQ9Imh0bWxfNWEzYzhjODMyNzc5NDNjYWFkYTI5ZjJmMTJhOTMwNmYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkRvdWdoIFZhbGUgUHJpY2U6ICQkIFJhdGluZzogNS4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzY3NmZiZmU2NTZkNDQ2MGM5NzRlMDc0ODYzZDJjY2E5LnNldENvbnRlbnQoaHRtbF81YTNjOGM4MzI3Nzk0M2NhYWRhMjlmMmYxMmE5MzA2Zik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9hZGZhNGE4YWIwMzA0N2IwYjcwMjc2ZjBmZDgzN2E5MC5iaW5kUG9wdXAocG9wdXBfNjc2ZmJmZTY1NmQ0NDYwYzk3NGUwNzQ4NjNkMmNjYTkpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNDQ3YjVhYTY3ODQ2NGQ2MWI4NjZhZGY4YmJhZGE5N2EgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MTY5MSwgLTczLjk1ODk4N10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMDNlYzY3NDAxZjAxNGQ5M2I3YmQ3MTc0MjM5OTBmY2MgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzI2N2RlMjNhNWNhMDQwMzk4NzkxNGMxNWEwZmNhYmIzID0gJChgPGRpdiBpZD0iaHRtbF8yNjdkZTIzYTVjYTA0MDM5ODc5MTRjMTVhMGZjYWJiMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Sm9lJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMDNlYzY3NDAxZjAxNGQ5M2I3YmQ3MTc0MjM5OTBmY2Muc2V0Q29udGVudChodG1sXzI2N2RlMjNhNWNhMDQwMzk4NzkxNGMxNWEwZmNhYmIzKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzQ0N2I1YWE2Nzg0NjRkNjFiODY2YWRmOGJiYWRhOTdhLmJpbmRQb3B1cChwb3B1cF8wM2VjNjc0MDFmMDE0ZDkzYjdiZDcxNzQyMzk5MGZjYykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9hYjVlNTRhMzUyMDY0Mjk0ODRkMWRkNGU4ZGFkYTM3MiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc4MjYsIC03My45NTM1MV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNzA2OTE4MWNhOGRkNDE1ZGI2NjE3OTI3MzRmODY0M2QgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzVmOTYyNmJlZTk1ODQ2ZDViNzFjOTY3MDRkOWM4MDY2ID0gJChgPGRpdiBpZD0iaHRtbF81Zjk2MjZiZWU5NTg0NmQ1YjcxYzk2NzA0ZDljODA2NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TWFyaW5hcmEgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNzA2OTE4MWNhOGRkNDE1ZGI2NjE3OTI3MzRmODY0M2Quc2V0Q29udGVudChodG1sXzVmOTYyNmJlZTk1ODQ2ZDViNzFjOTY3MDRkOWM4MDY2KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2FiNWU1NGEzNTIwNjQyOTQ4NGQxZGQ0ZThkYWRhMzcyLmJpbmRQb3B1cChwb3B1cF83MDY5MTgxY2E4ZGQ0MTVkYjY2MTc5MjczNGY4NjQzZCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9iNTNlZjQ1YmY4YmI0N2ZiYmFkN2Y5Y2VlY2Q4MmI4NCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjY5NDQ2NywgLTczLjk4MjkzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yY2YyNjJmYTJjZWI0NWIxOWM2YjA3OTA3YzZmOGMyOCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNDY1OTM4YzUzNGU2NDRhMmEyODExYTBjOWI5NjkwOWEgPSAkKGA8ZGl2IGlkPSJodG1sXzQ2NTkzOGM1MzRlNjQ0YTJhMjgxMWEwYzliOTY5MDlhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Gb3JubyBSb3NzbyBQcmljZTogJCQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMmNmMjYyZmEyY2ViNDViMTljNmIwNzkwN2M2ZjhjMjguc2V0Q29udGVudChodG1sXzQ2NTkzOGM1MzRlNjQ0YTJhMjgxMWEwYzliOTY5MDlhKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2I1M2VmNDViZjhiYjQ3ZmJiYWQ3ZjljZWVjZDgyYjg0LmJpbmRQb3B1cChwb3B1cF8yY2YyNjJmYTJjZWI0NWIxOWM2YjA3OTA3YzZmOGMyOCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8xMjQ3NTFmMzIxMGI0MTcxOWJjZjFiZWU1NWNjMjI1ZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc5NzE1NywgLTczLjkzNDg4XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9jMzE4YmY0ZmQwMGQ0MmM2YjYwZGQ1NTkzZjI5NTFlOCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMzA4MThlODU4MzkxNDhiMWE5YmQ2M2ExY2Q1NWUxMTAgPSAkKGA8ZGl2IGlkPSJodG1sXzMwODE4ZTg1ODM5MTQ4YjFhOWJkNjNhMWNkNTVlMTEwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QYXRzeSdzIFBpenplcmlhIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jMzE4YmY0ZmQwMGQ0MmM2YjYwZGQ1NTkzZjI5NTFlOC5zZXRDb250ZW50KGh0bWxfMzA4MThlODU4MzkxNDhiMWE5YmQ2M2ExY2Q1NWUxMTApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMTI0NzUxZjMyMTBiNDE3MTliY2YxYmVlNTVjYzIyNWUuYmluZFBvcHVwKHBvcHVwX2MzMThiZjRmZDAwZDQyYzZiNjBkZDU1OTNmMjk1MWU4KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzlhY2ExNTYyMjJiMTQxNDRiZGY5YjY5MTJmODRhMWEyID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzI4MTMsIC03NC4wMDE4OF0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYzc1ZDAzZTJkMzRhNGU5ZGIzODYxMDkxZjJiNjcyNzUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzVjYWRlNjA2YWZiYTQxOGE5ZGIxODU5MmEwMGRkNzJiID0gJChgPGRpdiBpZD0iaHRtbF81Y2FkZTYwNmFmYmE0MThhOWRiMTg1OTJhMDBkZDcyYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U29uZyBFIE5hcHVsZSBQcmljZTogJCQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYzc1ZDAzZTJkMzRhNGU5ZGIzODYxMDkxZjJiNjcyNzUuc2V0Q29udGVudChodG1sXzVjYWRlNjA2YWZiYTQxOGE5ZGIxODU5MmEwMGRkNzJiKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzlhY2ExNTYyMjJiMTQxNDRiZGY5YjY5MTJmODRhMWEyLmJpbmRQb3B1cChwb3B1cF9jNzVkMDNlMmQzNGE0ZTlkYjM4NjEwOTFmMmI2NzI3NSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl85ODU1NDNhZTNhMTI0NDI2YWUxNDQxODBmNGMxY2ZmYSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcyODk0OSwgLTczLjk1Mzk5OV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNzU0YjY0OTBmNjY4NDk2NzliNzJkYzBlNzM0ZWI0YWEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzVjOGIxNmEwYWU4NDQzOTE5MDAzNWU1ZTNhZTgxOGU1ID0gJChgPGRpdiBpZD0iaHRtbF81YzhiMTZhMGFlODQ0MzkxOTAwMzVlNWUzYWU4MThlNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Rm9ybmluby0gR3JlZW5wb2ludCBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNzU0YjY0OTBmNjY4NDk2NzliNzJkYzBlNzM0ZWI0YWEuc2V0Q29udGVudChodG1sXzVjOGIxNmEwYWU4NDQzOTE5MDAzNWU1ZTNhZTgxOGU1KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzk4NTU0M2FlM2ExMjQ0MjZhZTE0NDE4MGY0YzFjZmZhLmJpbmRQb3B1cChwb3B1cF83NTRiNjQ5MGY2Njg0OTY3OWI3MmRjMGU3MzRlYjRhYSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9mOTk0NjE3MmM0N2U0ZTBhYjNiNTY2OGM2YzA3NDAxMyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxODksIC03My45NTY4NDNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzJhODliMWE1YTA4MTQzODk4MWQwMWIwMWM0N2QzNTkxID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF81MTc1YjhkZGI1ODg0M2Q5ODEyZTQzMTU5N2QzY2Q4MCA9ICQoYDxkaXYgaWQ9Imh0bWxfNTE3NWI4ZGRiNTg4NDNkOTgxMmU0MzE1OTdkM2NkODAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlZpbm5pZSdzIFBpenplcmlhIFByaWNlOiAkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzJhODliMWE1YTA4MTQzODk4MWQwMWIwMWM0N2QzNTkxLnNldENvbnRlbnQoaHRtbF81MTc1YjhkZGI1ODg0M2Q5ODEyZTQzMTU5N2QzY2Q4MCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9mOTk0NjE3MmM0N2U0ZTBhYjNiNTY2OGM2YzA3NDAxMy5iaW5kUG9wdXAocG9wdXBfMmE4OWIxYTVhMDgxNDM4OTgxZDAxYjAxYzQ3ZDM1OTEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfODE5NmQyMTlmYmQwNDc2YTk0MGFhZjgxZGExNjM1NzEgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MjA4ODgzLCAtNzMuOTk2MjQwNF0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYjJkNWY5NmQ2OGM3NGFhMmI1M2RiOTViZDNmZDYzNzggPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzUwYmZiNjA3ZGJiNDQ5Njc4Y2FjNGY2Yzc3YjY2YThlID0gJChgPGRpdiBpZD0iaHRtbF81MGJmYjYwN2RiYjQ0OTY3OGNhYzRmNmM3N2I2NmE4ZSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tm9saXRhIFBpenphIFByaWNlOiAkIFJhdGluZzogNC41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2IyZDVmOTZkNjhjNzRhYTJiNTNkYjk1YmQzZmQ2Mzc4LnNldENvbnRlbnQoaHRtbF81MGJmYjYwN2RiYjQ0OTY3OGNhYzRmNmM3N2I2NmE4ZSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl84MTk2ZDIxOWZiZDA0NzZhOTQwYWFmODFkYTE2MzU3MS5iaW5kUG9wdXAocG9wdXBfYjJkNWY5NmQ2OGM3NGFhMmI1M2RiOTViZDNmZDYzNzgpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNDA0YzRiOThlNzU5NDNhMGFiMjQ1MjVlNmE3NDhhYWUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MzM0MzQ2LCAtNzMuOTkxNjkyM10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMjhkMDIxOTQ0ZmIxNGZlNGJkNjRkOTc2YzlkMDA1ODIgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzYxYmY1MjFiNjEzZjRkOGI4MzU5N2YxOWVjZjI4NGY5ID0gJChgPGRpdiBpZD0iaHRtbF82MWJmNTIxYjYxM2Y0ZDhiODM1OTdmMTllY2YyODRmOSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+UmliYWx0YSBQaXp6YSBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMjhkMDIxOTQ0ZmIxNGZlNGJkNjRkOTc2YzlkMDA1ODIuc2V0Q29udGVudChodG1sXzYxYmY1MjFiNjEzZjRkOGI4MzU5N2YxOWVjZjI4NGY5KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzQwNGM0Yjk4ZTc1OTQzYTBhYjI0NTI1ZTZhNzQ4YWFlLmJpbmRQb3B1cChwb3B1cF8yOGQwMjE5NDRmYjE0ZmU0YmQ2NGQ5NzZjOWQwMDU4MikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8xZjYzZWM3M2ZhZWE0NmI1ODQyNGNlZWE1OWEyOWQ3ZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc1MDIwNTk5MzY1MjMsIC03My45OTUzMjMxODExNTIzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF82MTY3MWM3MzM5ZGU0OGFhODI5OGI4OWUzMjcwMWVlYSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfODZiYzY4ZDU2YmVlNDk1ZmEzNjY4MmUwYzc5NjdhOGYgPSAkKGA8ZGl2IGlkPSJodG1sXzg2YmM2OGQ1NmJlZTQ5NWZhMzY2ODJlMGM3OTY3YThmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OWSBQaXp6YSBTdXByZW1hIFByaWNlOiAkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzYxNjcxYzczMzlkZTQ4YWE4Mjk4Yjg5ZTMyNzAxZWVhLnNldENvbnRlbnQoaHRtbF84NmJjNjhkNTZiZWU0OTVmYTM2NjgyZTBjNzk2N2E4Zik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8xZjYzZWM3M2ZhZWE0NmI1ODQyNGNlZWE1OWEyOWQ3ZS5iaW5kUG9wdXAocG9wdXBfNjE2NzFjNzMzOWRlNDhhYTgyOThiODllMzI3MDFlZWEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMWY3OGFkZjEwNDAxNDc5NmEyNTk1OTQ4MjliNDExZmQgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43Mjk0Mzk5LCAtNzMuOTgxMjVdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzgzYmYyZDBmNTNiMTQzNmI5NDRjMzY0ZjZhNjcxMjUzID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8wZDU4YzZiNmRmOTg0YTYyOTcxYmFlNmYyODBlMzFlMiA9ICQoYDxkaXYgaWQ9Imh0bWxfMGQ1OGM2YjZkZjk4NGE2Mjk3MWJhZTZmMjgwZTMxZTIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJha2VyJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfODNiZjJkMGY1M2IxNDM2Yjk0NGMzNjRmNmE2NzEyNTMuc2V0Q29udGVudChodG1sXzBkNThjNmI2ZGY5ODRhNjI5NzFiYWU2ZjI4MGUzMWUyKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzFmNzhhZGYxMDQwMTQ3OTZhMjU5NTk0ODI5YjQxMWZkLmJpbmRQb3B1cChwb3B1cF84M2JmMmQwZjUzYjE0MzZiOTQ0YzM2NGY2YTY3MTI1MykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl84MDk4MjJkZTAxZDU0ZmJmOGQ2ZWYwODEwN2U3Y2EzZiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxODIyLCAtNzMuOTkxMjNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2Y1MmNjNzNjNTg5YzQyODQ5OTc3MjM4MjY2YzQwMzY5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8zOWVlNzQxZTEyNzY0OTAxODA0ZDRiODdmNmM0YWE5YiA9ICQoYDxkaXYgaWQ9Imh0bWxfMzllZTc0MWUxMjc2NDkwMTgwNGQ0Yjg3ZjZjNGFhOWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPldpbGxpYW1zYnVyZyBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9mNTJjYzczYzU4OWM0Mjg0OTk3NzIzODI2NmM0MDM2OS5zZXRDb250ZW50KGh0bWxfMzllZTc0MWUxMjc2NDkwMTgwNGQ0Yjg3ZjZjNGFhOWIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfODA5ODIyZGUwMWQ1NGZiZjhkNmVmMDgxMDdlN2NhM2YuYmluZFBvcHVwKHBvcHVwX2Y1MmNjNzNjNTg5YzQyODQ5OTc3MjM4MjY2YzQwMzY5KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzQxYjM1ZTY3NWExNjRkYjlhNDQ4OWU1YWM3YTNkN2JjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzQ5MjQ2OCwgLTczLjk0MjAwNzFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzkxODc2MjFhOWI0YzQ3YjFiMzQ2NjJlYmUxMTJjZWQ1ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF82NzVkOWExNzIxYTU0NmMwYTk5ZGI0ODk4ZGVkMTk2MSA9ICQoYDxkaXYgaWQ9Imh0bWxfNjc1ZDlhMTcyMWE1NDZjMGE5OWRiNDg5OGRlZDE5NjEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkx1enpvJ3MgTmVhcG9saXRhbiBQaXp6YSAmIFdpbmUgQmFyIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF85MTg3NjIxYTliNGM0N2IxYjM0NjYyZWJlMTEyY2VkNS5zZXRDb250ZW50KGh0bWxfNjc1ZDlhMTcyMWE1NDZjMGE5OWRiNDg5OGRlZDE5NjEpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNDFiMzVlNjc1YTE2NGRiOWE0NDg5ZTVhYzdhM2Q3YmMuYmluZFBvcHVwKHBvcHVwXzkxODc2MjFhOWI0YzQ3YjFiMzQ2NjJlYmUxMTJjZWQ1KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzIwNGUyMjBkOTI1NTQ4ZjA5ZTM4NjJkYzE4NzYxMzI5ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzIxNTI3MDk5NjA5NCwgLTczLjk5NzA4NTU3MTI4OTFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzc3N2FiZDlhZGM2MjQ2MWJhNjA2NmI3MWNlYzJiZjkxID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF85ZDBmYWRhMDdkNGQ0ODdkOGE4NzE3NzlhNTk1MmQ4NCA9ICQoYDxkaXYgaWQ9Imh0bWxfOWQwZmFkYTA3ZDRkNDg3ZDhhODcxNzc5YTU5NTJkODQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNoYW1waW9uIFBpenphIFByaWNlOiAkIFJhdGluZzogNC41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzc3N2FiZDlhZGM2MjQ2MWJhNjA2NmI3MWNlYzJiZjkxLnNldENvbnRlbnQoaHRtbF85ZDBmYWRhMDdkNGQ0ODdkOGE4NzE3NzlhNTk1MmQ4NCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8yMDRlMjIwZDkyNTU0OGYwOWUzODYyZGMxODc2MTMyOS5iaW5kUG9wdXAocG9wdXBfNzc3YWJkOWFkYzYyNDYxYmE2MDY2YjcxY2VjMmJmOTEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfOTIzYWFkMGY2MTZhNDhlOTg5MDAxMTQ4N2U0ZjUyMDUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC42ODA3MDIsIC03My45Njc0NTVdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzQzZjNmM2NkZWI0MDRlM2JhZGQ5Yjg1YTAyOTJlMzU5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9kOWY3ZTRjMWU5YTI0M2I3YWVlYmEzZDhhOTNmYjI2NCA9ICQoYDxkaXYgaWQ9Imh0bWxfZDlmN2U0YzFlOWEyNDNiN2FlZWJhM2Q4YTkzZmIyNjQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkJyb29rbHluJ3MgSG9tZXNsaWNlIFBpenplcmlhIFByaWNlOiAkIFJhdGluZzogNC41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzQzZjNmM2NkZWI0MDRlM2JhZGQ5Yjg1YTAyOTJlMzU5LnNldENvbnRlbnQoaHRtbF9kOWY3ZTRjMWU5YTI0M2I3YWVlYmEzZDhhOTNmYjI2NCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl85MjNhYWQwZjYxNmE0OGU5ODkwMDExNDg3ZTRmNTIwNS5iaW5kUG9wdXAocG9wdXBfNDNmM2YzY2RlYjQwNGUzYmFkZDliODVhMDI5MmUzNTkpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfOGQxM2JlMTBkNzJhNGY2NThkNjk5ZmJkYzZhOTk5NzggPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MzQ4MywgLTczLjk4MjgyXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9jMDMxN2YxZmRlNGE0NDE1YTBkN2YzMjkwYzMyYzdkZSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZDFkY2I1ZTA4ZjgxNGExMGFiODBmZTE5NTRlYTNmNDAgPSAkKGA8ZGl2IGlkPSJodG1sX2QxZGNiNWUwOGY4MTRhMTBhYjgwZmUxOTU0ZWEzZjQwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Qb3N0byBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYzAzMTdmMWZkZTRhNDQxNWEwZDdmMzI5MGMzMmM3ZGUuc2V0Q29udGVudChodG1sX2QxZGNiNWUwOGY4MTRhMTBhYjgwZmUxOTU0ZWEzZjQwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzhkMTNiZTEwZDcyYTRmNjU4ZDY5OWZiZGM2YTk5OTc4LmJpbmRQb3B1cChwb3B1cF9jMDMxN2YxZmRlNGE0NDE1YTBkN2YzMjkwYzMyYzdkZSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl83MjM5ZDVmNTM1NmU0ODJjOTk5OWMyMGFiMTU3N2ZkOCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcwOTM5NDcsIC03NC4wMDQ4OTc3XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8wNDQyNGRkNzc2ZTE0ZDIwODcxYmMwYmRlYjgxMWJhMCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNGVmMmFhOGE5ZDIzNDFiYTk1NDJlMGNjYThlOTFmYTUgPSAkKGA8ZGl2IGlkPSJodG1sXzRlZjJhYThhOWQyMzQxYmE5NTQyZTBjY2E4ZTkxZmE1IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5LZXN0w6kgUGl6emEgJiBWaW5vIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8wNDQyNGRkNzc2ZTE0ZDIwODcxYmMwYmRlYjgxMWJhMC5zZXRDb250ZW50KGh0bWxfNGVmMmFhOGE5ZDIzNDFiYTk1NDJlMGNjYThlOTFmYTUpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNzIzOWQ1ZjUzNTZlNDgyYzk5OTljMjBhYjE1NzdmZDguYmluZFBvcHVwKHBvcHVwXzA0NDI0ZGQ3NzZlMTRkMjA4NzFiYzBiZGViODExYmEwKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2I2MzI3ZGFkY2ZlZTRkY2M4MDliN2NjYTVhZjAzOTMwID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzU5MTY3Njc2Nzg0OCwgLTczLjk1MjU0NjY4ODc5ODldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzk5ZTE2NGMzMDc1NTQ4ZTU4YzM3ZmIyNGY3YzU0NDliID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF81MjQzMDJhMjU1ZmM0ODNhYjE1Yjk3OTgyNGRmMGY5MCA9ICQoYDxkaXYgaWQ9Imh0bWxfNTI0MzAyYTI1NWZjNDgzYWIxNWI5Nzk4MjRkZjBmOTAiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlBpY2NvbG8gVHJhdHRvcmlhIFByaWNlOiAkIFJhdGluZzogMy4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzk5ZTE2NGMzMDc1NTQ4ZTU4YzM3ZmIyNGY3YzU0NDliLnNldENvbnRlbnQoaHRtbF81MjQzMDJhMjU1ZmM0ODNhYjE1Yjk3OTgyNGRmMGY5MCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9iNjMyN2RhZGNmZWU0ZGNjODA5YjdjY2E1YWYwMzkzMC5iaW5kUG9wdXAocG9wdXBfOTllMTY0YzMwNzU1NDhlNThjMzdmYjI0ZjdjNTQ0OWIpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfYzI1NGYzZGRjZDczNDk4OTk2M2MzYTNlYjdmNGJhMjkgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MDg0ODkwNzE0MjYzLCAtNzMuOTYxMDk0MDcxMTYzOV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZjdhMTlmYmJlM2ZjNGY1MjgxNWVkMTNjYmUwYjcyMzYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzA2YmU1MjA3Yjc5ZTQwNGJhYWYxODMzMjVmYmQzNzU5ID0gJChgPGRpdiBpZD0iaHRtbF8wNmJlNTIwN2I3OWU0MDRiYWFmMTgzMzI1ZmJkMzc1OSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Um9lYmxpbmcgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZjdhMTlmYmJlM2ZjNGY1MjgxNWVkMTNjYmUwYjcyMzYuc2V0Q29udGVudChodG1sXzA2YmU1MjA3Yjc5ZTQwNGJhYWYxODMzMjVmYmQzNzU5KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2MyNTRmM2RkY2Q3MzQ5ODk5NjNjM2EzZWI3ZjRiYTI5LmJpbmRQb3B1cChwb3B1cF9mN2ExOWZiYmUzZmM0ZjUyODE1ZWQxM2NiZTBiNzIzNikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl80YTMwMGY5MTJhMWI0YTM1OGUxMzM1NDU4NjA3MmQ5YSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc3NzE1NiwgLTczLjk1MjYzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zOWM1MGIzZjMzNjU0NTM5Yjc4ZTYyM2FmMWYzOGJjNyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNTY4NDNhZDhlNGY4NDZhMGI0MjBhNWQ3NDQ0MWY0NWUgPSAkKGA8ZGl2IGlkPSJodG1sXzU2ODQzYWQ4ZTRmODQ2YTBiNDIwYTVkNzQ0NDFmNDVlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QUVIgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMzljNTBiM2YzMzY1NDUzOWI3OGU2MjNhZjFmMzhiYzcuc2V0Q29udGVudChodG1sXzU2ODQzYWQ4ZTRmODQ2YTBiNDIwYTVkNzQ0NDFmNDVlKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzRhMzAwZjkxMmExYjRhMzU4ZTEzMzU0NTg2MDcyZDlhLmJpbmRQb3B1cChwb3B1cF8zOWM1MGIzZjMzNjU0NTM5Yjc4ZTYyM2FmMWYzOGJjNykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl84ODc5Y2RmNTI2NDg0OTUwYWVkNTRmMTI3MDk5ZTFjYiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc1OTQ1ODMsIC03My45ODY4MDQ4XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80ODI0NjZlM2I4Y2E0MmQzYTg5ODRkOTZiMTRmNjk4NiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYWM0YTE0ZDc0MjVjNDE3Nzk5NWMwYmNlY2JjMzdlMzEgPSAkKGA8ZGl2IGlkPSJodG1sX2FjNGExNGQ3NDI1YzQxNzc5OTVjMGJjZWNiYzM3ZTMxIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QYXR6ZXJpYSBQZXJmZWN0IFBpenphIFByaWNlOiAkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzQ4MjQ2NmUzYjhjYTQyZDNhODk4NGQ5NmIxNGY2OTg2LnNldENvbnRlbnQoaHRtbF9hYzRhMTRkNzQyNWM0MTc3OTk1YzBiY2VjYmMzN2UzMSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl84ODc5Y2RmNTI2NDg0OTUwYWVkNTRmMTI3MDk5ZTFjYi5iaW5kUG9wdXAocG9wdXBfNDgyNDY2ZTNiOGNhNDJkM2E4OTg0ZDk2YjE0ZjY5ODYpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNmI3ZjViYTk4YzhmNDc5ZjhjOGJjOWY5ZDRlMzZhNmUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43NjQ1OCwgLTczLjk4MjQ3XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9mYTc5MmZmMWYxOTQ0NWM2ODI2YzVkNTM0ZDc3MGIzMyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfOGQ0YWIzNDRkYjllNDhkOTg5OGViNDllMmRkZWJhZGYgPSAkKGA8ZGl2IGlkPSJodG1sXzhkNGFiMzQ0ZGI5ZTQ4ZDk4OThlYjQ5ZTJkZGViYWRmIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij45OSBDZW50IEZyZXNoIFBpenphIFByaWNlOiAkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2ZhNzkyZmYxZjE5NDQ1YzY4MjZjNWQ1MzRkNzcwYjMzLnNldENvbnRlbnQoaHRtbF84ZDRhYjM0NGRiOWU0OGQ5ODk4ZWI0OWUyZGRlYmFkZik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl82YjdmNWJhOThjOGY0NzlmOGM4YmM5ZjlkNGUzNmE2ZS5iaW5kUG9wdXAocG9wdXBfZmE3OTJmZjFmMTk0NDVjNjgyNmM1ZDUzNGQ3NzBiMzMpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNWI5NjQ4NjM0NGQ1NGQ5NWE2ZDMwZGEzMzY0ZGE0NDEgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43ODI3MiwgLTczLjk0NzgxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9mYzBiMjUzZjBkYjk0ZWZhYWMxYjg4MTE0MGMwNGVkNiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfOWM0MjdiNDNmM2RhNGQ2NDk4MGY1OTZkYWZmNDdiNGMgPSAkKGA8ZGl2IGlkPSJodG1sXzljNDI3YjQzZjNkYTRkNjQ5ODBmNTk2ZGFmZjQ3YjRjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OaWNrJ3MgUGl6emEgUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwX2ZjMGIyNTNmMGRiOTRlZmFhYzFiODgxMTQwYzA0ZWQ2LnNldENvbnRlbnQoaHRtbF85YzQyN2I0M2YzZGE0ZDY0OTgwZjU5NmRhZmY0N2I0Yyk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl81Yjk2NDg2MzQ0ZDU0ZDk1YTZkMzBkYTMzNjRkYTQ0MS5iaW5kUG9wdXAocG9wdXBfZmMwYjI1M2YwZGI5NGVmYWFjMWI4ODExNDBjMDRlZDYpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZjRiYjRlN2VkMmMzNGU1Y2I5OGVkNzFlNzViODAyYzYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MDQ5OSwgLTc0LjAwOTYxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9iOWM2YmIzZmFlYjc0ODkwOWJiNWQ4N2UxMzY0NGRlMiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfODk2ZjU5YjkwMDFkNDBmMDgzZTA3MjBiZjY1MjI3Y2IgPSAkKGA8ZGl2IGlkPSJodG1sXzg5NmY1OWI5MDAxZDQwZjA4M2UwNzIwYmY2NTIyN2NiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5VbmRlcmdyb3VuZCBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9iOWM2YmIzZmFlYjc0ODkwOWJiNWQ4N2UxMzY0NGRlMi5zZXRDb250ZW50KGh0bWxfODk2ZjU5YjkwMDFkNDBmMDgzZTA3MjBiZjY1MjI3Y2IpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZjRiYjRlN2VkMmMzNGU1Y2I5OGVkNzFlNzViODAyYzYuYmluZFBvcHVwKHBvcHVwX2I5YzZiYjNmYWViNzQ4OTA5YmI1ZDg3ZTEzNjQ0ZGUyKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzVlYTYwMTM4NDExZjQ4ODE4YjNmODY3ZTVkNjZkMjhjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzI4OTIsIC03My45ODQxMV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMTBjMTZlMzg4ZTQ3NDk1MWJmMWU5YTEwMTMwZWM2YmUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2YyN2M3ZDRkZTY3ZjQyZmFhNjFkYzYxMjI2ZTQ2NDM1ID0gJChgPGRpdiBpZD0iaHRtbF9mMjdjN2Q0ZGU2N2Y0MmZhYTYxZGM2MTIyNmU0NjQzNSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Sm9lICYgUGF0J3MgUGl6emVyaWEgUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzEwYzE2ZTM4OGU0NzQ5NTFiZjFlOWExMDEzMGVjNmJlLnNldENvbnRlbnQoaHRtbF9mMjdjN2Q0ZGU2N2Y0MmZhYTYxZGM2MTIyNmU0NjQzNSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl81ZWE2MDEzODQxMWY0ODgxOGIzZjg2N2U1ZDY2ZDI4Yy5iaW5kUG9wdXAocG9wdXBfMTBjMTZlMzg4ZTQ3NDk1MWJmMWU5YTEwMTMwZWM2YmUpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNjRjNTFhMWRlN2IyNDFlZTg1YTk4YjRiZDNkMTdmMmUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43NjA4NTEyLCAtNzMuOTY5NDgwOV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfOTljZjIyZDdiM2ViNDYyOTg4NzMzZTYyOGE2ODYwZjggPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzYwODg2ZWJlNDhkNzQ2ZWI5N2IwZGFlMDEzMTI4Y2Y2ID0gJChgPGRpdiBpZD0iaHRtbF82MDg4NmViZTQ4ZDc0NmViOTdiMGRhZTAxMzEyOGNmNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TXkgUGllIFBpenplcmlhIFJvbWFuYSBQcmljZTogJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF85OWNmMjJkN2IzZWI0NjI5ODg3MzNlNjI4YTY4NjBmOC5zZXRDb250ZW50KGh0bWxfNjA4ODZlYmU0OGQ3NDZlYjk3YjBkYWUwMTMxMjhjZjYpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNjRjNTFhMWRlN2IyNDFlZTg1YTk4YjRiZDNkMTdmMmUuYmluZFBvcHVwKHBvcHVwXzk5Y2YyMmQ3YjNlYjQ2Mjk4ODczM2U2MjhhNjg2MGY4KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzUzODUxYTYyZjExMDQxYjZiNjBlNzY4YWY2YWU0ZjJiID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzA1MTcsIC03My45MTUxNTVdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzNhNjQzNmRmY2YzODQ0MjNiNmRjMjZiNTkxZGJhNWNjID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8wMWY4MmUxOTRjZWM0MzgxYTRmYzY1MzZjY2RjYjQ1MiA9ICQoYDxkaXYgaWQ9Imh0bWxfMDFmODJlMTk0Y2VjNDM4MWE0ZmM2NTM2Y2NkY2I0NTIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlNpY2lseSdzIEJlc3QgUGl6emVyaWEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfM2E2NDM2ZGZjZjM4NDQyM2I2ZGMyNmI1OTFkYmE1Y2Muc2V0Q29udGVudChodG1sXzAxZjgyZTE5NGNlYzQzODFhNGZjNjUzNmNjZGNiNDUyKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzUzODUxYTYyZjExMDQxYjZiNjBlNzY4YWY2YWU0ZjJiLmJpbmRQb3B1cChwb3B1cF8zYTY0MzZkZmNmMzg0NDIzYjZkYzI2YjU5MWRiYTVjYykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9lMjdhZmMwNTZjZTM0MTM2ODNlNzkxZTBkNWU1YjIzNiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjY2MTYzLCAtNzMuOTkzMzRdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2FlZmVkODUyZmE1YTQzNzM4NmVmZWQxYzhhODUzY2Q5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9kMjA2YTY1NDg3YzE0ZDYyYjQ4MjAyNzk3NDRmOTNlNiA9ICQoYDxkaXYgaWQ9Imh0bWxfZDIwNmE2NTQ4N2MxNGQ2MmI0ODIwMjc5NzQ0ZjkzZTYiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkx1aWdpJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYWVmZWQ4NTJmYTVhNDM3Mzg2ZWZlZDFjOGE4NTNjZDkuc2V0Q29udGVudChodG1sX2QyMDZhNjU0ODdjMTRkNjJiNDgyMDI3OTc0NGY5M2U2KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2UyN2FmYzA1NmNlMzQxMzY4M2U3OTFlMGQ1ZTViMjM2LmJpbmRQb3B1cChwb3B1cF9hZWZlZDg1MmZhNWE0MzczODZlZmVkMWM4YTg1M2NkOSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl82NDIxMTkxOTgwNjk0Y2IzYjc2ZWMxMTUxNzY2ZGJmOCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjczMjExMjQ1NTc3MDQsIC03My45ODM5MDU2ODI4ODgyXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF84M2NmOWZkZjQxYzI0NzYyYmQ1YjNhYjVlNzFlZWZhYyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZjBhZmE0MWM3ZTgxNDdkMDkyMDczZDgwNTMwMGFjYmEgPSAkKGA8ZGl2IGlkPSJodG1sX2YwYWZhNDFjN2U4MTQ3ZDA5MjA3M2Q4MDUzMDBhY2JhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5BcnRpY2hva2UgQmFzaWxsZSdzIFBpenphIFByaWNlOiAkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzgzY2Y5ZmRmNDFjMjQ3NjJiZDViM2FiNWU3MWVlZmFjLnNldENvbnRlbnQoaHRtbF9mMGFmYTQxYzdlODE0N2QwOTIwNzNkODA1MzAwYWNiYSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl82NDIxMTkxOTgwNjk0Y2IzYjc2ZWMxMTUxNzY2ZGJmOC5iaW5kUG9wdXAocG9wdXBfODNjZjlmZGY0MWMyNDc2MmJkNWIzYWI1ZTcxZWVmYWMpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfZjQzZWQ5OWRkOGRmNDAzMmFjYzNkZTRhYTRkZmI5ZWIgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MTAzNzIsIC03My45Njc5NF0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfZWViNzU1N2QxYjA3NDljMzk1YzE2NGNhYTgwNzU4ZGUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzY2ZWFiODlmYTc3ZDQ2NDI4M2E5NGFiMDkwN2NhZjQ2ID0gJChgPGRpdiBpZD0iaHRtbF82NmVhYjg5ZmE3N2Q0NjQyODNhOTRhYjA5MDdjYWY0NiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+QmFyYW5vIFByaWNlOiAkJCQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZWViNzU1N2QxYjA3NDljMzk1YzE2NGNhYTgwNzU4ZGUuc2V0Q29udGVudChodG1sXzY2ZWFiODlmYTc3ZDQ2NDI4M2E5NGFiMDkwN2NhZjQ2KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2Y0M2VkOTlkZDhkZjQwMzJhY2MzZGU0YWE0ZGZiOWViLmJpbmRQb3B1cChwb3B1cF9lZWI3NTU3ZDFiMDc0OWMzOTVjMTY0Y2FhODA3NThkZSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8yODk1Yjk1ODk4N2Y0MWM0YTYwN2UzODNjOTQzY2RlZiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcyMzYyNSwgLTczLjk4ODU1NV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNTg3YmQyNGU3NDIxNDc3N2JjMzQwZDBiNGVhMzljMmQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzA4MzhjMDhjOGYyMDRlNWNhYWFiOTEzOTY2ZTUyMjMwID0gJChgPGRpdiBpZD0iaHRtbF8wODM4YzA4YzhmMjA0ZTVjYWFhYjkxMzk2NmU1MjIzMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TGlsIEZyYW5raWUncyBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNTg3YmQyNGU3NDIxNDc3N2JjMzQwZDBiNGVhMzljMmQuc2V0Q29udGVudChodG1sXzA4MzhjMDhjOGYyMDRlNWNhYWFiOTEzOTY2ZTUyMjMwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzI4OTViOTU4OTg3ZjQxYzRhNjA3ZTM4M2M5NDNjZGVmLmJpbmRQb3B1cChwb3B1cF81ODdiZDI0ZTc0MjE0Nzc3YmMzNDBkMGI0ZWEzOWMyZCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9lODVkMTJjNzU2NGI0NWE3YmVhZmE3MWQzMWZhNDRjZCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxNzc0OTA0ODUwNzUsIC03NC4wNDQxOTI0MzU1MTI1XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zZTc4NThmNDVhMzI0OGNhYWZhY2JhMjI5MzhhMzljZCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYjAxMTQ5MzAxYzkwNGYwOTkzNGMwMTU0YmI4YWI3MTEgPSAkKGA8ZGl2IGlkPSJodG1sX2IwMTE0OTMwMWM5MDRmMDk5MzRjMDE1NGJiOGFiNzExIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5SYXp6YSBQaXp6YSBBcnRpZ2lhbmFsZSBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfM2U3ODU4ZjQ1YTMyNDhjYWFmYWNiYTIyOTM4YTM5Y2Quc2V0Q29udGVudChodG1sX2IwMTE0OTMwMWM5MDRmMDk5MzRjMDE1NGJiOGFiNzExKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2U4NWQxMmM3NTY0YjQ1YTdiZWFmYTcxZDMxZmE0NGNkLmJpbmRQb3B1cChwb3B1cF8zZTc4NThmNDVhMzI0OGNhYWZhY2JhMjI5MzhhMzljZCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl83NzY2Yzk3YjhhZDk0MWU0ODQwYmMyNjAyMjU2OTE3ZiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjczOTI1OTc1ODQ2MTIsIC03NC4wMDg4MTMwNjkyMTg3XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yZGI5ZWY1M2M5MGI0ZTY4OWIzNjRhZjFkZjNmZjQyMSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNDE5ZDI3Yjc0ZmY4NDU0MzkxNDEwNmRmMmJmYzE2ZWEgPSAkKGA8ZGl2IGlkPSJodG1sXzQxOWQyN2I3NGZmODQ1NDM5MTQxMDZkZjJiZmMxNmVhIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5TSU3DkiBQSVpaQSBQcmljZTogJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8yZGI5ZWY1M2M5MGI0ZTY4OWIzNjRhZjFkZjNmZjQyMS5zZXRDb250ZW50KGh0bWxfNDE5ZDI3Yjc0ZmY4NDU0MzkxNDEwNmRmMmJmYzE2ZWEpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNzc2NmM5N2I4YWQ5NDFlNDg0MGJjMjYwMjI1NjkxN2YuYmluZFBvcHVwKHBvcHVwXzJkYjllZjUzYzkwYjRlNjg5YjM2NGFmMWRmM2ZmNDIxKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzg4ODRmYWZhMTQ5ZDQxMzdhYzRjYTFmMjg2OWRhYWVkID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzMwMzMsIC03My45ODQwM10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMjYwNzY5ZGFmMTlhNDA0MWIwYzE2Yzg5YmUxZDkwOTcgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2JjYzk3NGQxNjQwYjQxMzlhOTExNmJmZGY2YTNlN2YzID0gJChgPGRpdiBpZD0iaHRtbF9iY2M5NzRkMTY0MGI0MTM5YTkxMTZiZmRmNmEzZTdmMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U2F1Y2UgUGl6emVyaWEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMjYwNzY5ZGFmMTlhNDA0MWIwYzE2Yzg5YmUxZDkwOTcuc2V0Q29udGVudChodG1sX2JjYzk3NGQxNjQwYjQxMzlhOTExNmJmZGY2YTNlN2YzKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzg4ODRmYWZhMTQ5ZDQxMzdhYzRjYTFmMjg2OWRhYWVkLmJpbmRQb3B1cChwb3B1cF8yNjA3NjlkYWYxOWE0MDQxYjBjMTZjODliZTFkOTA5NykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9hM2Y1ODRiNDY3MjY0MmZjYWZkYThmNjg5NjM2M2NmMCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjYzNDc0MSwgLTc0LjAyODQyNTldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzVlZGExZmYxODQwZjQ1Yzg5ODA4Nzc1NWNiYTgwMDJkID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF80ZTY5NjMxNmRkODc0ZmNiYjBlODlmOTNiMTU2MTAyNCA9ICQoYDxkaXYgaWQ9Imh0bWxfNGU2OTYzMTZkZDg3NGZjYmIwZTg5ZjkzYjE1NjEwMjQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxvbWJhcmRvJ3Mgb2YgQmF5IFJpZGdlIFByaWNlOiAkJCBSYXRpbmc6IDUuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81ZWRhMWZmMTg0MGY0NWM4OTgwODc3NTVjYmE4MDAyZC5zZXRDb250ZW50KGh0bWxfNGU2OTYzMTZkZDg3NGZjYmIwZTg5ZjkzYjE1NjEwMjQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYTNmNTg0YjQ2NzI2NDJmY2FmZGE4ZjY4OTYzNjNjZjAuYmluZFBvcHVwKHBvcHVwXzVlZGExZmYxODQwZjQ1Yzg5ODA4Nzc1NWNiYTgwMDJkKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2UyYjM3NzE2OGQ4NTQ0NGM4NmQwYjJmMWU1ZmE0ZDJjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzU0NTE3MDcxMjcwMSwgLTczLjk3NjU5NzAwMDU3NjNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzc1MTVjN2Y3YTA4YjRhMTk5MzBhNmYxYzg1ZjJiNTM0ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9iODNhMDE3ZDIyNzc0ZjcxOWMyZDVmMjRiMWFjN2VlYiA9ICQoYDxkaXYgaWQ9Imh0bWxfYjgzYTAxN2QyMjc3NGY3MTljMmQ1ZjI0YjFhYzdlZWIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPlJvYmVydGEncyBQaXp6YSBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNzUxNWM3ZjdhMDhiNGExOTkzMGE2ZjFjODVmMmI1MzQuc2V0Q29udGVudChodG1sX2I4M2EwMTdkMjI3NzRmNzE5YzJkNWYyNGIxYWM3ZWViKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2UyYjM3NzE2OGQ4NTQ0NGM4NmQwYjJmMWU1ZmE0ZDJjLmJpbmRQb3B1cChwb3B1cF83NTE1YzdmN2EwOGI0YTE5OTMwYTZmMWM4NWYyYjUzNCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8zMzUwODdhMTgyZTc0MTA5YjYwMmY3NmRmMzM4ZTc2NyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc0MzU1MywgLTczLjk4MDAyNl0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfM2MxNWQ5NDMwMWE3NGJmYzk2MjQyYTM4ZmMyZmI0Y2EgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzJiYWI0NzM3ZjdjZDQwZWViODFmYzVkN2Y0YmUyMWMwID0gJChgPGRpdiBpZD0iaHRtbF8yYmFiNDczN2Y3Y2Q0MGVlYjgxZmM1ZDdmNGJlMjFjMCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+Tk9STUEgR2FzdHJvbm9taWEgU2ljaWxpYW5hIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zYzE1ZDk0MzAxYTc0YmZjOTYyNDJhMzhmYzJmYjRjYS5zZXRDb250ZW50KGh0bWxfMmJhYjQ3MzdmN2NkNDBlZWI4MWZjNWQ3ZjRiZTIxYzApOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMzM1MDg3YTE4MmU3NDEwOWI2MDJmNzZkZjMzOGU3NjcuYmluZFBvcHVwKHBvcHVwXzNjMTVkOTQzMDFhNzRiZmM5NjI0MmEzOGZjMmZiNGNhKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2EyN2Y4MGJlYzVmZDQyZjg5OTZkYWU3ODk4NzczOGJjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzYyNzExLCAtNzMuOTg2NzA4XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9iNmYzNWU5OTY5YTA0Mjg1YTBkY2Q5NzAzOTBhMWQ1MiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYmIxZDhiNmZlMjk4NDE5Njk5ZTQxMTAzZTlhYzI4OWIgPSAkKGA8ZGl2IGlkPSJodG1sX2JiMWQ4YjZmZTI5ODQxOTY5OWU0MTEwM2U5YWMyODliIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Eb24gQW50b25pbyBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYjZmMzVlOTk2OWEwNDI4NWEwZGNkOTcwMzkwYTFkNTIuc2V0Q29udGVudChodG1sX2JiMWQ4YjZmZTI5ODQxOTY5OWU0MTEwM2U5YWMyODliKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2EyN2Y4MGJlYzVmZDQyZjg5OTZkYWU3ODk4NzczOGJjLmJpbmRQb3B1cChwb3B1cF9iNmYzNWU5OTY5YTA0Mjg1YTBkY2Q5NzAzOTBhMWQ1MikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8zYzMxYjc5ZDBlN2I0N2ZmYjM4ZmY1OGY2YzI5ZDNjYSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxODk4NjUxMTIzMDUsIC03My45OTY5MzI5ODMzOTg0XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zZWE0OWQ4MDY1ZTY0NWM5ODBkYmFjZTg4MmRkYTA5MSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMzFmZGY3MjdiODQ0NDgwODk1ZDU3YzEzNzJhODM3M2UgPSAkKGA8ZGl2IGlkPSJodG1sXzMxZmRmNzI3Yjg0NDQ4MDg5NWQ1N2MxMzcyYTgzNzNlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5NYXJnaGVyaXRhIE5ZQyBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfM2VhNDlkODA2NWU2NDVjOTgwZGJhY2U4ODJkZGEwOTEuc2V0Q29udGVudChodG1sXzMxZmRmNzI3Yjg0NDQ4MDg5NWQ1N2MxMzcyYTgzNzNlKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzNjMzFiNzlkMGU3YjQ3ZmZiMzhmZjU4ZjZjMjlkM2NhLmJpbmRQb3B1cChwb3B1cF8zZWE0OWQ4MDY1ZTY0NWM5ODBkYmFjZTg4MmRkYTA5MSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8zNDhiYTU3NmIwN2M0NjllYjBlY2UxYjYwOGI2MGUwMiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjgwNTU4NywgLTczLjk0NzU0N10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMGQ0N2EwN2ExYTk3NGM5YTlhMDc3YTU5N2ZmNjlhYzEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2Q1MDI1Y2I1YzYwNjQ0MzA4YjVjYzRmMjM4YWNiNjM3ID0gJChgPGRpdiBpZD0iaHRtbF9kNTAyNWNiNWM2MDY0NDMwOGI1Y2M0ZjIzOGFjYjYzNyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+U290dG9jYXNhIFBpenplcmlhIC0gSGFybGVtIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8wZDQ3YTA3YTFhOTc0YzlhOWEwNzdhNTk3ZmY2OWFjMS5zZXRDb250ZW50KGh0bWxfZDUwMjVjYjVjNjA2NDQzMDhiNWNjNGYyMzhhY2I2MzcpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMzQ4YmE1NzZiMDdjNDY5ZWIwZWNlMWI2MDhiNjBlMDIuYmluZFBvcHVwKHBvcHVwXzBkNDdhMDdhMWE5NzRjOWE5YTA3N2E1OTdmZjY5YWMxKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzQ5ODMxZDY5OWQxOTRmNDVhNTRiYTc0YzljY2NkMjI1ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzYyODk2MSwgLTczLjkyMDcwODFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzA5YjkwZWE4MjQ3ZjRmMTI5ODc4ZGNhNjY0ZWU3MTNkID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF80M2IxMDY5NDQwZWU0ZmMxOTlhMTBkNWNhOGY4MTlmYSA9ICQoYDxkaXYgaWQ9Imh0bWxfNDNiMTA2OTQ0MGVlNGZjMTk5YTEwZDVjYThmODE5ZmEiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk1pbGtmbG93ZXIgUHJpY2U6ICQkIFJhdGluZzogNC41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzA5YjkwZWE4MjQ3ZjRmMTI5ODc4ZGNhNjY0ZWU3MTNkLnNldENvbnRlbnQoaHRtbF80M2IxMDY5NDQwZWU0ZmMxOTlhMTBkNWNhOGY4MTlmYSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl80OTgzMWQ2OTlkMTk0ZjQ1YTU0YmE3NGM5Y2NjZDIyNS5iaW5kUG9wdXAocG9wdXBfMDliOTBlYTgyNDdmNGYxMjk4NzhkY2E2NjRlZTcxM2QpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMmUyZDg4M2FmMWU3NGRhOWI2M2E3YTQ1Yzk4MjA4N2MgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MzA2MTYsIC03My45ODM0ODQ2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF83NTk0YzczMjlkMjI0YWRmYjRjODMxYzM3OTM3MDdkYSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfM2ZjNjI5OWVhNmM1NGU1OWIxYzU4OWUwZGYwOGNjODQgPSAkKGA8ZGl2IGlkPSJodG1sXzNmYzYyOTllYTZjNTRlNTliMWM1ODllMGRmMDhjYzg0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MdXp6b3MgTGEgUGl6emEgTmFwb2xldGFuYSBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNzU5NGM3MzI5ZDIyNGFkZmI0YzgzMWMzNzkzNzA3ZGEuc2V0Q29udGVudChodG1sXzNmYzYyOTllYTZjNTRlNTliMWM1ODllMGRmMDhjYzg0KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzJlMmQ4ODNhZjFlNzRkYTliNjNhN2E0NWM5ODIwODdjLmJpbmRQb3B1cChwb3B1cF83NTk0YzczMjlkMjI0YWRmYjRjODMxYzM3OTM3MDdkYSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl84MDYwOTE1NmUyZDc0MzIxOTAxOGM2MDljNzYwN2ZlMiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjgwNDQ3LCAtNzMuOTY2MTZdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzg4NjRkMTU2NGJkMDQ4OGZiNDZjNmY0OWY4MzU2ZjRiID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9iNjU1OWJhZjEwY2M0ZWZlOWU5N2Y4NzI0ZjY4MTMzZCA9ICQoYDxkaXYgaWQ9Imh0bWxfYjY1NTliYWYxMGNjNGVmZTllOTdmODcyNGY2ODEzM2QiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPktvcm9uZXQgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiAzLjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfODg2NGQxNTY0YmQwNDg4ZmI0NmM2ZjQ5ZjgzNTZmNGIuc2V0Q29udGVudChodG1sX2I2NTU5YmFmMTBjYzRlZmU5ZTk3Zjg3MjRmNjgxMzNkKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzgwNjA5MTU2ZTJkNzQzMjE5MDE4YzYwOWM3NjA3ZmUyLmJpbmRQb3B1cChwb3B1cF84ODY0ZDE1NjRiZDA0ODhmYjQ2YzZmNDlmODM1NmY0YikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl85NjQ1OWIwZTM0ZDE0ODIxYmRiNDNhNzg1NGY3MTg4NiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjgwMjkwNjksIC03My45NTEwODI1XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8xMDQ4YzM0NWIxNDg0NzNkYjE0MTA0YjQ0NWE3Y2M2NyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNmRkZTgwYWIyMDQwNGJhYzgyNzgzYTkzZWYzMjk2MjQgPSAkKGA8ZGl2IGlkPSJodG1sXzZkZGU4MGFiMjA0MDRiYWM4Mjc4M2E5M2VmMzI5NjI0IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5IYXJsZW0gUGl6emEgQ28gUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzEwNDhjMzQ1YjE0ODQ3M2RiMTQxMDRiNDQ1YTdjYzY3LnNldENvbnRlbnQoaHRtbF82ZGRlODBhYjIwNDA0YmFjODI3ODNhOTNlZjMyOTYyNCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl85NjQ1OWIwZTM0ZDE0ODIxYmRiNDNhNzg1NGY3MTg4Ni5iaW5kUG9wdXAocG9wdXBfMTA0OGMzNDViMTQ4NDczZGIxNDEwNGI0NDVhN2NjNjcpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMDhlZTMwNDZmZmZlNDk0M2E4ZDZmMzI1NjE0MDMyZTYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43NDUwNiwgLTczLjk5ODM0XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8zNDUwNTk0YzA0YWU0MjU3OWMxMTcxMjdkOTVkZWU1OCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfY2MwYjk3ZDA4MDBjNDM3MmIwNDRmMTQ0MTI5YTY0Y2UgPSAkKGA8ZGl2IGlkPSJodG1sX2NjMGI5N2QwODAwYzQzNzJiMDQ0ZjE0NDEyOWE2NGNlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5MaW9ucyAmIFRpZ2VycyAmIFNxdWFyZXMgRGV0cm9pdCBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zNDUwNTk0YzA0YWU0MjU3OWMxMTcxMjdkOTVkZWU1OC5zZXRDb250ZW50KGh0bWxfY2MwYjk3ZDA4MDBjNDM3MmIwNDRmMTQ0MTI5YTY0Y2UpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMDhlZTMwNDZmZmZlNDk0M2E4ZDZmMzI1NjE0MDMyZTYuYmluZFBvcHVwKHBvcHVwXzM0NTA1OTRjMDRhZTQyNTc5YzExNzEyN2Q5NWRlZTU4KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzRiNDZjMjM0NTc4YTQ3MDE5OWMyZTZlMGIwYjE4NzViID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzYyMzM3NywgLTczLjk5Njc1NzldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2E4MmE1ZGNmYmE2NjRjMTJhZjZkZTA5MTUwZmJmMWUzID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8yNDRhMDllNzI4M2U0OGQ1OTMzZjcxYWRkZDg5ZGIyOSA9ICQoYDxkaXYgaWQ9Imh0bWxfMjQ0YTA5ZTcyODNlNDhkNTkzM2Y3MWFkZGQ4OWRiMjkiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkNvcm5lciBTbGljZSBQcmljZTogJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9hODJhNWRjZmJhNjY0YzEyYWY2ZGUwOTE1MGZiZjFlMy5zZXRDb250ZW50KGh0bWxfMjQ0YTA5ZTcyODNlNDhkNTkzM2Y3MWFkZGQ4OWRiMjkpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNGI0NmMyMzQ1NzhhNDcwMTk5YzJlNmUwYjBiMTg3NWIuYmluZFBvcHVwKHBvcHVwX2E4MmE1ZGNmYmE2NjRjMTJhZjZkZTA5MTUwZmJmMWUzKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzMxMTI5ZTc2ODQ2MTQxOWM5OGMzODFlOWI5MGFhMDhkID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjY4MjIxLCAtNzMuOTg2OTM2XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80NWU5NWI1ZGQ0OTI0YWYzOWNjMTY2NWI5ZjBmNTY3NCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYjAyMGY3NDUxMTAzNGNjOTkyZmM3ODI4OTQwNGNiNjIgPSAkKGA8ZGl2IGlkPSJodG1sX2IwMjBmNzQ1MTEwMzRjYzk5MmZjNzgyODk0MDRjYjYyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QZXBwaW5vJ3MgQnJpY2sgT3ZlbiBQaXp6YSAmIFJlc3RhdXJhbnQgUHJpY2U6ICQkIFJhdGluZzogNC41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzQ1ZTk1YjVkZDQ5MjRhZjM5Y2MxNjY1YjlmMGY1Njc0LnNldENvbnRlbnQoaHRtbF9iMDIwZjc0NTExMDM0Y2M5OTJmYzc4Mjg5NDA0Y2I2Mik7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8zMTEyOWU3Njg0NjE0MTljOThjMzgxZTliOTBhYTA4ZC5iaW5kUG9wdXAocG9wdXBfNDVlOTViNWRkNDkyNGFmMzljYzE2NjViOWYwZjU2NzQpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNTNkNmZhYTg5Yzc5NDJkZWJiM2Y0ZTNlZGQzOGVlMjYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MDkxMzIsIC03NC4wMDQ2NzZdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2M0MzM1YmQ3MDg0YjQxZDE5NjVjMGNjZjY1NjAyODU1ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF84Y2I0ZDNiZDVkMWI0ZTQxYjZjZTAzNTNlNzQ5ZTA0NyA9ICQoYDxkaXYgaWQ9Imh0bWxfOGNiNGQzYmQ1ZDFiNGU0MWI2Y2UwMzUzZTc0OWUwNDciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPktlc3RlIFdhbGwgU3RyZWV0IFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jNDMzNWJkNzA4NGI0MWQxOTY1YzBjY2Y2NTYwMjg1NS5zZXRDb250ZW50KGh0bWxfOGNiNGQzYmQ1ZDFiNGU0MWI2Y2UwMzUzZTc0OWUwNDcpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNTNkNmZhYTg5Yzc5NDJkZWJiM2Y0ZTNlZGQzOGVlMjYuYmluZFBvcHVwKHBvcHVwX2M0MzM1YmQ3MDg0YjQxZDE5NjVjMGNjZjY1NjAyODU1KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzZkNTRhNzBjOTdmZTRiMWI4Y2FhNzA1MzUzZjUzNDgwID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzI4MTUsIC03My44OTIwNl0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNTRiNWNjYjMxOWNiNDI0ZDhkNjY4MjdmZjVlMWMyM2MgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2Q0NmZkMGM1Nzg4NDQ0Njg5NDA5YTkzY2NmZTY3MzlhID0gJChgPGRpdiBpZD0iaHRtbF9kNDZmZDBjNTc4ODQ0NDY4OTQwOWE5M2NjZmU2NzM5YSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TWFzcGV0aCBQaXp6YSBQcmljZTogJCBSYXRpbmc6IDMuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81NGI1Y2NiMzE5Y2I0MjRkOGQ2NjgyN2ZmNWUxYzIzYy5zZXRDb250ZW50KGh0bWxfZDQ2ZmQwYzU3ODg0NDQ2ODk0MDlhOTNjY2ZlNjczOWEpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNmQ1NGE3MGM5N2ZlNGIxYjhjYWE3MDUzNTNmNTM0ODAuYmluZFBvcHVwKHBvcHVwXzU0YjVjY2IzMTljYjQyNGQ4ZDY2ODI3ZmY1ZTFjMjNjKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2EyOTI2OTJiYmJkMTQ2YjU5MzhmNzU4ZGQ4NzA4MThkID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzEzOTQ5OSwgLTczLjk1NTYzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80MWIxYWFkZjVmODU0OWU0OWEzNTIwNmUyYWYyYTEwMCA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYzQwOTVmZTc5MGU0NGQyNGJlOTVjMDZmZWZhMzExNzIgPSAkKGA8ZGl2IGlkPSJodG1sX2M0MDk1ZmU3OTBlNDRkMjRiZTk1YzA2ZmVmYTMxMTcyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5Sb3NhJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNDFiMWFhZGY1Zjg1NDllNDlhMzUyMDZlMmFmMmExMDAuc2V0Q29udGVudChodG1sX2M0MDk1ZmU3OTBlNDRkMjRiZTk1YzA2ZmVmYTMxMTcyKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2EyOTI2OTJiYmJkMTQ2YjU5MzhmNzU4ZGQ4NzA4MThkLmJpbmRQb3B1cChwb3B1cF80MWIxYWFkZjVmODU0OWU0OWEzNTIwNmUyYWYyYTEwMCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9kZTAyNTE3MzExYmM0Yzg3OGRjMmRjZTU3OWQ1NDhhYyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcwMjc2LCAtNzMuOTE3MV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNTUzMDM3MGE4MDdjNDk1N2FlOGM5YjliNDRkYjRlYWUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzAxZTU3OTE2NmM4YzQ4OWI5OGUzMWYyZDE2OTFlZWVhID0gJChgPGRpdiBpZD0iaHRtbF8wMWU1NzkxNjZjOGM0ODliOThlMzFmMmQxNjkxZWVlYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+T3BzIFByaWNlOiAkJCBSYXRpbmc6IDQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81NTMwMzcwYTgwN2M0OTU3YWU4YzliOWI0NGRiNGVhZS5zZXRDb250ZW50KGh0bWxfMDFlNTc5MTY2YzhjNDg5Yjk4ZTMxZjJkMTY5MWVlZWEpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZGUwMjUxNzMxMWJjNGM4NzhkYzJkY2U1NzlkNTQ4YWMuYmluZFBvcHVwKHBvcHVwXzU1MzAzNzBhODA3YzQ5NTdhZThjOWI5YjQ0ZGI0ZWFlKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzg5MTc5Y2VmMzI5NjQ0NWQ5MDVhOTMyYTdhMmNhMDQ1ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzQ1OSwgLTczLjg4MzRdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzU1MjI0MWQxMjg5ZTQyMTQ5MWFiNDRjMDlhZGI2OTVjID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF85NTgyM2NmZDNjNzk0NGU1YjA0MTBiYWU4OWE4NmM0YyA9ICQoYDxkaXYgaWQ9Imh0bWxfOTU4MjNjZmQzYzc5NDRlNWIwNDEwYmFlODlhODZjNGMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkxvdWllJ3MgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfNTUyMjQxZDEyODllNDIxNDkxYWI0NGMwOWFkYjY5NWMuc2V0Q29udGVudChodG1sXzk1ODIzY2ZkM2M3OTQ0ZTViMDQxMGJhZTg5YTg2YzRjKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzg5MTc5Y2VmMzI5NjQ0NWQ5MDVhOTMyYTdhMmNhMDQ1LmJpbmRQb3B1cChwb3B1cF81NTIyNDFkMTI4OWU0MjE0OTFhYjQ0YzA5YWRiNjk1YykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl84YTZlNjFhNWQwODI0MmM3ODJhYjQ1MGFkNjhlNTQwOCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc2NDkyLCAtNzMuOTc3OTRdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2JmY2FhNDM5M2YzYjQ1YWVhMDc2MzA4M2YwMzM4NzYyID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9iZmViNTQxZjRjYWE0ZDZjYjA4YTdlMjY3ZTgxNzNhNSA9ICQoYDxkaXYgaWQ9Imh0bWxfYmZlYjU0MWY0Y2FhNGQ2Y2IwOGE3ZTI2N2U4MTczYTUiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkFuZ2VsbydzIENvYWwgT3ZlbiBQaXp6ZXJpYSBQcmljZTogJCQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYmZjYWE0MzkzZjNiNDVhZWEwNzYzMDgzZjAzMzg3NjIuc2V0Q29udGVudChodG1sX2JmZWI1NDFmNGNhYTRkNmNiMDhhN2UyNjdlODE3M2E1KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzhhNmU2MWE1ZDA4MjQyYzc4MmFiNDUwYWQ2OGU1NDA4LmJpbmRQb3B1cChwb3B1cF9iZmNhYTQzOTNmM2I0NWFlYTA3NjMwODNmMDMzODc2MikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8wYjRiMGEwNzUwOTg0MzBjOGI2YTg3NTcxNWM2Y2ZlNyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc0NDUsIC03My45ODEzOV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNGFmN2ZjMzVlMmMwNDBlYzkxMjJlMjY4ZDVmYTM2MTEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzVmMzA1OTE4MmE1NTRiYWViYzZiN2I1ODJkMGJiOGJjID0gJChgPGRpdiBpZD0iaHRtbF81ZjMwNTkxODJhNTU0YmFlYmM2YjdiNTgyZDBiYjhiYyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+VmV6em8gUHJpY2U6ICQkIFJhdGluZzogNC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzRhZjdmYzM1ZTJjMDQwZWM5MTIyZTI2OGQ1ZmEzNjExLnNldENvbnRlbnQoaHRtbF81ZjMwNTkxODJhNTU0YmFlYmM2YjdiNTgyZDBiYjhiYyk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8wYjRiMGEwNzUwOTg0MzBjOGI2YTg3NTcxNWM2Y2ZlNy5iaW5kUG9wdXAocG9wdXBfNGFmN2ZjMzVlMmMwNDBlYzkxMjJlMjY4ZDVmYTM2MTEpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNmI0ZTVkODY1YjlhNDJiYTkzZWFhMTU1ZWUwZjkwZGEgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC42OTg1LCAtNzMuOTkyNzNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzEwZWEwMDI1OGQ3ODQ2MDE4NzRiZjljNjhhMmNhNThjID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9hYmM3NWFlODg5MTQ0MWUwOWVmOGFmY2I2Yzk5ZjU1MiA9ICQoYDxkaXYgaWQ9Imh0bWxfYWJjNzVhZTg4OTE0NDFlMDllZjhhZmNiNmM5OWY1NTIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkZhc2NhdGkgUGl6emEgUHJpY2U6ICQgUmF0aW5nOiA0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMTBlYTAwMjU4ZDc4NDYwMTg3NGJmOWM2OGEyY2E1OGMuc2V0Q29udGVudChodG1sX2FiYzc1YWU4ODkxNDQxZTA5ZWY4YWZjYjZjOTlmNTUyKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzZiNGU1ZDg2NWI5YTQyYmE5M2VhYTE1NWVlMGY5MGRhLmJpbmRQb3B1cChwb3B1cF8xMGVhMDAyNThkNzg0NjAxODc0YmY5YzY4YTJjYTU4YykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8zYmQwOWY4MzZjNDk0M2Q3YjEwZmI4ZmY2MjZlNDRkZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxOTY5NzksIC03My45OTUxMTc1XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF81ZmNiMmM0NWU4NDc0ZDliYTQ2MzMyZjliOTI5OWE2NSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMzVjZDc5ZjU4MzdhNDE5ZmJlYTM5YWEyODU5Y2YzMzggPSAkKGA8ZGl2IGlkPSJodG1sXzM1Y2Q3OWY1ODM3YTQxOWZiZWEzOWFhMjg1OWNmMzM4IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5QaWFjZXJlIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81ZmNiMmM0NWU4NDc0ZDliYTQ2MzMyZjliOTI5OWE2NS5zZXRDb250ZW50KGh0bWxfMzVjZDc5ZjU4MzdhNDE5ZmJlYTM5YWEyODU5Y2YzMzgpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfM2JkMDlmODM2YzQ5NDNkN2IxMGZiOGZmNjI2ZTQ0ZGUuYmluZFBvcHVwKHBvcHVwXzVmY2IyYzQ1ZTg0NzRkOWJhNDYzMzJmOWI5Mjk5YTY1KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2IwN2FiYjdhNDM3ZDRmZTRiYmI1YjEzNDZlMzQ5ZGQ3ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzYzOTQsIC03My45ODgzMzk5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwX2UzY2MxZmM1NTkwODRkYzg5NTkwYzQwN2IyM2FiYTMyKTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8xYjE1NDBiZDI0NTU0Mzc2ODUyMGUzODA2ZGNlYjNjYSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfYTBjNTcyZTY0YWEwNDNlNjhjNDAwYjUzMWU1MzgwZjkgPSAkKGA8ZGl2IGlkPSJodG1sX2EwYzU3MmU2NGFhMDQzZTY4YzQwMGI1MzFlNTM4MGY5IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5CIFNpZGUgUGl6emEgJiBXaW5lIEJhciBQcmljZTogJCQgUmF0aW5nOiA0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMWIxNTQwYmQyNDU1NDM3Njg1MjBlMzgwNmRjZWIzY2Euc2V0Q29udGVudChodG1sX2EwYzU3MmU2NGFhMDQzZTY4YzQwMGI1MzFlNTM4MGY5KTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2IwN2FiYjdhNDM3ZDRmZTRiYmI1YjEzNDZlMzQ5ZGQ3LmJpbmRQb3B1cChwb3B1cF8xYjE1NDBiZDI0NTU0Mzc2ODUyMGUzODA2ZGNlYjNjYSkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl84NWEwOTViZGIzNTA0NzNhOGIwYmViZGQxNmI2MjVjYSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcwNzAxNSwgLTc0LjAwOTczNV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF9lM2NjMWZjNTU5MDg0ZGM4OTU5MGM0MDdiMjNhYmEzMik7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfODdkN2ZkMmE3MWIyNDYyZGI4MjBkNmMxYzljYWQxYjEgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzdjZTZlN2EwYmY1ZTQ4ZGU5MmFiMDhjMmUxNDBlZGYzID0gJChgPGRpdiBpZD0iaHRtbF83Y2U2ZTdhMGJmNWU0OGRlOTJhYjA4YzJlMTQwZWRmMyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmVhcG9saXRhbiBFeHByZXNzIFByaWNlOiAkJCBSYXRpbmc6IDMuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF84N2Q3ZmQyYTcxYjI0NjJkYjgyMGQ2YzFjOWNhZDFiMS5zZXRDb250ZW50KGh0bWxfN2NlNmU3YTBiZjVlNDhkZTkyYWIwOGMyZTE0MGVkZjMpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfODVhMDk1YmRiMzUwNDczYThiMGJlYmRkMTZiNjI1Y2EuYmluZFBvcHVwKHBvcHVwXzg3ZDdmZDJhNzFiMjQ2MmRiODIwZDZjMWM5Y2FkMWIxKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2NhMDhiZWUxMjgyYzQzYjZhMGFhNGY5ODg5Y2ZmODA2ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjkwNDU2MzkwMzgwOSwgLTczLjk2OTYyNzM4MDM3MTFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfZTNjYzFmYzU1OTA4NGRjODk1OTBjNDA3YjIzYWJhMzIpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzcxZThhNTBmMTUxYTQzODY5YjdiN2EwNWIyMDUwYTg5ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8wZGQyNTA1MzA1ODg0YzRhODg5NThiZDI4OGRlOGVmMyA9ICQoYDxkaXYgaWQ9Imh0bWxfMGRkMjUwNTMwNTg4NGM0YTg4OTU4YmQyODhkZThlZjMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPkdyYXppZWxsYSdzIFByaWNlOiAkJCBSYXRpbmc6IDQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF83MWU4YTUwZjE1MWE0Mzg2OWI3YjdhMDViMjA1MGE4OS5zZXRDb250ZW50KGh0bWxfMGRkMjUwNTMwNTg4NGM0YTg4OTU4YmQyODhkZThlZjMpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfY2EwOGJlZTEyODJjNDNiNmEwYWE0Zjk4ODljZmY4MDYuYmluZFBvcHVwKHBvcHVwXzcxZThhNTBmMTUxYTQzODY5YjdiN2EwNWIyMDUwYTg5KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKPC9zY3JpcHQ+ onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



## Summary

Nice work! In this lab, you've made multiple API calls to Yelp in order to paginate through a results set, performing some basic exploratory analysis and then creating a nice interactive map to display the results using Folium! Well done!
