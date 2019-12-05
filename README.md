# KB_Smishing_dacon
dacon 14th : 금융문자 분석 경진대회
주최: KB금융지주, DACON , KISA(한국인터넷진흥원)
데이터: KB금융그룹 및 KISA(한국인터넷진흥원)에서 제공받은 정상문자와 스미싱 문자

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

## 4. Modeling
- Baseline: RandomForest, Logistic Regression, Naive Bayes 등 사용
  - RandomForest(하이퍼파라미터 수정 전)만으로 (수정버전)0.94가 나옴
- RNN
  - Simple RNN
  - LSTM
  - Bi-directed LSTM
  ~~- GRU~~
  ~~- Bi-directed GRU~~
- ConvRNN
  - ConvRNN
  - ConvLSTM
  - ConvGRU
  

## 5. Ensemble
- 

## 6. Predict and Submission
