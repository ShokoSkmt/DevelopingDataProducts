---
title       : Life Expectancy at Birth by Country
subtitle    : One aspect of inequality in the world
author      : Shoko Sakamoto
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]    # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## What is 'Life Expectancy at Birth'(LEB)?

Life expectancy at birth indicates the number of years a newborn infant would live if prevailing patterns of mortality at the time of its birth were to stay the same throughout its life.[Source](http://data.worldbank.org/indicator/SP.DYN.LE00.IN)

- In most countries **LEB** has increased in past 50 years.
- There are few countries Where **LEB** has not increased at all.
- By applying Liner Regression Model, the coefficient of model shows the increase ratio of **LEB** per year.

## Top and bottom countries of LEB 
- Download data from [Source](http://data.worldbank.org/indicator/SP.DYN.LE00.IN)

- Apply Liner Regression Model to each country. Rank countries by coefficient.
- Rank countries by the latest data

--- .class #id
Calculate each country's coefficient from Liner Regression and store latest LED

```r
lmdata <- function (country) {
  x <- substr(names(LeData2),2,5)[3:55]; y <- LeData2[LeData2[1]==country][3:55]
  data <- data.frame(cbind(as.numeric(x), as.numeric(y))); names(data) <- c('X1', 'X2');data
}
df = data.frame(matrix(vector(), 0, 3, dimnames=list(c(), c("country", "slope","latest"))), stringsAsFactors=F)
for(i in 1:nrow(LeData2)) {
  cdata <- lmdata(as.character(LeData2[i,1])) 
  df[i,2] <- lm(X2 ~ X1, data=cdata)$coeff[2];df[i,1] <- as.character(LeData2[i,1])
  df[i,3] <- cdata[nrow(cdata),2]
} 
```
Bottom coefficient

```r
head(df[order(df$slope),],5)
```

```
##                country       slope   latest
## 222           Zimbabwe -0.14959585 58.04598
## 29            Botswana -0.12681140 46.99071
## 206            Ukraine -0.02299842 70.94415
## 119            Lesotho -0.02082916 48.83600
## 172 Russian Federation -0.01100417 70.46098
```

--- .class #id
Top coefficient

```r
head(df[order(-df$slope),],5)
```

```
##         country     slope   latest
## 127    Maldives 0.8362458 77.57383
## 103    Cambodia 0.7743360 71.40883
## 28       Bhutan 0.7371081 67.88927
## 199 Timor-Leste 0.7093241 67.02059
## 156        Oman 0.6864902 76.59105
```
Bottom LED in 2012

```r
head(df[order(df$latest),],5)
```

```
##                      country       slope   latest
## 180             Sierra Leone  0.20369426 45.32905
## 29                  Botswana -0.12681140 46.99071
## 119                  Lesotho -0.02082916 48.83600
## 192                Swaziland  0.04912371 48.85063
## 30  Central African Republic  0.13186950 49.47539
```

--- .class #id
Top LED in 2012

```r
head(df[order(-df$latest),],5)
```

```
##                 country     slope   latest
## 84 Hong Kong SAR, China 0.3102395 83.48049
## 99                Japan 0.2830013 83.09610
## 96                Italy 0.2678999 82.93659
## 95              Iceland 0.1848876 82.91707
## 33          Switzerland 0.2239492 82.69756
```

## Interactive comparison of **LEB** between two countries 
- [https://shokoskmt.shinyapps.io/project/](https://shokoskmt.shinyapps.io/project/)
- Check the differnce of **LEB** trend between two countries you concern.
- See the actual past **LEB** and predicted **LEB** (2013~)
