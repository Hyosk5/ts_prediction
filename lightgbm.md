# LihgtGBM

 ## GBM(Gradient Boosting Model):
     
     - Tree 기반 학습 알고리즘

     - 틀린 부분에 가중치를 더하면서 학습을 진행

     - 여러개의 Tree를 만들되 기존 모델을 조금씩 발전시켜 마지막에 합하는 개념

     - Boosting의 두가지 방식

         1. AdaBoost와 같이 모델이 틀린 데이터에 대해 Weight를 주는 방식

         2. GBDT와 같이 정답과 오답의 차이를 반복적으로 학습하는(Grdient) 방식



 ## LightGBM의 특징

     - Tree의 균형을 고려하지 않고 최대 손실 값을 가지는 leaf node를 지속적으로 분할하면서 비대칭적 구조를 가지게 되는 **Leaf-wise**로 트리 구성(예측 오류 손실 최소화)

     - 빠른 학습 속도

     - 상대적으로 적은 메모리 사용량
     
     - Catergorical feature 자동 변환 및 최적 분할

     - GPU 학습 지원

## LightGBM의 한계

     - 10000개 이하의 적은 데이터를 사용할 경우 과적합 가능성이 매우 큼

## LihgtGBM Hyperparameter

     - **Task**: 데이터에 대해 수행하고자 하는 임무(train, predict)

     - **application**: 모델 종류 선택(regression, binary, multiclass)

     - **boosting**: 알고리즘 선택(gbdt, rf, dart, goss)
     rf: random foreset
     dart: dropouts meet multiple additive regression trees
     goss: gradient-based one-sid sampling

     - **num_boost_round**: boositng iteration 수로 일반적으로 100이상

     - **learning_rate**: 최종 결과에 대해 각 Tree에 영향을 미치는 변수로 0.1, 0.001, 0.003 등이 일반적인 값

     - **device**: 학습 방법(cpu, gpu)

     - **num_leaves**: 트리모델의 복잡성 조절, 2^(max_depth)인 경우 depth_wise tree와 같은 수의 leaves를 가지게 하여 이보다 작아야 과적합 방지

     - **metric**: 학십 지표 파라미터 (mae, mse, binary_logloss, multi_logloss)

     - min_data_in_leaf: 과적합 방지에 중요한 파라미터, 값이 너무 크면 underfitting 발생하므로 10000건 이상의 데이터셋에서는 100~1000의 값이면 충분

     - max_depth: 트리의 depth 한계 지정하는 것으로 가능한 최대 값으로 설정(-1일 경우 최대 값)

     - feature_fraction: Boosting이 Randomforset일 경우 사용, 트리 모델 구성시 각각의 itertation을 돌 때 해당 수치만큼의 파라미터만 랜덤하게 선택

     - early_stopping_round: validation 데이터 중 하나의 지표가 향상되지 않았다면 학습 중단

     - lambda: regularization 정규화 정도

     - main_gain_to_split: 분기를 위한 최소한의 gain 설정. Tree의 분기 수 컨트롤에 사용

     - max_cat_group: 카테고리 수가 클 때 과적합을 방지. 카테고리 그룹을 max_cat_group 그룹으로 합치고 그룹 경계선에서 분기 포인트 찾음

     - max_bin: feature 값의 최대 bin 수

     - categorical_feature: 범주형 feature의 인덱스 의미

     - ignore_column: 범주형 feature로써 무시하는 feature

## 상황별 LihgtGBM 파라미터 튜닝

     1. 학습 속도 빠르게

     - bagging_fraction과 bagging_frequency를 설정하여 bagging 적용

     - feature_fraction을 설정하여 feature sub_sampling을 적용

     - max_bin을 작게 설정

     - save_binary 값을 통해 데이터 로딩 속도를 줄임

     - parallel learning 병렬 학습 적용

     2. 높은 정확도

     - max_bin 값 크게 설정(속도 느려짐)

     - learning_rate 값을 큰 num_iterations 값과 함께 사용

     - num_leaves 값 크게 설정(과적합 유발 가능성)

     - 학습 데이터량 증가

     - boosting 변수를 dart로 설정

     - 범주형 feature 사용

     3. 과적합 방지

     - max_bin 작은 값 설정

     - num_leaves 작은 값 설정

     - min_data_in_leaf와 min_sum_hessian_in_leaf 사용

     - bagging_fraction과 baggin_frequency 사용하여 bagging 적용

     - feature_fraction을 설정하여 feature sub-sampling 적용

     - lambda_l1, lambda_l2, min_gain_to_split 설정하여 regularization 적용

     - max_depth 설정하여 Deep Tree 방지

