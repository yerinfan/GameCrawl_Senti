
readme_content = """
# 🕹️ 게임 리뷰 감성 분석: 크롤링 & 모델 학습 모듈

Metacritic에서 게임 리뷰를 수집하고, 수집한 데이터를 기반으로 딥러닝 기반 감성 분석 모델을 학습하는 파이프라인을 학습한 프로젝트입니다.

---

## 📁 프로젝트 구성

```
.
├── models/                  # 학습된 모델 저장
│   ├── cnn_bilstm_model.h5
│   ├── cnn_en_sentiment.h5
│   └── tokenizer_final.pkl
├── data/                    # 크롤링/리뷰 데이터 저장
│   ├── merged_label_final.csv # 한글 리뷰
│   └── merged_reviews.csv   #영어 리뷰
├── reviews/                 # 게임 리뷰 CSV 모음
├── train.ipynb              # 한글 모델 학습 코드
├── train_en.ipynb           # 영어 모델 학습 코드
├── run_pc_reviews.py        # 리뷰 크롤링 실행 파일
├── title_crawler.py        # 리뷰 크롤링 실행 파일
└── README.md
```
---

## 🧩 기능 요약

### 📦 1. 게임 리뷰 크롤링

- `title_crawler.py`: Metacritic 게임의 제목과 URL을 Streamlit UI로 크롤링.
- `run_pc_reviews.py`: 전체 게임 리스트를 바탕으로 PC 플랫폼의 유저 리뷰 텍스트 및 평점 수집 자동화.

### 🔍 2. 감성 분류 모델 학습

- `train.ipynb`: 한국어 리뷰를 대상으로 CNN, BiLSTM, Hybrid 모델로 감성 분류 모델을 학습.
- 사용 데이터: lm studio로 번역된 영어 리뷰 + 팀원의 게임 리뷰 30만 건
- `train_en.ipynb`: 영어 리뷰 데이터를 기반으로 CNN 감성 분류 모델 학습.
- 사용 데이터: PS5, PC 카데고리 게임 리뷰 40만 건
- 점수 기준: `score >= 8` → positive, `score <= 3` → negative, 그 외는 neutral.

---

## 📊 기술 리포트

### 🎯 목적

- metacritic 사이트의 게임 리뷰를 크롤링하여 데이터 수집 및 감성 분석 학습

### 🔍 데이터 수집 방식

- Metacritic 웹사이트에서 Selenium을 통해 크롤링
- 게임 목록 → 각 게임 상세 리뷰 페이지 → 무한스크롤 로딩 → 리뷰 및 평점 추출
- 약 40만 건 이상의 리뷰 확보

### 🧠 모델 구성 (한글)

- 토큰화: Keras Tokenizer + Padding
- 모델 구조:
  - 1D CNN
  - BiLSTM
  - CNN + BiLSTM 하이브리드
- 정규화 및 과적합 방지:
  - Dropout
  - L2 Regularization
  - 불용어 제거 및 중복/짧은 문장 제거

### 🧠 모델 구성 (영어)

- 토큰화: Keras `Tokenizer` + 시퀀스 `padding`
- 모델 구조:
  - 1D CNN: 텍스트의 n-gram 패턴을 추출하는 컨볼루션 층
  - GlobalMaxPooling: 가장 강한 특징값 추출
  - Dense Layer: 은닉층 (ReLU 활성화)
  - Softmax Layer: 3-클래스 감성 분류 (`negative`, `neutral`, `positive`)
- 정규화 및 과적합 방지:
  - Dropout: 과적합을 방지하기 위해 은닉층에 적용
  - 입력 전처리:
    - 특수문자 제거
    - 3단어 미만 리뷰 필터링
    - OOV 토큰 처리: 훈련되지 않은 단어는 `<OOV>`로 대체


### ⚙️ 학습 환경

- Python 3.10 / TensorFlow 2.10 / Keras
- Colab 또는 GPU 지원 환경에서 학습 가능
- 다중 클래스 분류 (`positive`, `neutral`, `negative`)

### 💡 성능

- 하이브리드 CNN-BiLSTM 모델 기준(한글):
  - Validation Accuracy: 68~70%
  - 학습 데이터 약 30만개 기준

  - 혼동행렬   
  ![image](https://github.com/user-attachments/assets/a44278e6-4614-46aa-912d-d7ab2e20a0ec)
  - f1-score   
  ![image](https://github.com/user-attachments/assets/a1cb4b4e-ff35-495b-a4ae-4411e70ab640)


- CNN 모델 기준(영어):
  - Validation Accuracy: 90%
  - 학습 데이터 약 40만개 기준
  - 혼동행렬
  ![image](https://github.com/user-attachments/assets/10c8c1fe-0983-49c9-adb8-db448b8bc4a2)
  - f1-score
  ![image](https://github.com/user-attachments/assets/cb8b9b12-f20c-4d76-be12-d74d7b94c1f6)

---

### 프로젝트 진행하며 
1. metacritic 사이트는 리뷰 데이터를 js로 받아오기 때문에 selenium을 이용해야 함
- 과거에는 beautiful soup으로 크롤링한 글을 보고 따라했다가 헤맴
2. 50건 이상의 리뷰는 스크롤을 해야 확인 가능
- 페이지 형식으로 url 접속시 리뷰가 뜨긴하여 같은 리뷰인 줄 모르고 3만 건을 크롤링 했었음

---

## 🚀 실행 방법

```bash
# 가상환경 구성 권장
pip install -r requirement.txt
# 크롬 드라이버 다운로드
본인의 크롬 버전에 맞는 크롬 드라이버 설치

# 1. 크롤링 실행
python title_crawler.py
python run_pc_reviews.py

# 2. 모델 학습
# Jupyter 또는 Colab에서 train.ipynb, train_en.ipynb 열기
