# Optuna

 - 모델 학습 시 최적의 하이퍼파라미터 찾는 방법

 - 사이킷런의 GridSearchCV 패키지를 활용해 찾는 방법

 - Bayesian Optimization 라이브러리 활용해 찾는 방법

 - Optuna 라이브러리 활용해 찾는 방법


## Optuna를 활용하는 이유

 - Optuna는 하이퍼파라미터의 값을 조금 더 러프하게 지정해주면, 범위내에서 자동탐색을 통해 최적의 하이퍼파라미터를 도출해준다는 점과 시간이 조금 더 빠름

 - Bayesian Optimization은 수치 값만 활용이 가능하지만 Optuna는 string array도 활용 가능


## Optuna 활용 방법

```python
import optuna
from optuna import Trial, visualization
from optuna.samplers import TPESampler

# XGB 하이퍼 파라미터들 값 지정
def objectiveXGB(trial: Trial, X, y, test):
    param = {
        'n_estimators' : trial.suggest_int('n_estimators', 500, 4000),
        'max_depth' : trial.suggest_int('max_depth', 8, 16),
        'min_child_weight' : trial.suggest_int('min_child_weight', 1, 300),
        'gamma' : trial.suggest_int('gamma', 1, 3),
        'learning_rate' : 0.01,
        'colsample_bytree' : trial.suggest_discrete_uniform('colsample_bytree', 0.5, 1, 0.1),
        'nthread' : -1,
        # 'tree_method' : 'gpu_hist',
        # 'predictor' : 'gpu_predictor',
        'lambda' : trial.suggest_loguniform('lambda', 1e-3, 10.0),
        'alpha' : trial.suggest_loguniform('alpha', 1e-3, 10.0),
        'subsample' : trial.suggest_categorical('subsample', [0.6,0.7,0.8,1.0]),
        'random_state' : 1127
    }
    
    # 학습 모델 생성
    model = XGBRegressor(**param)
    xgb_model = model.fit(X, y, verbose=True) # 학습 진행
    
    # 모델 성능 확인
    score = mean_absolute_error(xgb_model.predict(X), y)
    
    return score
# MAE가 최소가 되는 방향으로 학습을 진행
# TPESampler : Sampler using TPE (Tree-structured Parzen Estimator) algorithm.
study = optuna.create_study(direction='minimize', sampler=TPESampler())

# n_trials 지정해주지 않으면, 무한 반복
study.optimize(lambda trial : objectiveXGB(trial, X, y, X_test), n_trials = 50)

print('Best trial : score {}, \nparams {}'.format(study.best_trial.value, study.best_trial.params))
```

 - optuna.trial.Trial.suggest_categorical() : 리스트 범위 내에서 값을 선택한다.

 - optuna.trial.Trial.suggest_int() : 범위 내에서 정수형 값을 선택한다.
 
 - optuna.trial.Trial.suggest_float() : 범위 내에서 소수형 값을 선택한다.
 
 - optuna.trial.Trial.suggest_uniform() : 범위 내에서 균일분포 값을 선택한다.
 
 - optuna.trial.Trial.suggest_discrete_uniform() : 범위 내에서 이산 균일분포 값을 선택한다.
 
 - optuna.trial.Trial.suggest_loguniform() : 범위 내에서 로그 함수 값을 선택한다.

```python
optuna.visualization.plot_param_importances(study) # 파라미터 중요도 확인 그래프
optuna.visualization.plot_optimization_history(study) # 최적화 과정 시각화
```