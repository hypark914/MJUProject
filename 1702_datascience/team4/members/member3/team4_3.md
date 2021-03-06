국내여행 참여자 수에 가구 소득이 영향을 미치는가?
================

### 들어가며

근래의 국내 여행 시장 약화 추세로 인해 국내 여행 진흥을 위한 다양한 시도가 이루어지고 있다. 그만큼 실질적인 국내 여행 참가자들의 데이터의 중요성도 높아졌기에 이를 알아봄으로써 국내 여행자들의 특성에 대한 파악을 할 수 있다. 본 보고서는 국내의 여러 지역으로 여행을 하는 여행자들은 어떤 특성을 가지고 있는지 알아보고, 가구 소득에 따른 여행자 수와의 관계를 지역별, 연도별 분석하고자 한다. 분석 결과 지역별, 연도별로 급격한 차이는 보이지 않았다. 지역별로 분석했을 때도 모든 지역이 비슷한 분포를 보였고, 연도별로 분석했을 때도 비슷한 분포를 보였다. 분석은 아래와 같이 2개의 주제를 가지고 분석했다.

1.  가구소득별 연도에 따른 평균 국내 여행자수 비교
2.  가구소득별 여행지에 따른 국내 여행자 수의 비율 비교

분석은 아래와 같이 3단계로 진행한다.

1.  데이터 파악 및 수정
2.  주제에 따른 데이터 가공 및 분석
3.  그래프 만들기

데이터

-   선정 이유 : 해당 데이터에는 소득별 지역, 연도 국내 여행자 수가 기록되어있기 때문에 분석에 용이하기 때문이다.

-   출처 : 국가통계포털(KOSIS)제공(국내통계-기관별통계-중앙행정기관-문화체육관광부-국민여행 실태조사-여행 경험률 및 여행참가 횟수-여행지별 국내여행 참가자수(2014~2016))

-   구성 : 2014년부터 2016년까지의 국내여행 참가자들을 대상으로 한 데이터이다. 대한민국 전국 시도별 국내 여행 참가자수를 통계분류(1)과 그 하위항목 분류인 통계분류(2)로 자세히 나누었다.

1.  국내 여행지(17곳) : 서울, 부산, 대구, 인천, 광주, 대전, 울산, 세종, 경기, 강원, 충북, 충남, 전북, 전남, 경북, 경남, 제주
2.  성별 (남자, 여자)
3.  연령(15세~19세, 20대, 30대, 40대, 50대, 60대이상)
4.  직업(전문관리, 사무, 서비스판매, 농어업, 기능노무)
5.  가구소득(50만원미만, 50만~100만원미만, 100만~200만원미만, 200만~300만원미만, 300만~400만원미만, 400만~500만원미만, 500만~600만원미만, 600만원 이상)
6.  학력(초졸이하, 중학교(재/졸), 고등학교(재/졸), 전문대(재/졸), 대학교(재/졸), 대학원(재/졸)

-   조사대상 범위 및 지역 : 가구 내 만 15세 이상 모든 가구원 조사 / 전국

-   조사주기 : 1년

### 여행지별 국내여행 참가자 수(2014~) 데이터 파악 및 수정

#### 데이터 불러오기

`readxl` 패키지를 로드한 뒤 `read_excel()`을 이용해 데이터를 불러온다. `데이터` sheet를 사용하고, 데이터를 확인했을 때 첫 번째 행이 변수명이었으므로 `col_names`는 `T`로 입력한다.

``` r
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 3.4.2

``` r
travel <- read_excel("travel2.xlsx", sheet = "데이터", col_names = T)
```

불러온 데이터가 어떤 속성을 가지고 있는지 `str()`함수를 이용하여 확인한다.

``` r
str(travel)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    476 obs. of  6 variables:
    ##  $ 통계분류(1): chr  "전체" NA NA NA ...
    ##  $ 통계분류(2): chr  "소계" NA NA NA ...
    ##  $ 항목       : chr  "서울" "부산" "대구" "인천" ...
    ##  $ 2014       : num  12580581 6665173 3554687 4464192 2263117 ...
    ##  $ 2015       : num  12451891 7158553 3163161 4407063 2135332 ...
    ##  $ 2016       : num  13237854 7414157 3137687 5420706 2401244 ...

#### 데이터 수정하기

위의 절차를 통해 불러온 데이터는 이상치가 존재하는 데이터이며, 수정을 해야함을 알 수 있다. 이상치가 원본 데이터의 구성에 의해 발생한 값이므로 삭제하는 대신 알맞는 값을 넣는다. 또한 에러가 나지 않도록 변수명을 영어로 바꾼다.

먼저 원본 데이터를 복사한다.

``` r
travel_new <- travel
```

`rename()`함수를 이용하여 travel\_new 데이터의 변수명을 영어 변수명으로 바꾼다. 이 때 사용할 함수는 `dplyr` 패키지에 들어있으므로 해당 패키지를 로드한다.

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 3.4.2

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
travel_new <- rename(travel_new, sta1="통계분류(1)",
                     sta2="통계분류(2)",
                     area="항목",
                     year_2014="2014",
                     year_2015="2015",
                     year_2016="2016")
```

다음은 `NA`값에 알맞는 내용을 지정해주고자 한다. `sta1` 변수에서는 대분류 통계로 총합, 성별, 연령, 직업, 소득, 학업에 대해 분류가 되어있으므로 각 통계 분류에 맞는 내용을 할당한다. `sta2` 변수에서는 대분류에 따른 소분류가 들어있으므로 그에 맞춰 각 부분에 맞는 내용을 할당한다.

``` r
#대분류
travel_new$sta1[1:17] <- "total"
travel_new$sta1[18:51] <- "sex"
travel_new$sta1[52:153] <- "age"
travel_new$sta1[154:238] <- "job"
travel_new$sta1[239:374] <- "income"
travel_new$sta1[375:476] <- "school"

#소분류
travel_new$sta2[1:17] <- "subtotal"
travel_new$sta2[18:34] <- "male"
travel_new$sta2[35:51] <- "female"
travel_new$sta2[52:68] <- "15-19"
travel_new$sta2[69:85] <- "20's"
travel_new$sta2[86:102] <- "30's"
travel_new$sta2[103:119] <- "40's"
travel_new$sta2[120:136] <- "50's"
travel_new$sta2[137:153] <- "60's"
travel_new$sta2[154:170] <- "전문관리"
travel_new$sta2[171:187] <- "사무"
travel_new$sta2[188:204] <- "서비스판매"
travel_new$sta2[205:221] <- "농어업"
travel_new$sta2[222:238] <- "기능노무"
travel_new$sta2[239:255] <- "50만원미만"
travel_new$sta2[256:272] <- "50-100만원미만"
travel_new$sta2[273:289] <- "100-200만원미만"
travel_new$sta2[290:306] <- "200-300만원미만"
travel_new$sta2[307:323] <- "300-400만원미만"
travel_new$sta2[324:340] <- "400-500만원미만"
travel_new$sta2[341:357] <- "500-600만원미만"
travel_new$sta2[358:374] <- "600만원이상"
travel_new$sta2[375:391] <- "초졸이하"
travel_new$sta2[392:408] <- "중학교(재/졸)"
travel_new$sta2[409:425] <- "고등학교(재/졸)"
travel_new$sta2[426:442] <- "전문대(재/졸)"
travel_new$sta2[443:459] <- "대학교(재/졸)"
travel_new$sta2[460:476] <- "대학원(재/졸)"
```

데이터를 수정한 결과는 아래와 같다.

``` r
str(travel_new)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    476 obs. of  6 variables:
    ##  $ sta1     : chr  "total" "total" "total" "total" ...
    ##  $ sta2     : chr  "subtotal" "subtotal" "subtotal" "subtotal" ...
    ##  $ area     : chr  "서울" "부산" "대구" "인천" ...
    ##  $ year_2014: num  12580581 6665173 3554687 4464192 2263117 ...
    ##  $ year_2015: num  12451891 7158553 3163161 4407063 2135332 ...
    ##  $ year_2016: num  13237854 7414157 3137687 5420706 2401244 ...

``` r
head(travel_new,10)
```

    ## # A tibble: 10 x 6
    ##     sta1     sta2  area year_2014 year_2015 year_2016
    ##    <chr>    <chr> <chr>     <dbl>     <dbl>     <dbl>
    ##  1 total subtotal  서울  12580581  12451891  13237854
    ##  2 total subtotal  부산   6665173   7158553   7414157
    ##  3 total subtotal  대구   3554687   3163161   3137687
    ##  4 total subtotal  인천   4464192   4407063   5420706
    ##  5 total subtotal  광주   2263117   2135332   2401244
    ##  6 total subtotal  대전   3123627   2984929   3497887
    ##  7 total subtotal  울산   1787673   1632410   2043956
    ##  8 total subtotal  세종    366885    333329    740710
    ##  9 total subtotal  경기  15283727  15451755  16826706
    ## 10 total subtotal  강원  12282959  11559005  11683223

### 가구소득별 연도에 따른 국내 여행자수 평균 비교

가구소득 데이터를 중심으로 `여행지별 국내여행 횟수(2014~2016)` 데이터를 분석하고자 한다. 연도별로 가구소득이 국내 여행자 수에 영향을 미치는지 확인하고자 한다. 극단치를 제외하고 평균적인 인원을 파악하기 위해 여행자 수는 평균 여행자 수를 이용한다.

#### 데이터 가공하기

가구소득을 중심으로 하는 제한된 데이터를 사용하고, 평균 여행자 수를 사용해야 하므로 `filter()`함수를 이용해 `sta1`에서 income`을 추출한다. 이후 가구소득별로 묶고`summarise()\`함수를 이용하여 연도별 여행자 수 총합과 평균을 구한다.

``` r
income <- travel_new %>% 
  filter(sta1 == "income") %>% 
  group_by(sta2) %>% 
  select(sta2, year_2014, year_2015, year_2016) %>% 
  summarise(sum_2014=sum(year_2014),
            sum_2015=sum(year_2015),
            sum_2016=sum(year_2016),
            mean_2014=mean(year_2014),
            mean_2015=mean(year_2015),
            mean_2016=mean(year_2016))

income
```

    ## # A tibble: 8 x 7
    ##              sta2 sum_2014 sum_2015 sum_2016 mean_2014 mean_2015
    ##             <chr>    <dbl>    <dbl>    <dbl>     <dbl>     <dbl>
    ## 1 100-200만원미만 11044824 12238329 11034636  649695.5  719901.7
    ## 2 200-300만원미만 21130463 20118824 20082014 1242968.4 1183460.2
    ## 3 300-400만원미만 20176370 18003502 24345996 1186845.3 1059029.5
    ## 4 400-500만원미만 19686333 19872386 21158481 1158019.6 1168963.9
    ## 5  50-100만원미만  3562220  3296903  4171032  209542.4  193935.5
    ## 6 500-600만원미만 18438357 18799445 18960888 1084609.2 1105849.7
    ## 7      50만원미만  2208871  1785478  1314418  129933.6  105028.1
    ## 8     600만원이상 19252793 19106917 20871578 1132517.2 1123936.3
    ## # ... with 1 more variables: mean_2016 <dbl>

출력된 결과에서 확인할 수 있듯이 `income`데이터는 `x`축 가구소득을 기준으로 `y`축에 각각의 연도별 평균값을 넣는 그래프로 사용할 수 있다. 하지만 2014~2016년의 결과를 한번에 볼 수 있는 그래프를 만들기 위해 아래와 같은 새로운 데이터를 가공한다.

``` r
income_year <- data.frame(income= rep(c("50만원미만", "50-100만원미만", "100-200만원미만", "200-300만원미만", "300-400만원미만", "400-500만원미만", "500-600만원미만", "600만원이상"), 3),
                          year= rep(c(2014,2015,2016), each=8),
                          sum= c(2208871,3562220,11044824,21130463,20176370,19686333,18438357,19252793,1785478,3296903,12238329,20118824,18003502,19872386,18799445,19106917,1314418,4171032,11034636,20082014,24345996,21158481,18960888,20871578),
                          mean= c(129933.6,129933.6,649695.5,1242968.4,1186845.3,1158019.6,1084609.2,1132517.2,105028.1,193935.5,719901.7,1183460.2,1059029.5,1168963.9,1105849.7,1123936.3,77318.71,245354.82,649096.24,1181294.94,1432117.41,1244616.53,1115346.35,1227739.88))

income_year
```

    ##             income year      sum       mean
    ## 1       50만원미만 2014  2208871  129933.60
    ## 2   50-100만원미만 2014  3562220  129933.60
    ## 3  100-200만원미만 2014 11044824  649695.50
    ## 4  200-300만원미만 2014 21130463 1242968.40
    ## 5  300-400만원미만 2014 20176370 1186845.30
    ## 6  400-500만원미만 2014 19686333 1158019.60
    ## 7  500-600만원미만 2014 18438357 1084609.20
    ## 8      600만원이상 2014 19252793 1132517.20
    ## 9       50만원미만 2015  1785478  105028.10
    ## 10  50-100만원미만 2015  3296903  193935.50
    ## 11 100-200만원미만 2015 12238329  719901.70
    ## 12 200-300만원미만 2015 20118824 1183460.20
    ## 13 300-400만원미만 2015 18003502 1059029.50
    ## 14 400-500만원미만 2015 19872386 1168963.90
    ## 15 500-600만원미만 2015 18799445 1105849.70
    ## 16     600만원이상 2015 19106917 1123936.30
    ## 17      50만원미만 2016  1314418   77318.71
    ## 18  50-100만원미만 2016  4171032  245354.82
    ## 19 100-200만원미만 2016 11034636  649096.24
    ## 20 200-300만원미만 2016 20082014 1181294.94
    ## 21 300-400만원미만 2016 24345996 1432117.41
    ## 22 400-500만원미만 2016 21158481 1244616.53
    ## 23 500-600만원미만 2016 18960888 1115346.35
    ## 24     600만원이상 2016 20871578 1227739.88

#### 그래프로 나타내기

가구소득별 평균 국내 여행자 수를 파악하기 위해 `ggplot2`를 로드한 뒤 `geom_col()` 함수를 이용해 평균빈도막대그래프를 만든다. `x축`에는 가구소득(income), `y축`에는 평균 국내 여행자 수(mean)을 지정하고 `fill` 파라미터를 사용하여 연도별로 색이 다르게 표현되게 한다. 또한 `scale_x_discrete(limits=c())`를 이용해 income의 순서를 바꾼다. 마지막으로 `ggtitle()`을 이용해 그래프 제목을 넣어준다.

``` r
library(ggplot2)
ggplot(data = income_year, aes(x=income, y=mean, fill=year)) + 
  geom_col()+
  scale_x_discrete(limits=c("50만원미만", "50-100만원미만", "100-200만원미만", "200-300만원미만", "300-400만원미만", "400-500만원미만", "500-600만원미만", "600만원이상"))+
  ggtitle('연도에 따른 소득-평균 여행자 수')
```

![](60151523_김현지_개인과제_files/figure-markdown_github/unnamed-chunk-9-1.png)

출력된 그래프를 보면 가구 소득에 따라 대체적으로 비슷한 모양의 분포가 나타난다는 것을 확인할 수 있었다. 다만 연도별 평균 여행자 수가 급격히 늘거나 줄지 않지만 가구 소득이 100만원 미만인 인원들과 100~200만원 미만인 여행자 수에서 급격한 차이가 보이며, 100~200만원 미만인 여행자 수와 200~300만원 미만인 여행자 수 또한 큰 차이를 보인다.

### 가구소득별 여행지에 따른 국내 여행자 수의 비율 비교

#### 데이터 가공하기

`filter()`함수를 이용해 `sta1`에서 `가구소득(income)`을 추출한다. 그 뒤 `group_by()`를 이용해 지역(area)별로 묶은 뒤 `mutate()`함수를 이용하여 지역별 총 여행자 수를 나타내는 `tot_area`와 지역별 여행자 수 비율을 나타내는 `ratio` 두 변수를 생성한다.

``` r
income2 <- travel_new %>% 
  filter(sta1 == "income") %>% 
  group_by(area) %>% 
  select(area,sta2, year_2014, year_2015, year_2016) %>% 
  mutate(tot_area=sum(year_2014,year_2015,year_2016),
         ratio=(year_2014+year_2015+year_2016)/tot_area*100) %>% 
  arrange(area)

income2
```

    ## # A tibble: 136 x 7
    ## # Groups:   area [17]
    ##     area            sta2 year_2014 year_2015 year_2016 tot_area     ratio
    ##    <chr>           <chr>     <dbl>     <dbl>     <dbl>    <dbl>     <dbl>
    ##  1  강원      50만원미만    333056    222083     80182 35525187  1.788368
    ##  2  강원  50-100만원미만    353378    305828    278744 35525187  2.640239
    ##  3  강원 100-200만원미만    876053    986705    761283 35525187  7.386424
    ##  4  강원 200-300만원미만   2094108   1960205   1829766 35525187 16.563119
    ##  5  강원 300-400만원미만   2194155   1610700   2415781 35525187 17.510495
    ##  6  강원 400-500만원미만   2030163   2268972   2213170 35525187 18.331515
    ##  7  강원 500-600만원미만   2209296   1991823   2096862 35525187 17.728214
    ##  8  강원     600만원이상   2192750   2212689   2007435 35525187 18.051626
    ##  9  경기      50만원미만    279076    305635    152790 47562189  1.550604
    ## 10  경기  50-100만원미만    387632    450687    596144 47562189  3.015973
    ## # ... with 126 more rows

#### 그래프로 나타내기

`x`축에 지역, `y`축에 여행자 수 비율을 나타내고, 가구소득에 따라 색깔을 다르게 표현할 수 있는`fill`파라미터를 사용한다. 또한 `scale_fill_discrete(limits=c())`를 이용해 income의 순서를 바꾼다. 마지막으로 `y축`에 비해 `x축` 변수가 많으므로 `x`축과 `y`축의 위치를 변경하는 `coor_flip()`함수와 `ggtitle()`을 이용해 그래프 제목을 넣어준다.

``` r
ggplot(data = income2, aes(x=area, y=ratio, fill=sta2)) + 
  geom_col() +
  scale_fill_discrete(limits=c("50만원미만", "50-100만원미만", "100-200만원미만", "200-300만원미만", "300-400만원미만", "400-500만원미만", "500-600만원미만", "600만원이상"))+
  coord_flip()+
  ggtitle('소득별 여행지-여행자 수')
```

![](60151523_김현지_개인과제_files/figure-markdown_github/unnamed-chunk-11-1.png)

출력된 그래프를 보면 지역에 따라 대체적으로 비슷한 모양의 비율 분포가 나타난다는 것을 확인할 수 있었다. 다만 세종의 경우 다른 지역에 비해 '300~400만원미만'의 가구소득에 해당하는 여행자 수가 많았다.

### 끝내며

이 분석은 이민 등의 이유로 해당 년도에 여행을 갔던 사람이 조사 대상에 들어가지 못했을 수 있다는 점과 같이 정말 해당 년도에 여행한 모든 인원을 조사할 수 없다는 한계가 있다. 또한 동일 인원이 여러 지역을 여행했을 가능성이 있으며, 이에 대한 조사 결과는 나와있지 않기 때문에 해석이 불가하다. 추후 분석하게 된다면 성별 대비 소득 데이터, 연령 대비 소득 데이터를 추가해서 소득별 어떤 성별 또는 연령이 어느 지역에 여행을 많이 가는지 분석해보고 싶다.
