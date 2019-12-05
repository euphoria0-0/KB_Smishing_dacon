# KB_Smishing_dacon
- dacon 14th : 금융문자 분석 경진대회
- 주최: KB금융지주, DACON , KISA(한국인터넷진흥원)
- 데이터: KB금융그룹 및 KISA(한국인터넷진흥원)에서 제공받은 정상문자와 스미싱 문자

## 1. EDA
- 데이터 불균형
  - 정상인 문자 277,242건, 스미싱 문자 18,703건으로 데이터 불균형 현상(93.7%:6.3%)
  - <code> Counter({0: 277242, 1: 18703})
  0.0631975535994864 </code>
- 
## 2. Sampling
- Mixed Sampling
  - 정상 문자 중 ~건 비복원추출, 스미싱 문자 18,703건을 복원추출
  
## 3. Preprocessing
- 한글, 영어만 세팅
- XXX, XXXXX 등 비식별 처리된 개인정보 혹은 금융회사정보는 지움
- Mecab Tokenizer 사용
  - Dacon에서 배포한 윈도우용 Mecab 사용(Colab)
  - 로컬은 우분투 Mecab

## 4. Modeling
- Baseline: RandomForest, Logistic Regression, Naive Bayes 등 사용
  - RandomForest(하이퍼파라미터 수정 전)만으로 (수정버전)0.94가 나옴
- RNN
  - Simple RNN
  - LSTM
  - Bi-directed LSTM
  - ~~GRU~~
  - ~~Bi-directed GRU~~
- ConvRNN
  - ConvRNN
  - ConvLSTM
  - ConvGRU
  
  
- Ensemble
 

## 6. Predict and Submission
- Submission for Public and Private
- __issue : Inference time (스미싱 문자 예측에 걸리는 시간) 순위 30% 점수 반영__
  - train된 모델이 있을 때 test 데이터를 올바른 형태로 전처리+predict하는 시간입니다 (train 전처리와 모델 training 시간은 포함되지 않습니다).

------

## 고민일지(~12.05)
1. 데이터 불균형으로 인한 샘플링 문제
  - 언더샘플링 즉, 스미싱문자의 수 18,703건만 고려하는 것은 정상문자 데이터 손실이라 생각
  - 오버샘플링은 너무 많이 불려야 함
  - SMOTE 등의 샘플링은 범주형에 적절하다고 생각하지 않는다. 특히나 텍스트에는 서로 상이한 문자들이 들어와서 맥락이 안 맞다고 생각이 들었다.
  - 따라서 Mixed Sampling. 스미싱 조금 불리고 정상 조금 추출. __그 적절한 threshold를 모르겠다!!!!__
    - 음.. 간단한 LR 모델로 threshold를 구해볼까..? 생각중
    
2. 텍스트 전처리
  - Mecab이 빨라서 Twitter 등 제외(순위에 테스트 데이터셋의 예측 시간(inference time) 포함되므로)
  - 다만, Mecab이 너무 잘게 형태소를 분리한다고 생각된다.
  - __Mecab으로 자른 것들만 포함 vs 조사만 제거 vs 조사, 어미 제거__
  - 사용자 정의 사전을 추가하고 싶은데 로컬(노트북)에서는 우분투에 할 수 있지만 코랩에서는 배치파일을 받아서 사용한 것이므로 사용자 사전을 추가할 수 없다ㅠ 그래서 일단 for 문으로 dictionary이용해 수정하고 있는데 이것에 추후 inference time에 얼마나 영향을 주게 될지 모르겠다 ㅠㅠ
  
3. 모델링
  - 일단 베이스라인 모델로도 잘 나오는 편이다. ~~(모든 참가자들의 점수가 이 이상이다 ㅋ)~~
  - 현재까지 Bi-directed LSTM 모델이 가장 잘 나온 것
  - ConvBiGRU는 왜때문에 0.7?
  - CNN+LSTM으로 최종 모델을 선택할 예정이다.
  - __오버피팅이 너무 무섭당__
  - 앙상블은 고민중이다.
  
  
  
  
