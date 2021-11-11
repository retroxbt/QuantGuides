# Introduction to Quantitative Crypto Analysis in Python

## GM!

In this tutorial I'm going to show you how to do some basic crypto
analysis using [Google Colab](https://colab.research.google.com/). Google colab
is a powerful tool I use every day and gives you access to python notebooks in your browser.

Using this we are going to create a defi screener to visualise respective
performance over the past 24 hours. We will be using the [coingecko API](https://www.coingecko.com/en/api/documentation).

I want this to be as interactive as possible, I encourage you to open up a [notebook](https://colab.research.google.com/#create=true)
and code with me!

Let's get started.

Most things in data science involve 3 things:
- Data retrieval
- Data manipulation
- Data visualisation

Python doesn't natively support this so we must import them!

```{python}
import requests # Data retrieval
import pandas as pd # Data manipulation & visualisation
```

Hit shift+enter and that will run the code in the cell.

## Data retrieval
As mentioned above we will use 
coingecko's free [API](https://www.coingecko.com/en/api/documentation) 
to get the price data we need. The api takes parameters, and returns a json
object that will later need to be parsed.

eg:
![1](https://i.gyazo.com/93c74ee5c9891820c8db6193dbc3bd7d.png)

But we don't want just the eth price, we want the price of many coins,
what about my portfolio or the whole defi 1.0 sector?

To do this we must construct a url string that the coingecko API will understand.

```{python}
coins = ["curve-dao-token",
         "rune",
         "the-graph",
         "alpha-finance",
         "yearn-finance",
         "aave",
         "kyber-network",
         "uniswap",
         "maker",
         "sushi",
         "snx",
         "compound-governance-token"]
url = "https://api.coingecko.com/api/v3/simple/price?ids=%2C"
url_end = "&vs_currencies=usd&include_24hr_change=true"

for coin in coins:
  url += coin + "%2C" #concatenates(adds) each coin in coins array to url with a separator

url += url_end #concatenates url_end to url
print(url)

result = (requests.get(url)).json() #data request
print(result)
```

Run this code and it should print a really long url. This url was used
in a HTTP GET method to request our data from coingecko servers. The `.json()`
is a function that encodes the json object into a python dictionary that we can work with. 

## Data manipulation

Now we need to manipulate the data into something that can be visualised.
Luckily most of the work has already been done. Pandas is a very powerful
data manipulation tool which can turn our python dictionary into a dataframe 
with a few lines of code.

```{python}
df = pd.DataFrame.from_dict(result)
df = df.transpose() #flip the dataframe
df
```

Run this code in a new cell, it will output our dataframe like so:
![2](https://i.gyazo.com/66cc01f1599aa8791ce8dc909b4ddf77.png)

## Data visualisation
Now lets make our data pretty and plot it on a horizontal bar graph. In a new cell,
run this code:

```{python}
df["usd_24h_change"].plot(kind='barh', color='k')
```
![3](https://i.gyazo.com/1b78289ddb0b70be43e74384cd3979cc.png)
Damn rune shit the bed...

Let's change the colour of the graph to something more appropriate.
![4](https://i.gyazo.com/5be0a979b6cfb163d8493e4f181d6336.png)
Much better!

Wow you are basically a quant now. If you understand the code
in this basic example I encourage you to make your own changes
or even make time series charts to track price over time. I hope this is useful to you,
let me ([@retroxbt](https://twitter.com/retroxbt)) know if you want more of these type of short tutorials. :)

Sign up to [FTX](ftx.com/#a=retro) using code `retro` for a fee discount.

