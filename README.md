# 🧠 Healthcare AI & Biomedical Study Record Portfolio

안녕하세요.  
2025.09.27 ~ 2026.03 동안 진행한 **OZ Coding School Healthcare AI BootCamp 과정**에서 수행한 연구, 프로젝트 및 실습 기록중 도메인 지식을 공부하고 모델링 하는데 흥미를 보였던 내용을 정리한 포트폴리오입니다.

진행하면서 쓴 정리 블로그 링크 입니다.  
👉 https://velog.io/@gudwns5863/posts

저는 기계시스템공학을 기반으로 **의료 인공지능(Healthcare AI)**과 **생체신호 분석 분야**를 목표로 학습을 진행해 왔으며,  
의료 데이터 분석 → 모델링일 여러차례 진행해 보았습니다.

---

# 📂 Repository Contents

---

## 📑 1. Paper Review

의료 AI 프로젝트를 진행하면서 읽은 논문들을 정리한 공간입니다.

### 주요 주제
- Computer Vision
- 보행 분석 (Gait Analysis)
- 낙상 위험도 (Fall Risk Prediction)
- 균형 평가 (Balance Assessment)
- 생체신호 처리 (Biomedical Signal Processing)
- 센서 기반 인간 움직임 분석

데이터 특성 · 사용된 알고리즘 · 임상적 의미 중심으로 리뷰 및 발표를 진행했습니다.  
발표 후 2~3번 더 읽고 모르는 내용 또는 놓친 부분을 추가적으로 이해하려고 시도하였습니다.

---

## 🫁 2. CNN 기반 폐 영상 분석 모델  
### 폐렴 흉부 X-ray 이미지 데이터셋 2진분류 과제

의료 영상 데이터를 활용하여 폐렴 여부를 예측하는 딥러닝 모델을 구축 과제 정리입니다.

**모델 주소**  
https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia?select=chest_xray  
kaggle 폐렴 흉부 x-ray 이미지 데이터셋 활용

### 폴더 구조
train (Pneumonia / Normal)
test (Pneumonia / Normal)
valid (Pneumonia / Normal)


### 모델 전처리
- 입력크기 : 150 x 150 RGB 이미지
- rescaling 을 통해 픽셀값 0~1 사이의 값으로 정규화
- prefetch(autotune) 적용으로 학습 중 데이터 로딩 병목을 최소화 하여 학습 속도 개선

### 데이터 증강
의료 영상 데이터는 개수가 적기 때문에 과적합 매우 많이 발생 → 인위적으로 데이터를 조금씩 변형시켜 일반화 성능 향상 시켜야 함

사용한 증강
- Random Horizontal Flip
- Random Rotation
- Random Zoom

### CNN Backbone
- Conv2D → ReLU → MaxPooling 반복
- 채널 수 32 → 64 → 128 → 128

### Classifier
- 손실함수 : Binary Crossentropy
- Optimizer : Adam
- Metric : Accuracy, Precision, Recall, F1  
  (의료 데이터는 accuracy 만으로 평가하지 않음)

### results
accuracy: 0.8462 - f1: 0.8868 - loss: 0.3638 - precision: 0.8210 - recall: 0.9641 

---

## 🚬 3. Smoke Status Prediction (Hackathon)

daicon 건강검진 데이터를 활용한 흡연 여부 예측 머신러닝 모델

**Accuracy : 76.533**

daicon 제공 개인 혈액 검사 및 건강 지표 데이터를 기반으로  
흡연자(Smoker) / 비흡연자(Non-Smoker)를 예측하는 이진 분류 모델 개발

### 핵심 수행 내용
- 결측치 처리
- 이상치 제거 (Outlier Handling)
- 파생 변수 생성 (Feature Engineering) — 총 8가지 기법 적용
- 데이터 분포 분석 (EDA)
- AutoGluon 기반 모델 학습 및 자동 앙상블

### 평가 지표
- Accuracy 기반 평가

### 배운 점
의료 데이터는 모델보다 전처리와 변수 설계가 성능을 결정한다는 것을 경험한 프로젝트입니다.

---

## 🧬 4. IVF Pregnancy Success Prediction (Hackathon)

daicon 난임 환자의 시험관 아기(IVF) 시술 성공 여부 예측 AI 프로젝트

**ROC-AUC : 0.742**

난임 환자 임상 데이터를 활용한 환자 임신 성공 가능성 예측 머신러닝 모델 개발

### 문제 배경
- IVF 성공률은 평균적으로 20~40% 수준
- 반복 시술로 인한 경제적/심리적 부담 존재
- 의료진의 경험에 의존한 의사결정 구조

### 목표
환자의 시술 데이터를 기반으로 개인 맞춤형 임신 성공 확률을 예측하는 의사결정 보조 모델 구축

### 사용 데이터 특징
- 시술 당시 나이
- 시술 유형 (IVF, ICSI 등)
- 배아 관련 정보
- 임신/출산 이력
- 불임 원인 포함 67가지 피쳐 전처리 학습

### 모델링
- CatBoost
- LightGBM
- XGBoost
- Optuna → 하이퍼파라미터 최적화
- 앙상블 모델 적용 → 최적의 모델 개발

### 핵심 작업 (논문 탐색)

**Can repeat IVF/ICSI cycles compensate for the natural decline in fertility with age?**  
*an estimate of cumulative live birth rates over multiple IVF/ICSI cycles in Chinese advanced-aged population*

고령 여성 관련 난임 논문을 확인하고 파생변수 생성 시도

- 고령 여성은 시험관 여러 번 하면 자연적인 가임력 감소를 어느 정도 만회할 수 있나? 를 실제 데이터로 분석한 논문
- 논문 결론에서 고령 여성도 반복하면 성공률이 올라가긴 한다 하지만 나이 증가로 인한 가임력을 완전히 회복하지 못한다는 걸 확인
- 40세 이후에는 여러 번 시도해도 성공률 증가가 제한적이다 결론 확인 → 35세 이상 / 40세 이상 (파생변수 생성) 구간화 후 학습 진행
- 파생변수 생성 후 ROC-AUC 가 전반적으로 상승하며, 특히 40세 이상 고위험군의 시술 실패 확인하는 recall 수치 집중적 향상됨 확인

논문 링크  
https://www.aging-us.com/article/203055/text

---

# 🛠️ Skills

**Language & Tools**  
Python, Machine Learning, Scikit-learn, XGBoost, LightGBM, CatBoost, AutoGluon, Optuna,  
Data Analysis, Pandas, Numpy, Matplotlib, Seaborn, Deep Learning, PyTorch (CNN)

---

# 🎯 Research Interest
- Biomedical Signal Processing
- Gait Analysis
- Fall Risk Prediction
- Personalized Healthcare AI

---

# 📌 Goal
기계공학에서 배우는 인간 움직임 분석 및 신호 해석을 의료 데이터 분석과 결합하여  
실제 임상 및 일상생활에서 활용 가능한 기술을 다루는 의공학 연구자가 되는 것이 목표입니다.

---

# 📬 Contact
- Email: gudwns5863@jbnu.ac.kr
- Phone: 010-9617-5863
