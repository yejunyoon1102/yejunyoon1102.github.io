---
layout : post
title : "데이터 분석 : R 스터디 1 ~ 3장"
date : 2025-01-31
categories : [Data Analysis, R]
---

# 기본 발제

### 1장 : 안녕, R?

**R 활용 분야** 

- 통계 분석
- 머신러닝 모델링
- 텍스트 마이닝
- 소셜 네트워크 분석
- 지도 시각화
- 주식 분석
- 이미지 분석
- 사운드 분석
- 웹 애플리케이션 개발

구글 스칼라에 등록된 학술 논문 중에서 R의 사용률이 매우 높음!!

**데이터 분석 도구**

- GUI 방식
    - 엑셀이나 SPSS처럼 화면의 메뉴와 버튼을 마우스로 클릭하면서 작업하는 형태
- 프로그래밍 방식
    - R이나 SAS처럼 키보드로 명령어를 입력하면서 작업하는 형태

**R & 파이썬?**

- R
    - 데이터 분석용으로 만들어진 언어
    - 데이터 처리와 통계 분석에 특화
- 파이썬
    - 소프트웨어를 개발하는데 사용하는 프로그래밍 언어
    - 데이터 분석 기능 + 텐서플로(딥러닝)

### 2장 : R 데이터 분석 환경 만들기

![1.png](/assets/img/posts/R/R_study_1st/basic/1.png)

R 처음 실행 시 소스(Source)창 항상 열어두기!! → (콘솔 창 오른쪽 가장자리에 네모 2개)

**명령어를 실행하는 콘솔 창**

![2.png](/assets/img/posts/R/R_study_1st/basic/2.png)

프롬프트에 명령어를 입력하고 실행하면 결과가 출력됨

- Console : 프롬프트
- Terminal : 시스템 쉘을 이용해 운영 체제를 조작할 수 있음
- Jobs : 여러 R 스크립트를 동시에 병렬로 실행 가능

**명령어를 기록하는 소스 창**

![3.png](/assets/img/posts/R/R_study_1st/basic/3.png)

메모장 같은 일종의 문서 편집기

여기에 명령어나 메모를 자유롭게 기록 가능!!

소스 창에 입력한 명령어로 만들어진 문서를 스크립트(Script)라고 함

소스 창에 입력한 행에서 Ctrl + Enter를 누르면 소스 창의 명령어가 콘솔 창으로 넘어가 실행되고 결과물이 출력

- 한번에 하고 싶은데?
    - 마우스로 드래그/Shift + 방향키를 눌러 실행하려는 명령어를 블록으로 지정한 후 Ctrl + Enter를 누르면 됨
    - Ctrl + A 로 싹 다 한번에 실행시킬수도
- 근데…왜..?
    - 콘솔 창에 명령어를 한 줄씩 입력하는 방식으로 하면 흐름이 안 보이잖아!

**생성한 데이터를 보여 주는 환경 창**

![4.png](/assets/img/posts/R/R_study_1st/basic/4.png)

분석 과정에서 생성한 데이터를 보여주는 기능

- Enviornment : 변수(Values), 입력값을 보여 줌
- History : 지금까지 어떤 명령어를 실행했는지 확인 가능
- Connections : SQL, Spark 등 다양한 데이터베이스에 연결 가능
- Tutorial : 튜토리얼을 이용해 R기초 문법 익힘

**폴더에 있는 파일을 보여 주는 파일 창**

![5.png](/assets/img/posts/R/R_study_1st/basic/5.png)

윈도우의 파일 탐색기와 비슷한 기능

R 스튜디오에서 파일을 불러오거나 저장할 때 참조할 위치인 워킹 디렉토리(Working Directory)의 내용물을 보여 줌

- Files : 워킹 디렉토리를 보여 줌
- Plots : 그래프를 보여 줌
- Packages : 설치된 패키지 목록을 보여 줌
- Help : help() 함수를 실행하면 도움말을 보여 줌
- Viewer : 분석 결과를 HTML 등 웹 문서로 출력한 모습을 보여 줌

**프로젝트 만들기**

데이터 분석을 하다 보면 소스 코드, 이미지, 문서, 외부 프로그램에서 생성된 자료 등 수많은 파일을 활용 → 프로젝트(Project)기능을 이용하면 파일을 효율적으로 관리 가능

여러 가지 분석 작업을 동시에 진행할 때도 파일들을 프로젝트 폴더별로 관리하면 편리

1. R 스튜디오 오른쪽 위에 대문자로 R이라고 쓰여 있는 육각형 모양의 버튼을 클릭해 New Project 클릭
2. New Directoy 버튼 클릭
    
    ![6.png](/assets/img/posts/R/R_study_1st/basic/6.png)
    
3. Project Type 창이 나타나면 맨 위의 New Project 버튼 클릭
4. Directory name 항목에 새로 만들 프로젝트 이름 입력 후 Create Project
    - 프로젝트 이름 & 폴더 경로에 한글이 들어가면 오류가 발생할 수 있으니 주의!!!
    
    ![7.png](/assets/img/posts/R/R_study_1st/basic/7.png)
    

프로젝트를 만들면 파일 창에 표시되는 위치가 프로젝트 폴더로 바뀜

**프로젝트 활용 - 스크립트 관리하기**

한 프로젝트에서 하나의 스크립트만 이용해 작업할 때도 있지만, 여러 개의 스크립트를 동시에 활용하면서 작업하는 경우도

스크립트 파일을 저장하지 않은 상태일 때는 소스 창 왼쪽 위에 Untitled1 이라는 임의의 이름이 표시

Ctrl + S를 눌러 스크립트를 저장

File 메뉴 아래에 있는 새 문서 버튼을 클릭한 후 맨 위의 R Script 클릭

![8.png](/assets/img/posts/R/R_study_1st/basic/8.png)

![9.png](/assets/img/posts/R/R_study_1st/basic/9.png)

R 스튜디오는 기본적으로 자동 저장 기능을 갖추고 있음

R 스튜디오를 종료할 때 Save 를 클릭하면 데이터가 저장되면서 종료됨

R 스튜디오를 다시 실행하면 최근에 사용한 프로젝트와 데이터를 자동으로 불러옴

**워킹 데렉토리**

분석 결과를 저장하거나 외부에서 파일을 불러올 때 사용하는 폴더를 워킹 디렉토리라고 함

프로젝트를 만들면 프로젝트 폴더가 워킹 디렉토리로 지정됨

**유용한 환경 설정**

- 글로벌 옵션(Global Options)
    - R 스튜디오 사용 전반에 영향을 미치는 옵션
- 프로젝트 옵션(Project Options)
    - 해당 프로젝트에만 영향을 미치는 옵션, 프로젝트가 열려 있는 상태에서만 활성화됨
- 한글 파일이 깨질 경우
    - Tools → Project Options를 클릭한 후 Code Editing 탭을 클릭
    - Text encoding 항목을 UTF-8로 바꾸면 한글로 작성된 스크립트도 정상적으로 출력됨

**실행 예시**

![10.png](/assets/img/posts/R/R_study_1st/basic/10.png)

실행 결과의 앞에 표시된 대괄호 안 숫자는 결괏값이 몇 번째 순서에 위치하는지 의미하는 인덱스(Index) 값임

### 3장 : 데이터 분석을 위한 연장 챙기기

**‘변수’ 이해하기**

다양한 값을 지니고 있는 하나의 속성을 변수(Variable)라고 함

![11.png](/assets/img/posts/R/R_study_1st/basic/11.png)

R에서 변수를 만들 때는 왼쪽을 향한 화살표 기호 **`<-`** 사용

ex) a <- 1 은 변수 a를 만들어 1을 집어넣으라!

- 변수명 생성 규칙
    - 실제 분석에서는 score, grade 처럼 알아보기 쉽고 기억하기 쉽도록 의미를 담아 이름을 정함
    - 모든 변수를 소문자로 만드는 습관!!!

**<여러 값으로 구성된 변수 만들기>**

**`c()`** 함수는 여러 개의 값을 넣는 기능을 함(Combine의 머리글자)

```r
var1 <- c(1, 2, 5, 7, 8)
var1

## [1] 1 2 5 7 8
```

콜론( **`:`** )을  이용해 시작 숫자와 마지막 숫자를 입력하면 1씩 증가하면서 연속된 숫자로 변수를 만듦

```r
var2 <- c(1 : 5)
var2

## [1] 1 2 3 4 5
```

**`seq()`** 함수로도 연속 값을 지닌 변수를 만들 수 있음, 괄호 안에 시작 숫자와 마지막 숫자를 쉼표로 구분하여 입력하는 형태

```r
var3 <- seq(1, 5)
var3

## [1] 1 2 3 4 5
```

**`by`** 파라미터를 이용하면 일정한 간격을 두고 연속된 숫자로 된 변수를 만들 수 있음

```r
var4 <- seq(1, 10, by = 2)
var4

## [1] 1 3 5 7 9
```

여러 값으로 구성된 변수로도 연산 가능!! 변수끼리 or 변수 & 숫자 조합도 가능

```r
var1

## [1] 1 2 5 7 8

var1+2
## [1] 3 4 7 9 10
```

```r
var1

## [1] 1 2 5 7 8

var2

## [1] 1 2 3 4 5

var1 + var2

## [1] 2 4 8 11 13
```

**<문자로 된 변수 만들기>**

변수에 문자를 넣을 때는 반드시 문자 앞에 따옴표” 를 붙여야 함

```r
str1 <- "a"
str1

## [1] "a"
```

출력된 값의 앞뒤에 따옴표가 붙어 있으면 문자로 구성된 변수라는 것을 의미

```r
str2 <- "text"
str2

## [1] "text"

str3 <- "Hello World!"
str3

## [1] "Hello World!"
```

**`c()`** 함수를 이용하면 여러 개의 문자로 구성된 변수를 만들 수 있음

```r
str4 <- c("a", "b", "c")
str4

## [1] "a" "b" "c"

str5 <- c("Hello!", "World", "is", "good!")
str5

## [1] "Hello!" "World" "is"   "good!"
```

(단어들 중 가장 긴 단어의 길이를 기준으로 출력 구간을 정하기 때문에 “is” 와 “good!” 사이의 간격이 벌어진 형태로 출력

숫자로 된 변수로는 연산할 수 있지만, 문자로 된 변수로는 연산할 수 없음!!!

단어들을 붙이거나 자르는 등 문자로 된 데이터로 분석 작업을 하려면 문자 처리 기능을 가지고 있는 함수를 이용해야

```r
str1+2

## Error in str1 + 2 : non-numeric argument to binary operator
```

**‘함수’ 이해하기**

데이터 분석은 함수를 이용해서 변수를 조작하는 일

**<숫자를 다루는 함수 이용하기>**

함수는 ‘함수 이름’과 ‘괄호’로 구성됨

평균을 구하는 함수인 **`mean()`**

```r
x <- c(1, 2, 3)
x

## [1] 1 2 3

mean(x)
## [1] 2
```

최댓값을 구하는 함수인 **`max()`**, 최솟값을 구하는 함수인 **`min()`**

```r
max(x)

## [1] 3

min(x)

## [1] 1
```

**<문자를 다루는 함수 이용하기>**

문자로 된 변수를 다루려면 문자 처리 전용 함수를 이용해야 함

여러 문자를 합쳐 하나로 만드는 함수인 **`paste()`**

**`collapse = “ , “`** : 단어들을 쉼표로 구분하도록 설정하는 파라미터

```r
str5

## [1] "Hello!" "World" "is"    "good!"

paste(str5, collapse = " , ")

## [1] "Hello!, World, is, good!"

paste(str5, collapse = " ")

## [1] "Hello! World is good!"
```

함수의 결과물을 바로 출력할 수 도 있지만, 새 변수에 집어넣을 경우

```r
x_meam <- mean(x)
x_mean

## [1] 2

str5_paste <- paste(str5, collapse = " ")
str5_paste

## [1] "Hello! World is good!"
```

**‘패키지’ 이해하기**

함수를 사용하려면 패키지를 먼저 설치하고 불러들여야 함

예를 들어 ggplot2에는 ggplot(), qplot(), geom_histogram() 등 수십 가지 그래프 관련 함수가 들어 있음

패키지는 한 번만 설치하면 되지만 패키지를 로드하는 작업은 R 스튜디오를 새로 시작할 때마다 반복해야 함!!!

**<ggplot2 패키지 설치하기>**

패키지를 설치할 때는 **`install.packages()`**를 이용함

괄호 안에 설치할 패키지 이름의 앞뒤에 반드시 따옴표를 넣어야 함

```r
install.packages("gglpot2")
```

패키지를 로드할 때는 **`library()`** 함수 사용

이 때는 패키지 이름의 앞뒤에 따옴표를 넣어도 되고 넣지 않아도 됨

```r
library(ggplot2)
```

```r
x <- c("a", "a", "b", "c")
x

## [1] "a" "a" "b" "c"

# 빈도 막대 그래프 출력
qplot(x)
```

![12.png](/assets/img/posts/R/R_study_1st/basic/12.png)

**<ggplot2의 mpg 데이터로 그래프 만들기>**

qplot()의 data 파라미터에 mpg 데이터를 지정하고, 그래프의 x축을 결정하는 파라미터에 hwy(자동차가 고속도로에서 1갤런에 몇 마일을 가는지 나타낸 변수)를 지정해 ‘고속도로 연비별 빈도 막대 그래프’ 생성

```r
qplot(data = mpg, x = hwy)
```

![13.png](/assets/img/posts/R/R_study_1st/basic/13.png)

```r
# x축 cty

qplot(data = mpg, x = cty)
```

![14.png](/assets/img/posts/R/R_study_1st/basic/14.png)

```r
# x축 drv, y축 hwy, 선 그래프 형태

qplot(data = mpg, x = drv, y = hwy, geom = 'line')
```

![15.png](/assets/img/posts/R/R_study_1st/basic/15.png)

```r
# x축 drv, y축 hwy, 선 그래프 형태

qplot(data = mpg, x = drv, y = hwy, geom = 'boxplot')
```

![16.png](/assets/img/posts/R/R_study_1st/basic/16.png)

```r
# x축 drv, y축 hwy, 상자 그림 형태, drv별 색 표현

qplot(data = mpg, x = drv, y = hwy, geom = "boxplot", colour = drv)
```

![17.png](/assets/img/posts/R/R_study_1st/basic/17.png)

**함수의 기능이 궁금할때**

파라미터를 지정하는 방식 등 문법이 기억나지 않거나 헷갈릴 때

→ 함수명 앞에 물음표를 넣어 Help 함수를 실행!!

```r
?qplot
```

![18.png](/assets/img/posts/R/R_study_1st/basic/18.png)