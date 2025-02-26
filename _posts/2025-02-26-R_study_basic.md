---
layout : post
title : "데이터 분석 : R 스터디 10 ~ 12장"
date : 2025-02-26
categories : [Data Analysis, R]
---
# 기본 발제

## 10장 : 텍스트 마이닝

### 10-1 : 힙합 가사 텍스트 마이닝

문자로 된 데이터에서 가치 있는 정보를 얻어 내는 분석 기법을 **“텍스트 마이닝(Text mining)”** 이라고 함

텍스트 마이닝을 할 때 가장 먼저 하는 작업은 문장을 구성하는 어절들이 어떤 품사로 되어 있는지 파악하는 **형태소 분석(Morphology Analysis)**

→ 어절들의 품사를 파악한 후 명사, 동사, 형용사 등 의미를 지닌 품사의 단어들을 추출해 각 단어가 얼마나 등장했는지 확인

**텍스트 마이닝 준비하기**

KoNLP(Korean Natural Language Processing) 패키지를 이용하면 한글 텍스트의 형태소를 분석할 수 있음

1. 자바 설치하기
2. KoNLP 의존성 패키지 설치하기
    
    어떤 패키지는 다른 패키지의 기능을 이용하기 때문에 다른 패키지를 함께 설치해야만 작동함. 이처럼 패키지가 의존하고 있는 패키지를 ‘의존성 패키지’라고 함
    
    ```r
    install.packages(c("stringr", "hash", "tau","Sejong", "RSQLite", "devtools"), type = "binary")
    ```
    
3. KoNLP 패키지 설치하기
    
    ```r
    install.packages("remotes")
    remotes :: install_github("haven-jeon/KoNLP",
                              upgrade = "never",
                              INSTALL_opts = c("--no-multiarch"))
    ```
    
4. scala-library-2.11.8.jar 파일 다운로드하기
    
    ```r
    download.file(
      url = "https://github.com/youngwoos/Doit_R/raw/master/Data/scala-library-2.11.8.jar",
      destfile = paste0(.libPaths()[1], "/KoNLP/Java/scala-library-2.11.8.jar"))
    ```
    
5. 데이터 불러오기
    
    ```r
    txt <- readLines("hiphop.txt")
    head(txt)
    
    [1] "\"보고 싶다"                 
    [2] "이렇게 말하니까 더 보고 싶다"
    [3] "너희 사진을 보고 있어도"     
    [4] "보고 싶다"                   
    [5] "너무 야속한 시간"            
    [6] "나는 우리가 밉다" 
    ```
    
6. 특수문자 제거하기

문자처리 패키지인 **`stringr의 str_replace_all()`**을 이용해 문장에 들어 있는 특수문자를 빈칸으로 수정

```r
install.packages("stringr")
library(stringr)

txt <- str_replace_all(txt, "\\W", " ")
```

**`\\W`** 는 특수문자를 의미하는 정규표현식!!

**가장 많이 사용된 단어 알아보기**

1. 명사 추출하기

명사를 보면 문장이 무엇에 대한 내용인지 파악 가능

KoNLP의 **`extractNoun()`**을 이용하면 문장에서 명사 추출 가능

```r
extractNoun("대한민국의 영토는 한반도와 그 부속도서로 한다")

[1] "대한"     "민국"     "영토"    
[4] "한반도와" "부속도서" "한"  
```

1. 힙합 가사에서 명사를 추출하고, 각 단어가 몇 번씩 사용됐는지 빈도표 생성
    
    ```r
    # 가사에서 명사 추출
    
    nouns <- extractNoun(txt)
    
    # 추출한 명사 list를 문자열 벡터로 변환, 단어별 빈도표 생성
    
    wordcount <- table(unlist(nouns))
    
    # 데이터 프레임으로 변환
    
    df_word <- as.data.frame(wordcount, stringsAsFactors = F)
    
    # 변수명 수정
    
    df_word <- rename(df_word,
                      word = Var1,
                      freq = Freq)
    ```
    

extractNoun()은 출력 결과를 리스트 형태로 반환

1. 자주 사용된 단어 빈도표 만들기
    
    ```r
    # 두 글자 이상 단어 추출
    
    df_word <- filter(df_word, nchar(word) >= 2)
    ```
    
2. 빈도 순으로 정렬한 후 상위 20개 단어를 추출
    
    ```r
     top_20 <- df_word %>%
      arrange(desc(freq)) %>%
      head(20)
    
    top_20
     
     word freq
    1   you   89
    2    my   86
    3   YAH   80
    4    on   76
    5  하나   75
    6  오늘   51
    ...
    ```
    

**워드 클라우드 만들기**

워드 클라우드(Word cloud)는 단어의 빈도를 구름 모양으로 표현한 그래프

단어의 빈도에 따라 글자의 크기와 색깔이 다르게 표현

1. 패키지 준비하기
    
    ```r
    install.packages("wordcloud")
    
    library(wordcloud)
    library(RColorBrewer)
    ```
    
2. 단어 색상 목록 만들기
    
    ```r
    # Dark2 색상 목록에서 8개 색상 추출
    
    pal <- brewer.pal(8, "Dark2")
    ```
    
3. 난수 고정하기

wordcloud()는 함수를 실행할 때마다 난수를 이용해 매번 다른 모양의 워드 클라우드를 만들어 냄

항상 동일한 워드클라우드가 생성되도록 난수 고정

```r
set.seed(1234)
```

1. 워드 클라우드 만들기
    
    ```r
    wordcloud(words = df_word$word,  # 단어
              freq = df_word$freq,   # 빈도
              min.freq = 2,          # 최소 단어 빈도
              max.words = 200,       # 표현 단어 수
              random.order = F,      # 고빈도 단어 중앙 배치
              rot.per = .1,          # 회전 단어 비율
              scale = c(4, 0.3),    # 단어 크기 범위
              colors = pal)          # 색상 목록
    ```
    
    ![1.png](/assets/img/posts/R/R_study_5th/basic/1.png)
    
2. 단어 색상 바꾸기
    
    ```r
    pal <- brewer.pal(9, "Blues")[5:9]
    set.seed(1234)
    
    wordcloud(words = df_word$word,
              freq = df_word$freq,
              min.freq = 2,
              max.words = 200,
              random.order = F,
              rot.per = .1,
              scale = c(4, 0.3),
              colors = pal
    ```
    

### 10-2 : 국정원 트윗 텍스트 마이닝

**국정원 트윗 텍스트 마이닝**

1. 데이터 준비하기
    
    ```r
    # 데이터 로드
    
    twitter <- read.csv("twitter.csv")
    
    # 변수명 수정
    
    twitter <- rename(twitter,
                      no = 번호,
                      id = 계정이름,
                      date = 작성일,
                      tw = 내용)
    
    # 특수문자 제거
    
    twitter$tw <- str_replace_all(twitter$tw, "\\W", " ")
    head(twitter$tw)
    
    [1] "민주당의 ISD관련 주장이 전부 거짓으로 속속 드러나고있다  미국이 ISD를 장악하고 있다고 주장하지만 중재인 123명 가운데 미국인은 10명뿐이라고 한다 "                                                                               
    [2] "말로만  미제타도   사실은  미제환장   김정일 운구차가 링컨 컨티넬탈이던데 북한의 독재자나 우리나라 종북들이나 겉으로는 노동자  서민을 대변한다면서 고급 외제차  아이팟에 자식들 미국 유학에 환장하는 위선자들인거죠"  
    ```
    
2. 단어 빈도표 만들기
    
    ```r
    # 트윗에서 명사추출
    
    nouns <- extractNoun(twitter$tw)
    
    # 추출한 명사 list를 문자열 벡터로 변환, 단어별 빈도표 생성
    
    wordcount <- table(unlist(nouns))
    
    # 데이터 프레임으로 변환
    
    df_word <- as.data.frame(wordcount,stringsAsFactors = FALSE)
    
    # 변수명 수정
    
    df_word <- rename(df_word,
                      word = Var1,
                      freq = Freq)
    ```
    
    stringsAsFactors = FALSE 파라미터로 데이터프레임을 만들 때 factor 타입으로 변환되는것을 방지
    
3. 두 글자 이상으로 된 단어 추출, 빈도 순으로 정렬
    
    ```r
    df_word <- filter(df_word, nchar(word) >=2)
    
    top20 <- df_word %>%
      arrange(desc(freq)) %>%
      head(20)
    
    top20
    
    word freq
    1    북한 1167
    2    민국  804
    3    대한  800
    4    우리  778
    5    좌파  640
    6    국민  505
    7    들이  411
    8    세력  406
    9    친북  387
    ...
    ```
    
4. 단어 빈도 막대 그래프 만들기
    
    ```r
    library(ggplot2)
    
    order <- arrange(top20, freq)$word
    
    ggplot(data = top20, aes(x = word, y = freq)) +
      ylim(0, 2500) +
      geom_col() +
      coord_flip() +
      scale_x_discrete(limit = order) +
      geom_text(aes(label = freq), hjust = 0.1)
    ```
    
    ![2.png](/assets/img/posts/R/R_study_5th/basic/2.png)
    
5. 워드 클라우드 만들기
    
    ```r
    pal <- brewer.pal(9, "Blues")[5:9]
    set.seed(1234)
    
    wordcloud(words = df_word$word,  # 단어
              freq = df_word$freq,   # 빈도
              min.freq = 10,         # 최소 단어 빈도
              max.words = 150,       # 표현 단어 수
              random.order = F,      # 고빈도 단어 중앙 배치
              rot.per = 0,           # 회전 단어 비율
              scale = c(2, 0.5),     # 단어 크기 범위
              colors = pal)          # 색상 목록
    ```
    
    ![3.png](/assets/img/posts/R/R_study_5th/basic/3.png)
    

## 11장 : 지도 시각화

### 11-1 : 미국 주별 강력 범죄율 단계 구분도 만들기

지역별 통계치를 색깔의 차이로 표현한 지도를 **단계 구분도(Choropleth Map)**이라고 함

단계 구분도를 보면 인구나 소득 같은 특성이 지역별로 얼마나 다른지 쉽게 이해 가능

**미국 주별 강력 범죄율 단계 구분도 만들기**

1. 패키지 준비하기
    
    ```r
    nstall.packages("mapproj")
    install.packages("ggiraphExtra")
    library(ggiraphExtra)
    ```
    
2. 미국 주별 범죄 데이터 준비하기
    
    ```r
    # R에 내장된 1973년 미국 주(state)별 강력 범죄율 정보
    
    str(USArrests)
    
    'data.frame':	50 obs. of  4 variables:
     $ Murder  : num  13.2 10 8.1 8.8 9 7.9 3.3 5.9 15.4 17.4 ...
     $ Assault : int  236 263 294 190 276 204 110 238 335 211 ...
     $ UrbanPop: int  58 48 80 50 91 78 77 72 80 60 ...
     $ Rape    : num  21.2 44.5 31 19.5 40.6 38.7 11.1 15.8 31.9 25.8 ...
    ```
    
    ```r
    head(USArrests)
    
              Murder Assault UrbanPop Rape
    Alabama      13.2     236       58 21.2
    Alaska       10.0     263       48 44.5
    Arizona       8.1     294       80 31.0
    Arkansas      8.8     190       50 19.5
    California    9.0     276       91 40.6
    Colorado      7.9     204       78 38.7
    ```
    
3. 전처리

USArrests  데이터는 지역명 변수가 따로 없고, 대신 행 이름이 지역명으로 되어 있으므로 tibble 패키지의 **`rownames_to_column()`**을 이용해 행 이름을 state 변수로 바꿔 새 데이터 프레임 생성

**`tolower()`** 을 이용해 모든 state 값을 소문자로 수정

```r
library(tibble)

crime <- rownames_to_column(USArrests, var = "state")
crime$state <- tolower(crime$state)

str(crime)

'data.frame':	50 obs. of  5 variables:
 $ state   : chr  "alabama" "alaska" "arizona" "arkansas" ...
 $ Murder  : num  13.2 10 8.1 8.8 9 7.9 3.3 5.9 15.4 17.4 ...
 $ Assault : int  236 263 294 190 276 204 110 238 335 211 ...
 $ UrbanPop: int  58 48 80 50 91 78 77 72 80 60 ...
 $ Rape    : num  21.2 44.5 31 19.5 40.6 38.7 11.1 15.8 31.9 25.8 ...
```

1. 미국 주 지도 데이터 준비하기

단계 구분도를 만들려면 지역별 위도, 경도 정보가 있는 지도 데이터가 필요

미국 주별 위경도 데이터가 있는 maps 패키지 설치 후, **`map_data()`**를 이용해 데이터 프레임으로 불러옴

```r
install.packages("maps")
library(ggplot2)
states_map <- map_data("state")
str(states_map)

'data.frame':	15537 obs. of  6 variables:
 $ long     : num  -87.5 -87.5 -87.5 -87.5 -87.6 ...
 $ lat      : num  30.4 30.4 30.4 30.3 30.3 ...
 $ group    : num  1 1 1 1 1 1 1 1 1 1 ...
 $ order    : int  1 2 3 4 5 6 7 8 9 10 ...
 $ region   : chr  "alabama" "alabama" "alabama" "alabama" ...
 $ subregion: chr  NA NA NA NA ...
```

1. 단계 구분도 만들기

ggiraphExtra 패키지의 **`ggChoropleth()`**를 이용해 단계 구분도 생성

살인 범죄 건수를 색깔로 표현하기 위해 aes의 fill에 Murder 변수를 지정, map_id에 지역 구분 기준이 되는 state 변수 추가

crime 데이터의 state 변수와 states_map 데이터의 region 변수는 미국 주 이름을 나타내는 동일한 값으로 구성

```r
ggChoropleth(data = crime,         # 지도에 표현할 데이터
             aes(fill = Murder,    # 색깔로 표현할 변수
                 map_id = state),  # 지역 기준 변수
             map = states_map)     # 지도 데이터
```

![4.png](/assets/img/posts/R/R_study_5th/basic/4.png)

1. 인터랙티브 단계 구분도 만들기
    
    interactive 파라미터를 TRUE로 설정하면 마우스 움직임에 반응하는 인터랙티브 단계 구분도 생성!!
    
    ```r
    ggChoropleth(data = crime,
                 aes(fill = Murder,
                     map_id = state),
                 map = states_map,
                 interactive = T)
    ```
    
    뷰어 창의 Export → Save as Web Page를 클릭하면 HTML 포맷으로 저장 가능 
    
    ![5.png](/assets/img/posts/R/R_study_5th/basic/5.png)
    

### 11-2 : 대한민국 시도별 인구, 결핵 환자 수 단계 구분도 만들기

**대한민국 시도별 인구 단계 구분도 만들기**

kormaps2014 패키지를 이용하면 대한민국의 지역 통계 데이터와 지도 데이터 사용 가능

1. 패키지 준비하기
    
    ```r
    install.packages("stringi")
    
    install.packages("devtools")
    devtools::install_github("cardiomoon/kormaps2014")
    
    library(kormaps2014)
    ```
    
2. 대한민국 시도별 인구 데이터 준비하기
- korplp1 : 2015년 센서스 데이터(시도별)
- korpop2 : 2015년 센서스 데이터(시군구별)
- korpop3 : 2015년 센서스 데이터(읍면동별)
    
    ```r
    str(korpop1)
    
    'data.frame':	17 obs. of  25 variables:
     $ C행정구역별_읍면동     : Factor w/ 3819 levels "'00","'03","'04",..: 5 455 681 832 995 1096 1181 1246 1264 1874 ...
     $ 행정구역별_읍면동      : Factor w/ 3398 levels "  가경동","  가곡동",..: 3388 3387 3383 3392 3382 3384 3390 3389 3379 3378 ...
     $ 시점                   : int  2015 2015 2015 2015 2015 2015 2015 2015 2015 2015 ...
     $ 총인구_명              : int  9904312 3448737 2466052 2890451 1502881 1538394 1166615 204088 12479061 1518040 ...
     $ 남자_명                : int  4859535 1701347 1228511 1455017 748867 772243 606924 103210 6309661 768241 ...
     $ 여자_명                : int  5044777 1747390 1237541 1435434 754014 766151 559691 100878 6169400 749799 ...
     ...
    ```
    
1. 변수명이 한글로 되어 있으면 오류가 발생할 수 있으니 영문자로 수정
    
    ```r
    library(dplyr)
    korpop1 <- rename(korpop1, pop = 총인구_명, name = 행정구역별_읍면동)
    ```
    
2. 대한민국 시도 지도 데이터 준비하기
- kormap1 : 2014년 한국 행정 지도(시도별)
- kormpa2 : 2014년 한국 행정 지도(시군구별)
- kormap3 : 2014년 한국 행정 지도(읍면동별)
    
    ```r
    str(kormap1)
    
    'data.frame':	8831 obs. of  15 variables:
     $ id       : chr  "0" "0" "0" "0" ...
     $ long     : num  138 138 138 138 138 ...
     $ lat      : num  50.7 50.7 50.7 50.7 50.7 ...
     $ order    : int  1 2 3 4 5 6 7 8 9 10 ...
     $ hole     : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
     $ piece    : Factor w/ 113 levels "1","2","3","4",..: 1 1 1 1 1 1 1 1 1 1 ...
     ....
    ```
    
1. 단계 구분도 만들기

지역 기준이 되는 code 변수가 숫자 코드로 되어 있기 때문에 지도에 마우스 커서를 올리면 코드가 표시

→ 코드 대신 지역명이 표시되도록 tooltip에 지역명 변수 name 지정

```r
ggChoropleth(data = korpop1,       # 지도에 표현할 데이터
             aes(fill = pop,       # 색깔로 표현할 변수
                 map_id = code,    # 지역 기준 변수
                 tooltip = name),  # 지도 위에 표시할 지역명
             map = kormap1,        # 지도 데이터
             interactive = T)      # 인터랙티브
```

![6.png](/assets/img/posts/R/R_study_5th/basic/6.png)

**대한민국 시도별 결핵 환자 수 단계 구분도 만들기**

kormaps2014 패키지에는 지역별 결핵 환자 수에 대한 정보를 담고 있는 tbc데이터 존재

tbc 데이터의 NewPts(결핵 환자 수) 변수를 이용해 시도별 결핵 환자 수 단계구분도 생성

```r
str(tbc)

'data.frame':	255 obs. of  5 variables:
 $ name1 : Factor w/ 18 levels "강원","경기",..: 1 2 3 4 5 6 7 8 9 10 ...
 $ code  : int  32 31 38 37 24 22 25 21 11 29 ...
 $ name  : Factor w/ 17 levels "강원도","경기도",..: 1 2 3 4 5 6 7 8 9 10 ...
 $ year  : Factor w/ 15 levels "2001","2002",..: 1 1 1 1 1 1 1 1 1 1 ...
 $ NewPts: int  1396 4843 1749 2075 658 1406 1345 3188 11178 NA ...
```

```r
ggChoropleth(data = tbc,          # 지도에 표현할 데이터
             aes(fill = NewPts,   # 색깔로 표현할 변수
                 map_id = code,   # 지역 기준 변수
                 tooltip = name), # 지도 위에 표시할 지역명
             map = kormap1,       # 지도 데이터
             interactive = T)     # 인터랙티브
```

![7.png](/assets/img/posts/R/R_study_5th/basic/7.png)

## 12장 : 인터랙티브 그래프

### 12-1 : ploty 패키지로 인터랙티브 그래프 만들기

인터랙티브 그래프(Interactive Graph)란, 마우스 움직임에 반응하며 실시간으로 형태가 변하는 그래프

그래프를 HTML 포맷으로 저장하면, 일반 사용자들도 웹 브라우저를 이용해 그래프 조작 가능

**인터랙티브 그래프 만들기**

1. 패키지 준비하기
    
    ```r
    install.packages("plotly")
    library(plotly)
    ```
    
2. ggplot2로 그래프 만들기

ggplot2로 만든 그래프를 plotly 패키지의 ggplotly()에 적용하면 인터랙티브 그래프 생성

```r
library(ggplot2)
p <- ggplot(data = mpg, aes(x = displ, y = hwy, col = drv)) + geom_point()
```

1. 인터랙티브 그래프 만들기
    
    ```r
    ggploty(p)
    ```
    
    ![8.png](/assets/img/posts/R/R_study_5th/basic/8.png)
    

마우스를 드래그하면 특정 영역을 확대할 수 있음

1. HTML로 저장하기

뷰어 창에서 Export → Save as Web Page를 클릭하면 인터랙티브 그래프를 HTML 포맷으로 저장 가능

1. 인터랙티브 막대 그래프 만들기

산점도 외에도 ggplot2 패키지로 만든 그래프는 ggplotly()를 이용해 인터랙티브 그래프로 만들 수 있음

ggplot2 패키지에 내장된 diamonds 데이터를 이용

→ 다이아몬드 5만여 개의 캐럿, 컷팅 방식, 겨격 등의 속성을 담은 데이터

```r
p <- ggplot(data = diamonds, aes(x = cut, fill = clarity)) +
  geom_bar(position = "dodge")

ggplotly(p)
```

![9.png](/assets/img/posts/R/R_study_5th/basic/9.png)

### 12-2 : dygraphs 패키지로 인터랙티브 시계열 그래프 만들기

시간에 따른 데이터의 변화를 표현한 인터랙티브 시계열 그래프

마우스로 시간 축으로 움직이면서 시간에 따라 데이터가 어떻게 변하는지 자세히 살펴볼 수 있음

**인터랙티브 시계열 그래프 만들기**

dygraphs 패키지로 인터랙티브 시계열 그래프

1. dygraphs 패키지 설치
    
    ```r
    install.packages("dygraphs")
    library(dygraphs)
    ```
    
2. economics 데이터 불러오기
    
    ```r
    economics <- ggplot2 :: economics
    head(economics)
    
    # A tibble: 6 × 6
      date         pce    pop psavert uempmed
      <date>     <dbl>  <dbl>   <dbl>   <dbl>
    1 1967-07-01  507. 198712    12.6     4.5
    2 1967-08-01  510. 198911    12.6     4.7
    3 1967-09-01  516. 199113    11.9     4.6
    4 1967-10-01  512. 199311    12.9     4.9
    5 1967-11-01  517. 199498    12.8     4.7
    6 1967-12-01  525. 199657    11.8     4.8
    # ℹ 1 more variable: unemploy <dbl>
    ```
    
3. dygraphs 패키지를 이용해 인터랙티브 시계열 그래프를 만드려면 데이터가 시간 순서 속성을 지니는 xts 데이터 타입으로 되어 있어야함
    
    xts()를 이용해 실업자 수를 xts 타입으로 변경
    
    ```r
    library(xts)
    eco <- xts(economics$unemploy, order.by = economics$date)
    head(eco)
    
               [,1]
    1967-07-01 2944
    1967-08-01 2945
    1967-09-01 2958
    1967-10-01 3143
    1967-11-01 3066
    1967-12-01 3018
    ```
    
4. 인터랙티브 시계열 그래프 만들기
    
    ```r
    # 그래프 생성
    
    dygraph(eco)
    ```
    
    ![10.png](/assets/img/posts/R/R_study_5th/basic/10.png)
    
    선 위에 마우스 커서를 올리면 그래프 오른쪽 위에 날짜와 실업자 수가 표시
    
    마우스를 드래그하면 특정 기간 확대 가능, 더블클릭시 원상복구
    
    1. 날짜 범위 선택
    
    dygraph()에 %>%를 이용해 dyRangeSelector()를 추가하면 그래프 아래에 날짜 범위 선택 기능 추가
    
    ```r
    dygraph(eco) %>% dyRangeSelector()
    ```
    
    ![11.png](/assets/img/posts/R/R_study_5th/basic/11.png)
    

6. 여러 값 표현하기

인터랙티브 시계열 그래프에서 여러 값을 동시에 표현 가능

```r
# 저축률

eco_a <- xts(economics$psavert, order.by = economics$date)

# 실업자 수

eco_b <- xts(economics$unemploy/1000, order.by = economics$date)
```

1. 전처리
    
    ```r
    eco2 <- cbind(eco_a, eco_b)                     # 데이터 결합
    colnames(eco2) <- c("psavert", "unemploy")      # 변수명 바꾸기
    head(eco2)
    
               psavert unemploy
    1967-07-01    12.6    2.944
    1967-08-01    12.6    2.945
    1967-09-01    11.9    2.958
    1967-10-01    12.9    3.143
    1967-11-01    12.8    3.066
    1967-12-01    11.8    3.018
    ```
    
2. 그래프 생성
    
    ```r
    dygraph(eco2) %>% dyRangeSelector()
    ```
    
    ![12.png](/assets/img/posts/R/R_study_5th/basic/12.png)