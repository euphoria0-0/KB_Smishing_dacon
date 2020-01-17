# Dacon 

## 0. 문제 정의


## 1. 데이터 전처리 & EDA
1. 데이터 살펴보기
 - 데이터 불균형 문제
 - 텍스트 특성

2. 텍스트 전처리
 - 제거: 불용어, 한글 영문 외 문자, XXX 등 비식별처리된 문자
 - Mecab을 이용해 tokenizing
 - tokenizing 후 unigram, bi-gram 저장
 
3. 데이터 샘플링
 - 데이터 불균형 문제 해결을 위한 hybrid approach(Mixed Sampling)

4. 텍스트 전처리-2
 - 토큰화한 후 단어에 대하여 정수로 임베딩
 - 이때 모든 샘플의 길이를 800으로 동일하게 저장

## 2. 모델 구축 & 검증
1. 모델 구축
 - 평가지표는 AUC
 - 모델은 BiLSTM 이용
 - 데이터는 전처리된 Bi-gram 이용

2. 모델 학습
 - Unigram, Bigram 등 중 Bigram
 - simple한 BiLSTM 모델

## 3. 결과 및 결언
 - 기본적인 점수가 높게 나온 이유는?
 - 시도했으면 좋았을 것들
 

## 9. 참고
- 실행 환경: Python 3.6 and (Colab Notebooks or Ubuntu)
- 필요 라이브러리: 
