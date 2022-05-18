# 시계열 데이터 StationaryTest

 - 시계열 데이터에서 시계열 통계 모형 적용 전에 Stationary(정상성)를 반드시 확인해야 함.

 - Stationary(정상성) 시계열이란?

     1. 평균이 일정하다. 
     
     2. 분산이 존재하며, 상수이다. 
     
     3. 두 시점 사이의 자기공분산은 시차(時差, time lag)에만 의존한다. 

 - Stationary(정상성) 확인 방법은 다음의 여러 방법이 존재함.


## 시계열 그래프(Plotting) 

 - 실제 시계열 데이터의 그래프를 그려 육안으로 확인하는 방법


## 통계적 가설 검정(statistical hypothesis test)

 ### Agumented Dickey Fuller(ADF) Test

     - ADF 검정은 시계열에 단위근(unit root)이 존재 여부를 검정함으로써 Stationary(정상성)을 판단함. 만약, 단위근이 존재하면 Non-Stationary(비정상성)

     - ADF 검정의 귀무가설: Non-Stationary, 대립가설: Stationary 

     - p-value가 0.05보다 작으면 귀무가설 기각 -> Stationary한 시계열

     ```python
     from statsmodels.tsa.stattools import adfuller
     result = adfuller(list_data)
     print('ADF Statistic: %f' % result[0]) # 통계량
	 print('p-value: %f' % result[1])       # p-value 값
	 print('Critical Values:')
	 for key, value in result[4].items():   # 유의 수준에 따른 값
	 	print('\t%s: %.3f' % (key, value))
     ```
    
     - 통계량 값이 모든 유의 수준의 값들보다 작고 p-value가 0.05보다 작으면 Stationary한 시계열 데이터

     - 통계량 값이 특정 유의 수준의 값보다 크고 p-value가 0.05보다 크면 해당 유의 수준에서 Non-Stationary한 시계열 데이터

     - autolag 파라미터를 통해 lag 선정 방법을 선택 가능하며, 결과값의 usedlag를 통해 lag값 확인 가능

     - ADF 검정은 추세가 있는 비정상 시계열에 대해 Stationary(정상성) 판단을 잘하지만, 분산이 변하거나 계절성 있는 시계열은 제대로 검정하지 못함  

 ### Kwiatkowski-Phillips-Schmidt-Shin(KPSS) Test

     - KPSS 검정은 ADF 검정과 마찬가지로 시계열 데이터의 Stationary(정상성)을 판단함.

     - KPSS 검정의 귀무가설: Stationary, 대립가설: Non-Stationary 

     - p-value가 0.05보다 작으면 귀무가설 기각 -> Stationary한 시계열

     ```python
     from statsmodels.tsa.stattools import kpss
     result = kpss(list_data)
     print('ADF Statistic: %f' % result[0]) # 통계량
	 print('p-value: %f' % result[1])       # p-value 값
     print('lag: %f' % resykt[2])           # lag 값
	 print('Critical Values:')
	 for key, value in result[3].items():   # 유의 수준에 따른 값
	 	print('\t%s: %.3f' % (key, value))
     ```
    
     - 통계량 값이 모든 유의 수준의 값들보다 크고 p-value가 0.05보다 크면 Stationary한 시계열 데이터

     - 통계량 값이 특정 유의 수준의 값보다 작고 p-value가 0.05보다 작으면 해당 유의 수준에서 Non-Stationary한 시계열 데이터

     - nlags 파라미터를 통해 lag 선정 방법을 선택 가능하며, 결과값의 lags를 통해 lag값 확인 가능

     - KPSS 검정은 ADF 검정보다 Stationary(정상성) 검정을 더 잘함.


## 자기 상관(Auto-correlation)

 ### Auto-Correlation Function(자기상관함수)

     - statsmodels.graphics.tsaplots의 plot_acf 활용

     - lagged 한 자신과의 상관관계 확인 

     - plot의 파란 부분을 벗어나는 값이 없으면 Stationary(정상성) 시계열  

 ### Partial Auto-Correlation Function(편자기상관함수)

     - statsmodels.graphics.tsaplots의 plot_pacf 활용

     - plot의 파란 부분을 벗어나는 값이 없으면 Stationary(정상성) 시계열  
 