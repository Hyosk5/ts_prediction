# Catboost

 ## 기존 부스팅 모델들의 단점:
     
     1. 느린 학습 속도

     2. 과적합(overfitting)

 ## Catboost의 특징

     - XGboost와 같이 **Level-wise**로 트리 구성

     - Orderd Boosting: 일부만으로 잔차를 계산하여 모델을 구성하고 그 뒤 데이터의 잔차는 모델로 예측한 값을 사용

     - Random Permutation: 데이터의 순서를 셔플링하여 추출 -> 과적합 방지를 위해 트리 다각적으로 구성하려는  시도

     - Orderd Target Encoding: 범주형 변수 인코딩에서 이전 데이터의 타겟 값만을 사용하여 인코딩

     - Categorical Feature Combinations: 여러 개의 Categorical Feature에서 구분 값이 겹치는 형태의 Feature들은 하나로 묶음 -> Feature selection에 부담이 적어짐

     - OneHot Encoding: one_hot_max_size 파라미터를 설정하여 Cardinality가 해당 값 이하인 feature들은 One-hot Encoding

     - Optimized Parameter tuning: 기본 파라미터가 최적화가 잘 되어 있어 파라미터 튜닝에 크게 신경쓰지 않음(XGboost나 LightGBM은 파라미터 튜닝에 매우 민감) -> 파라미터 튜닝을 굳이 한다면 learning_rate, random_strength, L2_regulariser 등이 있지만 결과에 큰 차이가 없는 편

## Catboost의 한계

     - Sparse한 Matrix 처리 불가

     - 대부분의 데이터가 수치형 변수인 경우 LightGBM보다 학습 속도가 느림

## Catboost Hyperparameter

     - https://catboost.ai/en/docs/concepts/parameter-tuning 참고