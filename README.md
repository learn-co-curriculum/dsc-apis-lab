
# Building a Simple GIS with Yelp API and Folium - Lab

## Introduction 

So we have learned quite a bit about APIs and how they are now big buzzword in the tech industry. Think of it as a protocol for how to make requests and communicate with another server. We have seen how to mine Twitter for getting text data and apply basic frequency based NLP techniques to get some insight. 

One of the key aspects of being a data scientist is the ability to learn how a new API works, how to go through its specific authentication process (OAuth) and how to process the data structures that get returned as a response to our requests. It is a good practice to spend some time learning the API through the official documentation before sending in requests. 

On these lines, this lab requires you to learn another popular API (YELP Fusion) by following the provided detailed online documentation. We shall build a simple Geographical Information System (GIS) using the data from yelp.

## Objectives
You will be able to: 
* Successfully sign up for Yelp API 
* Create HTTP requests to get data from Yelp API
* Parse HTTP responses and perform data analysis on the data returned
* Create a simple geographical system on to view information about selected businesses, at a given location. 


## The Yelp Fusion API - v3


### Point your browser over to this [yelp page](https://www.yelp.com/developers/v3/manage_app) and create an app in order to obtain  `client_id` and `api_key` tokens. 

**NOTE:** You will be required to sign up using Google or Facebook etc. if you dont already have an account.

<img src="yelp_app.png" width=500>



After registration, you'll be presented with your account information and limits of your access. For Yelp, or any other API for that reason, you need to make sure that you dont surpass your request quota, otherwise, you may end up getting banned in some cases. Yelp shows this information to you as below:

<img src="quota.png" width=500>

### Save your api_key and client_id in the variables below:


```python
# Save your tokens in the following string variables
client_id = ''
api_key = ''
```

## The `yelpapi` 

The yelpapi is a pure Python implementation of the Yelp Fusion API (aka Yelp v3 API). It is simple, fast, and robust to any changes Yelp may make to the API in the future. See tha basic usage of this library on the [official Github repo](https://github.com/gfairchild/yelpapi). You may look out for other APIs to achieve this but for this lesson, we shall use it for sake of simplicity. 

First you must pip install the library


```python
# !pip install yelpapi
```

### Import `yelpapi ` into working environment and pass in the api_key as shown in the Github Repo 


```python
# Code here
```

### The Api request and response

Great so we can now start making API calls using the format:
```python
response = yelp_api.search_query(term =<search term>, 
                                 location=<search location>, 
                                 sort_by='rating', 
                                 limit=50)
```
We can pass in a lot more arguments to refine our search. [Here is a complete list of options that search API provides us](https://www.yelp.com/developers/documentation/v3/business_search)

* Make an API request using a simple criteria location and term
* save the response as `response` 
* inspect the type and contents of `response`.


```python
## Pass in a spcific term and location to make a call. 

# For this example, we are looking for chinese food in London.

term = 'Chinese food'
location = 'London'
```


```python
# Make an API call using chosen term and location

response = None
type(response)
print(response)

```

### JSON .. again ! 

We have a nice nifty little return now! As you can see, the contents of the response is formatted as a string but what kind of data structures does this remind you of?  

To start there's the outer curly brackets:  

#### {"businesses":   

Hopefully you're thinking 'hey that's just like a python dictionary!'

Then within that we have what appears to be a list of dictionaries:  

#### {"id": "jeWIYbgBho9vBDhc5S1xvg",

This response is an example of a JSON (JavaScript Object Notation) format that we've seen so many times before. We can simply treat it as a dictionary and process it further. 

### Inspect the values for all the keys in the response


```python
# inspect the key value pairs to understand the strcuture of data 


```

Whoops, what's going on here!? Well, notice from our previous preview of the response that we saw there were a hierarhcy within the response. Let's begin to investigate further to see what the problem is.

First, recall that the overall strucutre of the response was a dictionary. Let's look at what the keys are:


```python
response.keys()
```

Consult the Yelp API and learn what value is carried in each key. 

#### Continue to preview these keys  further to get a little better acquainted. 


```python
print('BUSINESS:', response['businesses'][0])

print('REGION:', response['region'])

print('TOTAL :',response['total'])
```

This makes more sense, so we are mainly interested in the `businesses` for our needs. 

### Print the names of businesses and included ratings 


```python
# Code here 
```

Great, now are are getting somewhere. It is a good idea at this stage to store this information as a dataframe for processing further. 
### Create a Pandas dataframe for contents of `businesses`
* Check the number of records in the dataframe
* Inspect the columns and head


```python
# Code here 
```

This is fantastic. We have successfully learned a new API , made requests to it, recieved and studied the response and stored the results in a dataframe and can now enjoy all the goodness of Pandas. Thats quite a bit of data engineering. 

### Visualize the location from search query
The `region` key in the response carries the geographical information for the region searched.
* Get the latitude / longitude information from `region`
* Create a folium map with these coordinates. 
* Use a zoom start value = 13


```python
import folium

# Code here
```

Expected Output:
![](london.png)

Nice. We can now extract the coordinate information for each business and plot it on this map.

### Get the business coordinates from dataframe for each business and plot on the map above


```python
# Code here 
```

Expected output:
![](markers.png)

Kool so we have everything in place but the visualization is still not very *Informative* so to speak. You can't tell which marker represents which business and also other information on business like rating, cost, links to user reviews etc. is still not visible. SO its geographical , but not exactly an Information System yet as you cant make any decisions on this information.  Here's as example of what it possible can look like
![](out.png)


For this you need to understand `folium.popup()` which let's you click on a marker to show a pop up window. This window acts more like an HTML page so you can easily format the information you present in the popup using following values:
* The official business logo/image:  `image_url`
* Name of the Business: `name`
* Price (how expensive): `price`
* Links to user reviews on Yelp: `url`

Doing this in HTML is not required , so we recommend that you first try to put in basic information in the popups as just text. As a next stage , you can start changing into HTML code to make it visually more appealing.

### Attempt to recreate the interactive visualization shown above.

Here's a good resource with code examples on [how to create folium popups](https://github.com/python-visualization/folium/blob/master/examples/Popups.ipynb)


```python
# Code here 
```

Wow . An Interactive Geographical information System backed by live data through API calling. 

<img src="star.jpg" width=300>

### More APIs to Checkout

* Google Maps
* Twitter
* AWS
* IBM's Watson
* Yelp

## Summary 

In this lab, we learned how to use the Yelp API with authentication, making calls, understanding the responses and creating interactive geographical visualizations in Folium. We encourage you to re-visit this lab again once you have studied some important machine learning algorithms to make predictions , find similarities, group/cluster businesses or classify them based on user criteria. 
