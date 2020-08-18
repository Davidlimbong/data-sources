
# Data Sourses
This repository contains a collection of packages, and functions in R that are often used to retrieve data from various sources.

## Stock market 

Stock market data can be obtained with the `tq_get()` function of the `tidyquant` packages. A more detailed explanation regarding functions can be found at [tidyquant documentation](https://www.rdocumentation.org/packages/tidyquant/versions/1.0.1/topics/tq_get).

```{r}
library(tidyquant)
BBCA <- tq_get(x = "BBCA.JK", get = "stock.prices", from = "2020-08-03")
```

## Twitter API

You can access data from Twiter using `rtweet` package. To get the access you need to create a *Twitter Developer Apps* first. The tutorial can be accessed on this [website](https://developer.twitter.com/en/docs/getting-started). After you have created a Twitter App, you need to create a token using the `create_token()` function from `rtweet` package. All the key access can be acquired from the Twitter App.

```{r}
library(rtweet)

# regis token
token <- create_token(app = "xxx",
                      consumer_key = "xxx",
                      consumer_secret = "xxx",
                      access_token = "xxx-xxx",
                      access_secret = "xxx")
```

After you have created a token, you may start to search for tweets. For this illustration, we want to search all tweets with a hashtag of `#COVID19`.
```{r}
# get twitter data
tweet_covid <- search_tweets(q = "#COVID19", # Search Query
                             n = 10, # Number of extracted tweets
                             include_rts = T, # Include Retweet?
                             retryonratelimit = T, # Retry when reach limit?
                             lang = "en") # Language = English

```

## Google Spreadsheet

You can read data from google spreadsheet directly using `googlesheets4`. `googlesheets4` provides an R interface to Google Sheets via the Sheets API v4. You can read the full documentation [here](https://googlesheets4.tidyverse.org/).  

```{r}
library(googlesheets4)
gapminer <- read_sheet("https://docs.google.com/spreadsheets/d/1U6Cf_qEOhiR9AZqTqS3mbMF3zt2db48ZP5v3rkrAEJY/edit#gid=780868077")
head(gapminer)
```

## Wikipedia views

Using `pageviews` package you can get the number of visitors of articles on Wikipedia.

```{r}
library(pageviews)
indo <- article_pageviews(project = "en.wikipedia", 
                  article = "Indonesia", 
                  start =as.Date('2018-01-01'), 
                  end = Sys.Date())
```


## General API
`httr` is a useful package for working with HTTP organised by HTTP verbs (GET(), POST(), etc). The following is an example of using the `httr` packages on [Jakarta open API](jakarta.go.id).

```{r}
# load libs
library(httr)
library(jsonlite)

# endpoint and token
url <- "http://api.jakarta.go.id/v1/tps/" 
key <- "xxx"

# get the data using GET function
rs_jak <- GET(url = url, add_headers(Authorization = key)) 

# convert data from JSON to dataframe using jsonlite packages
rs_data <- rs_jak %>% 
  content(as = "text") %>% 
  fromJSON()

```


## Database Connection

to connect R with your database you need an Interface to conenct them. `DBI` packages is a database interface (DBI) definition for communication between R and RDBMSs. You can read the full documentation of DBI packages [here](https://github.com/r-dbi/DBI). The following is an example of using the `DBI` packages to connect with `mySQL` RDBMS.

```{r}
# load libs
library(DBI)
library(RMySQL)

# create connection
cn <- dbConnect(drv = MySQL(),
                host ="xxx",
                user = "xxx",
                password= "xxx",
                dbname = "xxx",
                port = 1234
)

# fetch data
data_db <- dbSendQuery(conn = cn, statement = "Query here") %>% 
  fetch()

```
