# Team SIGCOM - 2024 DCC Project Summary
---

## 팀 소개
- **팀명:** TEAM SIGCOM (충북대학교 정보통신공학부)
- **팀 구성원:**
  - **이규하** (팀장, 21학번) - 프로젝트의 전반적인 기획과 팀 관리를 담당.
  - **곽도현** (팀원, 21학번) - 데이터 전처리 및 분석 작업에 기여.
  - **강찬솔** (팀원, 23학번) - 모델링 작업을 주도하며 코드 최적화와 검증 과정에 참여.
  - **허강민** (팀원, 21학번) - 평가 및 결과 시각화와 협업 필터링 기법을 적용하여 분석에 도움을 줌.
  
TEAM SIGCOM은 데이터와 인공지능 분야에서 실제 문제를 해결하고, 다양한 산업 분야에 실질적인 도움이 되는 연구를 진행하는 것을 목표로 합니다. 본 프로젝트에서는 데이터 크리에이터 캠프에 참가하여 실제 데이터를 다루는 과정에서 인공지능과 데이터 분석 기술을 심도 있게 배우고자 하였습니다.

## Team Introduction

- **Team Name:** TEAM SIGCOM (Chungbuk National University, Department of Information and Communication Engineering)
- **Team Members:**
  - **Gyuha Lee** (Team Leader, Class of 2021) - Oversees overall project planning and team management.
  - **Dohyun Kwak** (Member, Class of 2021) - Contributes to data preprocessing and analysis tasks.
  - **Chansol Kang** (Member, Class of 2023) - Leads the modeling process and participates in code optimization and validation.
  - **Kangmin Heo** (Member, Class of 2021) - Focuses on evaluation, results visualization, and applying collaborative filtering methods for analysis.

TEAM SIGCOM aims to address real-world problems through AI and data science, conducting research with practical applications across various industries. For this project, the team participated in the Data Creator Camp to deepen their understanding of AI and data analysis by working with real-world datasets.

---

## Mission 1: 데이터셋 분석 및 전처리

1. **데이터셋 구성 확인**
   - **폴더 구조 분석:** 주어진 데이터셋의 폴더 구조를 먼저 파악하여, 이후 분석과 학습에 필요한 데이터 접근 방식을 정립하였습니다. 
      - 폴더 구조는 `training_image`, `training_label`, `validation_image`, `validation_label`로 구분되어 있었으며, 각각 학습용과 검증용 데이터로 구성되어 있습니다.
   - **파일 명명 규칙 확인:** 각 이미지 파일은 `{W/T}_{이미지ID}_{시대}_{스타일}_{성별}.jpg` 형식으로 구성되어 있으며, 이를 통해 이미지의 성별, 시대, 스타일을 구분할 수 있었습니다. 이를 기반으로 성별과 스타일별 통계를 작성할 수 있도록 데이터 분석의 방향을 설정하였습니다.

2. **데이터 전처리**
   - **배경 제거 실험:** 
      - 데이터의 노이즈를 줄이고 모델의 정확도를 높이기 위해 이미지에서 배경을 제거하는 작업을 수행하였습니다. 이 과정에서 `DeepLabv3`, `YOLOv5`, `RemBG` 세 가지 모델을 사용하여 각각의 성능을 평가하였습니다.
   - **모델 비교 결과 및 최종 선정:** 
      - `DeepLabv3`: 픽셀 단위 이미지 분할을 통해 배경을 제거하였으나, 옷 영역이 손상되는 문제가 발생해 적절하지 않다고 판단하였습니다.
      - `YOLOv5`: 객체 탐지 모델을 사용하여 이미지에서 특정 객체를 탐지하고 배경을 제거했지만, 탐지 오류로 인해 완전한 배경 제거가 어려웠습니다.
      - `RemBG`: U²-Net 기반으로 배경을 제거하였으며, 옷을 입은 영역만 남기고 나머지 배경을 검은색으로 처리하여 최적의 전처리 성능을 보였습니다. 최종적으로 RemBG 모델을 선택하였습니다.
   - **전처리 환경 설정:** 
      - Google Colab 환경에서 배경 제거를 시도했으나, Colab 런타임 세션이 종료되면 파일이 사라지는 문제를 해결하기 위해 로컬 PC에서 작업을 병행하여 안정적인 전처리 환경을 확보하였습니다.


## Mission 1: Dataset Analysis and Preprocessing

### 1. Dataset Structure Analysis

- **Folder Structure Review:**  
  The dataset was analyzed to establish data access methods necessary for future tasks. The dataset is organized into four categories: `training_image`, `training_label`, `validation_image`, and `validation_label`, clearly separated for training and validation purposes.
  
- **File Naming Convention Review:**  
  Each image file follows the naming format `{W/T}_{imageID}_{era}_{style}_{gender}.jpg`, which encodes gender, era, and style information. This structure guided the direction for statistical analysis by gender and style.

### 2. Data Preprocessing

- **Background Removal Experimentation:**  
  To reduce noise and enhance model accuracy, background removal was performed using three methods: `DeepLabv3`, `YOLOv5`, and `RemBG`.

- **Model Performance Comparison and Final Selection:**
  - **`DeepLabv3`**: Although it effectively removed the background at a pixel level, clothing areas were often damaged, rendering it unsuitable.
  - **`YOLOv5`**: Object detection-based background removal showed promise but was limited by detection errors.
  - **`RemBG`**: Built on U²-Net, this method retained only the clothing area while masking the background in black, yielding the best preprocessing results. Consequently, `RemBG` was selected as the final method.

- **Preprocessing Environment Configuration:**  
  While initially utilizing Google Colab for preprocessing, runtime interruptions prompted the use of local PCs to ensure stable and consistent processing.

---

## Mission 2: 모델 학습 및 파라미터 최적화

1. **모델 구조 및 학습 파라미터 설정**
   - **모델 선정:** 
      - CNN 기반의 ResNet-18 모델을 선택하였습니다. 사전 학습 가중치를 사용하지 않고 처음부터 학습을 시작하여 데이터셋에 맞게 모델을 최적화하였습니다.
   - **학습 파라미터 설정:** 
      - `Batch Size`와 `Learning Rate` 등 다양한 파라미터를 조정하여 최적의 학습 결과를 얻기 위해 실험을 반복하였습니다.
   - **학습 환경 구성:** 
      - Google Colab의 GPU (L4, A100)와 팀원들의 로컬 PC (4070 Super GPU)를 활용하여 모델을 학습하였고, 각 환경에서 Validation Accuracy와 학습 속도를 비교하여 최적화된 파라미터를 도출하였습니다.

2. **학습 결과 및 최적화 과정**
   - **Colab 환경:** 
      - A100 GPU 환경에서 `Batch Size: 128`, `Learning Rate: 0.001`로 학습한 결과, Validation Accuracy가 64.14%로 나타나 최적의 성능을 확인하였습니다.
   - **Batch Size와 Learning Rate에 따른 검증 성능:** 
      - Batch Size를 크게 설정할수록 학습 속도는 빨라졌으나 Colab에서 메모리 사용량이 증가하여 런타임 문제가 발생할 수 있어, 최적의 배치 크기를 128로 설정하였습니다.
      - 학습률이 너무 높으면 Validation Loss가 증가하는 문제를 발견하여 적절한 값을 찾아 설정하였습니다.
   - **성능 최적화:** 
      - Colab과 로컬 환경에서의 GPU 성능 차이를 바탕으로 학습 속도와 메모리 사용량을 최적화하고, 최적의 검증 정확도를 유지할 수 있도록 파라미터를 조정하였습니다.

## Mission 2: Model Training and Parameter Optimization

### 1. Model Selection and Training Parameter Configuration

- **Model Selection:**  
  ResNet-18, a CNN-based model, was chosen. Training commenced from scratch without pretrained weights to optimize the model for the dataset.

- **Training Parameter Adjustment:**  
  Parameters such as `Batch Size` and `Learning Rate` were iteratively adjusted to achieve optimal results.

- **Training Environment Setup:**  
  Both Google Colab GPUs (L4, A100) and local PCs (4070 Super GPU) were employed for training. Validation accuracy and training speed were compared across environments to optimize parameters.

### 2. Training Results and Optimization Process

- **Colab Performance Results:**  
  Using the A100 GPU, the team achieved a Validation Accuracy of 64.14% with `Batch Size: 128` and `Learning Rate: 0.001`.

- **Impact of Batch Size and Learning Rate on Validation Performance:**  
  Larger batch sizes improved training speed but increased memory usage, occasionally causing runtime issues. A batch size of 128 was determined to be optimal. Adjusting the learning rate prevented issues such as increased validation loss, enabling fine-tuning of the training process.

- **Performance Optimization:**  
  The differences in GPU performance between Colab and local environments informed adjustments to balance training speed, memory usage, and validation accuracy.

---

## Mission 3: 협업 필터링을 통한 스타일 선호 예측

1. **협업 필터링 기법 도입 및 유사도 계산**
   - **기법 선정:** 
      - User-based Filtering과 Item-based Filtering 방식을 적용하여 각각의 장단점을 비교하고, 각 방식이 스타일 선호 예측에 미치는 영향을 분석하였습니다.
   - **User-based Filtering:** 
      - 스타일 선호도가 비슷한 응답자들 간의 유사성을 바탕으로 새로운 스타일에 대한 선호 여부를 예측하는 방식입니다. 응답자들 간의 유사도는 코사인 유사도를 사용하여 계산하였고, 이를 통해 유사한 응답자가 선호하는 스타일을 예측하는 구조를 설정하였습니다.
   - **Item-based Filtering:** 
      - 특정 스타일을 선호하는 응답자가 유사한 스타일도 선호할 가능성이 있다는 가정 하에 스타일 간 유사성을 바탕으로 예측을 수행하였습니다. ResNet-18의 중간 레이어에서 추출한 특징 벡터를 사용하여 각 스타일의 유사도를 계산하였고, 코사인 유사도를 활용하여 최종 선호도를 예측하였습니다.

2. **예측 성능 평가 및 결과 분석**
   - **중간 레이어 활용 이유:** 
      - 중간 레이어에서 추출한 특징 벡터는 이미지의 저수준 정보와 고수준 정보의 균형을 유지하는 벡터로, 과적합 없이 일반화된 특징을 제공하기 때문에 스타일 간의 유사도를 효과적으로 계산할 수 있었습니다.
   - **User-based vs. Item-based Filtering 성능 비교:** 
      - User-based Filtering은 응답자 개개인의 취향을 반영할 수 있는 장점이 있지만, 특정 스타일에 대한 데이터가 부족한 경우 예측 성능이 저하되는 문제가 있었습니다.
      - Item-based Filtering은 스타일 간 유사성을 바탕으로 예측하여 데이터가 부족한 상황에서도 성능이 안정적이었습니다. 이는 응답자의 스타일 선호가 다양하거나 데이터 분포가 불균형한 경우에도 일관된 성능을 유지할 수 있는 장점으로 작용하였습니다.

3. **최종 결과 요약 및 향후 과제**
   - **검증 결과 요약:** 
      - Item-based Filtering 방식을 사용하여 새로운 스타일에 대한 예측을 수행하였으며, 유사한 스타일 간 관계를 기반으로 예측 정확도를 높일 수 있었습니다.
   - **향후 방향:** 
      - 다양한 필터링 기법을 더욱 심화하여 예측 모델의 성능을 향상시키고, 사용자 맞춤형 AI 기반 추천 시스템을 개발하는 데 활용할 계획입니다.
      - 추가적으로 학습 데이터의 범위를 확장하고, 모델의 일반화 성능을 높이기 위한 다양한 데이터 증강 기법과 최신 AI 모델을 적용할 예정입니다.

## Mission 3: Style Preference Prediction via Collaborative Filtering

### 1. Collaborative Filtering and Similarity Calculation

- **Method Selection:**  
  Both User-based Filtering and Item-based Filtering approaches were applied to analyze their impact on style preference prediction.

- **User-based Filtering:**  
  This method predicts preferences for new styles based on the similarity between respondents' preferences, calculated using cosine similarity.

- **Item-based Filtering:**  
  Assuming respondents who prefer certain styles are likely to favor similar ones, this approach used feature vectors extracted from ResNet-18’s intermediate layers to compute style similarities and make predictions.

### 2. Prediction Performance Evaluation

- **Use of Intermediate Layers:**  
  Feature vectors from intermediate layers balance low-level and high-level information, offering generalized features that effectively calculate style similarities without overfitting.

- **Comparison of User-based and Item-based Filtering:**
  - **User-based Filtering:** Reflects individual preferences but suffers when data for specific styles is sparse.
  - **Item-based Filtering:** Achieves stable performance even in cases of data imbalance by leveraging style-to-style relationships, ensuring consistent predictions.

### 3. Summary and Future Work

- **Summary of Validation Results:**  
  Predictions using Item-based Filtering improved accuracy by focusing on relationships between styles.

- **Future Directions:**  
  Plans include enhancing prediction models through advanced collaborative filtering techniques, expanding the dataset, and applying data augmentation and cutting-edge AI methods to improve generalization and accuracy.


---

## 주요 성과 및 결과

- **배경 제거 성능 향상:** RemBG 모델을 활용하여 이미지 배경을 효과적으로 제거하였고, 이를 통해 데이터의 노이즈가 줄어들어 학습의 정확도가 높아졌습니다.
- **최적의 모델 학습 설정 도출:** 학습률, 배치 크기, GPU 환경 설정을 통해 Validation Accuracy에서 최고 64.14%를 달성하였습니다.
- **협업 필터링 기법의 적용:** User-based와 Item-based Filtering 방식을 비교하고, 스타일 유사성에 따른 예측을 통해 정확도를 높였습니다.
- **환경별 성능 분석:** Colab과 로컬 환경에서의 학습 속도 및 정확도 차이를 비교하여 최적의 학습 환경을 도출하고, 실시간으로 학습 상태를 모니터링할 수 있도록 설정하였습니다.

## Key Achievements and Outcomes

- **Enhanced Background Removal:**  
  The adoption of the RemBG model significantly reduced noise in the dataset, improving model accuracy.

- **Optimized Training Settings:**  
  The team achieved a Validation Accuracy of 64.14% through adjustments to learning rate, batch size, and GPU configurations.

- **Implementation of Collaborative Filtering:**  
  Both User-based and Item-based Filtering methods were applied, with Item-based Filtering providing consistent accuracy even in data-scarce scenarios.

- **Performance Insights:**  
  Differences in performance between Colab and local environments were analyzed, leading to the identification of an optimal training environment.

---

### 결론 및 향후 발전 방향

이번 프로젝트를 통해 TEAM SIGCOM은 데이터 분석 및 인공지능 모델의 실무 적용 경험을 쌓고, 각종 전처리 및 파라미터 설정을 통한 학습 성능 최적화를 경험하였습니다. 이 과정에서 습득한 기술은 향후 더 복잡한 데이터와 다양한 모델을 다룰 때 유용하게 활용될 것입니다. 앞으로도 AI와 데이터 분석 기술을 더욱 심화하여, 실질적인 문제 해결을 위한 연구를 지속하고, 예측 모델을 통해 사회적 가치 창출에 기여하고자 합니다. 특히, 추가적인 파라미터 최적화와 최신 AI 기법을 도입하여 더 높은 예측 정확도와 범용성을 확보할 계획입니다.

## Conclusion and Future Developments

Through this project, TEAM SIGCOM gained hands-on experience in applying AI and data analysis techniques to real-world problems. The team successfully optimized training processes and parameters, improving their understanding of practical model deployment. These skills will prove invaluable in future projects involving more complex datasets and models. Moving forward, the team plans to deepen its expertise in AI and data analysis, leveraging advanced predictive models to create solutions that deliver social value. Further parameter optimization and the adoption of state-of-the-art AI techniques will be prioritized to achieve higher accuracy and versatility.

---

### 참고 문헌 및 자료
- **ResNet-18 모델 아키텍처 및 특징 벡터 추출법**
- **데이터 전처리 및 배경 제거 방법론**
- **협업 필터링 기법의 이론 및 실제 적용 사례**
- **Colab 및 로컬 GPU 환경의 성능 비교와 최적화 전략**

## References
- **ResNet-18 Model Architecture and Feature Vector Extraction**
- **Data Preprocessing and Background Removal Techniques**
- **Collaborative Filtering Methods: Theory and Practical Applications**
- **Performance Comparisons of Colab and Local GPU Environments**
