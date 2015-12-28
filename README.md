Tidy up all of the R builtin datasets.

``` r
library(tidyr)
library(dplyr)
library(stringr)
library(lubridate)

knitr::opts_chunk$set(comment='#>')
```

``` r
ability.cov_tidy <- ability.cov %>%
  as.data.frame() %>%
  select(-n.obs, -center) %>%
  mutate(test = rownames(.)) %>%
  gather(cov, value, -test) %>%
  tbl_df()

ability.cov_tidy 
```

    #> Source: local data frame [36 x 3]
    #> 
    #>       test         cov  value
    #>      (chr)      (fctr)  (dbl)
    #> 1  general cov.general 24.641
    #> 2  picture cov.general  5.991
    #> 3   blocks cov.general 33.520
    #> 4     maze cov.general  6.023
    #> 5  reading cov.general 20.755
    #> 6    vocab cov.general 29.701
    #> 7  general cov.picture  5.991
    #> 8  picture cov.picture  6.700
    #> 9   blocks cov.picture 18.137
    #> 10    maze cov.picture  1.782
    #> ..     ...         ...    ...

``` r
airmiles_tidy <- airmiles %>%
  as.data.frame() %>%
  setNames('airmiles') %>%
  mutate(year = time(airmiles)) %>%
  tbl_df()

airmiles_tidy
```

    #> Source: local data frame [24 x 2]
    #> 
    #>    airmiles  year
    #>       (dbl) (dbl)
    #> 1       412  1937
    #> 2       480  1938
    #> 3       683  1939
    #> 4      1052  1940
    #> 5      1385  1941
    #> 6      1418  1942
    #> 7      1634  1943
    #> 8      2178  1944
    #> 9      3362  1945
    #> 10     5948  1946
    #> ..      ...   ...

``` r
# gawd time series are awful ...
# from http://stackoverflow.com/questions/5331901/transforming-a-ts-in-a-data-frame-and-back

dmn <- list(month.abb, unique(floor(time(AirPassengers))))
AirPassengers_df <- as.data.frame(matrix(AirPassengers, 12, dimnames = dmn))

AirPassengers_tidy <- AirPassengers_df %>%
  mutate(month = rownames(.)) %>%
  gather(year, value, -month) %>%
  tbl_df()

AirPassengers_tidy
```

    #> Source: local data frame [144 x 3]
    #> 
    #>    month   year value
    #>    (chr) (fctr) (dbl)
    #> 1    Jan   1949   112
    #> 2    Feb   1949   118
    #> 3    Mar   1949   132
    #> 4    Apr   1949   129
    #> 5    May   1949   121
    #> 6    Jun   1949   135
    #> 7    Jul   1949   148
    #> 8    Aug   1949   148
    #> 9    Sep   1949   136
    #> 10   Oct   1949   119
    #> ..   ...    ...   ...

``` r
# Figure out how to get the 'Qtr1' ... colnames
```

``` r
# Combine the beaver1 and beaver2 datasets ...
beaver1_tidy <- beaver1 %>%
  mutate(obs = seq_len(n())) %>%
  gather(key, value, -obs) %>%
  mutate(beaver = 1)

beaver2_tidy <- beaver2 %>%
  mutate(obs = seq_len(n())) %>%
  gather(key, value, -obs) %>%
  mutate(beaver = 2)

beavers_tidy <- rbind_list(beaver1_tidy, beaver2_tidy)

beavers_tidy
```

    #> Source: local data frame [856 x 4]
    #> 
    #>      obs    key value beaver
    #>    (int) (fctr) (dbl)  (dbl)
    #> 1      1    day   346      1
    #> 2      2    day   346      1
    #> 3      3    day   346      1
    #> 4      4    day   346      1
    #> 5      5    day   346      1
    #> 6      6    day   346      1
    #> 7      7    day   346      1
    #> 8      8    day   346      1
    #> 9      9    day   346      1
    #> 10    10    day   346      1
    #> ..   ...    ...   ...    ...

``` r
BOD_tidy <- BOD %>%
  mutate(obs = seq_len(n())) %>%
  gather(key, value, -obs) %>%
  tbl_df

BOD_tidy
```

    #> Source: local data frame [12 x 3]
    #> 
    #>      obs    key value
    #>    (int) (fctr) (dbl)
    #> 1      1   Time   1.0
    #> 2      2   Time   2.0
    #> 3      3   Time   3.0
    #> 4      4   Time   4.0
    #> 5      5   Time   5.0
    #> 6      6   Time   7.0
    #> 7      1 demand   8.3
    #> 8      2 demand  10.3
    #> 9      3 demand  19.0
    #> 10     4 demand  16.0
    #> 11     5 demand  15.6
    #> 12     6 demand  19.8

``` r
cars_tidy <- cars %>%
  mutate(obs = seq_len(n())) %>%
  gather(key, value, -obs) %>%
  tbl_df

cars_tidy
```

    #> Source: local data frame [100 x 3]
    #> 
    #>      obs    key value
    #>    (int) (fctr) (dbl)
    #> 1      1  speed     4
    #> 2      2  speed     4
    #> 3      3  speed     7
    #> 4      4  speed     7
    #> 5      5  speed     8
    #> 6      6  speed     9
    #> 7      7  speed    10
    #> 8      8  speed    10
    #> 9      9  speed    10
    #> 10    10  speed    11
    #> ..   ...    ...   ...

``` r
ChickWeight_tidy <- ChickWeight %>%
  gather(key, value, -Chick, convert = TRUE) %>%
  tbl_df

ChickWeight_tidy
```

    #> Source: local data frame [1,734 x 3]
    #> 
    #>     Chick    key value
    #>    (fctr)  (chr) (chr)
    #> 1       1 weight    42
    #> 2       1 weight    51
    #> 3       1 weight    59
    #> 4       1 weight    64
    #> 5       1 weight    76
    #> 6       1 weight    93
    #> 7       1 weight   106
    #> 8       1 weight   125
    #> 9       1 weight   149
    #> 10      1 weight   171
    #> ..    ...    ...   ...

``` r
# Already tidy
```

``` r
# Already tidy
```

``` r
# Already tidy
```

``` r
# Already tidy
```

``` r
# Already tidy
```

``` r
# Already tidy
```