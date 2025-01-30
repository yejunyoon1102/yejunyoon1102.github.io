---
layout : post
title : "데이터 분석 : lavaan_구조방정식모형(SEM) R 패키지"
date : 2025-01-28
categories : [Data Analysis, R]
---
# lavaan_구조방정식 모형(SEM) R 패키지

### 구조방정식 모형 (SEM)

R 통계 컴퓨팅 시스템에 구현된 구조방정식 모델링(SEM)을 위한 패키지

lavaan은 “latent variable anlysis(잠재 변수 분석)”의 약자로, 다양한 잠재 변수 모델을 탐색, 추정, 이해하는 데 사용할 수 있는 도구 모음을 제공

**구조방정식 모형**(Structural Equation Model)이란 관심 대상 변수들 사이의 구조적 관계를 일련의 방정식으로 구조화한 방법론(인과 모형 이라고 불리기도 함)

- 주요 활용 분야 : 마케팅, 경영학, 심리학, 경제학 등
- **잠재 변수(latent variable)** : 측정이 어려운 변수들
    - 이러한 잠재변수를 여러 **관측변수(observed variable)**들을 이용해 추정 및 정의하고, 구조 모형에서 활용함
    
    ![1.png](/assets/img/posts/R/R_study_1st/advanced/1.png)
    
- 경로도(Path Diagram) : 연구자가 적합하려는 모델의 간략한 개요를 시각적으로 표현한 도식
    - 관찰 변수 : 일반적으로 사각형으로 표시
    - 잠재변수 : (타)원으로 표시
    - 변수 간의 관계는 화살표로 표현
        - 단방향 : 한 변수가 다른 변수에 직접적인 영향을 미치는 것을 나타냄
        - 양방향 : 변수들 간의 설명되지 않는 상관관계를 나타냄
    - 오차 : 원(혹은 작은 원)으로 표시
        - 관측변수 오차 : 관측변수에 의해 잠재변수가 설명되지 못한 정도
        - 내생 잠재변수 오차 : 내생 잠재변수가 외생 잠재변수에 의해 설명되지 못한 정도
            - 외생(exogenous) 잠재변수 : 다른 변수에 영향을 주기만 하는 변수
            - 내생 (endogenous) 잠재변수 : 한번 이상 다른 변수에 의해 영향을 받는 변수
    
    ![2.png](/assets/img/posts/R/R_study_1st/advanced/2.png)
    

이런 경로도를 SEM 프로그램이 요구하는 입력 형식으로 변환하는 과정이 기존 패키지(EQS, LISREL, AMOS 등)이 힘듦 → lavaan!

### lavaan 모델 구문(lavaan model syntax)

1. **간단한 회귀 모델**

![3.png](/assets/img/posts/R/R_study_1st/advanced/3.png)

- β0 : 절편(intercept)
- β1, β2, β3, β4 : 회귀 계수(regression coefficients)
- ϵi : 잔차(residual error)

```r
y ~ x1 + x2 + x3 + x4
```

- ~ : 회귀 연산자
- 좌변 : 종속 변수(dependent variable)
- 우변 : 독립 변수(independent variables), +로 구분

특징 

- 절편과 잔차는 명시적으로 포함하지 않아도 자동으로 추정됨
- 사용자는 모델의 구조적 부분만 정의하면 되고, 나머지는 lm() 함수 등이 처리

1. **SEM 모델의 확장**

SEM 모델은 선형 회귀 모델의 확장으로 볼 수 있음

- 다중 회귀 방정식 : 여러 회귀 방정식을 동시에 사용할 수 있음
- 변수 간 역할 교차 : 한 방정식에서 독립 변수(외생 변수)인 변수가 다른 방정식에서는 종속 변수(내생 변수)가 될 수 있음

```r
y1 ~ x1 + x2 + x3 + x4
y2 ~ x5 + x6 + x7 + x8
y3 ~ y1 + y2
```

1. **잠재 변수 포함**

SEM 모델의 또 다른 확장은 연속형 잠재 변수(latent variables)를 포함한다는 점

lavaan은 잠재 변수를 종속 또는 독립 변수로 사용할 수 있음

```r
y ~ f1 + f2 + x1 + x2
f1 ~ x1 + x2
```

f1, f2 는 잠재 변수, 이는 모델의 구조적 부분에 해당함

1. **측정 모델 정의**

잠재 변수의 측정 부분은 특수 연산자 **`=~`** 를 사용하여 처리!

- 좌변 : 잠재 변수 이름
- 우변 : 잠재 변수의 지표, +로 구분

```r
f1 =~ item1 + item2 + item3
f2 =~ item4 + item5 + item6 + item7
f3 =~ f1 + f2
```

item1 ~ item7 : 관찰 변수(observed variables)

f1, f2 : 1차 요인

f3 : 2차 요인, 왜냐하면 f1과 f2 같은 잠재 변수를 지표로 가짐

1. **잔차(Residuals) 및 공분산(Covariances)**

**`~~`** 연산자를 사용하여 (잔차)분산과 공분산을 지정!

- 좌변과 우변이 동일 : 분산(variance)
- 좌변과 우변이 다름 : 공분산(covariance)

```r
item1 ~~ item1  # 분산
item1 ~~ item2  # 공분산
```

- 잔차와 비잔차 (공)분산의 구분은 lavaan에서 자동으로 처리함

1. **절편(intercept)**

lavaan에서는 관찰 변수와 잠재 변수의 절편을 단순한 회귀 공식으로 나타냄

**`~ 1`** 연산자를 사용하며, 이는 1만을 포함하는 회귀 공식

```r
item1 ~ 1  # 관찰 변수의 절편
f1 ~ 1     # 잠재 변수의 절편
```

![4.png](/assets/img/posts/R/R_study_1st/advanced/4.png)

예제 모델 구문

```r
myModel <- '
  # 회귀 관계
  y ~ f1 + f2
  y ~ x1 + x2
  f1 ~ x1 + x2
  
  # 잠재 변수 정의
  f1 =~ item1 + item2 + item3
  f2 =~ item4 + item5 + item6 + item7
  f3 =~ f1 + f2
  
  # (잔차) 분산 및 공분산
  item1 ~~ item1
  item1 ~~ item2
  
  # 절편
  item1 ~ 1
  f1 ~ 1
'
```

1. **lavaan 모델 지정 방식**
- cfa() , sem()
    - 최소한의 구문만 작성(잠재 변수 정의와 회귀 관계)
    - 자동 추가 : 잔차, 분산, 공분산 등 필수 매개변수 자동 추가
- lavaan()
    - 모든 매개변수를 명시적으로 정의해야 함
    - 자동 추가 없음 : 사용자가 모델 식별성과 매개변수를 완전히 제어
- lavaan() + auto.*
    - 일부 매개변수(예: 잔차, 분산)를 자동 추가하도록 설정

1. **모델의 적합도, 매개변수 검사**

주요 메서드

- **`summary()`** : 모델의 적합도 통계 및 매개변수 요약
    - **`fit.measures = TRUE`** : 추가 적합도 지수 표시
    - **`standardized = TRUE`** : 표준화된 매개변수 추정값 포함(Std.lv, Std.all 등 확인 가능)
        - Std.all : 표준화된 요인 적재량 확인!! (0.7 이상이면 우수한 것으로 판단)
    - **`rsquare = TRUE`** : 종속 변수의 R-squared 값 포함
    
    ```r
    summary(fit, fit.measures = TRUE)
    ```
    
- **`coef()`** : 모델에서 자유 매개변수 추정값 반환
- **`fitted()`** : 모델의 예측된 공분산 행렬 및 평균 벡터 반환
- **`resid()`** : 잔차(Residuals) 반환
- **`vcov()`** : 매개변수의 공분산 행렬 반환
- **`predict()`** : 요인 점수(Factoer scores) 계산
- **`logLik()`** : 모델의 로그우도(Log-likelihood) 반환
- **`AIC(), BIC()`** : 정보 기준 계산(최대우도추정 사용 시)
- **`update()`** : 기존 적합된 모델을 업데이트
- **`inspect()`** : 모델의 내부 표현을 확인. 매개변수의 시작값, 기울기 등 추출 가능
- **`parameterEstimates()`** : 매개변수 추정값을 데이터 프레임 형식으로 반환하여 추가 분석에 활용
    - **`ci = FALSE`** : 신뢰 구간 출력 생략
    - **`standardized = TRUE`** : 표준화된 추정값 포함
    
    ```r
    parameterEstimates(fit)
    
    Est <- parameterEstimates(fit, ci = FALSE, standardized = TRUE)
    subset(Est, op == "=~")  # 요인 적재만 필터링
    ```
    
- **`modificationIndices()`** : 모델 적합도를 개선하기 위한 수정 지수(Modification Indices, Mls) 와 예상 매개변수 변화(EPCs)를 제공
    
    ```r
    MI <- modificationIndices(fit)
    subset(MI, mi > 10)  # 수정 지수가 10 이상인 매개변수 필터링
    ```
    

### 예제 : 구조방정식 모델링(SEM)

정지 민주주의와 산업화 데이터를 사용하여 구조방정식 모델(SEM) 탐구

개발도상국에서의 정치 민주주의와 산업화 수준을 측정한 다양한 지표를 포함, 세 가지 잠재변수를 정의하고 이를 통해 SEM 분석 수행

- 측정 모델(Measurement Model) : 관찰 변수를 사용해 잠재 변수를 측정
    
    → 일종의 요인 분석(factor analysis) 과정, 많은 수의 변수 내에 잠재되어 있는 더 적은 수의 공통 요인을 찾아내는 분석
    
    - 탐색적 요인 분석(EFA)
        - 기존 이론이 없거나 신규 이론을 개발하고 싶을 때 주로 활용
        - 새로운 요인(잠재변수)를 탐색적으로 발견
    - 확정적 요인 분석(CFA)
        - 기존 이론 혹은 근거가 있어서 이를 따르고 싶을 때 주로 활용
        - 적절한 근거가 존재하는 상황에서는 활용이 더 간편
    
    특정 분야의 초기 연구는 탐색적 요인 분석이, 후기에는 확정적 요인 분석이 많이 이루어지는 경향이 있음
    
- 구조적 모델(Sturctural Model) : 잠재 변수들 간의 회귀 관계
- 잔차 공분산(Residual Covariances) : 특정 관찰 변수들 간의 잔차 상관관계

**구조 방정식 분석 흐름**

1. 가설 설정 & 데이터 수집
2. 측정 모형 개발
    - 관측 변수를 이용하여 잠재변수를 측정하는 측정모형의 개발
    - 다양한 성능 평가 지표들을 이용하여 모형 결정
        - 모형 적합도 : Chisq, RMSEA, GFI, CFI
        - 관측변수별 : 요인 적재량(factor loading)
        - 잠재변수별 : Cronbach’s Alpha, AVE, CR
        - 논리적 근거 및 MI 이용하여 모형 개선
        - 판별 타당도
        
        ![5.png](/assets/img/posts/R/R_study_1st/advanced/5.png)
        
3. 구조모형 분석
    - 핵심 가설 검증
    - 잠재변수 사이의 관계 및 영향 수준 분석
    - 모형 성능 점검

예제) 개발 도상국의 산업화 수준은 미래 민주화 수준에 영향을 미칠까?

주요 선진국들은 대체로 산업화 이후에 민주화를 이루었는데, 후발주자인 개발도상국들도 이러한 패턴을 따를지?

**가설**

- 1960년의 산업화 수준(ind60)은 1960년의 민주화 수준(dem60)에 영향을 미친다
- 1960년의 산업화 수준(ind60)은 1965년의 민주화 수준(dem65)에 영향을 미친다
- 1960년의 민주화 수준(dem60)은 1965년의 민주화 수준(dem65)에 영향을 미친다

**데이터**

- 민주화 수준
    - 언론 자유도 전문가 평점 지표(y1, y5)
    - 정치적 반대의 자유도 지표(y2, y6)
    - 선거 공정도 지표(y3, y7)
    - 선거로 선출된 입법부의 효과성 지표(y4, y8)
        - y1, y2, y3, y4 : 1960년 수치
        - y5, y6, y7, y8 : 1965년 수치
- 산업화 수준
    - 1인당 GNP(x1), (GNP : 국민 총생산)
    - 1인당 에너지 소비량(x2)
    - 산업 구조에서 노동 비중(x3)
        - x1, x2, x3 : 1960년 수치

```r
# lavaan 모델 구문

model <- '
  # 측정 모델
  ind60 =~ x1 + x2 + x3        # 1960년 산업화(ind60)를 측정하는 세 가지 관찰 변수
  dem60 =~ y1 + y2 + y3 + y4   # 1960년 정치 민주주의(dem60)를 측정하는 네 가지 관찰 변수
  dem65 =~ y5 + y6 + y7 + y8   # 1965년 정치 민주주의(dem65)를 측정하는 네 가지 관찰 변수

  # 구조적 모델 (잠재 변수 간의 관계)
  dem60 ~ ind60                # 1960년 정치 민주주의는 1960년 산업화에 의해 설명됨
  dem65 ~ ind60 + dem60        # 1965년 정치 민주주의는 1960년 산업화와 1960년 정치 민주주의에 의해 설명됨

  # 잔차 공분산 (관찰 변수 간 상관관계)
  y1 ~~ y5                     # 1960년과 1965년의 첫 번째 관찰 변수 간 잔차 공분산
  y2 ~~ y4 + y6                # 1960년 두 번째 변수와 1960년, 1965년의 변수 간 잔차 공분산
  y3 ~~ y7                     # 1960년 세 번째 변수와 1965년 세 번째 변수 간 잔차 공분산
  y4 ~~ y8                     # 1960년 네 번째 변수와 1965년 네 번째 변수 간 잔차 공분산
  y6 ~~ y8                     # 1965년 두 번째와 네 번째 변수 간 잔차 공분산
'

```

```r
# 모델 적합

fit <- sem(model, data = PoliticalDemocracy)
```

```r
# 결과 요약

summary(fit, standardized = TRUE)
```

![6.png](/assets/img/posts/R/R_study_1st/advanced/6.png)

![7.png](/assets/img/posts/R/R_study_1st/advanced/7.png)

![8.png](/assets/img/posts/R/R_study_1st/advanced/8.png)

```r
# 추가 적합도 지수 확인

summary(fit, fit.measures = TRUE)

# 특정 적합도 지수 확인

fitMeasures(fit, c("chisq", "pvalue", "rmsea", "gfi", "cfi"))
```

![9.png](/assets/img/posts/R/R_study_1st/advanced/9.png)

모형 개선이 필요하다면?

**수정 지수(MI)**를 이용해서 체계적인 모형 개선!

- Modification index (MI) : 수정 지수
- 모형을 수정했을 경우의 카이제곱값 변화(줄어드는 정도)

```r
# 모든 수정 지수 보기

modindices(fit)

# 수정 지수 정렬

modindices(fit2, sort. = TRUE)

# 특정 기준 이상만 표시

modindices(fit2, sort. = TRUE, minimum.value = 3)
```

![10.png](/assets/img/posts/R/R_study_1st/advanced/10.png)

MI > 3.84 : 통계적으로 유의미한 개선 가능성

→ 이론적 타당성을 검토 후 매개변수를 추가해야!!!!

이후 모델 재적합 및 적합도 지수 재평가

- 제안 1) y6 ~~ y8 : y6와 y8의 상관관계 (채택)
    - y6과 y8은 각각 1965년의 정치적 반대 자유도와 선거로 선출된 입법부의  효과성
- 제안 2) y2 =~ y5 : ind60의 측정에 y5포함 (기각)
    - ind60(60년대 산업화 수준)의 측정에 1965년 y5(언론자유도)를 포함하는 것은 부적절
- 제안 3) y2 ~~ y4 : y2와 y4의 상관관계 (채택)
    - y2와 y4는 각각 1960년의 정치적 반대 자유도와 선거로 선출된 입법부의 효과성

![11.png](/assets/img/posts/R/R_study_1st/advanced/11.png)

```r
# 잠재변수 차원 성능 지표

library(semTools)
reliability(fit3)
```

![12.png](/assets/img/posts/R/R_study_1st/advanced/12.png)

잠재변수의 신뢰도를 측정했다면 이렇게 정의된 잠재변수들이 구분되는지 확인해야함 → 판별 타당도(discriminant validity) 계산

- 특정 잠재변수의 AVE 값의 제곱근이 상관계수의 절대값보다 크면 잘 판별된 것
    - AVE 제곱근보다 큰 상관계수의 절대값이 존재하는 경우 연관 잠재변수들을 통합하는 것을 고려해 볼 수 있음

```r
# 잠재 변수 상관 행렬 확인

latent_corr <- lavInspect(sem1, what = "cor.lv")
print(latent_corr)

>>>

      ind60    dem60    dem65
ind60  1.000    0.743    0.612
dem60  0.743    1.000    0.821
dem65  0.612    0.821    1.000

# AVE 제곱근 계산

avevar <- reliability(sem1)['avevar',]
avevar_sqrt <- sqrt(avevar)
print(avevar_sqrt)
>>>

ind60 dem60 dem65
0.806 0.837 0.823
```

**시각화**

semPlot 이라는 패키지를 이용하여 구조방정식 분석 결과를 시각화 할 수 있음!

![13.png](/assets/img/posts/R/R_study_1st/advanced/13.png)