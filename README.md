
# API Connections, Dataframes + Pandas

### Objectives

* Explain an HTTP Request
* Explain OAuth
* Use the requests package to make HTTP Get Requests
* Use the requests to make HTTP Get Requests with headers and url parameters
* Create a DataFrame using the Pandas Package
* Use basic Pandas methods such as:
    * df.head() / df.tail()
    * df[col].plot(kind = 'barh')
* Visualize Results Using Folium

### Introduction  

APIs are a big buzzword in the tech industry. So what is an API you ask? API stands for Application Program Interface. Think of it as a protocol for how to make requests and communicate with another server.

But before we get to APIs, we should have a general understanding of how HTTP requests work. Often, we just type in a website domain into the url bar and hit go. Sometimes we don't even do that, we just google it and click the link. A lot is happening in the background. Let's explore this process a little further.  

#### HTTP Requests
HTTP stands for Hyper Text Transfer Protocol. This protocol (like many) was proposed by the Internet Engineering Task Force (IETF) through a request for comments (RFC). We're going to start with a very simple HTTP method: the get method.  

![](./images/http_requests.png)

To learn more about HTTP methods see:  
https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods

#### Python's Requests Package

The first thing to understand when dealing with APIs is how to make get requests in general.
To do this, we'll use the Python requests package.

http://docs.python-requests.org/en/master/

![](./images/requests_homepage.png)

### Getting Started


```python
import requests
```

##### Make a get request:


```python
response = requests.get('https://flatironschool.com')
print('Type:', type(response), '\n')
print('Response:', response, '\n')
print('Response text:\n', response.text)
```

    Type: <class 'requests.models.Response'> 
    
    Response: <Response [403]> 
    
    Response text:
     <html>
    <head><title>403 Forbidden</title></head>
    <body bgcolor="white">
    <center><h1>403 Forbidden</h1></center>
    <hr><center>nginx</center>
    </body>
    </html>
    


Hmmm, well that was only partially helpful. You can see that our request was denied. (This is shown by the response itself, which has the code 403, meaning forbidden.) Most likely, this is caused by permissioning from Flatiron School's servers, which may be blocking requests that appear to be from an automated platform.

In general, here's some common HTTP response codes you might come across:
![](./images/http_response_codes.gif)

Let's try another get request in the hopes of getting a successful (200) response.


```python
#The Electronic Frontier Foundation (EFF) website; advocating for data privacy and an open internet
response = requests.get('https://www.eff.org')
print(response)
print(response.text[:2500])
```

    <Response [200]>
    <!DOCTYPE html>
      <!--[if IEMobile 7]><html class="no-js ie iem7" lang="en" dir="ltr"><![endif]-->
      <!--[if lte IE 6]><html class="no-js ie lt-ie9 lt-ie8 lt-ie7" lang="en" dir="ltr"><![endif]-->
      <!--[if (IE 7)&(!IEMobile)]><html class="no-js ie lt-ie9 lt-ie8" lang="en" dir="ltr"><![endif]-->
      <!--[if IE 8]><html class="no-js ie lt-ie9" lang="en" dir="ltr"><![endif]-->
      <!--[if (gte IE 9)|(gt IEMobile 7)]><html class="no-js ie" lang="en" dir="ltr" prefix="fb: http://ogp.me/ns/fb# og: http://ogp.me/ns# article: http://ogp.me/ns/article# book: http://ogp.me/ns/book# profile: http://ogp.me/ns/profile# video: http://ogp.me/ns/video# product: http://ogp.me/ns/product#"><![endif]-->
      <!--[if !IE]><!--><html class="no-js" lang="en" dir="ltr" prefix="fb: http://ogp.me/ns/fb# og: http://ogp.me/ns# article: http://ogp.me/ns/article# book: http://ogp.me/ns/book# profile: http://ogp.me/ns/profile# video: http://ogp.me/ns/video# product: http://ogp.me/ns/product#"><!--<![endif]-->
    <head>
      <meta charset="utf-8" />
    <link href="https://www.eff.org/vi" rel="alternate" hreflang="vi" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed-114x114.png" sizes="114x114" />
    <link href="https://www.eff.org/ur" rel="alternate" hreflang="ur" />
    <link href="https://www.eff.org/tr" rel="alternate" hreflang="tr" />
    <link href="https://www.eff.org/sh" rel="alternate" hreflang="sh" />
    <link href="https://www.eff.org/sv" rel="alternate" hreflang="sv" />
    <link href="https://www.eff.org/th" rel="alternate" hreflang="th" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed-72x72.png" sizes="72x72" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed-144x144.png" sizes="144x144" />
    <link rel="profile" href="http://www.w3.org/1999/xhtml/vocab" />
    <link rel="shortcut icon" href="https://www.eff.org/sites/all/themes/frontier/favicon.ico" type="image/vnd.microsoft.icon" />
    <meta name="HandheldFriendly" content="true" />
    <meta name="MobileOptimized" content="width" />
    <link rel="apple-touch-icon-precomposed" href="https://www.eff.org/sites/all/themes/phoenix/apple-touch-icon-precomposed.png" />
    <meta http-equiv="cleartype" content="on" />
    <link href="https://www.eff.org/ru" rel="alternate" hreflang="ru" />
    <link href="https://www.eff.org/es" rel="alternate" hreflang="es" />
    <link href


Success! As you can see, the response.text is the html code for the given url that we requested. In the background, this forms the basis for web browsers themselves. Every time you put in a new url or click on a link your computer makes a get request for that particular page and then the browser itself renders that page into a visual display on screen.

### OAuth  
Some requests are a bit more complicated. Often, websites require identity verification such as logins. This helps a variety of issues such as privacy concerns, limiting access to content and tracking users history. Going forward, OAuth has furthered this idea by allowing third parties such as apps access to user information without providing the underlying password itself.

In the words of the Internet Engineering Task Force, "The OAuth 2.0 authorization framework enables a third-party
application to obtain limited access to an HTTP service, either on
behalf of a resource owner by orchestrating an approval interaction
between the resource owner and the HTTP service, or by allowing the
third-party application to obtain access on its own behalf.  This
specification replaces and obsoletes the OAuth 1.0 protocol described
in RFC 5849."

See https://oauth.net/2/ or https://tools.ietf.org/html/rfc6749 for more details.

Alternatively, for a specific case check out [Yelp's authentication guide](https://www.yelp.com/developers/documentation/v3/authentication#where-is-my-client-secret-going), which we're about to check out!

#### Access Tokens
With that, lets go grab an access token from an API site and make some API calls!
Point your browser over to this [yelp page](https://www.yelp.com/developers/v3/manage_app) and start creating an app in order to obtain and api access token:

![](./images/yelp_app.png)

Now it's time to start making some api calls!


```python
#As a general rule of thumb, don't store passwords in a main file like this!
#Instead, you would normally store those passwords under a sub file like passwords.py which you would then import.
#This code snippet might not work if repeatedly used.
client_id = 'xNHtXRpNa-MXGFJJTHHUvw'
api_key = 'bIufucVD4DflHdSAhnJF7wJuBmJQAsnOdlKX2XgNZQkGZl8TcyFrGy0dtmBstVBVezqv-myfGeY7a6GZG4KQNOLGdD8YFVkTSLW0zfFc0MkiHCcyHc7urlbf_uMiW3Yx'
```

#### Example Request
https://www.yelp.com/developers/documentation/v3/get_started


```python
term = 'Mexican'
location = 'Astoria NY'
SEARCH_LIMIT = 10

url = 'https://api.yelp.com/v3/businesses/search'

headers = {
        'Authorization': 'Bearer {}'.format(api_key),
    }

url_params = {
                'term': term.replace(' ', '+'),
                'location': location.replace(' ', '+'),
                'limit': SEARCH_LIMIT
            }
response = requests.get(url, headers=headers, params=url_params)
print(response)
print(type(response.text))
print(response.text[:1000])
```

    <Response [200]>
    <class 'str'>
    {"businesses": [{"id": "jeWIYbgBho9vBDhc5S1xvg", "alias": "holy-guacamole-astoria", "name": "Holy Guacamole", "image_url": "https://s3-media1.fl.yelpcdn.com/bphoto/8IjT2jd7vKDSOmtdXPI-Zg/o.jpg", "is_closed": false, "url": "https://www.yelp.com/biz/holy-guacamole-astoria?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw", "review_count": 108, "categories": [{"alias": "mexican", "title": "Mexican"}, {"alias": "bars", "title": "Bars"}], "rating": 4.0, "coordinates": {"latitude": 40.756621, "longitude": -73.929336}, "transactions": ["delivery", "pickup"], "price": "$$", "location": {"address1": "3555 31st St", "address2": "", "address3": "", "city": "Astoria", "zip_code": "11106", "country": "US", "state": "NY", "display_address": ["3555 31st St", "Astoria, NY 11106"]}, "phone": "+19178327261", "display_phone": "(917) 832-7261", "distance": 1290.4274875130448}, {"id": "6AJwsgXr7YwsqneGVAdgzw", "alias": "las-c


#### JSON
MMMM Look at that! We have a nice nifty little return now! As you can see, the contents of the response is formatted as a string but what kind of data structures does this remind you of?  

To start there's the outer curly brackets:  
{"businesses":   

Hopefully you're thinking 'hey that's just like a python dictionary!'

Then within that we have what appears to be a list of dictionaries:  
[{"id": "jeWIYbgBho9vBDhc5S1xvg",

This response is an example of a json (Javascript Object Notation) format.
You can read more about json [here](https://www.json.org/), but it's pretty similar to the data structures you've already seen in python.


## DataFrames and Pandas
We can also take json and convert it into a **DataFrame**, a spreadsheet style object (ala excel), using the **Pandas** package: 


```python
#import the package under an alias (short typing in the future)
import pandas as pd
```


```python
#Create a dataframe
df = pd.DataFrame.from_dict(response.json())
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-36-3fd8d17ec009> in <module>()
          1 #Create a dataframe
    ----> 2 df = pd.DataFrame.from_dict(response.json())
    

    ~/anaconda3/lib/python3.6/site-packages/pandas/core/frame.py in from_dict(cls, data, orient, dtype, columns)
        983             raise ValueError('only recognize index or columns for orient')
        984 
    --> 985         return cls(data, index=index, columns=columns, dtype=dtype)
        986 
        987     def to_dict(self, orient='dict', into=dict):


    ~/anaconda3/lib/python3.6/site-packages/pandas/core/frame.py in __init__(self, data, index, columns, dtype, copy)
        346                                  dtype=dtype, copy=copy)
        347         elif isinstance(data, dict):
    --> 348             mgr = self._init_dict(data, index, columns, dtype=dtype)
        349         elif isinstance(data, ma.MaskedArray):
        350             import numpy.ma.mrecords as mrecords


    ~/anaconda3/lib/python3.6/site-packages/pandas/core/frame.py in _init_dict(self, data, index, columns, dtype)
        457             arrays = [data[k] for k in keys]
        458 
    --> 459         return _arrays_to_mgr(arrays, data_names, index, columns, dtype=dtype)
        460 
        461     def _init_ndarray(self, values, index, columns, dtype=None, copy=False):


    ~/anaconda3/lib/python3.6/site-packages/pandas/core/frame.py in _arrays_to_mgr(arrays, arr_names, index, columns, dtype)
       7313     # figure out the index, if necessary
       7314     if index is None:
    -> 7315         index = extract_index(arrays)
       7316 
       7317     # don't force copy because getting jammed in an ndarray anyway


    ~/anaconda3/lib/python3.6/site-packages/pandas/core/frame.py in extract_index(data)
       7362 
       7363             if have_dicts:
    -> 7364                 raise ValueError('Mixing dicts with non-Series may lead to '
       7365                                  'ambiguous ordering.')
       7366 


    ValueError: Mixing dicts with non-Series may lead to ambiguous ordering.


### Debugging
Whoops, what's going on here!? Well, notice from our previous preview of the response that we saw there were a hierarhcy within the response. Let's begin to investigate further to see what the problem is.

First, recall that the overall strucutre of the response was a dictionary. Let's look at what those values are.


```python
response.json().keys()
```




    dict_keys(['businesses', 'total', 'region'])



Now let's go a bit further and start to preview what's stored in each of the values for these keys.


```python
for key in response.json().keys():
    print(key)
    value = response.json()[key] #Use standard dictionary formatting
    print(type(value)) #What type is it?
    print('\n\n') #Seperate out data
```

    businesses
    <class 'list'>
    
    
    
    total
    <class 'int'>
    
    
    
    region
    <class 'dict'>
    
    
    


##### Let's continue to preview these further to get a little better acquainted.


```python
response.json()['businesses'][:2]
```




    [{'id': 'jeWIYbgBho9vBDhc5S1xvg',
      'alias': 'holy-guacamole-astoria',
      'name': 'Holy Guacamole',
      'image_url': 'https://s3-media1.fl.yelpcdn.com/bphoto/8IjT2jd7vKDSOmtdXPI-Zg/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/holy-guacamole-astoria?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw',
      'review_count': 108,
      'categories': [{'alias': 'mexican', 'title': 'Mexican'},
       {'alias': 'bars', 'title': 'Bars'}],
      'rating': 4.0,
      'coordinates': {'latitude': 40.756621, 'longitude': -73.929336},
      'transactions': ['delivery', 'pickup'],
      'price': '$$',
      'location': {'address1': '3555 31st St',
       'address2': '',
       'address3': '',
       'city': 'Astoria',
       'zip_code': '11106',
       'country': 'US',
       'state': 'NY',
       'display_address': ['3555 31st St', 'Astoria, NY 11106']},
      'phone': '+19178327261',
      'display_phone': '(917) 832-7261',
      'distance': 1290.4274875130448},
     {'id': '6AJwsgXr7YwsqneGVAdgzw',
      'alias': 'las-catrinas-mexican-bar-and-eatery-astoria',
      'name': 'Las Catrinas Mexican Bar & Eatery',
      'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/CKRiZUoyTUjs79bWnDEEpg/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/las-catrinas-mexican-bar-and-eatery-astoria?adjust_creative=xNHtXRpNa-MXGFJJTHHUvw&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=xNHtXRpNa-MXGFJJTHHUvw',
      'review_count': 163,
      'categories': [{'alias': 'mexican', 'title': 'Mexican'},
       {'alias': 'cocktailbars', 'title': 'Cocktail Bars'}],
      'rating': 4.0,
      'coordinates': {'latitude': 40.7614214682633,
       'longitude': -73.9246649456171},
      'transactions': ['delivery', 'pickup'],
      'price': '$$',
      'location': {'address1': '32-02 Broadway',
       'address2': '',
       'address3': None,
       'city': 'Astoria',
       'zip_code': '11106',
       'country': 'US',
       'state': 'NY',
       'display_address': ['32-02 Broadway', 'Astoria, NY 11106']},
      'phone': '+19177450969',
      'display_phone': '(917) 745-0969',
      'distance': 642.5257707161409}]




```python
response.json()['total']
```




    638




```python
response.json()['region']
```




    {'center': {'longitude': -73.92219543457031, 'latitude': 40.76688875374591}}



**As you can see, we're primarily interested in the 'bussinesses' entry. **   

**Let's go ahead and create a dataframe from that.**


```python
df = pd.DataFrame.from_dict(response.json()['businesses'])
print(len(df)) #Print how many rows
print(df.columns) #Print column names
df.head() #Previews the first five rows. 
#You could also write df.head(10) to preview 10 rows or df.tail() to see the bottom
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
      <td>holy-guacamole-astoria</td>
      <td>[{'alias': 'mexican', 'title': 'Mexican'}, {'a...</td>
      <td>{'latitude': 40.756621, 'longitude': -73.929336}</td>
      <td>(917) 832-7261</td>
      <td>1290.427488</td>
      <td>jeWIYbgBho9vBDhc5S1xvg</td>
      <td>https://s3-media1.fl.yelpcdn.com/bphoto/8IjT2j...</td>
      <td>False</td>
      <td>{'address1': '3555 31st St', 'address2': '', '...</td>
      <td>Holy Guacamole</td>
      <td>+19178327261</td>
      <td>$$</td>
      <td>4.0</td>
      <td>108</td>
      <td>[delivery, pickup]</td>
      <td>https://www.yelp.com/biz/holy-guacamole-astori...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>las-catrinas-mexican-bar-and-eatery-astoria</td>
      <td>[{'alias': 'mexican', 'title': 'Mexican'}, {'a...</td>
      <td>{'latitude': 40.7614214682633, 'longitude': -7...</td>
      <td>(917) 745-0969</td>
      <td>642.525771</td>
      <td>6AJwsgXr7YwsqneGVAdgzw</td>
      <td>https://s3-media3.fl.yelpcdn.com/bphoto/CKRiZU...</td>
      <td>False</td>
      <td>{'address1': '32-02 Broadway', 'address2': '',...</td>
      <td>Las Catrinas Mexican Bar &amp; Eatery</td>
      <td>+19177450969</td>
      <td>$$</td>
      <td>4.0</td>
      <td>163</td>
      <td>[delivery, pickup]</td>
      <td>https://www.yelp.com/biz/las-catrinas-mexican-...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>chela-and-garnacha-astoria</td>
      <td>[{'alias': 'mexican', 'title': 'Mexican'}, {'a...</td>
      <td>{'latitude': 40.7557171543477, 'longitude': -7...</td>
      <td>(917) 832-6876</td>
      <td>1316.297661</td>
      <td>AUyKmFjpaVLwc3awfUnqgQ</td>
      <td>https://s3-media1.fl.yelpcdn.com/bphoto/ChVbA1...</td>
      <td>False</td>
      <td>{'address1': '33-09 36th Ave', 'address2': '',...</td>
      <td>Chela &amp; Garnacha</td>
      <td>+19178326876</td>
      <td>$$</td>
      <td>4.5</td>
      <td>288</td>
      <td>[delivery, pickup]</td>
      <td>https://www.yelp.com/biz/chela-and-garnacha-as...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>de-mole-astoria-astoria</td>
      <td>[{'alias': 'mexican', 'title': 'Mexican'}]</td>
      <td>{'latitude': 40.7625999, 'longitude': -73.9129...</td>
      <td>(718) 777-1655</td>
      <td>917.683267</td>
      <td>jzVv_21473lAMYXIhVbuTA</td>
      <td>https://s3-media1.fl.yelpcdn.com/bphoto/v8jXvZ...</td>
      <td>False</td>
      <td>{'address1': '4220 30th Ave', 'address2': '', ...</td>
      <td>De Mole Astoria</td>
      <td>+17187771655</td>
      <td>$$</td>
      <td>4.0</td>
      <td>314</td>
      <td>[delivery, pickup]</td>
      <td>https://www.yelp.com/biz/de-mole-astoria-astor...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>maizal-restaurant-and-tequila-bar-astoria-2</td>
      <td>[{'alias': 'mexican', 'title': 'Mexican'}, {'a...</td>
      <td>{'latitude': 40.759331, 'longitude': -73.926035}</td>
      <td>(718) 406-9431</td>
      <td>900.451091</td>
      <td>QIsFsiOP3H_NkgeWST7GPA</td>
      <td>https://s3-media4.fl.yelpcdn.com/bphoto/VOGwDm...</td>
      <td>False</td>
      <td>{'address1': '3207 34th Ave', 'address2': None...</td>
      <td>Maizal Restaurant &amp; Tequila Bar</td>
      <td>+17184069431</td>
      <td>$$</td>
      <td>4.0</td>
      <td>257</td>
      <td>[delivery, pickup]</td>
      <td>https://www.yelp.com/biz/maizal-restaurant-and...</td>
    </tr>
  </tbody>
</table>
</div>



https://www.yelp.com/developers/documentation/v3/get_started
https://developers.google.com/maps/documentation/
    
And for a more complete code snippet check out:
https://github.com/Yelp/yelp-fusion/blob/master/fusion/python/sample.py

#### Visualization


```python
import folium
```


```python
#Retrieve the Latitude and Longitude from the Yelp Response
lat_long = response.json()['region']['center']
lat = lat_long['latitude']
long = lat_long['longitude']

#Create a map of the area
yelp_map = folium.Map([lat, long])
yelp_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfOTMwZTc5MjcxNDcwNGY1ZGJkOTYzMmZkZjA1ZTZiNmEgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzkzMGU3OTI3MTQ3MDRmNWRiZDk2MzJmZGYwNWU2YjZhIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YSA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YScsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDAuNzY2ODg4NzUzNzQ1OTEsLTczLjkyMjE5NTQzNDU3MDMxXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHpvb206IDEwLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbWF4Qm91bmRzOiBib3VuZHMsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBsYXllcnM6IFtdLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgd29ybGRDb3B5SnVtcDogZmFsc2UsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3CiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0pOwogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl84OThiOWEwMzhlNDk0YzczOTA1OWNhY2U2YWQwNjQwYiA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgJ2h0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nJywKICAgICAgICAgICAgICAgIHsKICAiYXR0cmlidXRpb24iOiBudWxsLAogICJkZXRlY3RSZXRpbmEiOiBmYWxzZSwKICAibWF4Wm9vbSI6IDE4LAogICJtaW5ab29tIjogMSwKICAibm9XcmFwIjogZmFsc2UsCiAgInN1YmRvbWFpbnMiOiAiYWJjIgp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YSk7CiAgICAgICAgCjwvc2NyaXB0Pg==" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



#### Adding the Mexican Restaurants


```python
folium.Marker?
```


```python
for row in df.index:
    lat_long = df['coordinates'][row]
    lat = lat_long['latitude']
    long = lat_long['longitude']
    name = df['name'][row]
    rating = df['rating'][row]
    price = df['price'][row]
    details = '{} Price: {} Rating:{}'.format(name,price,rating)
    marker = folium.Marker([lat, long])
    marker.add_to(yelp_map)
yelp_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><iframe src="data:text/html;charset=utf-8;base64,PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgPHNjcmlwdD5MX1BSRUZFUl9DQU5WQVMgPSBmYWxzZTsgTF9OT19UT1VDSCA9IGZhbHNlOyBMX0RJU0FCTEVfM0QgPSBmYWxzZTs8L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2FqYXguZ29vZ2xlYXBpcy5jb20vYWpheC9saWJzL2pxdWVyeS8xLjExLjEvanF1ZXJ5Lm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvanMvYm9vdHN0cmFwLm1pbi5qcyI+PC9zY3JpcHQ+CiAgICA8c2NyaXB0IHNyYz0iaHR0cHM6Ly9jZG5qcy5jbG91ZGZsYXJlLmNvbS9hamF4L2xpYnMvTGVhZmxldC5hd2Vzb21lLW1hcmtlcnMvMi4wLjIvbGVhZmxldC5hd2Vzb21lLW1hcmtlcnMuanMiPjwvc2NyaXB0PgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2Nkbi5qc2RlbGl2ci5uZXQvbnBtL2xlYWZsZXRAMS4yLjAvZGlzdC9sZWFmbGV0LmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9ib290c3RyYXAvMy4yLjAvY3NzL2Jvb3RzdHJhcC10aGVtZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vZm9udC1hd2Vzb21lLzQuNi4zL2Nzcy9mb250LWF3ZXNvbWUubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9yYXdnaXQuY29tL3B5dGhvbi12aXN1YWxpemF0aW9uL2ZvbGl1bS9tYXN0ZXIvZm9saXVtL3RlbXBsYXRlcy9sZWFmbGV0LmF3ZXNvbWUucm90YXRlLmNzcyIvPgogICAgPHN0eWxlPmh0bWwsIGJvZHkge3dpZHRoOiAxMDAlO2hlaWdodDogMTAwJTttYXJnaW46IDA7cGFkZGluZzogMDt9PC9zdHlsZT4KICAgIDxzdHlsZT4jbWFwIHtwb3NpdGlvbjphYnNvbHV0ZTt0b3A6MDtib3R0b206MDtyaWdodDowO2xlZnQ6MDt9PC9zdHlsZT4KICAgIAogICAgICAgICAgICA8c3R5bGU+ICNtYXBfOTMwZTc5MjcxNDcwNGY1ZGJkOTYzMmZkZjA1ZTZiNmEgewogICAgICAgICAgICAgICAgcG9zaXRpb24gOiByZWxhdGl2ZTsKICAgICAgICAgICAgICAgIHdpZHRoIDogMTAwLjAlOwogICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICBsZWZ0OiAwLjAlOwogICAgICAgICAgICAgICAgdG9wOiAwLjAlOwogICAgICAgICAgICAgICAgfQogICAgICAgICAgICA8L3N0eWxlPgogICAgICAgIAo8L2hlYWQ+Cjxib2R5PiAgICAKICAgIAogICAgICAgICAgICA8ZGl2IGNsYXNzPSJmb2xpdW0tbWFwIiBpZD0ibWFwXzkzMGU3OTI3MTQ3MDRmNWRiZDk2MzJmZGYwNWU2YjZhIiA+PC9kaXY+CiAgICAgICAgCjwvYm9keT4KPHNjcmlwdD4gICAgCiAgICAKCiAgICAgICAgICAgIAogICAgICAgICAgICAgICAgdmFyIGJvdW5kcyA9IG51bGw7CiAgICAgICAgICAgIAoKICAgICAgICAgICAgdmFyIG1hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YSA9IEwubWFwKAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgJ21hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YScsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICB7Y2VudGVyOiBbNDAuNzY2ODg4NzUzNzQ1OTEsLTczLjkyMjE5NTQzNDU3MDMxXSwKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIHpvb206IDEwLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgbWF4Qm91bmRzOiBib3VuZHMsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBsYXllcnM6IFtdLAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgd29ybGRDb3B5SnVtcDogZmFsc2UsCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3CiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0pOwogICAgICAgICAgICAKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgdGlsZV9sYXllcl84OThiOWEwMzhlNDk0YzczOTA1OWNhY2U2YWQwNjQwYiA9IEwudGlsZUxheWVyKAogICAgICAgICAgICAgICAgJ2h0dHBzOi8ve3N9LnRpbGUub3BlbnN0cmVldG1hcC5vcmcve3p9L3t4fS97eX0ucG5nJywKICAgICAgICAgICAgICAgIHsKICAiYXR0cmlidXRpb24iOiBudWxsLAogICJkZXRlY3RSZXRpbmEiOiBmYWxzZSwKICAibWF4Wm9vbSI6IDE4LAogICJtaW5ab29tIjogMSwKICAibm9XcmFwIjogZmFsc2UsCiAgInN1YmRvbWFpbnMiOiAiYWJjIgp9CiAgICAgICAgICAgICAgICApLmFkZFRvKG1hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YSk7CiAgICAgICAgCiAgICAKCiAgICAgICAgICAgIHZhciBtYXJrZXJfMGI5ZGM3NzZmNDZiNGRlZmExZmUzZDhmMjZmZWQ1ZjEgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43NTY2MjEsLTczLjkyOTMzNl0sCiAgICAgICAgICAgICAgICB7CiAgICAgICAgICAgICAgICAgICAgaWNvbjogbmV3IEwuSWNvbi5EZWZhdWx0KCkKICAgICAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICAgICApCiAgICAgICAgICAgICAgICAuYWRkVG8obWFwXzkzMGU3OTI3MTQ3MDRmNWRiZDk2MzJmZGYwNWU2YjZhKTsKICAgICAgICAgICAgCiAgICAKCiAgICAgICAgICAgIHZhciBtYXJrZXJfMDQ2MzM1MjNiMWYwNDJiMThhOGYzM2FhNzFkMDQxZWMgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43NjE0MjE0NjgyNjMzLC03My45MjQ2NjQ5NDU2MTcxXSwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBpY29uOiBuZXcgTC5JY29uLkRlZmF1bHQoKQogICAgICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgICAgICkKICAgICAgICAgICAgICAgIC5hZGRUbyhtYXBfOTMwZTc5MjcxNDcwNGY1ZGJkOTYzMmZkZjA1ZTZiNmEpOwogICAgICAgICAgICAKICAgIAoKICAgICAgICAgICAgdmFyIG1hcmtlcl84M2Q5ODhlZWEzYjY0ZDdlYTcxNDZlNjQ1ZmI4MDA4NCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc1NTcxNzE1NDM0NzcsLTczLjkyNzgxMTI5MjQxMl0sCiAgICAgICAgICAgICAgICB7CiAgICAgICAgICAgICAgICAgICAgaWNvbjogbmV3IEwuSWNvbi5EZWZhdWx0KCkKICAgICAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICAgICApCiAgICAgICAgICAgICAgICAuYWRkVG8obWFwXzkzMGU3OTI3MTQ3MDRmNWRiZDk2MzJmZGYwNWU2YjZhKTsKICAgICAgICAgICAgCiAgICAKCiAgICAgICAgICAgIHZhciBtYXJrZXJfNzBlYTk1OTcyOTc2NGZlODhlMjc1NGQxODYxYjJkMGQgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43NjI1OTk5LC03My45MTI5MDI4XSwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBpY29uOiBuZXcgTC5JY29uLkRlZmF1bHQoKQogICAgICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgICAgICkKICAgICAgICAgICAgICAgIC5hZGRUbyhtYXBfOTMwZTc5MjcxNDcwNGY1ZGJkOTYzMmZkZjA1ZTZiNmEpOwogICAgICAgICAgICAKICAgIAoKICAgICAgICAgICAgdmFyIG1hcmtlcl81MmI4NzhjYzg2MDc0NjM1YTI4Yzk0YTMzZjFhMjRkNiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc1OTMzMSwtNzMuOTI2MDM1XSwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBpY29uOiBuZXcgTC5JY29uLkRlZmF1bHQoKQogICAgICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgICAgICkKICAgICAgICAgICAgICAgIC5hZGRUbyhtYXBfOTMwZTc5MjcxNDcwNGY1ZGJkOTYzMmZkZjA1ZTZiNmEpOwogICAgICAgICAgICAKICAgIAoKICAgICAgICAgICAgdmFyIG1hcmtlcl8xNTdkYTI0ZTZlMDM0ZWY4ODlhY2Y5YWQ0M2Y2YTY1NiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc2MTIwMzM2Mzk0MjIsLTczLjkyNjE0MzY0NjI0MDJdLAogICAgICAgICAgICAgICAgewogICAgICAgICAgICAgICAgICAgIGljb246IG5ldyBMLkljb24uRGVmYXVsdCgpCiAgICAgICAgICAgICAgICAgICAgfQogICAgICAgICAgICAgICAgKQogICAgICAgICAgICAgICAgLmFkZFRvKG1hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YSk7CiAgICAgICAgICAgIAogICAgCgogICAgICAgICAgICB2YXIgbWFya2VyXzhhNDg5ZDI4MmE1NjQ5N2E4MWUwNmRmNzdmNmM2NDI2ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzYxNjcsLTczLjkxMDldLAogICAgICAgICAgICAgICAgewogICAgICAgICAgICAgICAgICAgIGljb246IG5ldyBMLkljb24uRGVmYXVsdCgpCiAgICAgICAgICAgICAgICAgICAgfQogICAgICAgICAgICAgICAgKQogICAgICAgICAgICAgICAgLmFkZFRvKG1hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YSk7CiAgICAgICAgICAgIAogICAgCgogICAgICAgICAgICB2YXIgbWFya2VyX2QwMWFiMWYzOGU4YjQ0OGNhYjM0YTI0YThiZTJmM2NjID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzY4MzcsLTczLjkzMzAzXSwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBpY29uOiBuZXcgTC5JY29uLkRlZmF1bHQoKQogICAgICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgICAgICkKICAgICAgICAgICAgICAgIC5hZGRUbyhtYXBfOTMwZTc5MjcxNDcwNGY1ZGJkOTYzMmZkZjA1ZTZiNmEpOwogICAgICAgICAgICAKICAgIAoKICAgICAgICAgICAgdmFyIG1hcmtlcl9jNmFlOGM0YzA0YTc0YzFlODFmMzRhZjg4NmNlNTE1MyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjc2MzE3NjgyMTEzNzksLTczLjkyODIyNjAxNTM0NDNdLAogICAgICAgICAgICAgICAgewogICAgICAgICAgICAgICAgICAgIGljb246IG5ldyBMLkljb24uRGVmYXVsdCgpCiAgICAgICAgICAgICAgICAgICAgfQogICAgICAgICAgICAgICAgKQogICAgICAgICAgICAgICAgLmFkZFRvKG1hcF85MzBlNzkyNzE0NzA0ZjVkYmQ5NjMyZmRmMDVlNmI2YSk7CiAgICAgICAgICAgIAogICAgCgogICAgICAgICAgICB2YXIgbWFya2VyX2FjYzc2M2E0NWRhMjRiZDk5MTA3MjQ0ZjUzODZkOWI1ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzU1ODUsLTczLjkyNDQ3XSwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBpY29uOiBuZXcgTC5JY29uLkRlZmF1bHQoKQogICAgICAgICAgICAgICAgICAgIH0KICAgICAgICAgICAgICAgICkKICAgICAgICAgICAgICAgIC5hZGRUbyhtYXBfOTMwZTc5MjcxNDcwNGY1ZGJkOTYzMmZkZjA1ZTZiNmEpOwogICAgICAgICAgICAKPC9zY3JpcHQ+" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



#### API Thoughtstarters

* Google Maps
* Twitter
* AWS
* IBM's Watson
* Yelp

## Summary

Congratulations! We've covered a lot here! We started with HTTP requests, one of the fundamental protocols underlying the internet that we know and love. From there, we further investigated OAuth and saw how to get an access token to use in an API such as yelp. Then we made some requests to retrieve information that came back as a json format. We then transformed this data into a dataframe using the Pandas package. Finally, we created an initial visualization of the data that we retrieved using folium.


```python
#Reinforce previous skills:
```
