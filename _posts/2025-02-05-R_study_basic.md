---
layout : post
title : "데이터 분석 : R 스터디 4 ~ 5장"
date : 2025-02-05
categories : [Data Analysis, R]
---
# 기본 발제

## 4장 : 데이터 프레임의 세계로!

### **데이터 프레임 이해하기**

데이터의 다양성 : ‘열’ 이 많은 것!!

- 행이 많다 → 컴퓨터가 느려짐 → 고사양 장비 구축
- 열이 많다 → 분석 방법의 한계 → 고급 분석 방법

### **데이터 프레임 만들기**

![1.png](/assets/img/posts/R/R_study_2nd/basic/1.png)

1. 변수 만들기

```r
english <- c(90, 80, 60, 70)
english

[1] 90 80 60 70

math <- c(50, 60, 100, 20)
math

[1] 50  60 100  20
```

1. 데이터 프레임 만들기

데이터 프레임을 만들 때는 **`data.frame()`** 을 이용

데이터 프레임을 구성할 변수를 괄호 안에 쉼표로 나열하면 됨

```r
df_midterm <- data.frame(english, math)
df_midterm

  english math
1      90   50
2      80   60
3      60  100
4      70   20
```

1. 학생의 반에 대한 정보 추가

```r
class<- c(1, 1, 2, 2)
class

[1] 1 1 2 2

df_midterm <- data.frame(english, math, class)
df_midterm

  english math class
1      90   50     1
2      80   60     1
3      60  100     2
4      70   20     2
```

1. 분석하기

**`mean()`** 을 이용해 평균 계산

```r
mean(df_midterm$english)

[1] 75

mean(df_midterm$math)

[1] 57.5
```

**`달러 기호($)`**는 데이터 프레임 안에 있는 변수를 지정할 때 사용

1. 데이터 프레임 한 번에 만들기

```r
df_midterm <- data.frame(english = c(90, 80, 60, 70), 
                         math = c(50, 60, 100, 20),
                         class= c(1, 1, 2, 2))
df_midterm

  english math class
1      90   50     1
2      80   60     1
3      60  100     2
4      70   20     2
```

코드가 길어질 경우 쉼표 뒤에서 Enter로 다음 줄로 넘기자!

### **외부 데이터 이용하기**

**엑셀 파일 불러오기**

1. 데이터 파일을 불러오기 위해 **가장 먼저** 해야 하는 작업은 현재 작업 중인 프로젝트 폴더에 불러올 파일을 삽입하는 것!!!!!!
2. readxl 패키지 설치하고 로드하기

```r
install.packages("readxl")
library(readxl)
```

1. 엑셀 파일 불러오기

readxl 패키지에서 제공하는 **`read_excel()`**을 이용해 엑셀 파일을 불러옴

엑셀 파일을 데이터 프레임으로 만드는 기능!

괄호 안에 불러올 엑셀 파일명을 넣어야 함, 확장자(.xlsx)까지 기입해야 하고, 양쪽에 따옴표(”)가 들어가야 함

R에서는 파일명을 지정할 때 항상 앞 뒤에 따옴표를 넣음!!!

```r
df_exam <- read_excel("excel_exam.xlsx")
df_exam

# A tibble: 20 × 5
      id class  math english science
   <dbl> <dbl> <dbl>   <dbl>   <dbl>
 1     1     1    50      98      50
 2     2     1    60      97      60
 3     3     1    45      86      78
 4     4     1    30      98      58
 5     5     2    25      80      65
 6     6     2    50      89      98
 7     7     2    80      90      45
 8     8     2    90      78      25
 9     9     3    20      98      15
10    10     3    50      98      45
11    11     3    65      65      65
12    12     3    45      85      32
13    13     4    46      98      65
14    14     4    48      87      12
15    15     4    75      56      78
16    16     4    58      98      65
17    17     5    65      68      98
18    18     5    80      78      90
19    19     5    89      68      87
20    20     5    78      83      58
```

프로젝트 폴더가 아닌 다른 폴더에 있는 엑셀 파일을 불러오려면 파일 경로를 지정하면 됨

경로를 지정할 때는 슬래시(/) 사용

```r
# 예시

df_exam <- read_excel("d:/easy_r/excel_exam.xlsx")
```

1. 분석하기

```r
 mean(df_exam$english)

[1] 84.9

mean(df_exam$science)

[1] 59.45
```

**엑셀 파일 첫 번째 행이 변수 명이 아니라면?**

![2.png](/assets/img/posts/R/R_study_2nd/basic/2.png)

첫 번째 행의 데이터가 변수명으로 지정되면서 유실되는 문제 발생!

```r
df_exam_novar <- read_excel("excel_exam_novar.xlsx")

New names:
• `1` -> `1...1`
• `1` -> `1...2`
• `50` -> `50...3`
• `50` -> `50...5`

df_exam_novar

# A tibble: 7 × 5
  `1...1` `1...2` `50...3`  `98` `50...5`
    <dbl>   <dbl>    <dbl> <dbl>    <dbl>
1       2       1       60    97       60
2       3       2       25    80       65
3       4       2       50    89       98
4       5       3       20    98       15
5       6       3       50    98       45
6       7       4       46    98       65
7       8       4       48    87       12
```

원본과 다르게 7행까지만 구성된 모습

이럴 때 **`col_names = F`** 파라미터를 설정하면 첫 번째 행을 변수명이 아닌 데이터로 인식해 불러옴

변수명은 ‘…숫자’ 로 자동 지정됨

```r
df_exam_novar <- read_excel("excel_exam_novar.xlsx", col_names = F)
New names:
• `` -> `...1`
• `` -> `...2`
• `` -> `...3`
• `` -> `...4`
• `` -> `...5`

df_exam_novar

# A tibble: 8 × 5
   ...1  ...2  ...3  ...4  ...5
  <dbl> <dbl> <dbl> <dbl> <dbl>
1     1     1    50    98    50
2     2     1    60    97    60
3     3     2    25    80    65
4     4     2    50    89    98
5     5     3    20    98    15
6     6     3    50    98    45
7     7     4    46    98    65
8     8     4    48    87    12
```

T/F = 참(TRUE)과 거짓(FALSE)중 하나로 구성되는 논리형 벡터

col_names = F → 열 이름을 가져올 것인가? 아니오!

**엑셀 파일에 시트가 여러 개 있다면?**

sheet 파라미터를 이용해 몇 번째 시트의 데이터를 불러올지 지정 가능!!

```r
df_exam_sheet <- read_excel("excel_exam_sheet.xlsx", sheet = 3)
df_exam_sheet

# A tibble: 8 × 5
     id class  math english science
  <dbl> <dbl> <dbl>   <dbl>   <dbl>
1     1     1    50      98      50
2     2     1    60      97      60
3     3     2    25      80      65
4     4     2    50      89      98
5     5     3    20      98      15
6     6     3    50      98      45
7     7     4    46      98      65
8     8     4    48      87      12
```

**CSV 파일 불러오기**

CSV 파일은 엑셀뿐 아니라 SAS, SPSS 등 데이터를 다루는 대부분의 프로그램에서 읽고 쓰기가 가능한 범용 데이터 파일

1. CSV 파일 불러오기

별도의 패키지를 설치하지 않고 R에 기본적으로 내장된 **`read.csv()`** 이용

```r
df_csv_exam <- read.csv("csv_exam.csv")
df_csv_exam

   id class math english science
1   1     1   50      98      50
2   2     1   60      97      60
3   3     1   45      86      78
4   4     1   30      98      58
5   5     2   25      80      65
6   6     2   50      89      98
7   7     2   80      90      45
8   8     2   90      78      25
9   9     3   20      98      15
10 10     3   50      98      45
11 11     3   65      65      65
12 12     3   45      85      32
13 13     4   46      98      65
14 14     4   48      87      12
15 15     4   75      56      78
16 16     4   58      98      65
17 17     5   65      68      98
18 18     5   80      78      90
19 19     5   89      68      87
20 20     5   78      83      58
```

첫 번째 행에 변수명이 없는 CSV 파일을 불러올 때는 **`header = F`** 파라미터 지정

(read_excel() 의 col_names = F 와 기능은 동일하지만 파라미터 이름이 다름)

**데이터 프레임을 CSV 파일로 저장하기**

```r
df_midterm <- data.frame(english = c(90, 80, 60, 70),
                                                 math = c(50, 60, 100, 20), 
                                                 class = c(1, 1, 2, 2))

write.csv(df_midterm, file = "df_midterm.csv")
```

R 내장 함수인 **`write.csv()`**를 이용해 데이터프레임을 CSV 파일로 저장

괄호 안에 저장할 데이터 프레임명을 지정하고, file 파라미터에 파일명 지정

저장한 파일은 프로젝트 폴더에 생성

**RDS 파일 활용하기**

RDS 파일은 R 전용 데이터 파일

R 전용 파일이므로 다른 파일들에 비해 R에서 읽고 쓰는 속도가 빠르고 용량이 적음

일반적으로 R에서 분석 작업을 할 때는 RDS 파일을 이용, R을 사용하지 않는 사람과 파일을 주고받을 때는 CSV 파일을 이용

1. 데이터프레임을 RDS 파일로 저장

**`saveRDS()`**를 이용해 데이터 프레임을 .rds 파일로 저장

괄호 안에 데이터 프레임명과 저장할 파일명을 지정

```r
saveRDS(df_midterm, file = "df_midterm.rds")
```

1. RDS 파일 불러오기

**`readRDS()`** 를 이용해 파일 불러오기

**`rm()`**을 이용해 데이터 삭제

```r
rm(df_midterm)
df_midterm

## Error in eval(expr, envir, enclos) : object 'df_midterm' not found
```

```r
df_midterm <- readRDS("df_midterm.rds")

df_midterm

 english math class
1      90   50     1
2      80   60     1
3      60  100     2
4      70   20     2
```

## 5장 : 데이터 분석 기초! - 데이터 파악하기, 다루기 쉽게 수정하기

### 데이터 파악하기

**데이터를 파악할 때 사용하는 함수들**

- head() : 데이터 앞부분 출력
- tail() : 데이터 뒷부분 풀력
- View() : 뷰어 창에서 데이터 확인
- dim() : 데이터 차원 출력
- str() : 데이터 속성 출력
- summary() : 요약 통계량 출력

 

```r
exam <- read.csv("csv_exam.csv")
```

1. head() : 데이터 앞부분 확인하기

앞에서부터 여섯 번째 행까지 출력!

```r
head(exam)

  id class math english science
1  1     1   50      98      50
2  2     1   60      97      60
3  3     1   45      86      78
4  4     1   30      98      58
5  5     2   25      80      65
6  6     2   50      89      98
```

데이터 프레임 이름 뒤에 쉼표를 쓰고 숫자를 입력하면 입력한 행까지의 데이터를 출력

```r
head(exam, 10)

   id class math english science
1   1     1   50      98      50
2   2     1   60      97      60
3   3     1   45      86      78
4   4     1   30      98      58
5   5     2   25      80      65
6   6     2   50      89      98
7   7     2   80      90      45
8   8     2   90      78      25
9   9     3   20      98      15
10 10     3   50      98      45
```

1. tail() : 데이터 뒷부분 확인하기

```r
tail(exam)

   id class math english science
15 15     4   75      56      78
16 16     4   58      98      65
17 17     5   65      68      98
18 18     5   80      78      90
19 19     5   89      68      87
20 20     5   78      83      58

tail(exam, 10)

   id class math english science
11 11     3   65      65      65
12 12     3   45      85      32
13 13     4   46      98      65
14 14     4   48      87      12
15 15     4   75      56      78
16 16     4   58      98      65
17 17     5   65      68      98
18 18     5   80      78      90
19 19     5   89      68      87
20 20     5   78      83      58
```

1. View() : 뷰어 창에서 데이터 확인하기

View()는 엑셀과 유사하게 생긴 ‘뷰어 창’에 원자료를 직접 보여 주는 기능

원자료를 눈으로 직접 확인해 보고 싶을 때 사용

맨 앞의 V는 대문자로 입력해야 함에 주의!!

```r
View(exam)
```

![3.png](/assets/img/posts/R/R_study_2nd/basic/3.png)

1. dim() : 데이터가 몇 행, 몇 열로 구성되어 있는지 알아보기

```r
dim(exam)

[1] 20  5
```

1. str() : 속성 파악하기

모든 변수의 속성을 한 눈에 파악하고 싶을 때 사용

```r
str(exam)

'data.frame':	20 obs. of  5 variables:
 $ id     : int  1 2 3 4 5 6 7 8 9 10 ...
 $ class  : int  1 1 1 1 2 2 2 2 3 3 ...
 $ math   : int  50 60 45 30 25 50 80 90 20 50 ...
 $ english: int  98 97 86 98 80 89 90 78 98 98 ...
 $ science: int  50 60 78 58 65 98 45 25 15 45 ...
```

출력 결과의 첫 번째 행에는 데이터의 속성이 무엇인지, 몇 개의 관측치와 변수로 구성되어 있는지 표시

1. summary() : 요약 통계량 산출하기

변수의 값을 요약한 ‘요약 통계량’ 을 산출하는 함수

```r
summary(exam)

       id            class        math      
 Min.   : 1.00   Min.   :1   Min.   :20.00  
 1st Qu.: 5.75   1st Qu.:2   1st Qu.:45.75  
 Median :10.50   Median :3   Median :54.00  
 Mean   :10.50   Mean   :3   Mean   :57.45  
 3rd Qu.:15.25   3rd Qu.:4   3rd Qu.:75.75  
 Max.   :20.00   Max.   :5   Max.   :90.00  
    english        science     
 Min.   :56.0   Min.   :12.00  
 1st Qu.:78.0   1st Qu.:45.00  
 Median :86.5   Median :62.50  
 Mean   :84.9   Mean   :59.45  
 3rd Qu.:98.0   3rd Qu.:78.00  
 Max.   :98.0   Max.   :98.00  
```

- Min : 최솟값
- 1st Qu : 1사분위수
- Median : 중앙값
- Mean : 평균
- 3rd Qu : 3사분위수
- Max : 최댓값

**mpg 데이터 파악하기**

```r
mpg <- as.data.frame(ggplot2::mpg)
```

**`as.data.frame()`** 은 데이터 속성을 데이터 프레임 형태로 바꾸는 함수

ggplot2 :: mpg는 ggplot2에 들어있는 mpg 데이터를 지칭하는 코드

- **`더블 콜론(::)`**을 이용하면 특정 패키지에 들어 있는 함수나 데이터를 지정 가능
1. head(), tail(), View()를 이용해 데이터 확인

```r
head(mpg)

  manufacturer model displ year cyl
1         audi    a4   1.8 1999   4
2         audi    a4   1.8 1999   4
3         audi    a4   2.0 2008   4
4         audi    a4   2.0 2008   4
5         audi    a4   2.8 1999   6
6         audi    a4   2.8 1999   6
       trans drv cty hwy fl   class
1   auto(l5)   f  18  29  p compact
2 manual(m5)   f  21  29  p compact
3 manual(m6)   f  20  31  p compact
4   auto(av)   f  21  30  p compact
5   auto(l5)   f  16  26  p compact
6 manual(m5)   f  18  26  p compact
```

```r
tail(mpg)

    manufacturer  model displ year cyl
229   volkswagen passat   1.8 1999   4
230   volkswagen passat   2.0 2008   4
231   volkswagen passat   2.0 2008   4
232   volkswagen passat   2.8 1999   6
233   volkswagen passat   2.8 1999   6
234   volkswagen passat   3.6 2008   6
         trans drv cty hwy fl   class
229   auto(l5)   f  18  29  p midsize
230   auto(s6)   f  19  28  p midsize
231 manual(m6)   f  21  29  p midsize
232   auto(l5)   f  16  26  p midsize
233 manual(m5)   f  18  26  p midsize
234   auto(s6)   f  17  26  p midsize
```

```r
View(mpg)
```

![4.png](/assets/img/posts/R/R_study_2nd/basic/4.png)

1. dim()을 이용해 데이터가 몇 행, 몇 열로 구성되어 있는지 확인

```r
dim(mpg)

[1] 234  11
```

자동차 234종에 대한 11개 변수로 구성되어 있다는 것

1. str()을 이용해 각 변수의 속성을 확인

```r
str(mpg)

'data.frame':	234 obs. of  11 variables:
 $ manufacturer: chr  "audi" "audi" "audi" "audi" ...
 $ model       : chr  "a4" "a4" "a4" "a4" ...
 $ displ       : num  1.8 1.8 2 2 2.8 2.8 3.1 1.8 1.8 2 ...
 $ year        : int  1999 1999 2008 2008 1999 1999 2008 1999 1999 2008 ...
 $ cyl         : int  4 4 4 4 6 6 6 4 4 4 ...
 $ trans       : chr  "auto(l5)" "manual(m5)" "manual(m6)" "auto(av)" ...
 $ drv         : chr  "f" "f" "f" "f" ...
 $ cty         : int  18 21 20 21 16 18 18 18 16 20 ...
 $ hwy         : int  29 29 31 30 26 26 27 26 25 28 ...
 $ fl          : chr  "p" "p" "p" "p" ...
 $ class       : chr  "compact" "compact" "compact" "compact" ...
```

mpg 데이터가 데이터 프레임이고, 234개의 관측치, 11개의 변수로 구성

manufacturer열의 값은 문자(chr, Character)로 이루어짐

- chr : 문자(Character)
- num : 소수점이 있는 실수(Numeric)
- int : 소수점이 없는 정수(Integer)

추가) 패키지에 들어 있는 데이터는 help 함수를 이용해 설명 글 확인 가능

→ ?mpg

1. summary()를 이용해 요약 통계량 확인

```r
summary(mpg)

 manufacturer          model          
 Length:234         Length:234        
 Class :character   Class :character  
 Mode  :character   Mode  :character  
                                      
                                      
                                      
     displ            year     
 Min.   :1.600   Min.   :1999  
 1st Qu.:2.400   1st Qu.:1999  
 Median :3.300   Median :2004  
 Mean   :3.472   Mean   :2004  
 3rd Qu.:4.600   3rd Qu.:2008  
 Max.   :7.000   Max.   :2008  
      cyl           trans          
 Min.   :4.000   Length:234        
 1st Qu.:4.000   Class :character  
 Median :6.000   Mode  :character  
 Mean   :5.889                     
 3rd Qu.:8.000                     
 Max.   :8.000                     
     drv                 cty       
 Length:234         Min.   : 9.00  
 Class :character   1st Qu.:14.00  
 Mode  :character   Median :17.00  
                    Mean   :16.86  
                    3rd Qu.:19.00  
                    Max.   :35.00  
      hwy             fl           
 Min.   :12.00   Length:234        
 1st Qu.:18.00   Class :character  
 Median :24.00   Mode  :character  
 Mean   :23.44                     
 3rd Qu.:27.00                     
 Max.   :44.00                     
    class          
 Length:234        
 Class :character  
 Mode  :character  
```

숫자로 된 변수는 여섯 가지 요약 통계량

문자로 된 변수는 요약 통계량을 계산할 수 없으니 값의 개수(Length)와 변수의 속성(Class, Mode)를 보여줌

### 변수명 바꾸기

데이터의 전반적인 특징을 파악하고 나면 이해하기 쉬운 단어로 변수명 수정 필요

**변수명 바꾸기**

1. 데이터 프레임 생성

```r
df_raw <- data.frame(var1 = c(1, 2, 1),
                      var2 = c(2, 3, 2))
df_raw

  var1 var2
1    1    2
2    2    3
3    1    2
```

1. rename()을 이용하기 위해 dplyr 패키지 설치하고 로드 (데이터를 원하는 형태로 가공할 시 사용)

```r
install.packages("dplyr")
library(dplyr)
```

1. 데이터 프레임 복사본 만들기

변수명을 바꾸기 전에 원본을 보유하기 위해 df_new라는 데이터 프레임 복사본 생성

데이터를 변형하는 작업을 할 때는 원본을 직접 사용하기보다 복사본을 만들어 사용하는 습관 필요!!!

```r
df_new <- df_raw
df_new

  var1 var2
1    1    2
2    2    3
3    1    2
```

1. 변수명 바꾸기

**`rename()`**을 이용해 변수명 변경

**`‘데이터 프레임명, 새 변수명 = 기존 변수명’`** 입력!!

순서가 바뀌면 실행되지 않으니 유의!!

```r
df_new <- rename(df_new, v2 = var2)
df_new

  var1 v2
1    1  2
2    2  3
3    1  2
```

1. 원본과 비교

```r
df_raw

  var1 var2
1    1    2
2    2    3
3    1    2
df_new

  var1 v2
1    1  2
2    2  3
3    1  2
```

### 파생변수 만들기

기존의 변수를 변형해 만든 변수를 ‘파생변수(Derived Variable)’ 라고 함

**변수 조합해 파생변수 만들기**

1. 데이터 프레임 생성

```r
df <- data.frame(var1 = c(4, 3, 8),
                 var2 = c(2, 6, 1))
df

  var1 var2
1    4    2
2    3    6
3    8    1
```

1. var1 과 var2 변수의 값을 더한 var_sum 이라는 파생 변수를 만들어 추가

```r
df$var_sum <- df$var1 + df$var2
df

  var1 var2 var_sum
1    4    2       6
2    3    6       9
3    8    1       9
```

데이터 프레임명에 $를 붙여 새로 만들 변수명 입력, <-로 계산 공식 할당

1. var1 과 var2를 더한 후 2로 나눠 var_mean이라는 파생변수 추가

```r
df$var_mean <- (df$var1 + df$var2)/2
df

  var1 var2 var_sum var_mean
1    4    2       6      3.0
2    3    6       9      4.5
3    8    1       9      4.5
```

**mpg 통합 연비 변수 만들기**

mpg 데이터에는 도시 연비를 의미하는 cty, 고속도로 연비를 의미하는 hwy

유형을  통합해 연비 변수 생성

```r
mpg <- as.data.frame(ggplot2 :: mpg)
mpg$total <- (mpg$cty + mpg$hwy)/2
head(mpg)

  manufacturer model displ year cyl
1         audi    a4   1.8 1999   4
2         audi    a4   1.8 1999   4
3         audi    a4   2.0 2008   4
4         audi    a4   2.0 2008   4
5         audi    a4   2.8 1999   6
6         audi    a4   2.8 1999   6
       trans drv cty hwy fl   class total
1   auto(l5)   f  18  29  p compact  23.5
2 manual(m5)   f  21  29  p compact  25.0
3 manual(m6)   f  20  31  p compact  25.5
4   auto(av)   f  21  30  p compact  25.5
5   auto(l5)   f  16  26  p compact  21.0
6 manual(m5)   f  18  26  p compact  22.0
```

```r
# 통합 연비 변수 평균

mean(mpg$total)

[1] 20.14957
```

**조건문을 활용해 파생변수 만들기**

전체 자동차 중에서 연비 기준을 충족해 ‘고연비 합격 판정’을 받은 자동차가 몇 대나 알아보는 상황

1. 기준값 정하기

```r
 summary(mpg$total)
 
   Min. 1st Qu.  Median    Mean 3rd Qu. 
  10.50   15.50   20.50   20.15   23.50 
   Max. 
  39.50 
```

```r
hist(mpg$total)
```

![5.png](/assets/img/posts/R/R_study_2nd/basic/5.png)

- total 연비의 평균과 중앙값이 약 20이다
- total 연비가 20 ~ 25 사이에 해당하는 자동차 모델이 가장 많다
- 대부분 25 이하이고, 25를 넘기는 자동차는 많지 않다

total 변수가 20을 넘기면 합격, 넘기지 못하면 불합격 설정

1. 합격 판정 변수 만들기

**`ifelse()`**는 가장 많이 사용하는 조건문 함수

지정한 조건에 맞을 때와 맞지 않을 때 서로 다른 값을 반환하는 기능

![6.png](/assets/img/posts/R/R_study_2nd/basic/6.png)

```r
mpg$test <- ifelse(mpg$total >= 20, "pass", "fail")
head(mpg, 20)

   manufacturer              model displ
1          audi                 a4   1.8
2          audi                 a4   1.8
3          audi                 a4   2.0
4          audi                 a4   2.0
5          audi                 a4   2.8
6          audi                 a4   2.8
7          audi                 a4   3.1
8          audi         a4 quattro   1.8
9          audi         a4 quattro   1.8
10         audi         a4 quattro   2.0
11         audi         a4 quattro   2.0
12         audi         a4 quattro   2.8
13         audi         a4 quattro   2.8
14         audi         a4 quattro   3.1
15         audi         a4 quattro   3.1
16         audi         a6 quattro   2.8
17         audi         a6 quattro   3.1
18         audi         a6 quattro   4.2
19    chevrolet c1500 suburban 2wd   5.3
20    chevrolet c1500 suburban 2wd   5.3
   year cyl      trans drv cty hwy fl
1  1999   4   auto(l5)   f  18  29  p
2  1999   4 manual(m5)   f  21  29  p
3  2008   4 manual(m6)   f  20  31  p
4  2008   4   auto(av)   f  21  30  p
5  1999   6   auto(l5)   f  16  26  p
6  1999   6 manual(m5)   f  18  26  p
7  2008   6   auto(av)   f  18  27  p
8  1999   4 manual(m5)   4  18  26  p
9  1999   4   auto(l5)   4  16  25  p
10 2008   4 manual(m6)   4  20  28  p
11 2008   4   auto(s6)   4  19  27  p
12 1999   6   auto(l5)   4  15  25  p
13 1999   6 manual(m5)   4  17  25  p
14 2008   6   auto(s6)   4  17  25  p
15 2008   6 manual(m6)   4  15  25  p
16 1999   6   auto(l5)   4  15  24  p
17 2008   6   auto(s6)   4  17  25  p
18 2008   8   auto(s6)   4  16  23  p
19 2008   8   auto(l4)   r  14  20  r
20 2008   8   auto(l4)   r  11  15  e
     class total test
1  compact  23.5 pass
2  compact  25.0 pass
3  compact  25.5 pass
4  compact  25.5 pass
5  compact  21.0 pass
6  compact  22.0 pass
7  compact  22.5 pass
8  compact  22.0 pass
9  compact  20.5 pass
10 compact  24.0 pass
11 compact  23.0 pass
12 compact  20.0 pass
13 compact  21.0 pass
14 compact  21.0 pass
15 compact  20.0 pass
16 midsize  19.5 fail
17 midsize  21.0 pass
18 midsize  19.5 fail
19     suv  17.0 fail
20     suv  13.0 fail
```

total이 20 이상이면 “pass” 를 부여하고 그렇지 않으면 “fail”을 부여해 test라는 변수를 생성하는 기능

1. 빈도표로 합격 판정 자동차 수 살펴보기

**`table()`**을 이용해 빈도표를 만들면 각각 몇 대인지 알 수 있음

```r
table(mpg$test)

fail pass 
 106  128 
```

1. 막대 그래프로 빈도 표현하기

```r
library(ggplot2)
qplot(mpg$test)
```

![7.png](/assets/img/posts/R/R_study_2nd/basic/7.png)

**중첩 조건문 활용하기**

total이 30 이상이면 A, 20 ~ 29는 B, 20 미만이면 C 등급으로 분류

세 가지 이상의 범주로 값을 부여하려면 ifelse() 안에 다시 ifelse()를 넣는 형식으로 조건문 작성

1. 연비에 따라 세 가지 종류의 등급을 부여해 grade 변수를 생성

```r
# A, B, C 등급 부여

mpg$grade <- ifelse(mpg$total >= 30, "A", 
                                        ifelse(mpg$total >= 20, "B", "C"))
head(mpg, 20)

   manufacturer              model displ
1          audi                 a4   1.8
2          audi                 a4   1.8
3          audi                 a4   2.0
4          audi                 a4   2.0
5          audi                 a4   2.8
6          audi                 a4   2.8
7          audi                 a4   3.1
8          audi         a4 quattro   1.8
9          audi         a4 quattro   1.8
10         audi         a4 quattro   2.0
11         audi         a4 quattro   2.0
12         audi         a4 quattro   2.8
13         audi         a4 quattro   2.8
14         audi         a4 quattro   3.1
15         audi         a4 quattro   3.1
16         audi         a6 quattro   2.8
17         audi         a6 quattro   3.1
18         audi         a6 quattro   4.2
19    chevrolet c1500 suburban 2wd   5.3
20    chevrolet c1500 suburban 2wd   5.3
   year cyl      trans drv cty hwy fl
1  1999   4   auto(l5)   f  18  29  p
2  1999   4 manual(m5)   f  21  29  p
3  2008   4 manual(m6)   f  20  31  p
4  2008   4   auto(av)   f  21  30  p
5  1999   6   auto(l5)   f  16  26  p
6  1999   6 manual(m5)   f  18  26  p
7  2008   6   auto(av)   f  18  27  p
8  1999   4 manual(m5)   4  18  26  p
9  1999   4   auto(l5)   4  16  25  p
10 2008   4 manual(m6)   4  20  28  p
11 2008   4   auto(s6)   4  19  27  p
12 1999   6   auto(l5)   4  15  25  p
13 1999   6 manual(m5)   4  17  25  p
14 2008   6   auto(s6)   4  17  25  p
15 2008   6 manual(m6)   4  15  25  p
16 1999   6   auto(l5)   4  15  24  p
17 2008   6   auto(s6)   4  17  25  p
18 2008   8   auto(s6)   4  16  23  p
19 2008   8   auto(l4)   r  14  20  r
20 2008   8   auto(l4)   r  11  15  e
     class total test grade
1  compact  23.5 pass     B
2  compact  25.0 pass     B
3  compact  25.5 pass     B
4  compact  25.5 pass     B
5  compact  21.0 pass     B
6  compact  22.0 pass     B
7  compact  22.5 pass     B
8  compact  22.0 pass     B
9  compact  20.5 pass     B
10 compact  24.0 pass     B
11 compact  23.0 pass     B
12 compact  20.0 pass     B
13 compact  21.0 pass     B
14 compact  21.0 pass     B
15 compact  20.0 pass     B
16 midsize  19.5 fail     C
17 midsize  21.0 pass     B
18 midsize  19.5 fail     C
19     suv  17.0 fail     C
20     suv  13.0 fail     C
```

1. 빈도표, 막대 그래프로 연비 등급 살펴보기

```r
table(mpg$grade)

  A   B   C 
 10 118 106 
 
qplot(mpg$grade)
```

![8.png](/assets/img/posts/R/R_study_2nd/basic/8.png)

**원하는 만큼 범주 만들기**

ifelse()를 더 중첩하면 원하는 만큼 범주의 수를 늘릴 수 있음

```r
# A, B, C, D 등급 부여

mpg$grade2 <- ifelse(mpg$total >= 30, "A", 
                                        ifelse(mpg$total > 25, "B", 
                                                     ifelse(mpg$total >=20, "C", "D")))
```