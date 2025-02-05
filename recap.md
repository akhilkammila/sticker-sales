# Sticker Sales Recap

## Competition Overview

Competition: https://www.kaggle.com/c/playground-series-s5e1

Given sales data for 5 years, predict the next 3 years
- given daily sales per store, country, product
- 5 products, 6 countries, 3 stores

Trends
- clear sinusodal trends for product sales
- clear day of week trends, country/store effects, etc.
- MISSED: effects are multiplicative (ex. store's effect is just a multiplier to sales)

## Own Progress

Tried LGBM with lag features
- this is what I am used to
- got a sinusodal wave for most predictions
- missed many effects (was very off for some countries, stores)

## Looking at Others

Found that multiplicative is the way to go
- tried multiplying country, store, day of week
- failed to fit sinusodal patterns (tried doing day of year effect, multiplying each day by a specific value - but failed)
    - this is too noisy (also overlaps with day of week, other patterns)
    - key is to use RATIOS (try to model the ratio of products to each other over time)
    - other key is to use RIDGE with SINUSOIDAL FEATURES (put a lot of dif. sin/cos waves into ridge, let it fit the trends)
    - ridge is preferred because it is global (it keeps predicting into the future, doesn't fail at the tails)

Way more data exploration
- people's multiplicative features were based on HARD FINDINGS
    - country multiplier based on GDP (this one is big - the plot of country sales ratio over time shows this)
    - store is just an independent multiplier
    - dow an independent multiplier
    - periodic trends per product
    - holidays lag effect

- big thing is data visualization
    - i tried to graph plots that don't isolate effects (ex. sales hued by country, hued by dow)
    - their plots were all ratios (instead of hue by effect, graph the ratio / total after splitting by that effect)
        - ex. if graphing doy, I just see that sales look higher for each around holidays
        - if graphing ratio, the holiday effect is not seen (if it is equal across)