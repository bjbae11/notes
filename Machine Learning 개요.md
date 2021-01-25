### Machine Learning

____



#### 분류와 회귀

- 분류(classification)
  - 이진 분류(binary classification): 둘 중 하나의 답을 정함
  - 다중 클래스 분류(multi-class classification): 둘 이상의 정해진 선택지 중에서 하나의 답을 정함
- 회귀(regression)
  - 연속된 값을 결과로 가짐 => 예측



#### 지도 학습과 비지도 학습

- 지도 학습(supervised learning)
  - label(정답)이 주어짐
  - 예측값과 실제값의 차이(오차)를 줄이는 방식으로 학습
- 비지도 학습(unsupervised learning)
  - label이 주어지지 않음



#### 과적합과 과소 적합

- 과적합(overfitting)
  - 훈련 데이터를 과도하게 학습함
  - 실제 서비스에서의 데이터에 대해 정확도가 떨어짐
  - Drop out, Early Stopping 과 같은 방법으로 방지할 수 있음
- 과소 적합(underfitting)
  - 테스트 데이터의 성능이 올라갈 여지가 있음에도 훈련을 덜 한 상태
  - 일반적으로 훈련 데이터에 대해서도 정확도가 낮음





____



#### 1. 선형 회귀(Linear Regression)

- 주어진 데이터로부터 y와 x의 관계를 가장 잘 나타내는 직선을 그린다



1. **단순 선형 회귀 분석**
   - y = Wx + b
2. **다중 선형 회귀 분석**
   - y = W1x1 + W2x2 + ... Wnxn + b



#### 2. 비용 함수, 목적 함수, 손실 함수

- 실제값과 예측값의 오차에 대한 식
- Objective Function: 값을 최소화 또는 최대화 하려는 목적을 가진 함수
- Cost Function, Loss Function: 값을 최소화하려는 목적을 가진 함수
- 회귀 문제의 경우 주로 **평균 제곱 오차(Mean Squared Error, MSE)**가 사용됨



#### 3. 로지스틱 회귀(Logistic Regression)

- 이진 분류 문제를 풀기 위한 대표적인 알고리즘

- 실제 값이 0 or 1의 두 가지 값 밖에 가지지 않으므로 예측값은 0과 1 사이의 값이 되어야 함 => 확률로 해석

- 선형 회귀의 경우 y값의 범위: - INF ~ INF => 이진 분류에 부적합

  

1. **시그모이드 함수(Sigmoid Function)**

   - 0 ~ 1 사이의 값

   - S자 형태

   - 입력값이 커지면 1에 수렴하고 작아지면 0에 수렴함

   - 비용함수: 크로스 엔트로피(Cross Entropy) 함수

     