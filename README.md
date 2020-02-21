# Dacon_14th__Prediction_Smishing
- dacon 14th : 금융문자 분석 경진대회
- 주최: KB금융지주, DACON , KISA(한국인터넷진흥원)
- 데이터: KB금융그룹 및 KISA(한국인터넷진흥원)에서 제공받은 정상문자와 스미싱 문자
    - 운영정책 및 개인정보보호를 위하여 대회 이후 데이터 다운로드 및 공유가 제한되어 있음

## 1. EDA
- 데이터 불균형
  - 정상인 문자 277,242건, 스미싱 문자 18,703건으로 데이터 불균형 현상(93.7%:6.3%)
  - <code> Counter({0: 277242, 1: 18703})
  0.0631975535994864 </code>

## 2. Sampling
- Mixed Sampling
  - 정상 문자 중 93,515건 __비복원추출__, 스미싱 문자 18,703x2건을 __복원추출__
  
## 3. Preprocessing
- Text Cleaning
  - Mecab을 이용하여 텍스트를 정제합니다.
    - 영문은 소문자화하고 한글, 영문 제외 모두 제거하였습니다.
    - 영문 중 xxx 등으로 비식별처리된 정보(개인,금융회사)는 모두 제거하였습니다.
    - 속도(inference time)을 고려해야 하므로 빠른 Mecab 형태소 분석기를 선택하였습니다.
  - Bi-gram을 생성합니다.
  - 모델을 학습하기 위해 텍스트 데이터를 시퀀스 데이터로 변환합니다. (integer encoding)
  - max_len을 정하기 위해 EDA를 하여 적당한 len을 결정하였습니다.

## 4. Modeling
- 시도한 모델들
  - Baseline: RandomForest, Logistic Regression, Naive Bayes 등 사용
    - RandomForest(하이퍼파라미터 수정 전)만으로 (수정버전)0.94가 나옴
  - RNN
    - Simple RNN
    - LSTM
    - Bi-directed LSTM: 0.96~9.99
    - ~~Bi-directed GRU~~:0.95~0.97
  - CNN+RNN
    - ConvRNN
    - ConvLSTM
    - ConvGRU
  - Ensemble
- Build Model
  - 모델 평가 지표는 AUC입니다.
  - 모델은 Bi-LSTM (Bi-Directional Long Short Term Memory)을 사용하였습니다.
  - 텍스트는 Bi-gram을 sequence로 변환한 데이터로 학습하였습니다.
  - EarlyStopping으로 과도한 학습을 하지 않고 적당한 score에 도달하면 조기종료 할 수 있도록 합니다.
 

## 5. Predict and Submission
- Submission for Public and Private
- __issue : Inference time (스미싱 문자 예측에 걸리는 시간) 순위 30% 점수 반영__
  - train된 모델이 있을 때 test 데이터를 올바른 형태로 전처리+predict하는 시간입니다 (train 전처리와 모델 training 시간은 포함되지 않습니다).


## 최종 제출을 위한 코드 정리 (01/14)

- 최종 순위는 데이콘에서만 가지고 있는 private test 데이터를 사용하여 AUC와 inference time을 측정합니다.
<code>
  Code_euphoria
</code>

    ├── 0_Data
      └─ train.csv
      └─ test.csv
    ├── 1_Model
      └─ model.json
      └─ model.h5
      └─ tokenizer.pickle
    ├── 2_Code_pred (here, Code_euphoria.ipynb)
    ├── 3_Code_train (here, Code_euphoria.ipynb)
    ├── PPT
    ├── readme.txt
  


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
  
4. Seed
    - Seed를 데이터 샘플링 시에만 적용했더니 결과가 계속 다르닼ㅋㅋ...
    - np, tf, K 등에도 모두 seed 고정

  
  



  
