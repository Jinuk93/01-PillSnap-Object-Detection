<div align="center">

# 💊 Pill Detection Project  
**Detect up to 4 pills per image — Classify & Localize with mAP Evaluation**

📅 **Period:** 2025.09.09 ~ 2025.09.25  
🏆 **Competition:** Bootcamp Kaggle Private Leaderboard  
🔗 **Dataset:** [AI Hub 경구약제 이미지 데이터 기반](https://www.kaggle.com/competitions/ai04-level1-project/submissions)

</div>

---

## 🧭 Overview

이 프로젝트의 목표는 **하나의 이미지 안에 존재하는 최대 4개의 알약의 이름(클래스)과 위치(바운딩 박스)** 를<br> 
정확히 검출하는 것입니다. 여러 모델과 이미지 전처리 기법을 실험하여 최적의 성능을 달성하는 것을 목표로 했습니다.

📈 **평가지표:** `mAP@[0.75:0.95]`  
💡 **주요 과제:** 클래스 불균형 완화, 데이터 정제, 성능 향상 모델링

## 📑 프로젝트 보고서
- 📘 [5팀_프로젝트 보고서.pdf](https://github.com/user-attachments/files/22525383/5._.pdf)

## 🧭 팀 프로젝트 Notion
- 🌐 [팀 노션 페이지 바로가기](https://coal-sheet-752.notion.site/_AI-4-5-2770d71ee9698043b590c63f18ba22ea)

## 👥 개인별 정리 (협업일지 & 실험 폴더)

| 이름 | 협업일지 (Notion) | 개인 실험 폴더 |
|------|-------------------|----------------|
| **김진욱** | [Notion 바로가기](https://coal-sheet-752.notion.site/2770d71ee96980d6a8a9dde19e062d32?v=2770d71ee9698064a0e7000cc1b47e24&source=copy_link) | [./Personal/KJW/](./Personal/KJW/) |
| **박병현** | [Notion 바로가기](https://famous-gorilla-33d.notion.site/AI-_-_-269c7c1a009280dfb556e494268ea975?source=copy_link) | [./Personal/PBH/](./Personal/PBH/) |
| **오형주** | [Notion 바로가기](https://rose-laugh-280.notion.site/AI-09-09-09-24-2778de3ce62b80079a87e7926bbc98c5?source=copy_link) | [./Personal/OHJ/](./Personal/OHJ/) |
| **이현석** | [Notion 바로가기](https://bubbly-psychology-181.notion.site/Codeit-2252dfb1ef688054a879c45c276e8d85?source=copy_link) | [./Personal/LHS/](./Personal/LHS/) |
| **진수경** | [Notion 바로가기](https://puzzled-salto-827.notion.site/2696a4a5ec8380adb0bfd72fec737b86?v=2696a4a5ec8380fe9893000cccc037c7) | [./Personal/JSG/](./Personal/JSG/) |
| **함건희** | [Notion 바로가기](https://nostalgic-apricot-f75.notion.site/277fd289d4ef809880e8eef10d388fd3?v=277fd289d4ef8168aa96000c6c160de3&source=copy_link) | [./Personal/HKH/](./Personal/HKH/) |


---

## 📂 Dataset

- **Source:** AI Hub 경구약제 이미지 데이터 (가공 버전 제공)  
- **Format:**  
  - Images: PNG  
  - Annotation: COCO format (JSON)  
- **구성:**  
  - `train_images/` — 학습 이미지  
  - `train_annotations/` — 어노테이션 JSON  
  - `test_images/` — 테스트 이미지  

> 각 이미지에는 다수의 알약이 포함되어 있으며,  
> `bbox`와 `category_id`를 통해 위치 및 클래스를 구분합니다.

📌 **Annotation 주요 필드**
| 필드명 | 설명 |
|--------|------|
| `image_id` | 이미지 고유 ID |
| `category_id` | 알약 클래스 |
| `bbox` | `[x, y, width, height]` 좌표 |
| `annotation_id` | 고유한 바운딩 박스 ID |

---

## 🧮 Evaluation & Submission

📊 **평가 기준**
- **팀 단위:** 발표 + 보고서 + GitHub Repository  
- **개인 단위:** 발표 + 협업일지  

🧾 **Submission File Format**
| Column | Description |
|--------|-------------|
| `annotation_id` | 객체 고유 ID |
| `image_id` | 이미지 ID |
| `category_id` | 클래스 ID |
| `bbox_x, bbox_y, bbox_w, bbox_h` | 바운딩 박스 좌표 |
| `score` | 예측 신뢰도 |

📄 **예시**

```
csv
annotation_id,image_id,category_id,bbox_x,bbox_y,bbox_w,bbox_h,score
1,1,1,156,247,211,456,0.91
2,1,24,498,40,460,474,0.78
3,1,11,579,700,260,473,0.27
```

---
<div align="center">
  
  ## ✨ What We Actually Did (핵심 시도 & 결과)

</div>

### 1️⃣ 데이터·라벨 품질 우선
1. **라벨 누락 보정 → 점수 상승 :** `0.96520 → 0.97617`  
  → 누락된 annotation 보강 효과 (모델이 실제 객체를 더 정확히 학습)
2. **bbox 겹침 완화 → 소폭 상승 :** `0.96520 → 0.96650`  
  → 잘못된 라벨 학습 방지로 안정성 향상
3. **선명도(Sharpen/CLAHE) 과적용은 역효과 :** `0.99004 → 0.98532`  
  → 과도한 강화로 색 변색·노이즈 학습 발생  

💬 **결론 :** “증강을 세게”보다 **라벨 정합성/품질 관리**가 성능 향상의 핵심이었다.

---

### 2️⃣ 모델·훈련 전략
- **YOLOv12 계열 중심 실험**
- **Epoch 증가 :** 임계점까지는 성능 향상, 이후 진동 (Overfitting 경향)  
- **Rotation·강증강(RandomAffine) :** 과하면 성능 저하  
- **TTA :** 큰 차이 없음  
- **모델 크기 증가 :** 성능은 ↑, 하지만 **VRAM 제약**으로 대형 모델 실험 제한

---

### 3️⃣ 최종 성과
🏁 **Kaggle 최종 점수 :** `0.993` (**1위 달성!**)  
💡 **핵심 요인 :** 강력한 모델 선택 + 데이터·라벨 품질 관리  
📚 **참고 문헌 :** Ultralytics / AutoAugment / RandAugment / AugMix
<img width="1300" height="116" alt="image" src="https://github.com/user-attachments/assets/5c7c7491-a8b7-48b8-b2dc-9bad34eb7d2d" />
<img width="1447" height="556" alt="image" src="https://github.com/user-attachments/assets/6d9b741b-7bd8-4134-abbf-d22824e74c7b" />

---
<div align="center">
  
  ## 👥 Team Contributions (가설 → 행동 → 결과 → 교훈)

</div>

### 🧠 김진욱 — *클래스 불균형 & 모델 선택*
- **단순 오버샘플링 :** 희귀 클래스 ↑ 기대했으나, 다수 클래스까지 복제되어 **개선 미미 (mAP50–95 = 0.8837)**  
- **Sticker 방식(객체만 학습) :** 문맥 상실 → **성능 하락 (≈ 0.771)**  
- **최종 세팅:** YOLOv12m / `imgsz=960` / `epochs=10` / `AdamW(auto)`  
  → 로컬 `mAP50–95=0.588`, Kaggle `0.993` (**1위**)  
> **교훈 :** 단순 “데이터 수 늘리기”보다 **문맥 유지 + 모델 선택**이 핵심.

---

### 👩‍💻 진수경 — *불균형/문맥 보존 재현 & 검증*
- Sticker 실패 원인 분석 후 **문맥 유지형 접근**으로 전환  
- 실험 재현성 확보 (파라미터·데이터 버전 관리)

---

### ⚙️ 함건희 — *훈련 스케줄링·증강·사이즈 비교*
- **Epoch 10→30 :** `0.74 → 0.83`, 50에서도 **0.83 (임계점)**  
- **Rotation(에폭별 변화) :** 성능 하락  
- **TTA :** 유의미한 개선 없음  
- **YOLO size :** `n(10→50ep: 0.84→0.91)`, `s(10→50ep: 0.88→0.92)`  
  → VRAM 한계로 `m/l` 모델 실험 불가  
> **교훈 :** “과한 변형”은 독, **합리적 에폭·사이즈**로 현실적 최적점을 찾기.

---

### 🧩 이현석 — *Pseudo-labeling·CIoU 보정·랜덤서치*
- **라벨 누락 보강 (pseudo-labeling):** `0.965 → 0.976 (+0.011)`  
- **CIoU 보정:** 중심/비율 고려로 bbox 더 타이트하게 → `0.983 (+0.007)`  
- **RandomAffine 강하면↓, HSV 적절히는 유지/소폭↑, Mixup 0.0~0.2 유사**  
- **랜덤서치(2,187 조합 중 15 trial / 13h):** 최고 `0.92590`, 통계적 유의성은 부족  
> **교훈 :** 가장 큰 지렛대는 **라벨 품질(보정)**, 증강은 **보수적으로**.

---

### 🧪 박병현 — *선명도·배포 동선*
- **CLAHE + Unsharp :** 색 변색/그림자 부작용 → **CLAHE 단독**으로 완화  
- **Inference 단계 :** 자동 선명도 옵션 추가 (`infer.py` 내 처리, 원본 미수정)  
- **YOLOv12s :** Kaggle `0.98532`, **YOLOv12 XL**은 자원 부담으로 중단  
> **교훈 :** 개인 자원 한계에서는 **경량 + 정돈 데이터**가 현실적 선택.

---

### 🧰 오형주 — *문서/코드 유지보수* 
- 팀 코드 정리 및 문서화, 실험 서포트 기여

---
<div align="center">
  
  ## 📊 실험 요약 표

</div>

| 트랙 | 시도 | 핵심 결과 | 결론 |
|------|------|------------|------|
| 라벨 누락 보정 | pseudo-label → CIoU 보정 | `0.965 → 0.983` | 라벨 품질이 최우선 |
| bbox 겹침 완화 | 겹침 해결 | `0.96520 → 0.96650` | 정합성↑로 소폭 개선 |
| 선명도(Sharpen/CLAHE) | 과적용 | `0.99004 → 0.98532` | 과도한 전처리 역효과 |
| 클래스 불균형 | 단순 복제·Sticker | 개선 미미/하락 | 문맥 유지형 접근 필요 |
| 모델·훈련 | YOLOv12m, epoch 최적 | `Kaggle 0.993 (1위)` | 강한 모델 + 데이터 품질 |

