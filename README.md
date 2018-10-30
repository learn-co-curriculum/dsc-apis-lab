
# Building a Simple GIS with Yelp API and Folium - Lab

## Introduction 

So we have learned quite a bit about APIs and how they are now big buzzword in the tech industry. Think of it as a protocol for how to make requests and communicate with another server. We have seen how to mine Twitter for getting text data and apply basic frequency based NLP techniques to get some insight. 

One of the key aspects of being a data scientist is the ability to learn how a new API works, how to go through its specific authentication process (OAuth) and how to process the data structures that get returned as a response to our requests. It is a good practice to spend some time learning the API through the official documentation before sending in requests. 

On these lines, this lab requires you to learn another popular API (YELP Fusion) by following the provided detailed online documentation. We shall build a simple Geographical Information System (GIS) using the data from yelp.

### Objectives
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

    {'businesses': [{'id': 'QE5aa5N-dbfvafLsjn7TVg', 'alias': 'bamboo-flute-fitzrovia', 'name': 'Bamboo Flute', 'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/kYQZjHmzt8zy1634OsAsxQ/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/bamboo-flute-fitzrovia?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 29, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}], 'rating': 4.5, 'coordinates': {'latitude': 51.5228746, 'longitude': -0.1418939}, 'transactions': [], 'price': '£', 'location': {'address1': '145 Cleveland Street', 'address2': '', 'address3': '', 'city': 'Fitzrovia', 'zip_code': 'W1T 6QH', 'country': 'GB', 'state': 'XGL', 'display_address': ['145 Cleveland Street', 'Fitzrovia W1T 6QH', 'United Kingdom']}, 'phone': '+442073872738', 'display_phone': '+44 20 7387 2738', 'distance': 1189.5082547156321}, {'id': 'h2cCoDNQOPd51HFwvuAGNg', 'alias': 'hakkasan-london-3', 'name': 'Hakkasan', 'image_url': 'https://s3-media2.fl.yelpcdn.com/bphoto/IcS9yzJnweDO4DsfJ94jAw/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/hakkasan-london-3?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 216, 'categories': [{'alias': 'cantonese', 'title': 'Cantonese'}], 'rating': 4.0, 'coordinates': {'latitude': 51.5171482803943, 'longitude': -0.13180578932657}, 'transactions': [], 'price': '££££', 'location': {'address1': '8 Hanway Place', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'W1T 1HD', 'country': 'GB', 'state': 'XGL', 'display_address': ['8 Hanway Place', 'London W1T 1HD', 'United Kingdom']}, 'phone': '+442079277000', 'display_phone': '+44 20 7927 7000', 'distance': 558.9650039747677}, {'id': 'sYwBQ7mJYhB35nn-_SZstQ', 'alias': 'yauatcha-london-7', 'name': 'Yauatcha', 'image_url': 'https://s3-media4.fl.yelpcdn.com/bphoto/K7D6nwyZbZFODMy4jEfr0A/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/yauatcha-london-7?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 456, 'categories': [{'alias': 'dimsum', 'title': 'Dim Sum'}, {'alias': 'seafood', 'title': 'Seafood'}, {'alias': 'noodles', 'title': 'Noodles'}], 'rating': 4.0, 'coordinates': {'latitude': 51.5137076071076, 'longitude': -0.135128831605189}, 'transactions': [], 'price': '£££', 'location': {'address1': '15-17 Broadwick Street', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'W1F 0DL', 'country': 'GB', 'state': 'XGL', 'display_address': ['15-17 Broadwick Street', 'London W1F 0DL', 'United Kingdom']}, 'phone': '+442074948888', 'display_phone': '+44 20 7494 8888', 'distance': 112.59840998419494}, {'id': 'pdFiFtol9YI__9ROOXUIYA', 'alias': 'lanzhou-noodle-bar-london', 'name': 'Lanzhou Noodle Bar', 'image_url': 'https://s3-media4.fl.yelpcdn.com/bphoto/IKy91e7hb9w14m4eA9HH1w/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/lanzhou-noodle-bar-london?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 300, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}, {'alias': 'noodles', 'title': 'Noodles'}], 'rating': 4.0, 'coordinates': {'latitude': 51.5116034713013, 'longitude': -0.127834377873126}, 'transactions': [], 'price': '£', 'location': {'address1': '33 Cranbourne Street', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'WC2H 7AD', 'country': 'GB', 'state': 'XGL', 'display_address': ['33 Cranbourne Street', 'London WC2H 7AD', 'United Kingdom']}, 'phone': '+442074674546', 'display_phone': '+44 20 7467 4546', 'distance': 578.4743547816091}, {'id': 'chEEcQbc8PbidTeXK34H9g', 'alias': 'hakkasan-mayfair-london-2', 'name': 'Hakkasan Mayfair', 'image_url': 'https://s3-media4.fl.yelpcdn.com/bphoto/7l0tXtBLbWOtWTCQVJLLYw/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/hakkasan-mayfair-london-2?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 116, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}], 'rating': 4.0, 'coordinates': {'latitude': 51.5103202323384, 'longitude': -0.144935268434918}, 'transactions': [], 'price': '££££', 'location': {'address1': '17 Bruton Street', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'W1J 6QB', 'country': 'GB', 'state': 'XGL', 'display_address': ['17 Bruton Street', 'London W1J 6QB', 'United Kingdom']}, 'phone': '+442079071888', 'display_phone': '+44 20 7907 1888', 'distance': 681.4450716267161}, {'id': '6iCxFP7Oc0qG_6ByenO0sw', 'alias': 'silk-road-london-2', 'name': 'Silk Road', 'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/pUXusTIbeDRInvi6fkNjmA/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/silk-road-london-2?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 43, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}], 'rating': 4.5, 'coordinates': {'latitude': 51.4739712, 'longitude': -0.0896081}, 'transactions': [], 'price': '££', 'location': {'address1': '49 Camberwell Church Street', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'SE5 8TR', 'country': 'GB', 'state': 'XGL', 'display_address': ['49 Camberwell Church Street', 'London SE5 8TR', 'United Kingdom']}, 'phone': '+442077034832', 'display_phone': '+44 20 7703 4832', 'distance': 5403.445838977789}, {'id': 'Mrv3dA42EioqBz50t_f24Q', 'alias': 'hunan-london', 'name': 'Hunan', 'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/sEgrRrMVkWaky7Yc7dvdgg/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/hunan-london?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 38, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}], 'rating': 4.5, 'coordinates': {'latitude': 51.490493, 'longitude': -0.1526547}, 'transactions': [], 'price': '££££', 'location': {'address1': '51 Pimlico Road', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'SW1W 8NE', 'country': 'GB', 'state': 'XGL', 'display_address': ['51 Pimlico Road', 'London SW1W 8NE', 'United Kingdom']}, 'phone': '+442077305712', 'display_phone': '+44 20 7730 5712', 'distance': 2741.6022690860036}, {'id': 'MpdG-D0nKwGEZp0ApTtBaA', 'alias': 'barshu-london-2', 'name': 'Barshu', 'image_url': 'https://s3-media2.fl.yelpcdn.com/bphoto/jaGznOhEvqqcJXKCYt12Ug/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/barshu-london-2?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 96, 'categories': [{'alias': 'szechuan', 'title': 'Szechuan'}], 'rating': 4.0, 'coordinates': {'latitude': 51.512835, 'longitude': -0.130857}, 'transactions': [], 'price': '£££', 'location': {'address1': '28 Frith Street', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'W1D 5LF', 'country': 'GB', 'state': 'XGL', 'display_address': ['28 Frith Street', 'London W1D 5LF', 'United Kingdom']}, 'phone': '+442072878822', 'display_phone': '+44 20 7287 8822', 'distance': 357.1885184880618}, {'id': '5-_YNjtvoG-oYt1qAO3tVw', 'alias': 'bugis-street-brasserie-london', 'name': 'Bugis Street Brasserie', 'image_url': 'https://s3-media1.fl.yelpcdn.com/bphoto/kgtAwZ3oF2wh05ndSzQ1zA/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/bugis-street-brasserie-london?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 43, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}, {'alias': 'singaporean', 'title': 'Singaporean'}], 'rating': 4.0, 'coordinates': {'latitude': 51.4936621211568, 'longitude': -0.183931280273902}, 'transactions': [], 'price': '££', 'location': {'address1': 'Millennium Hotels & Resorts', 'address2': '4-18 Harrington Gardens', 'address3': '', 'city': 'London', 'zip_code': 'SW7 4LH', 'country': 'GB', 'state': 'XGL', 'display_address': ['Millennium Hotels & Resorts', '4-18 Harrington Gardens', 'London SW7 4LH', 'United Kingdom']}, 'phone': '+442073316211', 'display_phone': '+44 20 7331 6211', 'distance': 3946.1405823998143}, {'id': '3ea8x8r0gsVy8ltQycldsA', 'alias': 'gold-mine-london', 'name': 'Gold Mine', 'image_url': 'https://s3-media2.fl.yelpcdn.com/bphoto/aihm1QdPS5Mq79T_9rP7tA/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/gold-mine-london?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 66, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}], 'rating': 4.0, 'coordinates': {'latitude': 51.5133018493652, 'longitude': -0.187491998076439}, 'transactions': [], 'price': '££', 'location': {'address1': '102 Queensway', 'address2': '', 'address3': '', 'city': 'London', 'zip_code': 'W2 3RR', 'country': 'GB', 'state': 'XGL', 'display_address': ['102 Queensway', 'London W2 3RR', 'United Kingdom']}, 'phone': '+442077928331', 'display_phone': '+44 20 7792 8331', 'distance': 3571.4713192257427}], 'total': 1200, 'region': {'center': {'longitude': -0.135955810546875, 'latitude': 51.51283552118349}}}


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

    businesses
    <class 'dict_values'>
    total
    <class 'dict_values'>
    region
    <class 'dict_values'>


Whoops, what's going on here!? Well, notice from our previous preview of the response that we saw there were a hierarhcy within the response. Let's begin to investigate further to see what the problem is.

First, recall that the overall strucutre of the response was a dictionary. Let's look at what the keys are:


```python
response.keys()
```




    dict_keys(['businesses', 'total', 'region'])



Consult the Yelp API and learn what value is carried in each key. 

#### Continue to preview these keys  further to get a little better acquainted. 


```python
print('BUSINESS:', response['businesses'][0])

print('REGION:', response['region'])

print('TOTAL :',response['total'])
```

    BUSINESS: {'id': 'QE5aa5N-dbfvafLsjn7TVg', 'alias': 'bamboo-flute-fitzrovia', 'name': 'Bamboo Flute', 'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/kYQZjHmzt8zy1634OsAsxQ/o.jpg', 'is_closed': False, 'url': 'https://www.yelp.com/biz/bamboo-flute-fitzrovia?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw', 'review_count': 29, 'categories': [{'alias': 'chinese', 'title': 'Chinese'}], 'rating': 4.5, 'coordinates': {'latitude': 51.5228746, 'longitude': -0.1418939}, 'transactions': [], 'price': '£', 'location': {'address1': '145 Cleveland Street', 'address2': '', 'address3': '', 'city': 'Fitzrovia', 'zip_code': 'W1T 6QH', 'country': 'GB', 'state': 'XGL', 'display_address': ['145 Cleveland Street', 'Fitzrovia W1T 6QH', 'United Kingdom']}, 'phone': '+442073872738', 'display_phone': '+44 20 7387 2738', 'distance': 1189.5082547156321}
    REGION: {'center': {'longitude': -0.135955810546875, 'latitude': 51.51283552118349}}
    TOTAL : 1200


This makes more sense, so we are mainly interested in the `businesses` for our needs. 

### Print the names of businesses and included ratings 


```python
# Code here 
```

    bamboo-flute-fitzrovia - 4.5
    hakkasan-london-3 - 4.0
    yauatcha-london-7 - 4.0
    lanzhou-noodle-bar-london - 4.0
    hakkasan-mayfair-london-2 - 4.0
    silk-road-london-2 - 4.5
    hunan-london - 4.5
    barshu-london-2 - 4.0
    bugis-street-brasserie-london - 4.0
    gold-mine-london - 4.0


Great, now are are getting somewhere. It is a good idea at this stage to store this information as a dataframe for processing further. 
### Create a Pandas dataframe for contents of `businesses`
* Check the number of records in the dataframe
* Inspect the columns and head


```python
# Code here 
```

    10
    Index(['alias', 'categories', 'coordinates', 'display_phone', 'distance', 'id',
           'image_url', 'is_closed', 'location', 'name', 'phone', 'price',
           'rating', 'review_count', 'transactions', 'url'],
          dtype='object')





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>alias</th>
      <th>categories</th>
      <th>coordinates</th>
      <th>display_phone</th>
      <th>distance</th>
      <th>id</th>
      <th>image_url</th>
      <th>is_closed</th>
      <th>location</th>
      <th>name</th>
      <th>phone</th>
      <th>price</th>
      <th>rating</th>
      <th>review_count</th>
      <th>transactions</th>
      <th>url</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>bamboo-flute-fitzrovia</td>
      <td>[{'alias': 'chinese', 'title': 'Chinese'}]</td>
      <td>{'latitude': 51.5228746, 'longitude': -0.1418939}</td>
      <td>+44 20 7387 2738</td>
      <td>1189.508255</td>
      <td>QE5aa5N-dbfvafLsjn7TVg</td>
      <td>https://s3-media3.fl.yelpcdn.com/bphoto/kYQZjH...</td>
      <td>False</td>
      <td>{'address1': '145 Cleveland Street', 'address2...</td>
      <td>Bamboo Flute</td>
      <td>+442073872738</td>
      <td>£</td>
      <td>4.5</td>
      <td>29</td>
      <td>[]</td>
      <td>https://www.yelp.com/biz/bamboo-flute-fitzrovi...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hakkasan-london-3</td>
      <td>[{'alias': 'cantonese', 'title': 'Cantonese'}]</td>
      <td>{'latitude': 51.5171482803943, 'longitude': -0...</td>
      <td>+44 20 7927 7000</td>
      <td>558.965004</td>
      <td>h2cCoDNQOPd51HFwvuAGNg</td>
      <td>https://s3-media2.fl.yelpcdn.com/bphoto/IcS9yz...</td>
      <td>False</td>
      <td>{'address1': '8 Hanway Place', 'address2': '',...</td>
      <td>Hakkasan</td>
      <td>+442079277000</td>
      <td>££££</td>
      <td>4.0</td>
      <td>216</td>
      <td>[]</td>
      <td>https://www.yelp.com/biz/hakkasan-london-3?adj...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>yauatcha-london-7</td>
      <td>[{'alias': 'dimsum', 'title': 'Dim Sum'}, {'al...</td>
      <td>{'latitude': 51.5137076071076, 'longitude': -0...</td>
      <td>+44 20 7494 8888</td>
      <td>112.598410</td>
      <td>sYwBQ7mJYhB35nn-_SZstQ</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/K7D6nw...</td>
      <td>False</td>
      <td>{'address1': '15-17 Broadwick Street', 'addres...</td>
      <td>Yauatcha</td>
      <td>+442074948888</td>
      <td>£££</td>
      <td>4.0</td>
      <td>456</td>
      <td>[]</td>
      <td>https://www.yelp.com/biz/yauatcha-london-7?adj...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>lanzhou-noodle-bar-london</td>
      <td>[{'alias': 'chinese', 'title': 'Chinese'}, {'a...</td>
      <td>{'latitude': 51.5116034713013, 'longitude': -0...</td>
      <td>+44 20 7467 4546</td>
      <td>578.474355</td>
      <td>pdFiFtol9YI__9ROOXUIYA</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/IKy91e...</td>
      <td>False</td>
      <td>{'address1': '33 Cranbourne Street', 'address2...</td>
      <td>Lanzhou Noodle Bar</td>
      <td>+442074674546</td>
      <td>£</td>
      <td>4.0</td>
      <td>300</td>
      <td>[]</td>
      <td>https://www.yelp.com/biz/lanzhou-noodle-bar-lo...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>hakkasan-mayfair-london-2</td>
      <td>[{'alias': 'chinese', 'title': 'Chinese'}]</td>
      <td>{'latitude': 51.5103202323384, 'longitude': -0...</td>
      <td>+44 20 7907 1888</td>
      <td>681.445072</td>
      <td>chEEcQbc8PbidTeXK34H9g</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/7l0tXt...</td>
      <td>False</td>
      <td>{'address1': '17 Bruton Street', 'address2': '...</td>
      <td>Hakkasan Mayfair</td>
      <td>+442079071888</td>
      <td>££££</td>
      <td>4.0</td>
      <td>116</td>
      <td>[]</td>
      <td>https://www.yelp.com/biz/hakkasan-mayfair-lond...</td>
    </tr>
  </tbody>
</table>
</div>



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
