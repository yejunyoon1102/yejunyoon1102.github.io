---
layout : post
title : "데이터 분석 : R 스터디 8 ~ 9장"
date : 2025-02-19
categories : [Data Analysis, R]
---
# 기본발제

## 8장 : 그래프 만들기

### 8-1 : R로 만들 수 있는 그래프 살펴보기

R에서 만들 수 있는 그래프 종류

- 2차원 그래프
- 3차원 그래프
- 지도 그래프
- 네트워크 그래프
- 시간에 따라 변화하는 모선 차트
- 마우스 조작에 반응하는 인터랙티브 그래프

![1.png](/assets/img/posts/R/R_study_4th/basic/1.png)

→ 그래프를 만들 때는 ggplot2 패키지 이용!!

### 8-2 : 산점도 - 변수 간 관계 표현하기

데이터를 x축과 y축에 점으로 표현한 그래프를 산점도(Scatter Plot)이라고 함

ggplot2 패키지에는 레이어(layer)구조 라는 것이 존재

- 1단계 : 배경 설정(축)
- 2단계 : 그래프 추가(점, 막대, 선)
- 3단계 : 설정 추가(축 범위, 색, 표식)

**산점도 만들기**

1. 패키지 불러오기

```r
library(ggplot2)
```

1. 배경 설정하기

**`data`**에 그래프를 그리는 데 사용할 데이터를 지정

**`aes`**에는 x축과 y축에 사용할 변수를 지정

```r
# x축에 displ(배기량) 변수, y축에 hwy(고속도로 연비)변수로 배경 생성

ggplot(data = mpg, aes(x = displ, y = hwy))
```

![2.png](/assets/img/posts/R/R_study_4th/basic/2.png)

1. 그래프 추가하기

**`+`**  기호를 이용해 그래프 유형을 지정하는 함수를 추가

산점도를 그리는 함수 : **`geom_point()`**

```r
# 배경에 산점도 추가

ggplot(data = mpg, aes(x = displ, y = hwy)) + geom_point()
```

![3.png](/assets/img/posts/R/R_study_4th/basic/3.png)

산점도에 표시된 점들은 각각의 관측치 (행) 을 의미

→ 그래프를 보면 전반적으로 배기량이 클수록 고속도로 연비가 낮은 모습!

1. 축 범위를 조정하는 설정 추가하기

축은 기본적으로 최솟값에서 최댓값까지 모든 범위의 데이터가 표현되도록 설정

→ 데이터 전체가 아니라 일부만 표현하고 싶을 때!!

**`xlim()`**을 이용해 x축 조정, **`ylim()`**을 이용해 y축 조정

축이 시작되는 값과 끝나는 값을 쉼표로 나열하면 됨

```r
# x축 범위 3 ~ 6, y축 범위 10 ~ 30으로 지정

ggplot(data = mpg, aes(x = displ, y = hwy)) + 
  geom_point() + 
  xlim(3, 6) +
  ylim(10, 30)
```

![4.png](/assets/img/posts/R/R_study_4th/basic/4.png)

**그래프를 이미지 파일로 저장하기**

플롯 창 메뉴의 Export 버튼을 클릭하면 그래프를 이미지로 저장 가능

**ggplot() vs qplot()**

- qplot()은 기능은 많지 않지만 문법이 간단하기 때문에 주로 전처리 단계에서 데이터를 빠르게 확인해보는 용도
- 최종적으로 분석 결과를 보고하기 위해서는 ggplot()을 사용

### 8-3 : 막대 그래프 - 집단 간 차이 표현하기

**평균 막대 그래프 만들기**

1. 집단별 평균표 만들기

평균 막대 그래프를 만드려면 집단별 평균표로 구성된 데이터 프레임이 필요

```r
# 구동 방식별 평균 고속도로 연비 데이터 구성

library(dplyr)

df_mpg <- mpg %>%
  group_by(drv) %>%
  summarise(mean_hwy = mean(hwy))

df_mpg

# A tibble: 3 × 2
  drv   mean_hwy
  <chr>    <dbl>
1 4         19.2
2 f         28.2
3 r         21  
```

1. 그래프 생성하기

**`aes`**의 x축에 범주를 나타내는 변수 지정, y축에 평균값을 나타내는 변수 지정

**`+`** 기호로 연결해 막대 그래프를 만드는 함수 **`geom_col()`** 추가

```r
ggplot(data = df_mpg, aes(x = drv, y = mean_hwy)) + geom_col()
```

![5.png](/assets/img/posts/R/R_study_4th/basic/5.png)

1. 크기 순으로 정렬하기

막대는 기본적으로 범주의 알파벳 순서로 정렬

**`reorder()`**를 사용하면 막대를 값의 크기 순으로 정렬 가능

괄호 안에 x축 변수와 정렬 기준으로 삼을 변수를 지정하면 됨

정렬 기준 변수 앞에 - 기호를 붙이면 내림차순으로 정렬

```r
ggplot(data = df_mpg, aes(x = reorder(drv, -mean_hwy), y = mean_hwy)) + 
geom_col()
```

![6.png](/assets/img/posts/R/R_study_4th/basic/6.png)

**빈도 막대 그래프 만들기**

빈도 막대 그래프는 값의 개수(빈도)로 막대의 길이를 표현한 그래프

y축 없이 x축만 지정하고 **`geom_bar()`** 사용

```r
ggplot(data = mpg, aes(x = drv)) + geom_bar()
```

![7.png](/assets/img/posts/R/R_study_4th/basic/7.png)

x축에 연속 변수를 지정하면 값의 분포를 파악 가능

```r
ggplot(data = mpg, aes(x = hwy)) + geom_bar()
```

![8.png](/assets/img/posts/R/R_study_4th/basic/8.png)

### 8-4 : 선 그래프 - 시간에 따라 달라지는 데이터 표현하기

데이터를 선으로 표현한 그래프를 선 그래프(Line Chart)라고 함

일별 환율처럼, 일정 시간 간격을 두고 나열된 데이터를 시계열 데이터라고 하고

시계열 데이터를 선으로 표현한 그래프를 시계열 그래프 라고 함

**시계열 그래프 만들기**

ggplot2 패키지에 들어있는 economics 데이터 활용(미국의 경제지표를 월별로 나타낸 데이터)

**`geom_line()`**을 이용해 선 그래프 생성

```r
# x축에는 시간을 나타내는 data, y축에는 실업자수를 의미하는 unemploy

ggplot(data = economics, aes(x = date, y = unemploy)) + geom_line()
```

![9.png](/assets/img/posts/R/R_study_4th/basic/9.png)

### 8-5 : 상자 그림 - 집단 간 분포 차이 표현하기

상자 그림(Box Plot)은 데이터의 분포(퍼져 있는 형태)를 직사각형 상자 모양으로 표현한 그래프

**상자 그림 만들기**

**`geom_boxplot()`** 사용

```r
ggplot(data = mpg, aes(x = drv, y = hwy)) + geom_boxplot()
```

![10.png](/assets/img/posts/R/R_study_4th/basic/10.png)

출력된 그래프로 각 구동 방식의 고속도로 연비 분포를 알 수 있음

- 4륜구동은 17 ~ 22 사이에 대부분의 자동차가 모여 있음. 중앙값이 상자 밑면에 가까운 것을 보면 낮은 값 쪽으로 치우친 형태의 분포.
- 전륜구동은 26 ~ 29 사이의 좁은 범위에 자동차가 모여 있는 뾰족한 형태의 분포. 수염의 위, 아래에 점 표식이 있는 것을 보면 연비가 극단적으로 높거나 낮은 자동차들이 존재.
- 후륜구동은 17 ~ 24 사이의 넓은 범위에 자동차가 분포하고 있다는 것을 알 수 있음. 수염이 짧고 극단치가 없는 것을 보면 대부분의 자동차가 사분위 범위에 해당한다는 것을 알 수 있음.

## 9장 : 데이터 분석 프로젝트 - ‘한국인의 삶을 파악하라!’

### 9-1 : ‘한국복지패널데이터’ 분석 준비하기

1. 데이터 준비하기

2016년에 발간된 복지패널데이터로 6,914가구, 16,664명에 대한 정보 존재

1. 패키지 설치 및 로드

실습에 사용될 데이터는 통계분석 소프트웨어인 SPSS 전용 파일로 되어 있음

**`foreign`** 패키지를 이용하면 SPSS, SAS STATA 등 다양한 통계분석 소프트웨어의 파일 불러오기 가능

```r
install.packages("foreign")

library(foreign)    # SPSS 파일 불러오기
library(dplyr)      # 전처리
library(ggplot2)    # 시각화
library(readxl)     # 엑셀 파일 불러오기
```

1. 데이터 불러오기

foreign 패키지의 **`read.spss()`**를 이용해 복지패널데이터를 불러옴

```r
# 데이터 불러오기

raw_welfare <- read.spss(file = "Koweps_hpc10_2015_beta1.sav", to.data.frame = T)

# 복사본 만들기

welfare <- raw_welfare

```

to.data.frame = T는 SPSS 파일을 데이터 프레임 형태로 변환하는 기능

1. 데이터 검토하기

```r
head(welfare)
tatil(welfare)
View(welfare)
dim(welfare)
str(welfare)
summary(welfare)

...
```

1. 변수명 바꾸기

분석에 사용할 몇 개의 변수를 이해하기 쉬운 변수명으로 교체

```r
welfare <- rename(welfare,
                  sex = h10_g3,
                  birth = h10_g4,
                  marriage = h10_g10,
                  religion = h10_g11,
                  income = p1002_8aq1,
                  code_job = h10_eco9,
                  code_region = h10_reg7)
```

**데이터 분석 절차**

- 1단계 : 변수 검토 및 전처리
- 2단계 : 변수 간 관계 분석

### 9-2 : 성별에 따른 월급 차이 - “성별에 따라 월급이 다를까?”

성별에 따른 월급 차이가 있을까..?

→ 성별이랑 월급에 대한 데이터 전처리 이후 성별 월급 평균표에 그래프 만들기!

**성별 변수 검토 및 전처리**

1. 변수 검토하기

class() 로 성별 변수의 타입을 파악하고, table()로 각 범주에 몇 명이 있는지 확인

```r
class(welfare$sex)

[1] "numeric"

table(welfare$sex)

   1    2 
7578 9086 

```

1. 전처리

코드북을 보면 성별 변수의 값이 1이면 남자, 2이면 여자를 의미 (9는 무응답)

데이터에 이상치가 있는지 검토하고, 분석에서 이상치를 제외할 수 있도록 NA를 부여해야함

값이 9인 경우도 결측 처리 해야

```r
# 이상치 확인

table(welfare$sex)

   1    2 
7578 9086 
```

여기선 이상치가 없지만, 만약 이상치가 있다면 제외해주어야 함

```r
# 이상치 결측 처리

welfare$sex <- ifelse(welfare$sex == 9, NA, welfare$sex)

table(is.na(welfare$sex))

FALSE 
16664 
```

성별 변수의 값을 이해하기 쉽게 바꾸기!

```r
# 성별 항목 이름 부여

welfare$sex <- ifelse(welfare$sex == 1, "male", "feamle")

table(welfare$sex)

female   male 
  9086   7578 
```

```r
# 시각화

qplot(welfare$sex)
```

![11.png](/assets/img/posts/R/R_study_4th/basic/11.png)

**월급 변수 검토 및 전처리**

1. 변수 검토하기

코드북을 보면 월급은 ‘일한 달의 월 평균 임금’을 의미하며 1만원 단위로 기록

성별 변수는 범주 변수이기 때문에 table()로 각 범주의 빈도를 확인하면 특징을 파악할 수 있음

월급 변수는 연속 변수이기 때문에 table()을 이용하면 너무 많은 항목이 출력되므로 summary()로 요약 통계량을 확인

```r
class(welfare$income)

[1] "numeric"

summary(welfare$income)

   Min. 1st Qu.  Median    Mean 3rd Qu. 
    0.0   122.0   192.5   241.6   316.6 
   Max.    NA's 
 2400.0   12030
```

income은 numerix 타입이고 0 ~ 2400만원 사이의 값을 지님

122 ~ 316만원 사이에 가장 많이 분포

평균은 241.6만원, 중앙값은 192.5만원으로 전반적으로 낮은 값으로 치우쳐짐

```r
# 시각화

qplot(welfare$income) + xlim(0, 1000)
```

![12.png](/assets/img/posts/R/R_study_4th/basic/12.png)

1. 전처리

월급은 1 ~ 9998 사이의 값을 지니며 모름 또는 무응답은 9999로 코딩되어 있음

```r
# 이상치 확인

summary(welfare$income)

   Min. 1st Qu.  Median    Mean 3rd Qu. 
    0.0   122.0   192.5   241.6   316.6 
   Max.    NA's 
 2400.0   12030
```

최솟값이 0, 최댓값이 2400이고 결측치가 12030개 있는 모습

→ 직업이 없어서 월급을 받지 않는 응답자는 결측치로 처리됨

또한 코드북에는 1 ~ 9998 사이의 값을 지니는데 0이 최솟값임

→ 0 또한 결측 처리 해주어야 함

```r
# 이상치 결측 처리

welfare$income <- ifelse(welfare$income %in% c(0, 9999), NA, welfare$income)

table(is.na(welfare$income))

FALSE  TRUE 
 4620 12044 
```

**성별에 따른 월급 차이 분석하기**

1. 성별 월급 평균표 만들기

```r
sex_income <- welfare %>%
  filter(!is.na(income)) %>%
  group_by(sex) %>%
  summarise(mean_income = mean(income))

sex_income

# A tibble: 2 × 2
  sex    mean_income
  <chr>        <dbl>
1 female        163.
2 male          312.
```

월급 평균이 남자는 312만원, 여자는 163만원으로, 평균적으로 여성보다 남성의 월급이 약 150만원 많음

1. 그래프 만들기

```r
ggplot(data = sex_income, aes(x = sex, y = mean_income)) + geom_col()
```

![13.png](/assets/img/posts/R/R_study_4th/basic/13.png)

### 9-3 : 나이와 월급의 관계 - “몇 살 때 월급을 가장 많이 받을까?”

나이에 따라 월급이 어떻게 다르지..?

→ 나이, 월급에 대한 변수 검토 및 전처리 후 나이에 따른 월급 평균표 만들고 그래프 시각화

**나이 변수 검토 및 전처리**

1. 변수 검토하기

```r
class(welfare$birth)

[1] "numeric"

summary(welfare$birth)

   Min. 1st Qu.  Median    Mean 3rd Qu. 
   1907    1946    1966    1968    1988 
   Max. 
   2014
```

```r
qplot(welfare$birth)
```

![14.png](/assets/img/posts/R/R_study_4th/basic/14.png)

1. 전처리

태어난 연도는 1900 ~ 2014 사이의 값을 가지며, 모름/무응답은 9999로 코딩되어 있음

```r
# 이상치 확인

summary(welfare$birth)

   Min. 1st Qu.  Median    Mean 3rd Qu. 
   1907    1946    1966    1968    1988 
   Max. 
   2014
   
# 결측치 확인

table(is.na(welfare$birth))

FALSE 
16664
```

이상치와 결측치가 없네? 만약 있다면 밑에처럼 처리

```r
welfare$birth <- ifelse(welfare$birth == 9999, NA, welfare$birth)

table(is.na(welfare$birth))

FALSE 
16664
```

1. 파생변수 만들기 - 나이

2015년도에 조사가 진행되었으므로 2015에서 태어난 연도를 뺀 후 1을 더해 나이를 구함

```r
welfare$age <- 2015 - welfare$birth + 1
summary(welfare$age)

  Min. 1st Qu.  Median    Mean 3rd Qu. 
   2.00   28.00   50.00   48.43   70.00 
   Max. 
 109.00 
```

**나이와 월급의 관계 분석하기**

1. 나이에 따른 월급 평균표 만들기

```r
age_income <- welfare %>%
  filter(!is.na(income)) %>%
  group_by(age) %>%
  summarise(mean_income = mean(income))

head(age_income)

# A tibble: 6 × 2
    age mean_income
  <dbl>       <dbl>
1    20        121.
2    21        106.
3    22        130.
4    23        142.
5    24        134.
6    25        145.
```

1. 그래프 만들기

x축을 나이, y축을 월급으로 지정하고 나이에 따른 월급의 변화가 표현되도록 선 그래프 활용

```r
ggplot(data = age_income, aes(x = age, y = mean_income)) + geom_line()
```

![15.png](/assets/img/posts/R/R_study_4th/basic/15.png)

→ 20대 초반에 100만원 가량의 월급을 받고, 이후 지속적으로 증가하다가 50대 무렵 제일 많아지며, 그 이후로 지속적으로 감소하다가 70세 이후에는 20대보다 낮은 월급을 받는 모습

### 9-4 : 연령대에 따른 월급 차이 - “어떤 연령대의 월급이 가장 많을까?”

앞에서는 각 나이별 평균 월급을 분석했다면, 이번에는 나이를 연령대로 비교!

**연령대 변수 검토 및 전처리하기**

1. 파생변수 만들기 - 연령대
- 초년 : 30세 미만
- 중년 30 ~ 59세
- 노년 60세 이상

```r
# 연령대 파생변수 만들기

welfare <- welfare %>%
  mutate(ageg = ifelse(age < 30, "young",
                       ifelse(age <= 59, "middle", "old")))

table(welfare$ageg)

middle    old  young 
  6049   6281   4334 
```

```r
qplot(welfare$ageg)
```

![16.png](/assets/img/posts/R/R_study_4th/basic/16.png)

**연령대에 따른 월급 차이 분석하기**

1. 연령대별로 평균 월급이 다른지 알아보기 위한 월급 평균표 만들기

```r
ageg_income <- welfare %>%
  filter(!is.na(income)) %>%
  group_by(ageg) %>%
  summarise(mean_income = mean(income))

ageg_income

# A tibble: 3 × 2
  ageg   mean_income
  <chr>        <dbl>
1 middle        282.
2 old           125.
3 young         164.
```

1. 그래프 만들기

```r
ggplot(data = ageg_income, aes(x = ageg, y = mean_income)) + geom_col()
```

![17.png](/assets/img/posts/R/R_study_4th/basic/17.png)

ggplot()은 막대를 변수의 알파벳 순으로 설정

범주 순서를 지정하려면 **`scale_x_discrete(limits = c())`** 에 범주 순서 지정

```r
ggplot(data = ageg_income, aes(x = ageg, y = mean_income)) + geom_col() +
  scale_x_discrete(limits = c("young", "middle", "old"))
```

![18.png](/assets/img/posts/R/R_study_4th/basic/18.png)

표와 그래프를 보면 중년이 280만원 정도로 가장 많은 월급을 받음

노년은 125만원 정도로 초년이 받는 163만원보다 적음

### 9-5 : 연령대 및 월급 차이 - “성별 월급 차이는 연령대별로 다를까?”

성별 월급 차이가 연령대에 따라 다른지 분석하기

**연령대 및 성별 월급 차이 분석하기**

1. 연령대 및 성별 월급 평균표 만들기

```r
sex_income <- welfare %>%
  filter(!is.na(income)) %>%
  group_by(ageg, sex) %>%
  summarise(mean_income = mean(income))

sex_income

# A tibble: 6 × 3
# Groups:   ageg [3]
  ageg   sex    mean_income
  <chr>  <chr>        <dbl>
1 middle female       188. 
2 middle male         353. 
3 old    female        81.5
4 old    male         174. 
5 young  female       160. 
6 young  male         171.
```

1. 그래프 만들기

막대가 성별에 따라 다른 색으로 표현되도록 fill에 sex를 지정

```r
ggplot(data = sex_income, aes(x = ageg, y = mean_income, fill = sex)) + 
geom_col() + 
scale_x_discrete(limits = c("young", "middle", "old"))
```

![19.png](/assets/img/posts/R/R_study_4th/basic/19.png)

출력된 그래프는 각 성별의 월급이 연령대 막대에 함께 표현되어 있어 차이를 비교하기 어려움

geom_col()의 position 파라미터를 “dodge”로 설정해 막대를 분리

```r
ggplot(data = sex_income, aes(x = ageg, y = mean_income, fill = sex)) + 
  geom_col(position = "dodge") + 
  scale_x_discrete((limits = c("young", "middle", "old")))
```

![20.png](/assets/img/posts/R/R_study_4th/basic/20.png)

성별 월급의 차이의 양상이 연령대별로 다르다는 것을 알 수 있음!

초년에는 차이가 크지 않다가 중년에 크게 벌어져 남성이 166만원 가량 더 많음

노년에는 차이가 줄어들지만 여전히 92만원 가량 더 많음

→ 남성의 경우 노년과 초년 간 월급 차이가 크지 않음

→ 노년이 초년보다 적은 월급을 받는 현상은 여성에서만 나타나고 있음

→ 초년보다 중년이 더 많은 월급을 받는 현상도 주로 남성에서 나타나고, 여성은 큰 차이가 없음

**나이 및 성별 월급 차이 분석하기**

```r
# 성별 연령별 월급 평균표 만들기

sex_age <- welfare %>%
  filter(!is.na(income)) %>%
  group_by(age, sex) %>%
  summarise(mean_income = mean(income))

head(sex_age)

# A tibble: 6 × 3
# Groups:   age [3]
    age sex    mean_income
  <dbl> <chr>        <dbl>
1    20 female        147.
2    20 male           69 
3    21 female        107.
4    21 male          102.
5    22 female        140.
6    22 male          118.
```

```r
# 그래프 만들기

ggplot(data = sex_age, aes(x = age, y = mean_income, col = sex)) + 
geom_line()
```

![21.png](/assets/img/posts/R/R_study_4th/basic/21.png)

→ 출력된 그래프를 보면 남성의 월급은 50세 전후까지 지속적으로 증가하다가 급격하게 감소하는 반면,

→ 여성은 30세 전후까지 약간 상승하다가 그 이후로는 지속적으로 완만하게 감소함

→ 성별 월급 격차는 30세부터 지속적으로 벌어져 50대 초반에 가장 크게 벌어지고, 이후로 점차 줄어들어 70대 후반이 되면 비슷한 수준이 됨

### 9-6 : 직업별 월급 차이 - “어떤 직업이 월급을 가장 많이 받을까?”

직업 변수를 검토하고 전처리 이후 직업별 월급 평균표를 만들어야함

**직업 변수 검토 및 전처리하기**

1. 변수 검토하기

```r
class(welfare$code_job)

[1] "numeric"

table(welfare$code_job)

111  120  131  132  133  134  135  139  141 
   2   16   10   11    9    3    7   10   35 
 149  151  152  153  159  211  212  213  221 
  20   26   18   15   16    8    4    3   17 
 222  223  224  231  232  233  234  235  236 
  31   12    4   41    5    3    6   48   14
  ...
```

code_job 변수는 직업 코드를 의미함

따라서 직업분류코드를 이용해 직업 명칭 변수를 만들어야 함

1. 전처리

```r
library(readxl)
list_job <- read_excel("Koweps_Codebook.xlsx", col_names = T, sheet = 2)
head(list_job)

# A tibble: 6 × 2
  code_job job                               
     <dbl> <chr>                             
1      111 의회의원 고위공무원 및 공공단체임원……
2      112 기업고위임원                      
3      120 행정 및 경영지원 관리자           
4      131 연구 교육 및 법률 관련 관리자     
5      132 보험 및 금융 관리자               
6      133 보건 및 사회복지 관련 관리자

dim(list_job)

[1] 149   2
```

차원을 보니 149개의 직업으로 분류되는 모습

1. 조인

```r
welfare <- left_join(welfare, list_job, by = "code_job")

welfare %>%
  filter(!is.na(code_job)) %>%
  select(code_job, job) %>%
  head(10)
  
                                  job
1                    경비원 및 검표원
2                              전기공
3  방문 노점 및 통신 판매 관련 종사자
4         기타 서비스관련 단순 종사원
5                     경영관련 사무원
6              문리 기술 및 예능 강사
7                         영업 종사자
8  방문 노점 및 통신 판매 관련 종사자
9    스포츠 및 레크레이션 관련 전문가
10                   매장 판매 종사자
```

welfare에 직업 명칭으로 된 job 변수가 결합된 모습!

**직업별 월급 차이 분석하기**

1. 직업별 월급 평균표 만들기

직업이 없거나 월급이 없는 사람은 제외

```r
job_income <- welfare %>%
  filter(!is.na(job) & !is.na(income)) %>%
  group_by(job) %>%
  summarise(mean_income = mean(income))

head(job_income)

# A tibble: 6 × 2
  job                           mean_income
  <chr>                               <dbl>
1 가사 및 육아 도우미                  80.2
2 간호사                              241. 
3 건설 및 광업 단순 종사원            190. 
4 건설 및 채굴 기계운전원             358. 
5 건설 전기 및 생산 관련 관리자       536. 
6 건설관련 기능 종사자                247. 
```

1. 월급을 내림차순으로 정렬하고 상위 10개 추출

```r
top10 <- job_income %>%
  arrange(desc(mean_income)) %>%
  head(10)

top10

# A tibble: 10 × 2
   job                            mean_income
   <chr>                                <dbl>
 1 금속 재료 공학 기술자 및 시험원……        845.
 2 의료진료 전문가                       844.
 3 의회의원 고위공무원 및 공공단체임원……        750 
 4 보험 및 금융 관리자                   726.
 5 제관원 및 판금원                      572.
 6 행정 및 경영지원 관리자               564.
 7 문화 예술 디자인 및 영상 관련 관리자……        557.
 8 연구 교육 및 법률 관련 관리자         550.
 9 건설 전기 및 생산 관련 관리자         536.
10 석유 및 화학물 가공장치 조작원        532.

```

1. 그래프 만들기

직업 이름이 길기 때문에 그래프를 기본값으로 만들면 안보임!

→ **`coord_flip()`**을 추가해 막대를 오른쪽으로 90도 회전

```r
# 상위 10개 직업 그래프 확인

ggplot(data = top10, aes(x = reorder(job, mean_income), y = mean_income)) +
  geom_col() + 
  coord_flip()
```

![22.png](/assets/img/posts/R/R_study_4th/basic/22.png)

```r
# 하위 직업 10개 그래프 확인

bottom10 <- job_income %>%
  arrange(mean_income) %>%
  head(10)

bottom10

# A tibble: 10 × 2
   job                          mean_income
   <chr>                              <dbl>
 1 가사 및 육아 도우미                 80.2
 2 임업관련 종사자                     83.3
 3 기타 서비스관련 단순 종사원         88.2
 4 청소원 및 환경 미화원               88.8
 5 약사 및 한약사                      89  
 6 작물재배 종사자                     92  
 7 농립어업관련 단순 종사원           102. 
 8 의료 복지 관련 서비스 종사자       104. 
 9 음식관련 단순 종사원               108. 
10 판매관련 단순 종사원               117.

ggplot(data = bottom10, aes(x = reorder(job, -mean_income), y = mean_income)) +
  geom_col() +
  coord_flip() +
  ylim(0, 850)

```

![23.png](/assets/img/posts/R/R_study_4th/basic/23.png)

### 9-7 : 성별 직업 빈도 - “성별로 어떤 직업이 가장 많을까?”

1. 성별 직업 빈도표 만들기

```r
# 남성 직업 빈도 상위 10개 추출

job_male <- welfare %>%
  filter(!is.na(job) & sex == "male") %>%
  group_by(job) %>%
  summarise(n = n()) %>%
  arrange(desc(n)) %>%
  head(10)

job_male

# A tibble: 10 × 2
   job                          n
   <chr>                    <int>
 1 작물재배 종사자            640
 2 자동차 운전원              251
 3 경영관련 사무원            213
 4 영업 종사자                141
 5 매장 판매 종사자           132
 6 제조관련 단순 종사원       104
 7 청소원 및 환경 미화원       97
 8 건설 및 광업 단순 종사원    95
 9 경비원 및 검표원            95
10 행정 사무원                 92
```

```r
# 여성 직업 빈도 상위 10개 추출

job_female <- welfare %>%
  filter(!is.na(job) & sex == "female") %>%
  group_by(job) %>%
  summarise(n = n()) %>%
  arrange(desc(n)) %>%
  head(10)

job_female

# A tibble: 10 × 2
   job                              n
   <chr>                        <int>
 1 작물재배 종사자                680
 2 청소원 및 환경 미화원          228
 3 매장 판매 종사자               221
 4 제조관련 단순 종사원           185
 5 회계 및 경리 사무원            176
 6 음식서비스 종사자              149
 7 주방장 및 조리사               126
 8 가사 및 육아 도우미            125
 9 의료 복지 관련 서비스 종사자   121
10 음식관련 단순 종사원           104
```

1. 그래프 만들기

```r
# 남성 직업 빈도 상위 10개 직업

ggplot(data = job_male, aes(x = reorder(job, n), y = n)) + 
  geom_col() +
  coord_flip()
```

![24.png](/assets/img/posts/R/R_study_4th/basic/24.png)

```r
# 여성 직업 빈도 상위 10개 직업

ggplot(data = job_female, aes(x = reorder(job, n), y = n)) +
  geom_col()+
  coord_flip()
```

![25.png](/assets/img/posts/R/R_study_4th/basic/25.png)

### 9-8 : 종교 유무에 따른 이혼율 - “종교가 있는 사람들이 이혼을 덜 할까?”

**종교 변수 검토 및 전처리하기**

1. 변수 검토하기

```r
class(welfare$religion)

[1] "numeric"

table(welfare$religion)

   1    2 
8047 8617 
```

1. 전처리

코드북을 보면 1 : 있음 / 2 : 없음의 모습

```r
# 종교 유무 이름 부여

welfare$religion <- ifelse(welfare$religion == 1, "yes", "no")
table(welfare$religion)

no  yes 
8617 8047 
```

**혼인 상태 변수 검토 및 전처리하기**

1. 변수 검토하기

```r
class(welfare$marriage)

[1] "numeric"
 
table(welfare$marriage)

   0    1    2    3    4    5    6 
2861 8431 2117  712   84 2433   26 
```

1. 파생변수 만들기 - 이혼 여부

코드북을 보면 배우자가 있을 경우 1, 이혼했을 경우 3으로 코딩되어음

```r
# 이혼 여부 변수 만들기

welfare$group_marriage <- ifelse(welfare$marriage == 1, "marriage",
                                 ifelse(welfare$marriage == 3, "divorce", NA))
table(welfare$group_marriage)

divorce marriage 
     712     8431 
     
table(is.na(welfare$group_marriage))

FALSE  TRUE 
 9143  7521 
```

출력 결과를 보면 결혼한 사람은 8431명, 이혼한 사람은 712명

둘중 어디에도 속하지 않아 결측치로 분류된 경우는 7521명

**종교 유무에 따른 이혼율 표 만들기**

1. 종교 유무에 따른 이혼율 표 만들기

종교 유무 및 결혼 상태별로 나눠 빈도를 구한 뒤, 각 종교 유무 집단의 전체 빈도로 나눠 비율을 구함

비율은 round()를 이용해 소수점 첫째 자리까지 표현

```r
religion_marriage <- welfare %>%
  filter(!is.na(group_marriage)) %>%
  group_by(religion, group_marriage) %>%
  summarise(n = n()) %>%
  mutate(tot_group = sum(n),
         pct = round(n/tot_group * 100, 1))

religion_marriage

# A tibble: 4 × 5
# Groups:   religion [2]
  religion group_marriage     n tot_group
  <chr>    <chr>          <int>     <int>
1 no       divorce          384      4602
2 no       marriage        4218      4602
3 yes      divorce          328      4541
4 yes      marriage        4213      4541
# ℹ 1 more variable: pct <dbl>
```

1. 앞에서 만든 표에서 이혼에 해당하는 값만 추출해 이혼율 표 생성

```r
# 이혼 추출

divorce <- religion_marriage %>%
  filter(group_marriage == "divorce") %>%
  select(religion, pct)

divorce

# A tibble: 2 × 2
# Groups:   religion [2]
  religion   pct
  <chr>    <dbl>
1 no         8.3
2 yes        7.2
```

1. 그래프 만들기

```r

ggplot(data = divorce, aes(x = religion, y = pct)) + geom_col()
```

![26.png](/assets/img/posts/R/R_study_4th/basic/26.png)

**연령대 및 종교 유무에 따른 이혼율 분석하기**

앞에서는 전체를 대상으로 종교 유무에 따른 이혼율 분석

이번에는 종교 유무에 따른 이혼율이 연령대별로 다른지 확인

1. 연령대별 이혼율 표 만들기

```r
ageg_marriage <- welfare %>%
  filter(!is.na(group_marriage)) %>%
  group_by(ageg, group_marriage) %>%
  summarise(n = n()) %>%
  mutate(tot_group = sum(n),
         pct = round(n/tot_group * 100, 1))

ageg_marriage

# A tibble: 6 × 5
# Groups:   ageg [3]
  ageg   group_marriage     n tot_group   pct
  <chr>  <chr>          <int>     <int> <dbl>
1 middle divorce          437      4918   8.9
2 middle marriage        4481      4918  91.1
3 old    divorce          273      4165   6.6
4 old    marriage        3892      4165  93.4
5 young  divorce            2        60   3.3
6 young  marriage          58        60  96.7
```

출력 결과를 보면 이혼율이 연령대별로 다르다는 것을 알 수 있음!

초년의 경우 결혼하거나 이혼한 사례가 적다는 것을 알 수 있음

→ 초년은 사례가 부족해 다른 연령대와 비교하기에 적합하지 않으므로 이후 제외

1. 연령대별 이혼율 그래프 만들기

```r
ageg_divorce <- ageg_marriage %>%
  filter(ageg != "young" & group_marriage == "divorce") %>%
  select(ageg, pct)

ageg_divorce

# A tibble: 2 × 2
# Groups:   ageg [2]
  ageg     pct
  <chr>  <dbl>
1 middle   8.9
2 old      6.6
```

```r
ggplot(data = ageg_divorce, aes(x = ageg, y = pct)) + geom_col()
```

![27.png](/assets/img/posts/R/R_study_4th/basic/27.png)

1. 연령대 및 종교 유무에 따른 이혼율 표 만들기

종교 유무에 따른 이혼율 차이가 연령대별로 다른지 확인

연령대, 종교 유무, 결혼 상태별로 집단을 나눠 빈도를 구한 뒤, 각 집단 전체 빈도로 나눠 비율을 구함

```r
# 연령대, 종교 유무, 결혼 상태별 비율표 만들기

ageg_religion_marriage <- welfare %>%
  filter(!is.na(group_marriage) & ageg != "young") %>%
  group_by(ageg, religion, group_marriage) %>%
  summarise(n = n()) %>%
  mutate(tot_group = sum(n),
         pct = round(n/tot_group * 100, 1))
ageg_religion_marriage

# A tibble: 8 × 6
# Groups:   ageg, religion [4]
  ageg   religion group_marriage     n
  <chr>  <chr>    <chr>          <int>
1 middle no       divorce          260
2 middle no       marriage        2421
3 middle yes      divorce          177
4 middle yes      marriage        2060
5 old    no       divorce          123
6 old    no       marriage        1761
7 old    yes      divorce          150
8 old    yes      marriage        2131
# ℹ 2 more variables: tot_group <int>,
#   pct <dbl>
```

```r
# 연령대 및 종교 유무별 이혼율 표 만들기

df_divorce <- ageg_religion_marriage %>%
  filter(group_marriage == "divorce") %>%
  select(ageg, religion, pct)

df_divorce

# A tibble: 4 × 3
# Groups:   ageg, religion [4]
  ageg   religion   pct
  <chr>  <chr>    <dbl>
1 middle no         9.7
2 middle yes        7.9
3 old    no         6.5
4 old    yes        6.6
```

1. 연령대 및 종교 유무에 따른 이혼율 그래프 만들기

종교 유무에 따라 막대 색깔을 다르게 하기 위해 fill 파라미터에 religion을 지정

geom_col()의 position 파라미터를 “dodge”로 설정해 막대 분리

```r
ggplot(data = df_divorce, aes(x = ageg, y = pct, fill = religion)) + 
  geom_col(position = "dodge")
```

![28.png](/assets/img/posts/R/R_study_4th/basic/28.png)

→ 노년은 종교 유무에 따른 이혼율 차이가 0.1%로 작고, 오히려 종교가 있는 사람들의 이혼율이 더 높음

### 9-9 : 지역별 연령대 비율 - “노년층이 많은 지역은 어디일까?”

**지역 변수 검토 및 전처리하기**

1. 변수 검토하기

```r
class(welfare$code_region)

[1] "numeric"

table(welfare$code_region)

   1    2    3    4    5    6    7 
2486 3711 2785 2036 1467 1257 2922 
```

1. 전처리

코드북의 내용을 참고해 지역 코드 목록을 생성

```r
# 지역 코드 목록 만들기

list_region <- data.frame(code_reigion = c(1 : 7),
                          region = c("서울",
                                     "수도권(인천/경기",
                                     "부산/경남/울산",
                                     "대구/경북",
                                     "대전/충남",
                                     "강원/충북",
                                     "광주/전남/전북/제주도"))

list_region

 code_reigion                region
1            1                  서울
2            2      수도권(인천/경기
3            3        부산/경남/울산
4            4             대구/경북
5            5             대전/충남
6            6             강원/충북
7            7 광주/전남/전북/제주도
```

```r
# 지역명 변수 추가

welfare <- left_join(welfare, list_region, by = "code_region")

welfare %>%
  select(code_region, region) %>%
  head
  
code_region region
1           1   서울
2           1   서울
3           1   서울
4           1   서울
5           1   서울
6           1   서울
```

**지역별 연령대 비율 분석하기**

1. 지역별 연령대 비율표 만들기

```r
region_ageg <- welfare %>%
  group_by(region, ageg) %>%
             summarise(n = n()) %>%
             mutate(tot_group = sum(n),
                    pct = round(n/tot_group * 100, 2))

head(region_ageg)

# A tibble: 6 × 5
# Groups:   region [2]
  region          ageg      n tot_group   pct
  <chr>           <chr> <int>     <int> <dbl>
1 강원/충북       midd…   417      1257  33.2
2 강원/충북       old     555      1257  44.2
3 강원/충북       young   285      1257  22.7
4 광주/전남/전북/제주도…… midd…   947      2922  32.4
5 광주/전남/전북/제주도…… old    1233      2922  42.2
6 광주/전남/전북/제주도…… young   742      2922  25.4
```

1. 그래프 만들기

```r
ggplot(data = region_ageg, aes(x = region, y = pct, fill = ageg)) +
  geom_col() +
  coord_flip()
```

![29.png](/assets/img/posts/R/R_study_4th/basic/29.png)

1. 노년층 비율 높은 순으로 막대 정렬하기

앞에서 만든 그래프는 막대가 밑에서부터 지역명 가나다 순으로 저렬되어 있음

```r
# 노년층 비율 내림차순 정렬

list_order_old <- region_ageg %>%
  filter(ageg == "old") %>%
  arrange(pct)

list_order_old

# A tibble: 7 × 5
# Groups:   region [7]
  region          ageg      n tot_group   pct
  <chr>           <chr> <int>     <int> <dbl>
1 수도권(인천/경기…… old    1109      3711  29.9
2 서울            old     805      2486  32.4
3 대전/충남       old     527      1467  35.9
4 부산/경남/울산  old    1124      2785  40.4
5 광주/전남/전북/제주도…… old    1233      2922  42.2
6 강원/충북       old     555      1257  44.2
7 대구/경북       old     928      2036  45.6
```

```r
# 지역명 순서 변수 만들기

order <- list_order_old$region
order

[1] "수도권(인천/경기"     
[2] "서울"                 
[3] "대전/충남"            
[4] "부산/경남/울산"       
[5] "광주/전남/전북/제주도"
[6] "강원/충북"            
[7] "대구/경북"  
```

지역명이 노년층 비율 순으로 정렬된 order 변수를 활용해 그래프 생성

앞에서 사용한 그래프 생성 코드에 **`scale_x_discrete()`** 를 추가하고 limits 파라미터에 order 변수를 지정

```r
ggplot(data = region_ageg, aes(x = region, y = pct, fill = ageg)) +
  geom_col() +
  coord_flip() +
  scale_x_discrete(limits = order)
```

![30.png](/assets/img/posts/R/R_study_4th/basic/30.png)

1. 연령대 순으로 막대 색깔 나열하기

앞에서 만든 그래프는 막대 색깔이 young, old, middle 순으로 나열됨

막대 색깔을 순서대로 나열하려면 fill 파라미터에 지정할 변수의 범주(levels) 순서를 지정하면 됨

현재 ageg 변수는 character 타입이기 때문에 levels가 없음

```r
class(region_ageg$ageg)

[1] "character"

levels(region_ageg$ageg)

NULL
```

factor()를 이용해 ageg 변수를 factor 타입으로 변환하고, level 파라미터를 이용해 순서 지정

```r
region_ageg$ageg <- factor(region_ageg$ageg,
                           level = c("old", "middle", "young"))

class(region_ageg$ageg)

[1] "factor"

levels(region_ageg$ageg)

[1] "old"    "middle" "young" 
```

```r
# 그래프 다시 생성

ggplot(data = region_ageg, aes(x = region, y = pct, fill = ageg)) +
  geom_col() +
  coord_flip() +
  scale_x_discrete(limits = order)
```

![31.png](/assets/img/posts/R/R_study_4th/basic/31.png)