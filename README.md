# 👁️ Retina Blood Vessel Segmentation using VGG16-UNet

Kaggle 망막 이미지 데이터셋을 활용하여 망막 이미지 내의 혈관 구조를 정밀하게 분할(Segmentation)하는 딥러닝 모델 구현 프로젝트입니다. VGG16을 Encoder로 활용한 UNet 구조를 통해 복잡한 혈관 네트워크 추출 성능을 개선하였습니다.

---

## 1. 프로젝트 개요
* **목표**: 망막 이미지 내 혈관의 픽셀 단위 분할 (Binary Segmentation)
* **모델 아키텍처**: VGG16-UNet (Pre-trained VGG16 Encoder)
* **데이터셋**: Kaggle Retina Image Dataset
---

## 2. 실험 기록

학습 과정에서 발생한 과적합 문제를 해결하고, 미세 혈관 분할 정밀도를 높이기 위해 다음과 같은 실험을 진행했습니다.

### ✅ 조기 종료 (Early Stopping) 최적화
* **현상**: 초기 `patience` 값을 10~20회로 설정했을 때, 모델이 혈관의 미세한 특징을 충분히 학습하기 전 조기 종료되는 문제 발생.
* **해결**: 실험적 조정을 통해 `patience`를 **50회**로 상향 조정.
* **결과**: 일시적인 손실(Loss) 변동에 대응하며 최적의 수렴 지점까지 충분한 학습 시간을 확보함.

### ✅ Dropout 도입 실험 및 분석
* **목표**: 얇은 혈관이 끊기거나 분할되지 않는 문제를 해결하기 위해 과적합 방지 기법인 Dropout 적용.
* **결과**:
    * **Dice Score**: 0.784 → 0.777 (**0.7% 하락**)
    * **Loss**: 0.324 → 0.325 (**상승**)
* **교훈**: Batch Normalization이 적용된 구조에서 Dropout을 추가하면 오히려 미세한 구조적 특징을 손실시킬 수 있음을 확인. 
* **최종 전략**: Dropout 대신 **조기 종료**와 **Weight Decay**를 사용하여 일반화 성능 유지.

---

## 3. 최종 성능 및 결과

검증 데이터셋(Validation Dataset) 기준 최종 성능은 다음과 같습니다.

| Metric | Score |
| :--- | :--- |
| **Dice Score** | **0.784** |
| **Loss** | **0.324** |

### 시각적 분석
* **굵은 혈관**: 높은 분할 정확도와 연속성을 보임.
* **미세 혈관**: 전반적인 구조 파악은 우수하나, 일부 얇은 혈관에서 단절 현상이 관찰되어 향후 개선 과제로 설정.

---

## 4. 기술 스택
* **Language**: Python
* **Framework**: PyTorch 
* **Architecture**: VGG16-UNet
* **Libraries**: OpenCV, Matplotlib, NumPy
---
