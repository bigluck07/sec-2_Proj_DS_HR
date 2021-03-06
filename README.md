# 프로젝트 개요
## 시나리오
빅데이터와 데이터 사이언스를 다루는 'Data State'는 회사가 진행하는 일부 과정을 성공적으로 통과한 사람들 중에서 데이터 과학자를 고용하기를 원합니다.

기업은 후보자 중 누가 이직이나 신규 취업을 원하는지 알고 싶어하는데, 이는 교육의 질이나 과정 계획, 후보자의 분류뿐 아니라 비용과 시간을 줄이는 데 도움이 되기 때문입니다.

인구통계학, 교육, 경험과 관련된 정보는 후보자 등록과 정규 수강생 등록에서 수집되었으며, 이 데이터 세트는 인사 연구를 위해 현재 직장을 그만두게 하는 요인을 이해하기 위해 사용될 수 있습니다.

현재 자격증명, 인구통계, 경험 데이터를 통해 후보자가 새로운 일자리를 찾거나 회사에서 일할 확률을 예측할 뿐만 아니라 직원 결정에 영향을 미치는 요인을 해석하게 됩니다.

따라서 교육과정을 이수 후 이직, 취업을 하게될 인원을 예측하고 이를 기반으로 기업에서 이탈할 가능성이 있는 직원을 예측합니다.

## 가설
- 도시개발점수가 낮은 곳에 사는 교육생은 도시개발점수가 높은곳으로 이직하고 싶어할 것이다.
- 근속 3년 이상인 교육생은 이직하려고 할 것이다.
- training_hours가 높은 교육생은 이직하고자 할 것이다.

## 확인사항
- target = 교육생의 이직/취업 여부
- 평가지표
  - accuracy': 모델의 정확도를 확인해주는 평가지표로 예측에 대한 맞은 정도를 알려준다. 따라서 기준모델에서는 2개의 요소 중 가장많은 요소의 비율이 보여진다고 할 수 있다.

  - 'AUC': 'accuracy'의 단점이될 수 있는 모델이 한 요소로 모두 찍었을 경우 보일 수 있는 높은 정확도를 구분할 수 있는 지표로 사용하면서도, 고정수치이기때문에 얼마나 모델이 잘 예측하는지 절대적 수치로 측정할 수 있으며, 분류 임계치가 고정되어있기에, 모델의 분류 임계점이 어떠한지에 관계없이 모델이 잘 예측하는지 측정할 수 있다.

## 모델(RandomizedSearchCV를 이용한 하이퍼파라미터 적용)
- 의사결정트리 분류모델
<img width="411" alt="스크린샷 2021-10-04 오후 4 13 28" src="https://user-images.githubusercontent.com/73811590/135808660-d9bf6d42-3042-4a28-ac96-9ed0f4db2a6e.png">

- 랜덤포레스트 분류모델
<img width="419" alt="스크린샷 2021-10-04 오후 4 15 31" src="https://user-images.githubusercontent.com/73811590/135808872-4a0005d7-36a4-4567-be2a-0a8630e0700a.png">

- LGBM 분류모델
<img width="409" alt="스크린샷 2021-10-04 오후 4 17 08" src="https://user-images.githubusercontent.com/73811590/135809081-187f20cb-72a6-4c49-8b1d-3de390dcdf5b.png">

- 최종선택모델: LGBM 분류모델

## 가설확인
1. 도시개발점수가 낮은 곳에 사는 교육생은 도시개발점수가 높은곳으로 이직하고 싶어할 것이다.
-> 0.6이하에서 이직이 빈번하게 일어나며, 0.92인 도시들는 이직율이 높다
<img width="933" alt="스크린샷 2021-10-04 오후 4 24 29" src="https://user-images.githubusercontent.com/73811590/135809988-d9881291-5026-4884-8780-58edad5a7f01.png">

![image](https://user-images.githubusercontent.com/73811590/135810259-d300d5a5-7cfe-49df-85a4-dd6fcece88a5.png)

2. 근속 3년 이상인 교육생은 이직하려고 할 것이다.
-> 4,2,3년차 순으로 이직률이 높으며 1년차에서도 일어난다.
![image](https://user-images.githubusercontent.com/73811590/135810389-da36d668-a218-4c8e-acc1-7539a494f4c8.png)

3. training_hours가 높은 교육생은 이직하고자 할 것이다.
-> 150시간 이하의 교육 받은 교육생이 이직을 하는 경향이 크다.
![image](https://user-images.githubusercontent.com/73811590/135810543-a68138c4-54c0-4b80-b75d-87a91f9e77d7.png)

4. 추가확인(근속년수와 교육시간의 상관관계)
5. -> 교육시간이 많은경우 이직이 확실히 적고, 일정 수준이하의 교육시간을 커리쿨럼으로 갖는 교육과정을 수료한 사람의 이직률은 높다.
![image](https://user-images.githubusercontent.com/73811590/135811080-0dd55ded-d515-46d5-b6e5-004c0f862373.png)


## 특성별 분석
- 특성 중요도
![image](https://user-images.githubusercontent.com/73811590/135810675-04ec60fe-523c-4f25-846e-751f5a7627f2.png)

<img width="307" alt="스크린샷 2021-10-04 오후 4 30 01" src="https://user-images.githubusercontent.com/73811590/135810736-818c2aeb-6cf1-4ade-8630-70068806c367.png">

## 
- clty: 지역번호가 높을 수록 이직이 빈번하며, 70-105, 160이상 의 도시들이 이직이 할 확률이 더 높다.
- city_de_in: 0.6이하에서 이직이 빈번하게 일어나며, 0.9 전후의 몇 도시들안에서 이직이 일어난다.
- gender: ahems 모든성별에서 이직이 일어나지만, 0.27(female)과 0.25(other)가 확률적으로 높다
- relevent_experience: 둘다 이직을 하지만 관련경험이 없는경우 더 빈번하다
- enrolled_university: 풀타임코스에서 이직이 더욱 빈번하다
- education_level: 학력이 학사(0.27378286), 석사(0.20520194)인경우 빈번하다
- major_discipline: 전공이 존재하면 이직을 할 경우가 높지만, STEM(0.2544967), Business Degree(0.25504855)의 경우 더 빈번하다
- experience: 20년 이상의 경력이 있는 교육생 이외는 모두 이직할 가능성이 있지만, 1년미만과 1년, 3년의 경력을 가진 교육생이 가장 높다
- company_size: 10/49의 규모를 가진 회사가 이직률이 가장 높지만 유의미 한 수치는 아니다.
- company_type: 'Other','Early Stage Startup이 가장 높다
- last_new_job: 4,2,3년차 순으로 이직률이 높으며 1년차에서도 일어난다.
- training_hours: 150시간 이하의 교육시간을 갖은 교육생의 이직률이 높다
![image](https://user-images.githubusercontent.com/73811590/135809502-55b20413-3f8e-4a5a-8dea-853135ce7c8e.png)ㅎㅘㄱ인
![image](https://user-images.githubusercontent.com/73811590/135809502-55b20413-3f8e-4a5a-8dea-853135ce7c8e.png)
