
readme_content = """
# 🕹️ 게임 리뷰 감성 분석: 크롤링 & 모델 학습 모듈

이 저장소는 Metacritic에서 게임 리뷰를 수집하고, 수집한 데이터를 기반으로 딥러닝 기반 감성 분석 모델을 학습하는 파이프라인을 포함하고 있습니다.

---

## 📁 프로젝트 구성

```
.
├── models/                  # 학습된 모델 저장
│   ├── cnn_bilstm_model.h5
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

### 🧠 모델 구성

- 토큰화: Keras Tokenizer + Padding
- 모델 구조:
  - 1D CNN
  - BiLSTM
  - CNN + BiLSTM 하이브리드
- 정규화 및 과적합 방지:
  - Dropout
  - L2 Regularization
  - 불용어 제거 및 중복/짧은 문장 제거

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
  - 
---

## 🚀 실행 방법

```bash
# 가상환경 구성 권장
conda activate nlp-tfgpu
# 크롬 드라이버 다운로드
본인의 크롬 버전에 맞는 크롬 드라이버 설치

# 1. 크롤링 실행
python run_pc_reviews.py

# 2. 모델 학습
# Jupyter 또는 Colab에서 train.ipynb, train_en.ipynb 열기
