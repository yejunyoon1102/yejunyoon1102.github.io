---
layout : post
title : "데이터 분석 : R 스터디 13 ~ 15장"
date : 2025-03-02
categories : [Data Analysis, R]
---
# 기본발제

## 13장 : 통계 분석 기법을 이용한 가설 검정

### 13-1 : 통계적 가설 검정이란?

- 통계 분석
    - 기술 통계 : 데이터를 요약해 설명하는 통계 기법
    - 추론 통계 : 단순히 숫자를 요약하는 것을 넘어 어떤 값이 발생할 확률을 계산하는 통계 기법

예를 들어, 수집된 데이터에서 성별에 따라 월급에 차이가 있는 것으로 나타났을 때, 이런 차이가 우연히 발생할 확률 계산

- 이런 차이가 우연히 발생할 확률 적음 → **통계적으로 유의하다**
- 이런 차이가 우연히 발생할 확률 큼 → **통계적으로 유의하지 않다**

데이터를 이용해 신뢰할 수 있는 결론을 내리려면 유의확률을 계산하는 통계적 가설 검정 절차를 거쳐야 함!!!!

유의확률을 이용해 가설을 검정하는 방밥 : **통계적 가설 검정**

**유의확률(p-value)**은 실제로는 집단 간 차이가 없는데 우연히 차이가 있는 데이터가 추출될 확률

### 13-2 : t 검정 - 두 집단의 평균 비교

t 검정(t-test)은 두 집단의 평균에 통계적으로 유의한 차이가 있는지 알아볼 때 사용하는 통계 분석 기법

**`t.test()`** 를 이용해 t 검정 가능

**compact 자동차와 suv 자동차의 도시 연비 t 검정**

1. mpg 데이터를 불러와 class, cty 변수만 남긴 귀 class 변수가 “compact”인 자동차와 “suv”인 자동차 추출
    
    ```r
    mpg <- as.data.frame(ggplot2::mpg)
    
    library(dplyr)
    mpg_diff <- mpg %>% 
      select(class, cty) %>% 
      filter(class %in% c("compact", "suv"))
    
    head(mpg_diff)
    
        class cty
    1 compact  18
    2 compact  21
    3 compact  20
    4 compact  21
    5 compact  16
    6 compact  18
    
    table(mpg_diff$class)
    
    compact     suv 
         47      62
    ```
    
2. t.test()를 이용해 t 검정 수행
    
    비교하는 집단의 분산이 같은지 여부에 따라 적용 공식이 다름
    
    집단 간 분산이 같을 때 var.equal에 T 지정
    
    ```r
    t.test(data = mpg_diff, cty ~ class, var.equal = T)
    
    Two Sample t-test
    
    data:  cty by class
    t = 11.917, df = 107, p-value < 2.2e-16
    alternative hypothesis: true difference in means between group compact and group suv is not equal to 0
    95 percent confidence interval:
     5.525180 7.730139
    sample estimates:
    mean in group compact     mean in group suv 
                 20.12766              13.50000 
    ```
    
    - p-value : 유의확률
        - 여기서는 0.05보다 작으므로 “compact 와 suv 간 평균 도시 연비 차이가 통계적으로 유의하다” 라고 해석 가능
    - sample estimates : 각 집단의 평균
        - compact는 20인 반면 suv는 13이므로 suv보다 compact의 도시 연비가 더 높다고 할 수 있음

**일반 휘발유와 고급 휘발유의 도시 연비 t 검정**

1. 데이터 전처리
    
    ```r
    mpg_diff2 <- mpg %>% 
      select(fl, cty) %>% 
      filter(fl %in% c("r", "p"))  # r:regular, p:premium
    
    table(mpg_diff2$fl)
    
    p   r 
     52 168 
    ```
    
2. t 검정 수행
    
    ```r
    t.test(data = mpg_diff2, cty ~ fl, var.equal = T)
    
    Two Sample t-test
    
    data:  cty by fl
    t = 1.0662, df = 218, p-value = 0.2875
    alternative hypothesis: true difference in means between group p and group r is not equal to 0
    95 percent confidence interval:
     -0.5322946  1.7868733
    sample estimates:
    mean in group p mean in group r 
           17.36538        16.73810 
    ```
    
    - p-value 가 0.05보다 한참 크기 때문에 “일반 휘발유와 고급 휘발유를 사용하는 자동차 간 도시 연비 차이가 통계적으로 유의하지 않다” 라고 해석 가능
    - sample estimates 부분을 보면 평균 차이가 나지만 이런 정도의 차이는 우연히 발생했을 가능성이 크다고 해석

### 13-3 : 상관분석 - 두 변수의 관계성 분석

상관분석은 두 연속 변수가 서로 관련이 있는지 검정하는 통계 분석 기법

상관계수를 통해 두 변수가 얼마나 관련되어 있는지, 관련성의 정도를 파악 가능

**실업자 수와 개인 소비 지출의 상관관계**

1. cor.test()를 이용해 상관분석
    
    ```r
    economics <- as.data.frame(ggplot2::economics)
    cor.test(economics$unemploy, economics$pce)
    
    Pearson's product-moment correlation
    
    data:  economics$unemploy and economics$pce
    t = 18.63, df = 572, p-value < 2.2e-16
    alternative hypothesis: true correlation is not equal to 0
    95 percent confidence interval:
     0.5608868 0.6630124
    sample estimates:
          cor 
    0.6145176 
    ```
    
    - p-value를 보면 실업자 수 와 개인 소비 지출의 상관이 통계적으로 유의하다라고 해석 가능
    - cor 값이 0.61이므로 실업자 수와 개인 소비 지출은 한 변수가 증가하면 다른 변수가 증가하는 정비례 관계임을 알 수 있음

**상관행렬 히트맵 만들기**

여러 변수의 관련성을 한 번에 알아보고자 할 경우, 모든 변수의 상관 관계를 나타낸 상관행렬을 만듦

1. cor()를 이용해 상관행렬 생성
    
    ```r
    head(mtcars)       # 데이터 확인
    
     mpg cyl disp  hp drat
    Mazda RX4         21.0   6  160 110 3.90
    Mazda RX4 Wag     21.0   6  160 110 3.90
    Datsun 710        22.8   4  108  93 3.85
    Hornet 4 Drive    21.4   6  258 110 3.08
    Hornet Sportabout 18.7   8  360 175 3.15
    Valiant           18.1   6  225 105 2.76
                         wt  qsec vs am gear
    Mazda RX4         2.620 16.46  0  1    4
    Mazda RX4 Wag     2.875 17.02  0  1    4
    Datsun 710        2.320 18.61  1  1    4
    Hornet 4 Drive    3.215 19.44  1  0    3
    Hornet Sportabout 3.440 17.02  0  0    3
    Valiant           3.460 20.22  1  0    3
                      carb
    Mazda RX4            4
    Mazda RX4 Wag        4
    Datsun 710           1
    Hornet 4 Drive       1
    Hornet Sportabout    2
    Valiant              1
    ```
    
    ```r
    car_cor <- cor(mtcars)  # 상관행렬 생성
    round(car_cor, 2)       # 소수점 셋째 자리에서 반올림해서 출력
    
          mpg   cyl  disp    hp  drat    wt
    mpg   1.00 -0.85 -0.85 -0.78  0.68 -0.87
    cyl  -0.85  1.00  0.90  0.83 -0.70  0.78
    disp -0.85  0.90  1.00  0.79 -0.71  0.89
    hp   -0.78  0.83  0.79  1.00 -0.45  0.66
    drat  0.68 -0.70 -0.71 -0.45  1.00 -0.71
    wt   -0.87  0.78  0.89  0.66 -0.71  1.00
    qsec  0.42 -0.59 -0.43 -0.71  0.09 -0.17
    vs    0.66 -0.81 -0.71 -0.72  0.44 -0.55
    am    0.60 -0.52 -0.59 -0.24  0.71 -0.69
    gear  0.48 -0.49 -0.56 -0.13  0.70 -0.58
    carb -0.55  0.53  0.39  0.75 -0.09  0.43
          qsec    vs    am  gear  carb
    mpg   0.42  0.66  0.60  0.48 -0.55
    cyl  -0.59 -0.81 -0.52 -0.49  0.53
    disp -0.43 -0.71 -0.59 -0.56  0.39
    hp   -0.71 -0.72 -0.24 -0.13  0.75
    drat  0.09  0.44  0.71  0.70 -0.09
    wt   -0.17 -0.55 -0.69 -0.58  0.43
    qsec  1.00  0.74 -0.23 -0.21 -0.66
    vs    0.74  1.00  0.17  0.21 -0.57
    am   -0.23  0.17  1.00  0.79  0.06
    gear -0.21  0.21  0.79  1.00  0.27
    carb -0.66 -0.57  0.06  0.27  1.00
    ```
    
    - mpg 열과 cyl 열이 교차되는 부분을 보면 상관계수가 -0.85이므로 연비가 높을수록 실린더 수가 적은 경향이 있음
    - cyl 열과 wt의 상관계수가 0.78이므로, 실린더 수가 많을수록 자동차가 무거운 경향이 있음
2. 여러 변수로 상관행렬을 만들면 너무 많은 숫자로 구성됨. corrplot 패키지의 corrplot()을 이용해 상관행렬을 히트맵으로 표시
    
    ```r
    install.packages("corrplot")
    library(corrplot)
    
    corrplot(car_cor)
    ```
    
    ![1.png](/assets/img/posts/R/R_study_6th/basic/1.png)
    
    상관계수가 클수록 원의 크기가 크고 색깔이 진함
    
    상관계수가 양수면 파란색, 음수면 빨간색 계열
    
3. corrplot()의 파라미터를 이용해 그래프 형태를 다양하게 바꿀 수 있음
    
    ```r
    corrplot(car_cor, method = "number")   # 원 대신 상관계수 표현
    ```
    
    ![2.png](/assets/img/posts/R/R_study_6th/basic/2.png)
    
4. 색깔로 표현
    
    ```r
    col <- colorRampPalette(c("#BB4444", "#EE9988", "#FFFFFF", "#77AADD", "#4477AA"))
    
    corrplot(car_cor,
             method = "color",       # 색깔로 표현
             col = col(200),         # 색상 200개 선정
             type = "lower",         # 왼쪽 아래 행렬만 표시
             order = "hclust",       # 유사한 상관계수끼리 군집화
             addCoef.col = "black",  # 상관계수 색깔
             tl.col = "black",       # 변수명 색깔
             tl.srt = 45,            # 변수명 45도 기울임
             diag = F)               # 대각 행렬 제외
    ```
    
    ![3.png](/assets/img/posts/R/R_study_6th/basic/3.png)
    

## 14장 : R Markdown으로 데이터 분석 보고서 만들기

### 14-1 신뢰할 수 있는 데이터 분석 보고서 만들기

R 마크다운을 활용하면 데이터 분석의 전 과정을 담은 보고서를 쉽게 만들 수 있음

HTML, 워드, PDF 등 다양한 포맷으로 저장

데이터 분석 보고서를 신뢰할 수 있으려면 동일한 분석 과정 거쳤을 때 동일한 분석 결과가 반복되어 나오도록 재현성(Reproducibility)을 갖춰야 함

### 14-2 R 마크다운 문서 만들기

**R 마크다운으로 데이터 분석 보고서 만들기**

1. File → New File → R Markdown
    
    ![4.png](/assets/img/posts/R/R_study_6th/basic/4.png)
    
    HTML, PDF, Word 중에서 저장할 문서 포맷을 정하고 OK를 누르면 문서가 만들어짐
    
2. 문서 생성하기
    
    마크다운 창 메뉴에서 뜨개질 모양의 버튼을 클릭하면 R 마크다운 문서 파일을 저장하는 창이 열림
    
    파일명을 입력하고 저장하면 마크다운 문서가 HTML 포맷으로 저장되고 새 창이 열림
    
3. 뜨개질 버튼의 오른쪽 화살표를 클릭하면 R 마크다운 문서를 HTML, PDF, Word 파일로 변환 가능

## 15장 : R 내장 함수, 변수 타입과 데이터 구조

### 15-1 : R 내장 함수로 데이터 추출하기

dplyr 패키지를 사용하지 않고 R에 기본적으로 내장되어 있는 함수들만 사용해도 데이터 추출

**행 번호로 추출하기**

1. 데이터 불러오기
    
    ```r
    
    exam <- read.csv("csv_exam.csv")
    ```
    
2. 데이터 출력
    
    ```r
    exam[]    # 조건 없이 전체 데이터 출력
    exam[1,]  # 1행 추출
    exam[2,]  # 2행 추출
    
       id class math english science
    1   1     1   50      98      50
    2   2     1   60      97      60
    3   3     1   45      86      78
    4   4     1   30      98      58
    5   5     2   25      80      65
    6   6     2   50      89      98
    ...
    ```
    

**조건을 충족하는 행 추출하기**

1. 쉼표 왼쪽에 조건을 입력하면 조건에 맞는 행 추출
    
    ```r
    exam[exam$class == 1,]  # class가 1인 행 추출
    
    id class math english science
    1  1     1   50      98      50
    2  2     1   60      97      60
    3  3     1   45      86      78
    4  4     1   30      98      58
    
    exam[exam$math >= 80,]  # 수학점수가 80점 이상인 행 추출
    
     id class math english science
    7   7     2   80      90      45
    8   8     2   90      78      25
    18 18     5   80      78      90
    19 19     5   89      68      87
    ```
    
2. 대괄호 안에 &와 |를 사용해 여러 조건을 동시에 충족하거나 하나 이상 충족하는 행 추출
    
    ```r
    # 1반 이면서 수학점수가 50점 이상
    exam[exam$class == 1 & exam$math >= 50,]
    
     id class math english science
    1  1     1   50      98      50
    2  2     1   60      97      60
    
    # 영어점수가 90점 미만이거나 과학점수가 50점 미만
    exam[exam$english < 90 | exam$science < 50,]
    
    id class math english science
    3   3     1   45      86      78
    5   5     2   25      80      65
    6   6     2   50      89      98
    7   7     2   80      90      45
    8   8     2   90      78      25
    9   9     3   20      98      15
    10 10     3   50      98      45
    ...
    ```
    

**열 번호로 변수 추출하기**

행을 추출할 때와 마찬가지 방식으로 대괄호 안 쉼표 오른쪽에 조건 입력

```r
exam[,1]  # 첫 번째 열 추출
[1]  1  2  3  4  5  6  7  8  9 10 11 12 13
[14] 14 15 16 17 18 19 20

exam[,2]  # 두 번째 열 추출
[1] 1 1 1 1 2 2 2 2 3 3 3 3 4 4 4 4 5 5 5 5
 
exam[,3]  # 세 번째 열 추출
[1] 50 60 45 30 25 50 80 90 20 50 65 45 46
[14] 48 75 58 65 80 89 78
```

**변수명으로 변수 추출하기**

변수를 추출할 때는 인덱스보다는 변수명을 활용하는것이 편리

```r
exam[, "class"]  # class 변수 추출

[1] 1 1 1 1 2 2 2 2 3 3 3 3 4 4 4 4 5 5 5 5

exam[, "math"]   # math 변수 추출

[1] 50 60 45 30 25 50 80 90 20 50 65 45 46
[14] 48 75 58 65 80 89 78

exam[,c("class", "math", "english")]  # class, math, english 변수

class math english
1      1   50      98
2      1   60      97
3      1   45      86
4      1   30      98
5      2   25      80
6      2   50      89
...
```

**행, 변수 동시 추출하기**

쉼표의 왼쪽과 오른쪽에 동시에 조건을 입력하면 행과 변수 조건이 모두 충족되는 데이터 추출

```r
# 행, 변수 모두 인덱스
exam[1,3]

[1] 50

# 행 인덱스, 열 변수명
exam[5, "english"]

[1] 80

# 행 부등호 조건, 열 변수명
exam[exam$math >= 50, "english"]

[1] 98 97 89 90 78 98 65 56 98 68 78 68 83

# 행 부등호 조건, 열 변수명
exam[exam$math >= 50, c("english", "science")]

  english science
1       98      50
2       97      60
6       89      98
7       90      45
8       78      25
10      98      45
...
```

하지만 dplyr 코드가 논리의 흐름대로 구조화되어 있기 때문에 내장 함수에 비해 가독성이 높고 이해하기 쉬움

### 15-2 : 변수 타입

1. 연속 변수 - Numeric 타입
    
    연속 변수는 키, 몸무게, 소득처럼 연속적이고 크기를 의미하는 값으로 구성된 변수
    
    양적 변수라고도 함
    
    R에서는 numeric으로 표현
    
2. 범주 변수 - Factor 타입
    
    범주 변수는 값이 대상을 분류하는 의미를 지니는 변수
    
    명목 변수라고도 함
    
    R에서는 factor로 표현
    

**변수 타입 간 차이 알아보기**

1. numeric 타입 변수와 factor 타입 변수 생성
    
    ```r
    var1 <- c(1,2,3,1,2)          # numeric 변수 생성
    var2 <- factor(c(1,2,3,1,2))  # factor 변수 생성
    ```
    
2. 생성한 두 변수간 차이 확인
    
    ```r
    var1  # numeric 변수 출력
    
    [1] 1 2 3 1 2
    
    var2  # factor 변수 출력
    
    [1] 1 2 3 1 2
    Levels: 1 2 3
    ```
    
    factor 변수를 실행하면 두 줄이 출력됨
    
    첫 번째 줄에는 변수에 있는 값들이 출력되고, 두 번째 줄에는 값이 어떤 범주로 구성되는지 의미하는 Levels가 출력
    
3. factor 변수는 연산이 안된다
    
    ```r
    var1+2  # numeric 변수로 연산
    
    [1] 3 4 5 3 4
    
    var2+2  # factor 변수로 연산
    
    [1] NA NA NA NA NA
    경고메시지(들):
    Ops.factor(var2, 2)에서: 요인(factors)에 대하여 의미있는 ‘+’가 아닙니다.
    ```
    
4. 변수 타입 확인하기
    
    class()를 이용하면 변수의 타입이 무엇인지 확인 가능
    
    ```r
    class(var1)
    
    [1] "numeric"
    
    class(var2)
    
    [1] "factor"
    ```
    
5. factor 변수의 구성 범주 확인하기
    
    levels()를 이용하면 factor 변수의 값이 어떤 범주로 구성되는지 확인 가능
    
    ```r
    levels(var1)
    
    NULL
    
    levels(var2)
    
    [1] "1" "2" "3"
    ```
    
6. 문자로 구성된 factor 변수
    
    factor 변수는 숫자로 구성될수도, 문자로 구성될수도
    
    ```r
    var3 <- c("a", "b", "b", "c")          # 문자 변수 생성
    var4 <- factor(c("a", "b", "b", "c"))  # 문자로 된 factor 변수 생성
    
    var3
    
    [1] "a" "b" "b" "c"
    
    var4
    
    [1] a b b c
    Levels: a b c
    ```
    
7. 변수의 타입 확인
    
    ```r
    class(var3)
    
    [1] "character"
    
    class(var4)
    
    [1] "factor"
    ```
    
8. 함수마다 적용 가능한 변수 타입이 다름
    
    예를 들어, 평균을 구하는 함수 mean()은 numeric 변수만 적용 가능
    
    ```r
    mean(var1)
    
    [1] 1.8
    
    mean(var2)
    
    [1] NA
    경고메시지(들):
    mean.default(var2)에서:
      인자가 수치형 또는 논리형이 아니므로 NA를 반환합니다
    ```
    

**변수 타입 바꾸기**

1. 변수 타입 변환
    
    ```r
    var2 <- as.numeric(var2)  # numeric 타입으로 변환
    mean(var2)                # 함수 재적용
    
    [1] 1.8
    ```
    
2. 타입 & 범주 확인
    
    ```r
    class(var2)               # 타입 확인
    
    [1] "numeric"
    
    levels(var2)              # 범주 확인
    
    NULL
    ```
    

as.numeric() 처럼 as. 으로 시작하는 함수들은 변수의 타입을 바꾸는 기능을 함. 이런 함수들을 ‘변환 함수’라고 함

- as.numeric() : numeric 으로 변환
- as.factor() : factor로 변환
- as.character() : character 로 변환
- as.Date() : Date로 변환
- as.data.frame() : Data Frame으로 변환

**다양한 변수 타입들**

- numeric : 실수
- interger : 정수
- complex : 복소수
- character : 문자
- logical : 논리
- factor : 범주
- Date : 날짜

### 15-3 : 데이터 구조

- 벡터(Vactor) : 1차원, 한 가지 변수 타입으로 구성
- 데이터 프레임(Data Frame) : 2차원, 다양한 변수 타입으로 구성
- 매트릭스(Matrix) : 2차원, 한 가지 변수 타입으로 구성
- 어레이(Array) : 다차원, 2차원 이상의 매트릭스
- 리스트(List) : 다차원, 서로 다른 데이터 구조 포함

**데이터 구조 비교하기**

1. 벡터
    
    하나의 값 또는 여러 개의 값으로 구성된 데이터 구조. 여러 변수 타입을 섞을 수 없고, 한 가지 타입으로먼 구성
    
    ```r
    # 벡터 만들기
    a <- 1
    a
    
    [1] 1
    
    b <- "hello"
    b
    
    [1] "hello"
    
    # 데이터 구조 확인 
    class(a)
    
    [1] "numeric"
    
    class(b)
    
    [1] "character"
    ```
    
2. 데이터 프레임
    
    행과 열로 구성된 2차원 데이터 구조. 다양한 변수 타입으로 구성 가능
    
    ```r
    # 데이터 프레임 만들기
    x1 <- data.frame(var1 = c(1,2,3),
                     var2 = c("a","b","c"))
    x1
    
     var1 var2
    1    1    a
    2    2    b
    3    3    c
    
    # 데이터 구조 확인
    class(x1)
    
    [1] "data.frame"
    ```
    
3. 매트릭스
    
    데이터 프레임과 마찬가지로 행과 열로 구성된 2차원 데이터 구조지만, 한 가지 변수 타입으로만 구성 가능
    
    ```r
    # 매트릭스 만들기 - 1~12로 2열
    x2 <- matrix(c(1:12), ncol = 2)
    x2
    
      [,1] [,2]
    [1,]    1    7
    [2,]    2    8
    [3,]    3    9
    [4,]    4   10
    [5,]    5   11
    [6,]    6   12
    
    # 데이터 구조 확인
    class(x2)
    
    [1] "matrix" "array" 
    ```
    
4. 어레이
    
    2차원 이상으로 구성된 매트릭스. 매트릭스와 마찬가지로 한 가지 변수 타입으로만 구성 가능
    
    ```r
    # array 만들기 - 1~20으로 2행 x 5열 x 2차원
    x3 <- array(1:20, dim = c(2, 5, 2))
    x3
    
    , , 1
    
         [,1] [,2] [,3] [,4] [,5]
    [1,]    1    3    5    7    9
    [2,]    2    4    6    8   10
    
    , , 2
    
         [,1] [,2] [,3] [,4] [,5]
    [1,]   11   13   15   17   19
    [2,]   12   14   16   18   20
    
    # 데이터 구조 확인
    class(x3)
    
    [1] "array"
    ```
    
5. 리스트
    
    모든 데이터 구조를 초함하는 데이터 구조. 여러 데이터 구조를 합해 하나의 리스트로 만들 수 있음
    
    ```r
    # 리스트 생성 - 앞에서 생성한 데이터 구조 활용
    x4 <- list(f1 = a,   # 벡터
               f2 = x1,  # 데이터 프레임
               f3 = x2,  # 매트릭스
               f4 = x3)  # 어레이
    x4
    
    $f1
    [1] 1
    
    $f2
      var1 var2
    1    1    a
    2    2    b
    3    3    c
    
    $f3
         [,1] [,2]
    [1,]    1    7
    [2,]    2    8
    [3,]    3    9
    [4,]    4   10
    [5,]    5   11
    [6,]    6   12
    
    $f4
    , , 1
    
         [,1] [,2] [,3] [,4] [,5]
    [1,]    1    3    5    7    9
    [2,]    2    4    6    8   10
    
    , , 2
    
         [,1] [,2] [,3] [,4] [,5]
    [1,]   11   13   15   17   19
    [2,]   12   14   16   18   20
    
    # 데이터 구조 확인
    class(x4)
    
    [1] "list"
    ```
    
    함수의 결과물이 리스트 형태로 반환되는 경우가 많기 때문에 리스트는 R에서 중요한 데이터 구조
    
    리스트를 활용하면 함수의 결과물에서 특정 값을 추출할 수 있음
    
    예를 들어 boxplot() 출력 결과는 리스트 형태이기 때문에 리스트 구조를 다루는 문법을 이용하면 원하는 값 추출
    
    ```r
    mpg <- ggplot2::mpg
    x <- boxplot(mpg$cty)
    x
    
    $stats
         [,1]
    [1,]    9
    [2,]   14
    [3,]   17
    [4,]   19
    [5,]   26
    
    $n
    [1] 234
    
    $conf
             [,1]
    [1,] 16.48356
    [2,] 17.51644
    
    $out
    [1] 28 28 33 35 29
    
    $group
    [1] 1 1 1 1 1
    
    $names
    [1] "1"
    
    x$stats[,1]     # 요약 통계량 추출
    
    [1]  9 14 17 19 26
    
    x$stats[,1][3]  # 중앙값 추출
    
    [1] 17
    
    x$stats[,1][2]  # 1분위수 추출
    
    [1] 14
    ```
    
    ![5.png](/assets/img/posts/R/R_study_6th/basic/5.png)