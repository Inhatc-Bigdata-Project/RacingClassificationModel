<div align=center>
  <h1>
  <h1>말달리자
  <h2>머신러닝 분류 분석을 이용한 경마 예측
</div>
<div align=right>
    <h3>2022-2 / 3학년 BigData 
</div>

---

<br>

## Introduce

경마 산업은 국내 합법 사행산업의 큰 비중을 차지한다.
그러나 사행성 도박이라는 인식, 타 스포츠 산업에 비해 분석 미미하다.


데이터 분석 모델을 제공하여 정보 활용 극대화,
건전한 국민 여가 스포츠로 인식 개선을 목표로 하고 있다.

## 경마란?

연령, 성별, 경주거리, 부담중량, 상금 등 정해진 조건하에서 2두 이상의 말을 달리게 하여 승부를 겨루는 경기에 돈을 걸고 즐기는 성인 레저


별명은 ‘왕들의 스포츠’(Sports of Kings).
결승지점에 도달한 것은 일반적으로 말의 코끝이 결승선을 통과했을 때로 봄

---

### 승식 종류

- 단승 : 1등하는 말
- 연승 : 3등 안으로 들어온 말
- 복승 : 순서와 상관없이 1,2등
- 복연승식 : 3등 이내 말 2마리
- 삼복승식 : 순서와 상괎없이 1,2,3등
- 쌍승식 : 순서대로 1,2등
- 삼쌍승식 : 순서대로 1,2,3등

### 경주 순위 추리 방법

- 한국 마사회 오늘의경주에서 경주 2~3일 전에 출전 마, 기수 등의 정보를 제공
- 출전표, 조교사별 출전 현황, 출전마 장구 사용현황, 종합 정보 제공

<div align=center>
    <h4> 종합정보 <br>
    <img src="https://user-images.githubusercontent.com/89768234/206367346-ac4b7b3a-4de9-414e-b9f6-4a7232f3c40b.png"
         width="90%"/>
    <img src="https://user-images.githubusercontent.com/89768234/206367630-47e24559-e6be-4a3e-8408-13c440e1a0d1.png"
         width="90%"/>
    <h5>- 해당 말의 경주 성적
    <h5>- 기수의 종합 성적 &nbsp;&nbsp; 
    <h5>- 부담 중량이 높은 말
    <h5>- 해당 말의 경기 기록
    <h6> (출처 : 한국마사회(오늘의경주))
</div>
<br>
<br>

---



<div align=center>
    <h1>Objectives
</div>

- **3등 이내로 들어온 말 (연승) 을 맞추는 모델 생성**

    - 3등 이내로 들어온 경우는 1 아닌 경우는 0 이기 때문에 분류 (Classication) 모델을 사용해야 함

    - 한국마사회에서 제공하는 다양한 데이터와  부모마 수득상금 데이터를 활용한 높은 정확도의 연승 예측 모델 제공

**본 프로젝트 에서는 가치에 중점을 둔 비즈니스 모델뿐만 아니라, 수익에 중점을 둔 수익 모델의 역할도 목표로 함**

---

## Crawling

#### 수집할 데이터
> 2년치 데이터 (2020년 10월 16일 ~ 2022년 09월 25일)의경주 결과, 경주마 정보, 기수 정보, 부모마 수득상금 데이터

1. BeautifulSoup를 이용한 정적 크롤링        으로 경주성적, 기수 데이터 크롤링

2. Selenium을 이용한 동적 크롤링으로 부모마 수득상금, 경주마 데이터 크롤링

  
크롤링 코드 라인이 매우 많아 따로 링크로 빼둠 : https://github.com/Inhatc-Bigdata-Project/RacingClassificationModel/blob/magnet9805/crawling.ipynb  
  
---

## Data Preprocessing

### 1. 국내 수득상금 제외 상금 원으로 변경  (USD, JPY 등)
- 원화 시세는 서울외국환중개(http://www.smbs.biz/) 기준 2019~2021년 평균 환율 데이터
> fd feature USD, JPY, AUD, GBP, NZD, RUB, EUR, CAD 포함 행 데이터 가져오기 <br>
md feature USD, JPY, AUD, GBP, NZD, RUB, EUR, CAD 포함 행 데이터 가져오기 

- 실행 순서
> 수득상금 종류 별 idx, fd, md 가져와서 외화 제거 후 환율 곱 후 스트링으로 변환 후 sql update 하는 방법

- 아래 코드에 원화 종류, fd, md 변경
> 2019~2021 년 평균 환율  <br>
USD 1163.37 / JPY 10.72 / AUD 827.52 / GBP 1524.88 / NZD 781.07 / RUB 16.66 / EUR 1334.53 / CAD 890.57 

<div align=center>
    <h5> 원 제외 상금 원으로 변경 <br>
    <img src="https://user-images.githubusercontent.com/89768234/206374901-e89022f0-dd54-4b84-91d7-ec7c7cefb6ef.png"
         width="50%"/>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206374912-04fc49d1-b170-41ca-a2c3-6efcacb84e58.png"
         width="50%"/>
</div>

<br>


### 2. 데이터 병합

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206375840-e9f10eb3-6c97-4aef-9d67-135118badddd.png"
         width="50%"/>
    <h5> 수집 데이터를 df에 저장
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206375845-bf6e184d-5313-4fb6-9519-4903eefdf1b7.png"
         width="50%"/>
    <h5> 수집 데이터를 하나의 df로 병합
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206375859-ceede0c9-6964-47a0-a986-927c2cde5ef6.png"
         width="50%"/>
    <h5> 병합 후 데이터 확인
</div>

<br>

### 3. NaN 결측치 처리, 부모마 수득상금 결측치 처리, 순위에 대한 결측치 처리

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206376605-f043c9ec-3c7f-41ea-855a-577c7f82db37.png"
         width="50%"/>
    <h5> NaN이 아닌 빈 문자열 결측치 np.NaN으로 변환
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206376621-f6c85ab4-a0fe-4451-a27b-b19cc6fa01b4.png"
         width="50%"/>
    <h5> 부모마 수득상금 전처리 알고리즘
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206376629-36b04576-e83d-487b-909c-b49d11bf2536.png"
         width="50%"/>
    <h5> 부모마 수득상금 전처리
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206377193-cdab1fee-fc92-4c78-903e-d29a25d84336.png"
         width="50%"/>
    <h5> 순위 결측치 행 제거 (출전취소 등)
    <h5> 순위가 0 인 feature는 당일 출전 X
→ 행 drop
</div>

#### 이 외에 결측치 없음을 확인

<br>

### 4. 경주마, 기수 전적을 date feature에 맞게 처리

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206377610-2a3bc6d4-34df-4672-9415-d2ae1c981eaf.png"
         width="50%"/>
    <h5> Index 5 경주마 순위가 3등 임에도
경주마 3등 feature가 같은 것을 확인
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206377621-484fdc23-f009-4292-a36f-6101826b007b.png"
         width="50%"/>
    <h5> 마명, date별 sort를 진행
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206377631-8bd45dad-dd70-40c4-9c57-768af3761009.png"
         width="50%"/>
    <h5> 당시의 date에 맞게 전적 처리 알고리즘
    <h5> 이전 행과 같은 마명을 가졌는지 확인 <br>
    만약 같다면, 순위에 따른 feature를 감소 <br>
    다를 경우, 새로운 마명을 tmp_name 에 저장
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206378369-63822a69-e1ec-4a0b-9c88-363a20b24ac3.png"
         width="50%"/>
    <h5> 기수에서도 동일한 issue, 경주마와 같은 알고리즘
</div>

<br>

### 5. 연승, 연령, 산지, 성별 feature mean encoding

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206378734-35533dca-2b83-4914-a9b9-cfc7871ef349.png"
         width="50%"/>
    <h5> 연령 feature 값에 ‘세’ 문자열 제거후
Typecasting to float
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206378752-7e3b7a1c-096a-484f-a136-f9f5fb3d6acf.png"
         width="50%"/>
    <h5> Target에 대한 mean()
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206378757-a2119970-0e5b-47ab-a8f3-bedb9be79f6b.png"
         width="50%"/>
    <h5> 기존 feature에 encoded 된 값을 매핑
</div>
<br>

---

## data visualization

### 1. 모든 feature의 상관관계 시각화

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206379352-578653dc-0d02-4512-ac3f-04d374b9315c.png"
         width="80%"/>
    <h5> 기수 3등이내횟수, 기수2등 feature의 일치율 1.00
    <h5>기수 3등이내횟수, 기수1등 feature의 일치율 0.99
    <h5>등등 기수 전적 관련 feature의 일치율이 매우 높게 나옴
    <h5>이 후 모델링 단계에서 다중공선성 방지를 위해 순위 컬럼과 가장 연관이 높은 기수 3등이내확률, 기수 1등을 제외한 기수 feature는 사용하지 않을 예정
</div>

<br>

### 2. 범주형 변수 분포율 시각화
<br>
<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206393198-764e7591-83db-4e57-b97c-380e35d3795c.png"
    width="80%"/>
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206383930-8e7770ec-db9d-4eb3-bb0a-305a7e1dee88.png"
    width="80%"/>
</div>

<br>

### 3. 부모마 수득상금 log 변환을 이용한 scailing


<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206384810-859c5c36-cc3e-476e-a3f4-1447094b8d34.png"
    width="100%"/>
    <h5> skewness(왜도): 1.859733 -> skewness(왜도): -0.736341
    <h5> kurtosis(첨도): 4.376952 -> kurtosis(첨도): 1.004840 
</div>

<br>

### 4. feature별 연승 시각화

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206385381-d97bfda5-0d12-465b-b4f5-ed3c1081274f.png"
    width="90%"/>
</div>

<br>

### 5. 연승별 수입 시각화

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206385689-86d920b8-8298-451e-82d6-34b3f0264bc5.png"
    width="100%"/>
</div>

<br>

### 6. feature별 수입 시각화


<div>
  <center calss="half">
  <img src="https://user-images.githubusercontent.com/90303745/206885709-31b77ebd-9dac-4cc2-b70a-01011cd5eda6.png" width="30%"/>
  <img src="https://user-images.githubusercontent.com/90303745/206885731-09e41e6e-8e5d-41a5-839b-03670a8716ca.png"
    width="30%"/>
  <img src="https://user-images.githubusercontent.com/90303745/206885734-d39e02c5-d974-4dc7-874a-cd77d37e575d.png"
    width="30%"/>
  </center

</div>


<br>




---

## 변수 선택
<br>

> - 변수 선택을 하기에 앞서 이 전 상관관계 시각화 결과 상관관계가 매우 높았던 기수 feature(3등이내 확률, 1등 수 제외)는 이용하지 않음
> - 18개의 독립변수 중 변수 선택법을 활용하여 feature 삭제 예정 
> - 변수 선택 방법으로 총 4가지 방법 사용
> - 4가지의 변수 선택 방법에서 랜덤포레스트 정확도가 가장 높은 변수 선택법의 독립변수를 채택

<br>

### 1. RandomForest를 통한 feature importance

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206387012-08c9e6eb-072b-4a86-8cd0-77e7df9aeb34.png"
    width="80%"/>
    <h5> 중요도가 낮은 하위 7개 feature 삭제
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206387343-29c848fd-9b4a-4733-b276-7279fa58e02d.png"
    width="80%"/>
</div>
<br>

### 2. 로지스틱 회귀를 이용한 coefficient 도출

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206387440-32dd3e47-d706-4f15-8304-129935e29272.png"
    width="25%"/>
    <h5> 중요도가 낮은 하위 7개 feature 삭제
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206387747-8c63dcd6-3cf5-483e-8ef4-d868d0dd2ea3.png"
    width="80%"/>
</div>
<br>


### 3. L1-based feature selection

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206387935-38240606-876d-40f4-8db4-30ccf2139537.png"
    width="80%"/>
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206388057-76b28fc9-7c55-4b67-9044-edf91f6c002b.png"
    width="80%"/>
</div>
<br>


### 4. Recursive Feature Elimination (RFE)를 이용한 feature importance 확인

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206388116-bdf63795-cf49-4b94-be38-791c81737430.png"
    width="25%"/>
    <h5> 임의 설정한 7개 feature 선정 및 제거
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206388240-18d5c4d5-a726-4488-b1b6-16c782dd1a51.png"
    width="80%"/>
</div>
<br>

---
### 최종 결과 RFE 정확도가 가장 높기에 RFE 변수 선택법 채택
---

<br>
<br>

## 사용할 분석 모델

    - Random Forest Classifier
    - Light Gradient Boosting Model Classifier
    - K Neighbors Classifier
    - Support Vector Machine Classifier


해당 4가지 분석 모델은 모두 파이썬 scikit-learn 라이브러리를 이용하며 따라서 분석 할 모든 feature를 수치화 해야 함.

모두 분류(Classification) 모델로 연승이냐(1) 아니냐(0) 를 예측하기 위함.


## 모델링 및 교차검증, 하이퍼파라미터 튜닝

### 1. Random Forest Classifier

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206388693-19d610e6-dfeb-432b-a483-775e9896698d.png"
    width="80%"/>
</div>
<br>

### 2. Light Gradient Boosting Model Classifier

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206389137-f6c25c16-873e-4a50-ac0c-e4d909b585c4.png"
    width="80%"/>
</div>
<br>

### 3. K Neighbors Classifier

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206389232-f4b5948e-d30f-48e4-8ede-4fc47e07229b.png"
    width="90%"/>
    <h5>scaling에 민감하기에 최대-최소 scailing 진행
    <h5>범위(k) 마다 정확도 체크
    <h5>K가 약 35일 경우 높은 정확도로 수렴함
    <br>
    <br>
    <img src="https://user-images.githubusercontent.com/89768234/206389546-dd3a3986-40ef-4eec-8084-c034d399a487.png"
        width="80%"/>
</div>
<br>


### 4. Support Vector Machine Classifier

<div align=center>
    <img src="https://user-images.githubusercontent.com/89768234/206389137-f6c25c16-873e-4a50-ac0c-e4d909b585c4.png"
    width="80%"/>
</div>
<br>

---
# Result

### 모델링 결과 RandomForest Classifier 모델의 정확도가 0.7827로 가장 높게 나와 해당 모델 채택

### 해당 모델을 제공함으로써 가치와 수익을 모두 이끌어 낼 수 있을 것으로 기대 됨

### 추가적으로 경주 당시의 주로상태나 날씨 데이터 등 다양한 데이터를 포함하여 더 높은 퍼포먼스를 이끌 수는 있지만 기술적 이슈가 있음


### Reference

    경마 데이터 분석 논문 
    - https://www.kci.go.kr/kciportal/ci/sereArticleSearch/ciSereArtiView.kci?sereArticleSearchBean.artiId=ART002068008

    로지스틱 회귀를 통한 경마의 입상확률모형
    - https://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE09762520

    a href 크롤링 
    - https://toentoi.tistory.com/43

    셀레니움 크롤링
    - https://coding-kindergarten.tistory.com/27),   
    - https://devyurim.github.io/python/crawler/2018/08/13/crawler-3.html
    - https://m.blog.naver.com/PostView.naver?blogId=yug311861&logNo=222423444065&targetKeyword=&targetRecommendationCode=1
    
    타이타닉 데이터 분석
    - https://dsbook.tistory.com/181
    
    랜덤 포레스트
    - https://todayisbetterthanyesterday.tistory.com/51

