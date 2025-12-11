<div align="center">

# 💊 Pill Detection : Object Detection & Classification
**"단 한 장의 이미지, 4개의 알약, 완벽한 분류와 위치 검출"**

</div>

---

## 1. 프로젝트 개요 (Overview)

**Pill Detection Project**는 한 이미지 내에 존재하는 최대 4개의 알약 객체를 탐지하고,<br> 정확한 클래스로 분류하는 **Object Detection 대회**입니다.

| 구분 | 상세 내용 |
| :--- | :--- |
| **기간 및 배경** | • **기간** : 2025.09.09 ~ 2025.09.25 (Bootcamp Kaggle Competition)<br>• **목표** : 이미지 내 알약의 <b>위치(Bbox)</b>와 <b>종류(Class)<b>를 동시에 예측 |
| **핵심 과제** | • **Data Quality** : 라벨 노이즈(누락, 겹침) 제거 및 데이터 정제<br>• **Imbalance** : 클래스 불균형 해소를 위한 증강 및 샘플링 전략 수립<br>• **Modeling** : 최신 YOLO 모델(v12) 도입 및 하이퍼파라미터 최적화 |
| **평가 지표** | • **mAP@[0.75:0.95]** (Mean Average Precision)<br>• 엄격한 IoU Threshold 기준을 적용하여 정밀한 탐지 성능 평가 |
| **최종 성과** | • **🏆 Kaggle Private Leaderboard 1위 달성 (Score : 0.993)** |

---

## 2. 최종 결과 (Final Results)

### 🏆 Leaderboard & Inference
다양한 모델링과 전처리 실험을 통해 최종적으로 <b>1위(0.993)</b>를 달성했습니다.

<p align="center">
  <img width="100%" alt="Leaderboard Result" src="https://github.com/user-attachments/assets/004dcfec-e0cd-4f37-89a5-d7a177afa9ed" />
  <img width="100%" alt="Inference Example" src="https://github.com/user-attachments/assets/3afac4c6-8766-479f-a8b4-948e1998fe7c" />
</p>

---

## 3. 팀 소개 (Team Contributions)
> **"데이터의 품질이 성능을 결정한다."는 원칙 아래 집요하게 파고든 팀입니다.**

| 프로필 | 이름 | 상세 기여 내용 (Key Contributions) |
| :---: | :---: | :--- |
| <img src="https://github.com/Jinuk93.png" width="100"> | **김진욱**<br>*(Modeling)* | • **클래스 불균형 실험** : 단순 오버샘플링(mAP 0.88) 및 Sticker(Copy-Paste) 방식 시도<br>• **인사이트 도출** : Sticker 방식의 실패(mAP 0.77)를 통해 **'문맥(Context) 유지'**의 중요성 입증<br>• **Final Model** : YOLOv12m / imgsz 960 / AdamW 최적화 (Kaggle 0.993 달성) |
| <img src="https://github.com/Geundol222.png" width="100"> | **이현석**<br>*(Labeling)* | • **Label Refinement** : 라벨 누락 데이터 전수 검사 및 보정 (**0.965 → 0.976**)<br>• **Bbox Correction** : 객체 중심과 비율을 고려한 CIoU 기반 좌표 미세 조정 (**0.976 → 0.983**)<br>• **AutoML** : Random Search(2,187 조합)를 통한 하이퍼파라미터 탐색 |
| <img src="https://github.com/rjsgml8723-crypto.png" width="100"> | **함건희**<br>*(Training)* | • **Scheduling** : Epoch 임계점(10~50ep) 비교 실험. 50ep에서 과적합(Oscillation) 확인<br>• **Augmentation** : Rotation, TTA 등 강한 증강 기법이 오히려 성능을 저하시킴을 규명<br>• **Model Size** : YOLO Size별(n, s, m) 성능 비교 및 최적 모델 선정 |
| <img src="https://github.com/jinsugyeong.png" width="100"> | **진수경**<br>*(Verification)* | • **Re-verification** : Sticker 방식 실패 원인(문맥 상실) 재검증 및 분석<br>• **Reproducibility** : 실험의 재현성 확보를 위한 파라미터 및 데이터 버전 관리<br>• **Collaboration** : 팀원 간 실험 결과 통합 및 성능 병목 구간 분석 |
| <img src="https://github.com/Kuron08.png" width="100"> | **박병현**<br>*(Preprocessing)* | • **Image Enhancement** : CLAHE, Unsharp Mask 등 선명도 개선 알고리즘 실험<br>• **Pipeline** : Inference 단계 자동 선명도 조절 파이프라인 구축 (`infer.py`)<br>• **Optimization** : 과도한 전처리(Sharpen)가 노이즈를 유발하여 성능 하락함을 확인 |
| <img src="https://github.com/Loonie95.png" width="100"> | **오형주**<br>*(Support)* | • **Documentation** : 실험 코드 정리 및 프로젝트 문서화 유지보수<br>• **Analysis Support** : 실험 결과 로그 정리 및 리포트 작성 지원 |

---

## 4. 핵심 실험 및 분석 (Key Experiments)

저희 팀은 <b>"모델보다 데이터"</b>라는 가설을 검증하기 위해 단계별 실험을 진행했습니다.

### 🧪 1. 데이터 품질 개선 (Data Quality)
가장 큰 성능 향상은 모델 변경이 아닌 **라벨 데이터 수정**에서 발생했습니다.
- **라벨 누락 보정** <br>: `mAP 0.965` → `0.976` (**+0.011**) 🔺
  - 육안 검수를 통해 Ground Truth가 누락된 데이터를 찾아내어 재라벨링 수행
- **Bbox 정제 (CIoU)** <br>: `mAP 0.976` → `0.983` (**+0.007**) 🔺
  - 객체 중심과 비율을 고려하여 Bbox 좌표를 미세 조정
- **겹침(Overlap) 완화** <br>: 잘못된 라벨 학습 방지로 안정성 향상 (`0.965` → `0.966`)

---

### 🧪 2. 전처리 및 증강 (Preprocessing)
"과도한 증강은 독이 된다"는 것을 확인했습니다.
- **Sharpen/CLAHE 과적용** <br>: `0.990` → `0.985` 📉 (색상 왜곡 및 노이즈로 인한 성능 저하)
- **Sticker(Copy-Paste)** <br>: 배경 문맥(Context)이 사라져 성능 하락 확인
- **결론** <br>: 원본 이미지의 특성을 해치지 않는 선에서의 **보수적인 증강** 채택

---

### 🧪 3. 모델링 및 학습 전략 (Modeling)
- **Model Selection** <br>: YOLOv8, v10, v11 비교 실험 끝에 **YOLOv12m** 선정
- **Epoch** <br>: 10 Epoch에서 50 Epoch까지 늘려보았으나, 특정 시점 이후 과적합(Overfitting) 발생 확인
- **Resolution** <br>: `imgsz=960`에서 최적 성능 도출

---

### 📊 실험 요약표 (Experiment Summary)

| 트랙 | 시도 (Attempt) | 핵심 결과 (Result) | 결론 (Conclusion) |
| :--- | :--- | :--- | :--- |
| **라벨 품질** | pseudo-label → CIoU 보정 | `0.965 → 0.983` 🔺 | **라벨 품질 개선이 최우선** (가장 큰 효과) |
| **데이터 정제** | Bbox 겹침 완화 | `0.965 → 0.966` 🔺 | 정합성 확보로 학습 안정성 소폭 개선 |
| **전처리** | Sharpen / CLAHE 과적용 | `0.990 → 0.985` 📉 | 과도한 전처리는 노이즈 유발로 역효과 |
| **불균형 해소** | 단순 복제 / Sticker | 개선 미미 / 하락 | 단순 증강보다 **문맥(Context) 유지**가 중요 |
| **최적화** | YOLOv12m + Epoch 10 | **Kaggle 0.993 (1위)** 🥇 | **강한 모델 + 고품질 데이터** 조합이 정답 |

---

## 5. 데이터셋 구조 (Dataset)

- **Source :** AI Hub 경구약제 이미지 데이터 (가공 버전 제공)
- **Format :** PNG / COCO JSON Format

```bash
DATASET/
├── train_images/          # 📄 학습 이미지
├── train_annotations/     # 🏷️ 학습용 어노테이션 (JSON)
├── test_images/           # 📄 테스트 이미지
└── sample_submission.csv  # 📝 제출 양식
```

**📌 Annotation 주요 필드**

| 필드명 | 설명 |
| :--- | :--- |
| `image_id` | 이미지 고유 ID |
| `category_id` | 알약 클래스 ID |
| `bbox` | `[x, y, width, height]` 좌표 |
| `annotation_id` | 고유 바운딩 박스 ID |

## 6. 개인별 실험 폴더 (Experiments)

팀원들이 각자 수행한 실험 코드는 아래 폴더에서 확인할 수 있습니다.

| 이름 | 경로 (Link) | 주요 내용 |
| :---: | :--- | :--- |
| **김진욱** | [📂 ./Personal/KJW/](./Personal/KJW/KJW.md) | YOLOv12 모델링, 클래스 불균형 실험 |
| **박병현** | [📂 ./Personal/PBH/](./Personal/PBH/PBH.md) | 이미지 전처리(CLAHE) 및 추론 파이프라인 |
| **오형주** | [📂 ./Personal/OHJ/](./Personal/OHJ/OHJ.md) | 베이스라인 코드 및 유틸리티 |
| **이현석** | [📂 ./Personal/LHS/](./Personal/LHS/LHS.md) | 라벨 노이즈 보정, Hyperparameter Tuning |
| **진수경** | [📂 ./Personal/JSG/](./Personal/JSG/JSG.md) | 실험 결과 검증 및 데이터 버전 관리 |
| **함건희** | [📂 ./Personal/HKH/](./Personal/HKH/HKH.md) | 학습 스케줄링(Epoch) 및 Augmentation 테스트 |

## 7. 기술 스택 (Tech Stack)

| 분류 | 스택 & 라이브러리 |
| :--- | :--- |
| **Language** | ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) |
| **Framework** | ![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white) ![Ultralytics](https://img.shields.io/badge/Ultralytics(YOLO)-00FFFF?style=flat-square&logo=yolo&logoColor=black) |
| **Data Processing** | ![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=pandas&logoColor=white) ![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat-square&logo=opencv&logoColor=white) ![Albumentations](https://img.shields.io/badge/Albumentations-FF0000?style=flat-square) |
| **Collaboration** | ![Notion](https://img.shields.io/badge/Notion-000000?style=flat-square&logo=notion&logoColor=white) ![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white) |

## 8. 관련 자료 (Documents)

| 구분 | 자료명 | 링크 |
| :--- | :--- | :---: |
| **📘 보고서** | **5팀 최종 프로젝트 보고서 (PDF)** | [다운로드](https://github.com/user-attachments/files/22525383/5._.pdf) |
| **🌐 협업** | **팀 노션(Notion) 페이지** | [바로가기](https://coal-sheet-752.notion.site/_AI-4-5-2770d71ee9698043b590c63f18ba22ea) |
| **🔗 대회** | **Kaggle Competition Link** | [바로가기](https://www.kaggle.com/competitions/ai04-level1-project/submissions) |
