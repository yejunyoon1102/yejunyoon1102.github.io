---
layout : post
title : "데이터 분석 : R 스터디 6 ~ 7장"
date : 2025-02-12
categories : [Data Analysis, R]
---
# 기본 발제

## 6장 : 자유자재로 데이터 가공하기

### 6-1 : 데이터 전처리 - 원하는 형태로 데이터 가공하기

**dplyr**은 데이터 전처리 작업에 가장 많이 사용되는 패키지

- filter() : 행 추출
- select() : 열(변수) 추출
- arrange() : 정렬
- mutate() : 변수 추가
- summarise() : 통계치 산출
- group_by() : 집단별로 나누기
- left_join() : 데이터 합치기(열)
- bind_rows() : 데이터 합치기(행)

### 6-2 : 조건에 맞는 데이터만 추출하기

dplyr 패키지의 **`filter()`**를 이용하면 원하는 데이터 추출 가능

**조건에 맞는 데이터만 추출하기**

1. 파일 불러오기

```r
exam <- read.csv("csv_exam.csv")
exam

   id class math english science
1   1     1   50      98      50
2   2     1   60      97      60
3   3     1   45      86      78
4   4     1   30      98      58
5   5     2   25      80      65
6   6     2   50      89      98
...
```

1. **`filter()`**를 이용해 ****1반 학생들의 데이터만 추출

```r
exam %>% filter(class == 1)

  id class math english science
1  1     1   50      98      50
2  2     1   60      97      60
3  3     1   45      86      78
4  4     1   30      98      58
```

dplyr 패키지는 **`%>%`** 기호(파이프 연산자)를 이용해 함수들을 나열하는 방식으로 코드 작성

→ exam을 출력하되, class 변수의 값이 1인 행만을 추출하라!

- ‘같다’ 조건 :  == (등호 2개)
- 함수의 파라미터 조건 : = (등호 1개)
1. 변수가 특정 값이 ‘아닌 경우’에 해당하는 데이터만 추출

```r
exam %>% filter(class != 1)

   id class math english science
1   5     2   25      80      65
2   6     2   50      89      98
3   7     2   80      90      45
4   8     2   90      78      25
5   9     3   20      98      15
6  10     3   50      98      45
...
```

- ‘같지 않다’ 조건 : != (등호 앞에 느낌표)

**초과, 미만, 이상, 이하 조건 걸기**

```r
exam %>% filter(math > 50)

   id class math english science
1   2     1   60      97      60
2   7     2   80      90      45
3   8     2   90      78      25
4  11     3   65      65      65
5  15     4   75      56      78
6  16     4   58      98      65
...
```

이 외에 

- < : 미만
- <= : 이하
- >= : 이상

**여러 조건을 충족하는 행 추출하기**

```r
# 1반이면서 수학 점수가 50점 이상인 경우

exam %>% filter(class ==1 & math >=50)

  id class math english science
1  1     1   50      98      50
2  2     1   60      97      60
```

‘그리고(and)’ 를 의미하는 기호 **`&`**를 사용해 조건 나열!

**여러 조건 중 하나 이상 충족하는 행 추출하기**

```r
# 수학 점수가 90점 이상이거나 영어 점수가 90점 이상인 경우

exam %>% filter(math >= 90 | english >= 90)

  id class math english science
1  1     1   50      98      50
2  2     1   60      97      60
3  4     1   30      98      58
4  7     2   80      90      45
5  8     2   90      78      25
6  9     3   20      98      15
...
```

‘또는 (or)’ 을 의미하는 기호 **`|`** (vertical bar) 를 사용해 조건 나열

**목록에 해당하는 행 추출하기**

```r
# 1, 3, 5반에 해당하면 추출

exam %>% filter(class ==1 | class == 3 | class == 5)

   id class math english science
1   1     1   50      98      50
2   2     1   60      97      60
3   3     1   45      86      78
4   4     1   30      98      58
5   9     3   20      98      15
6  10     3   50      98      45
...
```

%in% 기호(매치 연산자)를 사용하면 코드르 좀 더 간편하게 작성 가능!

**`%in%`** 기호는 변수의 값이 지정한 조건 목록에 해당하는지 확인하는 기능

```r
# 1, 3, 5반에 해당하면 추출

exam %>% filter(class %in% c(1, 3, 5))

   id class math english science
1   1     1   50      98      50
2   2     1   60      97      60
3   3     1   45      86      78
4   4     1   30      98      58
5   9     3   20      98      15
6  10     3   50      98      45
...
```

**추출한 행으로 데이터 만들기**

<- 기호를 이용하면 추출한 행으로 새로운 데이터 만들기 가능

```r
class1 <- exam %>% filter(class == 1)
class2 <- exam %>% filter(class == 2)

mean(class1$math)

[1] 46.25

mean(class2$math)

[1] 61.25
```

**R에서 사용하는 기호들**

- 논리 연산자 (조건을 지정할 때 사용하는 기호)
    - < : 작다
    - <= : 작거나 같다
    - >  : 크다
    - >=  : 크거나 같다
    - == : 같다
    - != : 같지 않다
    - | : 또는
    - & : 그리고
    - %in% : 매칭 확인
- 산술 연산자 (계산할 때 사용하는 기호)
    - + : 더하기
    - - : 빼기
    - * : 곱하기
    - / : 나누기
    - ^ , ** : 제곱
    - %/% : 나눗셈의 몫
    - %% : 나눗셈의 나머지

### 6-3 : 필요한 변수만 추출하기

**`select()`** 는 데이터에 들어 있는 수많은 변수 중 일부 변수만 추출해 활용하고자 할 때 사용

**변수 추출하기**

1. math 변수만 추출해 출력

```r
exam %>% select(math)

   math
1    50
2    60
3    45
4    30
5    25
6    50
...
```

1. 여러 변수 추출

```r
exam %>% select(class, math, english)

   class math english
1      1   50      98
2      1   60      97
3      1   45      86
4      1   30      98
5      2   25      80
6      2   50      89
...
```

쉼표를 넣어 변수명을 나열하면 여러 변수 동시에 추출 가능

1. 변수 제외

```r
exam %>% select(-math)

   id class english science
1   1     1      98      50
2   2     1      97      60
3   3     1      86      78
4   4     1      98      58
5   5     2      80      65
6   6     2      89      98
.
```

제외할 변수명 앞에 빼기 기호 - 를 입력

**dplyr 함수 조합하기**

dplyr 패키지의 함수들은 %>% 를 이용해 조합할 수 있다는 장점 존재

1. filter() 와 select() 조합하기

```r
# class가 1인 행만 추출한 다음 english 추출

exam %>% filter(class == 1) %>% select(english)

  english
1      98
2      97
3      86
4      98
```

1. 가독성 있게 줄 바꾸기

%>% 로 코드가 연결되는 부분에서 줄을 바꾸면 가독성 UP!!

실행할때는 dplyr 구문 전체를 함께 실행해야 됨에 주의

```r
# class가 1인 행만 추출한 다음 english 추출

exam %>%
  filter(class == 1) %>%
  select(english)
  
english
1      98
2      97
3      86
4      98
```

1. 일부만 추출하기

```r
exam %>%
  select(id, math) %>%
  head
  
id math
1  1   50
2  2   60
3  3   45
4  4   30
5  5   25
6  6   50
```

### 6-4 : 순서대로 정렬하기

**`arrange()`** 를 이용하면 데이터를 원하는 순서로 정렬 가능

정렬 기준으로 삼을 변수명을 () 안에 입력

**오름차순으로 정렬하기**

```r
exam %>% arrange(math)

   id class math english science
1   9     3   20      98      15
2   5     2   25      80      65
3   4     1   30      98      58
4   3     1   45      86      78
5  12     3   45      85      32
6  13     4   46      98      65
...
```

arrange()의 기본값이 오름차순 정렬!!

**내림차순으로 정렬하기**

```r
exam %>% arrange(desc(math))

   id class math english science
1   8     2   90      78      25
2  19     5   89      68      87
3   7     2   80      90      45
4  18     5   80      78      90
5  20     5   78      83      58
6  15     4   75      56      78
...
```

기준 변수를 **`desc()`** 에 적용!

**정렬 기준 변수 여러개 지정**

```r
exam %>% arrange(class, math)

   id class math english science
1   4     1   30      98      58
2   3     1   45      86      78
3   1     1   50      98      50
4   2     1   60      97      60
5   5     2   25      80      65
6   6     2   50      89      98
7   7     2   80      90      45
8   8     2   90      78      25
...
```

앞에 있는 변수(class) 기준으로 오름차순 정렬한 후,

각 class 안에서 math 점수를 기준으로 오름차순 정렬해 출력

### 6-5 : 파생변수 추가하기

**`mutate()`**를 사용하면 기존 데이터에 파생변수를 만들어 추가할 수 있음

새로 만들 변수명과 변수를 만들 때 사용할 공식 입력

**파생변수 추가하기**

1. 총합변수 생성

```r
exam %>%
  mutate(total = math + english + science) %>%
  head
  
id class math english science total
1  1     1   50      98      50   198
2  2     1   60      97      60   217
3  3     1   45      86      78   209
4  4     1   30      98      58   186
5  5     2   25      80      65   170
6  6     2   50      89      98   237
```

1. 여러 파생변수 한번에 추가

```r
exam %>%
  mutate(total = math + english + science, 
         mean = (math + english + science) / 3) %>%
  head
  
id class math english science total
1  1     1   50      98      50   198
2  2     1   60      97      60   217
3  3     1   45      86      78   209
4  4     1   30      98      58   186
5  5     2   25      80      65   170
6  6     2   50      89      98   237
      mean
1 66.00000
2 72.33333
3 69.66667
4 62.00000
5 56.66667
6 79.00000
```

쉼표를 이용해 한번에 추가!

1. mutate()에 ifelse() 적용하기

```r
exam %>% mutate(test = ifelse(science >= 60, "pass", "fail")) %>%
  head
  
id class math english science test
1  1     1   50      98      50 fail
2  2     1   60      97      60 pass
3  3     1   45      86      78 pass
4  4     1   30      98      58 fail
5  5     2   25      80      65 pass
6  6     2   50      89      98 pass
```

조건에 따라 다른 값 부여 가능

1. 추가한 변수를 dplyr 코드에 바로 활용하기

```r
exam %>%
  mutate(total = math + english + science) %>%  # 총합 변수 추가
  arrange(total) %>%  # 총합 변수 기준 정렬
  head  
  
id class math english science total
1  9     3   20      98      15   133
2 14     4   48      87      12   147
3 12     3   45      85      32   162
4  5     2   25      80      65   170
5  4     1   30      98      58   186
6  8     2   90      78      25   193
```

추가) dplyr 함수의 장점

- 변수명 앞에 데이터프레임을 반복해 입력하지 않기 때문에 코드가 간결해짐
    - ex) df$var_sum <- df$var1 + df$var2

### 6-6 집단별로 요약하기

집단별 평균이나 집단별 빈도처럼 각 집단을 요약한 값을 구할 때는 **`group_by()`** 와 **`summarise()`** 사용

- summarise()에 자주 사용하는 요약 통계량 함수
    - mean() : 평균
    - sd() : 표준편차
    - sum() : 합계
    - median() : 중앙값
    - min() : 최솟값
    - max() : 최댓값
    - n() : 빈도

**집단별로 요약하기**

1. summarise() 활용

```r
exam %>% summarise(mean_math = mean(math))

  mean_math
1     57.45
```

1. 집단별로 요약하기

```r
exam %>%
  group_by(class) %>%
  summarise(mean_math = mean(math))
  
# A tibble: 5 × 2
  class mean_math
  <int>     <dbl>
1     1      46.2
2     2      61.2
3     3      45  
4     4      56.8
5     5      78  
```

group_by()로 1차 데이터 분리 후에 summarise()로 요약 통계량 산출하는게 일반적!!

1. 여러 요약 통계량 한 번에 산출하기

```r
exam %>%
  group_by(class) %>%    # class별로 분리
  summarise(mean_math = mean(math),     # math 평균
            sum_math = sum(math),       # math 합계
            median_math = median(math), # math 중잉값 
            n = n())                    # 학생 수
            
# A tibble: 5 × 5
  class mean_math sum_math median_math     n
  <int>     <dbl>    <int>       <dbl> <int>
1     1      46.2      185        47.5     4
2     2      61.2      245        65       4
3     3      45        180        47.5     4
4     4      56.8      227        53       4
5     5      78        312        79       4
```

**`n()`** 은 데이터가 몇 행으로 되어 있는지 ‘빈도’를 구하는 기능을 함

여기서는 group_by()로 반 별로 집단을 나눴으니 각 반에 몇 명이 있는지 나타냄

n()은 특정 변수에 적용하는게 아니라 행 개수를 세는 것이기 때문에 괄호 안에 변수명 입력 X

1. 각 집단별로 다시 집단 나누기

```r
mpg %>%
  group_by(manufacturer, drv) %>%       # 회사별, 구동 방식별 분리
  summarise(mean_cty = mean(cty)) %>%   # cty 평균 산출
  head(10)
  
# A tibble: 10 × 3
# Groups:   manufacturer [5]
   manufacturer drv   mean_cty
   <chr>        <chr>    <dbl>
 1 audi         4         16.8
 2 audi         f         18.9
 3 chevrolet    4         12.5
 4 chevrolet    f         18.8
 5 chevrolet    r         14.1
 6 dodge        4         12  
 7 dodge        f         15.8
 8 ford         4         13.3
 9 ford         r         14.8
10 honda        f         24.4
```

**dplyr 조합하기**

problem) 회사별로 “suv” 자동차의 도시 및 고속도로 통합 연비 평균을 구해 내림차순으로 정렬하고, 1~5위까지 출력

1. 알고리즘 설계
- 회사별로 분리 → group_by()
- suv 추출 → filter()
- 통합 연비 변수 생성 → mutate()
- 통합 연비 평균 산출 → summarise()
- 내림차순 정렬 → arrange()
- 1~5위까지 출력 → head()
1. 구문 생성 (%>%로 연결)

```r
mpg %>%
  group_by(manufacturer) %>%
  filter(class == "suv") %>%
  mutate(tot = (cty + hwy)/2) %>%
  summarise(mean_tot = mean(tot)) %>%
  arrange(desc(mean_tot)) %>%
  head(5)
  
# A tibble: 5 × 2
  manufacturer mean_tot
  <chr>           <dbl>
1 subaru           21.9
2 toyota           16.3
3 nissan           15.9
4 mercury          15.6
5 jeep             15.6
```

### 6-7 : 데이터 합치기

여러 데이터를 합쳐 하나의 데이터로 만들 때??

- 열 추가 (JOIN)
- 행 추가 (UNION)

**가로로 합치기**

1. 데이터 프레임 생성

```r

test1 <- data.frame(id = c(1, 2, 3, 4, 5),
                    midterm = c(60, 80, 70, 90, 85))

test2 <- data.frame(id = c(1, 2, 3, 4, 5),
                    final = c(70, 83, 65, 95, 80))

test1

  id midterm
1  1      60
2  2      80
3  3      70
4  4      90
5  5      85
test2

  id final
1  1    70
2  2    83
3  3    65
4  4    95
5  5    80
```

1. dplyr 패키지의 **`left_join()`** 함수 이용

괄호 안에 합칠 데이터 프레임 나열, 기준으로 삼을 변수명을 by에 지정

by에 기준 변수를 지정할 때 변수명 앞뒤에 반드시 따옴표 입력!!

```r
total <- left_join(test1, test2, by = "id")
total

  id midterm final
1  1      60    70
2  2      80    83
3  3      70    65
4  4      90    95
5  5      85    80
```

**다른 데이터를 활용해 변수 추가하기**

1. 외부 데이터 생성

```r
name <- data.frame(class = c(1, 2, 3, 4, 5),
                   teacher = c("kim", "lee", "park", "choi", "jung"))
name

class teacher
1     1     kim
2     2     lee
3     3    park
4     4    choi
5     5    jung
```

1. class 변수를 기준으로 조인

```r
exam_new <- left_join(exam, name, by="class")
exam_new

id class math english science teacher
1   1     1   50      98      50     kim
2   2     1   60      97      60     kim
3   3     1   45      86      78     kim
4   4     1   30      98      58     kim
5   5     2   25      80      65     lee
6   6     2   50      89      98     lee
...
```

**세로로 합치기**

1. 데이터프레임 생성

```r
group_a <- data.frame(id = c(1, 2, 3, 4, 5),
                      test = c(60, 80, 70, 90, 85))

group_b <- data.frame(id = c(6, 7, 8, 9, 10),
                      test = c(70, 83, 65, 95, 80))

group_a

id test
1  1   60
2  2   80
3  3   70
4  4   90
5  5   85

group_b

id test
1  6   70
2  7   83
3  8   65
4  9   95
5 10   80
```

1. **`bind_rows()`**를 이용해 데이터를 세로로 합치기

```r
group_all <- bind_rows(group_a, group_b)
group_all

   id test
1   1   60
2   2   80
3   3   70
4   4   90
5   5   85
6   6   70
7   7   83
8   8   65
9   9   95
10 10   80
```

데이터를 세로로 합칠 때는 두 데이터의 변수명이 같아야!!

변수명이 다르다면 rename()을 통해 동일하게 맞춘 후에 합치기

## 7장 : 데이터 정제

### 7-1 : 결측치 정제

**결측치 찾기**

1. R에서는 결측치를 대문자 NA로 표기함

```r
df <- data.frame(sex = c("M", "F", NA, "M", "F"),
                 score = c(5, 4, 3, 4, NA))
df

sex score
1    M     5
2    F     4
3 <NA>     3
4    M     4
5    F    NA
```

NA 앞뒤에 따옴표가 없다는 것에 유의!!

따옴표가 있으면 결측치가 아니라 영문자 “NA”를 의미하게 됨

(문자로 구성된 변수는 NA가 <>에 감싸진 형태로 출력)

1. 결측치 확인하기

**`is.na()`** 를 이용하면 데이터에 결측치가 들어 있는지 알 수 있음!

```r
 is.na(df)
 
       sex score
[1,] FALSE FALSE
[2,] FALSE FALSE
[3,]  TRUE FALSE
[4,] FALSE FALSE
[5,] FALSE  TRUE
```

is. 으로 시작하는 함수는 해당 변수가 특정 속성을 지니고 있는지 확인한 후 TRUE 또는 FALSE 값을 반환하는 기능!

1. is.na()를 **`table()`**에 적용하면 데이터에 결측치가 몇 개 있는지 출력

```r
table(is.na(df))

FALSE  TRUE 
    8     2 
```

1. **`table(is.na())`**에 변수명을 지정하면 해당 변수에 결측치가 몇 개 있는지 알 수있음

```r
table(is.na(df$sex))

FALSE  TRUE 
    4     1 
    
table(is.na(df$score))

FALSE  TRUE 
    4     1 
```

1. 결측치가 포함된 데이터를 함수에 적용하면 정상적으로 연산 X

```r
mean(df$score)

[1] NA

sum(df$score)

[1] NA
```

**결측치 제거하기**

1. 결측치 있는 행 제거

is.na()를 **`filter()`**에 적용하면 결측치가 있는 행을 제거할 수 있음

```r
# score가 NA인 데이터만 력

df %>% filter(is.na(score))
  
  sex score
1   F    NA
```

1. **`!is.na()`** 활용

NA가 아닌 값, 결측치가 아닌 값을 입력하고, filter()에 적용하면 결측치를 제외하고 행을 추출

```r
df %>% filter(!is.na(score))

   sex score
1    M     5
2    F     4
3 <NA>     3
4    M     4
```

1. 추출한 데이터로 데이터 프레임 생성

```r
df_nomiss <- df %>% filter(!is.na(score))

mean(df_nomiss$score)

[1] 4

sum(df_nomiss$score)

[1] 16
```

1. 여러 변수를 동시에 결측치 제거해 데이터 추출

```r
df_nomiss <- df %>% filter(!is.na(score) & !is.na(sex))
df_nomiss

sex score
1   M     5
2   F     4
3   M     4
```

filter() 에 & 기호를 이용해 조건을 나열!

1. 결측치가 하나라도 있으면 제거하기

일일이 변수를 지정하지 않아도 **`na.omit()`** 함수를 이용하면 한 번에 제거 가능

```r
df_nomiss2 <- na.omit(df)
df_nomiss2

  sex score
1   M     5
2   F     4
4   M     4
```

한 번에 모두 제거하기 때문에 간편하지만,

분석에 필요한 행까지 손실된다는 단점!!

ex) 분석에 의미없는 변수에 결측치가 있다면, 정작 필요한 변수의 데이터까지 없어진다는 단점

→ filter()를 이용해 분석에 사용할 변수의 결측치만 제거하는 방식 권장

**함수의 결측치 제외 기능 이용하기**

mean()과 같은 수치 연산 함수들은 결측치를 제외하고 연산하도록 설정하는 **`na.rm`** 파라미터 지원!! (NA Remove)

na.rm을 TRUE로 설정하면 결측치를 제외하고 함수 적용

1. na.rm 파라미터 사용

```r
mean(df$score, na.rm = T)

[1] 4

sum(df$score, na.rm = T)

[1] 16
```

1. summarise()를 이용해 요약 통계량을 산출할 때도 na.rm 적용 가능

```r
# 결측치 임의 생성

exam <- read.csv("csv_exam.csv")
exam[c(3, 8, 15), "math"] <- NA  # 3, 8, 15행의 math에 NA 할당
exam

   id class math english science
1   1     1   50      98      50
2   2     1   60      97      60
3   3     1   NA      86      78
4   4     1   30      98      58
5   5     2   25      80      65
6   6     2   50      89      98
7   7     2   80      90      45
8   8     2   NA      78      25
...
```

코드에서 [ ]는 데이터의 위치를 지정하는 역할

대괄호 안에서 쉼표 왼쪽은 “행 위치”, 쉼표 오른쪽은 “열 위치를 의미”

```r
exam %>% summarise(mean_math = mean(math))

  mean_math
1        NA
```

math는 결측치를 포함하고 있기 때문에 수치 연산을 했을 때 NA가 출력되는 모습

1. na.rm 파리미터 이용

```r
exam %>% summarise(mean_math = mean(math, na.rm = T))

  mean_math
1  55.23529
```

1. 다른 수치 연산 함수에도 na.rm 적용

```r
exam %>% summarise(mean_math = mean(math, na.rm = T),
                   sum_math = sum(math, na.rm = T),
                   median_math = median(math, na.rm = T))
                   
mean_math sum_math median_math
1  55.23529      939          50
```

**결측치 대체하기**

- 평균이나 최빈값 같은 대표값으로 일괄 대체하기
- 통계 분석 기법으로 각 결측치의 예측값을 추정해 대체하기

**평균값으로 결측치 대체하기**

1. 앞에서 만든 exam 데이터에서 3, 8, 15행의 결측치를 평균값으로 대체

```r
mean(exam$math, na.rm =T)

[1] 55.23529
```

1. ifelse()를 이용해 NA값을 평균값으로 대체

```r
exam$math <- ifelse(is.na(exam$math), 55, exam$math)
table(is.na(exam$math))

FALSE 
   20 
   
exam

id class math english science
1   1     1   50      98      50
2   2     1   60      97      60
3   3     1   55      86      78
4   4     1   30      98      58
5   5     2   25      80      65
6   6     2   50      89      98
7   7     2   80      90      45
8   8     2   55      78      25
...
```

```r
mean(exam$math)

[1] 55.2
```

결측치가 없으므로 연산 함수를 적용하면 값이 정상적으로 출력

### 이상치 정제하기

정상 범주에서 크게 벗어난 값을 이상치(Outlier)라고 함

**이상치 제거하기 - 존재할 수 없는 값**

1. 이상치가 포함된 데이터 생성

```r
outlier <- data.frame(sex = c(1, 2, 1, 3, 2, 1),
                      score = c(5, 4, 3, 4, 2, 6))
outlier

sex score
1   1     5
2   2     4
3   1     3
4   3     4
5   2     2
6   1     6
```

1. 이상치 확인하기

table()을 이용한 빈도표를 생성 후 파악

```r
table(outlier$sex)

1 2 3 
3 2 1 

table(outlier$score)

2 3 4 5 6 
1 1 2 1 1
```

성별에 존재할 수 없는 값인 3 존재, 점수에 존재할 수 없는 값인 6 존재

1. 결측 처리하기

이상치를 결측치로 변환 → ifelse()를 이용해 이상치일 경우 NA 부여

```r
outlier$sex <- ifelse(outlier$sex ==3, NA, outlier$sex)
outlier

 sex score
1   1     5
2   2     4
3   1     3
4  NA     4
5   2     2
6   1     6

outlier$score <- ifelse(outlier$score > 5, NA, outlier$score)
outlier

 sex score
1   1     5
2   2     4
3   1     3
4  NA     4
5   2     2
6   1    NA
```

1. 결측치 제외

```r
# 성별에 따른 score 평균 구하기

outlier %>%
  filter(!is.na(sex) & !is.na(score)) %>%
  group_by(sex) %>%
  summarise(mean_score = mean(score))

# A tibble: 2 × 2
    sex mean_score
  <dbl>      <dbl>
1     1          4
2     2          3
```

**이상치 제거하기 - 극단적인 값**

논리적으로 존재할 수 있지만 극단적으로 크거나 작은 값을 ‘극단치’라고 함

어디까지를 정상 범위를 볼 것인지 정해야 함!!

- 논리적으로 판단
- 통계적인 기준 이용!
    - boxplot으로 중심에서 크게 벗어난 값을 극단치로 간주
        
        ![1.png](/assets/img/posts/R/R_study_3rd/basic/1.png)
        

**boxplot으로 극단치 기준 정하기**

1. boxplot() 함수로 박스플롯 생성

```r
boxplot(mpg$hwy)
```

![2.png](/assets/img/posts/R/R_study_3rd/basic/2.png)

hwy값을 크기 순으로 나열했을 때 하위 25% 지점에 18, 중앙에 24, 75% 지점에 27이  위치한다는 것을 알 수 있음

0 ~ 12 / 37 ~ 값이 극단치로 분류된다는 것을 알 수 있음!!

1. 박스플롯의 5가지 통계치 출력

```r
boxplot(mpg$hwy)$stats

     [,1]
[1,]   12
[2,]   18
[3,]   24
[4,]   27
[5,]   37
```

순서대로

- 아래쪽 극단치 경계
- 1사분위수
- 중앙값
- 3사분위수
- 위쪽 극단치 경계
1. 결측 처리하기

```r
# 12 ~ 37 벗어나면 NA 할당

mpg$hwy <- ifelse(mpg$hwy < 12 | mpg$hwy > 37, NA, mpg$hwy)
table(is.na(mpg$hwy))

FALSE  TRUE 
  231     3 
```

1. 결측치를 제외하고 분석 수행

```r
# 엔진 구동 방식에 따른 연비 평균 구하기

mpg %>%
  group_by(drv) %>%
  summarise(mean_by = mean(hwy, na.rm = T))
  
# A tibble: 3 × 2
  drv   mean_by
  <chr>   <dbl>
1 4        19.2
2 f        27.7
3 r        21  
```