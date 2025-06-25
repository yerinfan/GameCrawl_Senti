
readme_content = """
# 🕹️ 게임 리뷰 감성 분석: 크롤링 & 모델 학습 모듈

이 저장소는 Metacritic에서 게임 리뷰를 수집하고, 수집한 데이터를 기반으로 딥러닝 기반 감성 분석 모델을 학습하는 파이프라인을 포함하고 있습니다.

---

## 📁 프로젝트 구성
.
├── run_pc_reviews.py # 전체 PC 게임 리뷰 크롤러
├── title_crawler.py # 게임 제목 및 URL 수집 (Streamlit 포함)
├── train.ipynb # 한글 감성 분석 모델 학습 코드
├── train_en.ipynb # 영어 감성 분석 모델 학습 코드
├── reviews/ # 크롤링된 리뷰 CSV 저장 폴더
├── models/ # 학습된 모델 저장 위치 (예: cnn_bilstm_model.h5)
└── tokenizer_final.pkl # 학습에 사용된 Tokenizer 저장

---

## 🧩 기능 요약

### 📦 1. 게임 리뷰 크롤링

- `title_crawler.py`: Metacritic PS5 게임의 제목과 URL을 Streamlit UI로 크롤링.
- `run_pc_reviews.py`: 전체 게임 리스트를 바탕으로 PC 플랫폼의 유저 리뷰 텍스트 및 평점 수집 자동화.

### 🔍 2. 감성 분류 모델 학습

- `train.ipynb`: 한국어 리뷰를 대상으로 CNN, BiLSTM, Hybrid 모델로 감성 분류 모델을 학습.
- `train_en.ipynb`: 영어 리뷰 데이터를 기반으로 감성 분류 모델 학습.
- 점수 기준: `score >= 8` → positive, `score <= 3` → negative, 그 외는 neutral.

---

## 📊 기술 리포트 (Tech Report)

### 🎯 목적

- 게임 리뷰 기반의 감성 분석 모델을 구축하여 사용자 피드백을 자동으로 분류하고, 이를 Streamlit에서 실시간 시각화 가능하도록 지원합니다.

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

- 하이브리드 CNN-BiLSTM 모델 기준:
  - Validation Accuracy: 68~70%
  - 불균형 데이터로 인해 F1-score 기준 평가 권장
  - 학습 데이터 약 30~40만개 기준

---

## 🚀 실행 방법

```bash
# 가상환경 구성 권장
conda activate nlp-tfgpu

# 1. 크롤링 실행
python run_pc_reviews.py

# 2. 모델 학습
# Jupyter 또는 Colab에서 train.ipynb, train_en.ipynb 열기
