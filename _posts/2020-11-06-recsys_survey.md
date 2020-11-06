---
title: 추천시스템
date: 2020-11-06 15:30:00 +0900
categories: [Recommendation, Data Science]
tags: [recsys]     # TAG names should always be lowercase
math: true
---
## 추천시스템이란?
> 추천 시스템(推薦system)은 정보 필터링 (IF) 기술의 일종으로, 특정 사용자가 관심을 가질만한 정보 
(영화, 음악, 책, 뉴스, 이미지, 웹 페이지 등)를 추천하는 것이다. <https://ko.wikipedia.org/wiki/추천_시스템>

### 문제 상황
* 유저가 평가하지 않은 아이템들의 rating을 예측(estimation)해야 함
* Rating 예측이 가능하다면 가장 높은 점수를 가지는 아이템을 유저에게 추천해줄 수 있음  

### Rating 추론 방식에 따른 분류
1. Heuristic (Rule-based) method
    * 경험적(heuristic) 방법을 사용하여 rating을 예측하고 실험적 방법으로 성능을 확인
2. Model-based method
    * 특정 기준 (i.e. mean square error) 을 최적화하는 모델을 만들어 rating을 예측하는 방법 
    * optimal을 찾아서 계속 모델을 수정함

### Rating 예측 방법에 따른 분류
1. Content-based Filtering
2. Collaborative Filtering(CF, 협업필터링)
3. Hybrid Methods(1과 2를 모두 사용)

## Content-based Filtering
> 유저가 과거에 평가한 아이템과 유사도가 높은 아이템을 추천해주는 방법\
> 유저, 아이템은 각각의 feature로 표현 됨 

### 문서 추천 예시
* 문서(Content)는 키워드로 나타낼 수 있으며 각 키워드의 weight은 
term frequency/inverse document frequency (TF-IDF; TF와 IDF의 곱)로 계산할 수 있음
    * Term frequency(TF): 한 문서 안에 특정 키워드의 normalized frequency를 나타낸 것(최대값=1) \
        $$TF_{i,j}={\frac{f_{i,j}}{max_zf_{z,j}}}$$
    * Inverse document frequency(IDF): 전체 문서 중 특정 키워드가 등장한 문서의 수의 역수를 취한 것 \
        $$IDF_{i}=log{\frac{N}{n_{i}}}$$
        * 대부분의 문서에 등장하는 키워드는 희소성이 떨어지기 때문에 weight을 낮게 만들겠다는 intuition을 가짐
* 문서 벡터
    * 위의 방법으로 K개의 키워드의 weight으로 이루어진 벡터로 표현할 수 있음
* 유저 벡터
    * 유저가 평가한 문서 벡터를 종합하여(i.e. 평균) 표현할 수 있음
* 최종 추천
    * 유저가 평가하지 않은 문서들에 대하여 유저 벡터와 유사도(i.e. 코사인유사도, pcc)가 가장 높은 문서를 추천

### 한계점
1. Content 분석의 한계
    * Content-based 방법은 아이템을 잘 표현할 수 있는 feature를 추출하는 것이 중요함
    * 그러나 일부 도메인(오디오 stream, 비디오 stream, 그래픽 이미지 등) 에서는 
    충분한 feature 추출이 용이하지 않을 수 있음
2. 과잉 전문화(Overspecialization)
    * 유저가 예전에 평가한 아이템과 비슷한 아이템들만 추천되는 문제
        * 유저가 좋아할만한 다른 아이템들은 추천되지 않음
    * 유저가 예전에 평가한 아이템과 "너무" 비슷한 아이템들이 추천되는 문제
        * 유저가 이미 읽은 뉴스와 동일한 내용을 다룬 뉴스를 추천하는 것은 의미 없음
    * 추천에 다양성(diversity)를 주입하는 것이 중요할 수 있음
3. 새로운 유저 문제(New user problem)
    * 유저의 평가가 적은 경우 제대로 된 추천을 할 수 없음
    * 특정 키워드만을 가지고 유저의 특성을 제대로 반영할 수 없기 때문

## Colaborative Filtering (협업필터링)
> 유저와 성향이 비슷한 다른 유저가 과거에 평가한 아이템들을 추천해주는 방법 \
> 유저-아이템간 상호작용(interaction, feedback) 정보를 사용하는 방법

### 유저-아이템 Intercation의 종류
1. Explicit feedback
    * 유저가 아이템에 대해 명시적으로 rating을 매겼기 때문에 선호도를 직접적으로 알 수 있음
    * 예) 영화 평점 
2. Implicit feedback
    * 유저의 아이템에 대한 행동을 기록한 데이터
    * 유저의 선호도를 간접적으로만 반영함
    * 예) 유저의 아이템에 대한 클릭, 방문기록, 구매내역

### CF 방법 종류
1. Memory-based
    * Heuristic 한 룰을 이용한 추천
        * 예) 어떤 유저에 대하여 타겟 아이템을 평가한 이웃 유저들의 rating을 적절히(heuristic) 종합하여 rating 예측 
2. Model-based
    * 통계나 머신러닝 기법을 사용한 모델을 구성하여 추천
    * 예) Matrix factorization(MF)
    
### 한계
1. 새로운 유저 문제(New user problem)
    * 새로운 유저에 대한 정보가 적어서 유저의 성향을 파악하기 어려움
    * 해결
        * Hybrid 방법을 이용하여 해결 
        * Item-based CF를 이용하여 해결
2. 새로운 아이템 문제(New item problem)
    * 새로운 아이템을 평가한 유저가 적어서 충분히 많은 유저가 평가하기 전까지
    그 아이템이 추천되지 않을 수 있음
    * MF의 경우 매우 적은 유저에게만 해당 아이템이 추천될 수 있음 
    * 해결
        * Hybrid 방법을 이용하여 해결
3. Sparsity
    * 전체 아이템에 대한 유저 rating의 수가 매우 적은 경우 추천이 정확하지 않을 수 있음
    * 해결
        * 유저의 profile을 추가로 이용
        * 유저-유저 간 정보를 추가로 이용
