<div align="center">

# 💊 Pill Detection Project  
**Detect up to 4 pills per image — Classify & Localize with mAP Evaluation**

📅 **Period:** 2025.09.09 ~ 2025.09.25  
🏆 **Competition:** Bootcamp Kaggle Private Leaderboard  
🔗 **Dataset:** [AI Hub 경구약제 이미지 데이터 기반](https://www.kaggle.com/competitions/ai04-level1-project/submissions)

</div>

---

## 🧭 Overview
이 프로젝트의 목표는 <b>한 이미지 안의 최대 4개 알약의 이름(클래스)</b>과  
<b>위치(바운딩 박스)</b>를 정확히 검출하는 것입니다.  

여러 모델과 이미지 전처리 기법을 실험하며,  
**데이터 품질 향상과 클래스 불균형 해결**을 통한 성능 극대화를 목표로 했습니다.

📈 **평가지표:** `mAP@[0.75:0.95]`  
💡 **핵심 과제:** 클래스 불균형 완화 · 라벨 정제 · 성능 향상 모델링

---

## 📑 프로젝트 자료
- 📘 [5팀_프로젝트 보고서.pdf](https://github.com/user-attachments/files/22525383/5._.pdf)
- 🌐 [팀 노션 페이지 바로가기](https://coal-sheet-752.notion.site/_AI-4-5-2770d71ee9698043b590c63f18ba22ea)

---

## 🧪 개인별 실험 폴더 (Experiments)

| 👤 이름 | 💾 폴더 경로 |
|---------|---------------|
| **김진욱** | [./Personal/KJW/](./Personal/KJW/) |
| **박병현** | [./Personal/PBH/](./Personal/PBH/) |
| **오형주** | [./Personal/OHJ/](./Personal/OHJ/) |
| **이현석** | [./Personal/LHS/](./Personal/LHS/) |
| **진수경** | [./Personal/JSG/](./Personal/JSG/) |
| **함건희** | [./Personal/HKH/](./Personal/HKH/) |

---

## 📂 Dataset

- **Source:** AI Hub 경구약제 이미지 데이터 (가공 버전 제공)  
- **Format:** PNG / COCO JSON  
- **Structure:**  
  - `train_images/` — 학습 이미지  
  - `train_annotations/` — 어노테이션 JSON  
  - `test_images/` — 테스트 이미지  

> 각 이미지에는 다수의 알약이 포함되어 있으며  
> `bbox`와 `category_id`를 통해 위치 및 클래스를 구분합니다.

📌 **Annotation 주요 필드**

| 필드명 | 설명 |
|--------|------|
| `image_id` | 이미지 고유 ID |
| `category_id` | 알약 클래스 |
| `bbox` | `[x, y, width, height]` 좌표 |
| `annotation_id` | 고유 바운딩 박스 ID |

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
```csv
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
1. **라벨 누락 보정 → 점수 상승:** `0.96520 → 0.97617`  
   ↳ 누락된 annotation 보강으로 실제 객체 학습 정확도 향상
2. **bbox 겹침 완화 → 소폭 상승:** `0.96520 → 0.96650`  
   ↳ 잘못된 라벨 학습 방지로 안정성 향상
3. **Sharpen/CLAHE 과적용 → 역효과:** `0.99004 → 0.98532`  
   ↳ 과도한 전처리로 색 변형·노이즈 유발

> 💡 **결론:** “증강을 세게”보다 **라벨 정합성·품질 관리**가 성능 향상의 핵심.

---

### 2️⃣ 모델·훈련 전략
- **YOLOv12 계열 중심 실험**
- **Epoch 증가:** 임계점까지는 성능 향상, 이후 **진동(Overfitting)**  
- **Rotation·강증강(RandomAffine):** 과하면 성능 저하  
- **TTA:** 효과 미미  
- **모델 크기↑:** 성능↑지만 **VRAM 한계**로 대형 모델 실험 제한

---

### 3️⃣ 최종 성과
🏁 **Kaggle 최종 점수:** `0.993` (**1위 달성 🥇**)  
💡 **핵심 요인:** 강력한 모델 선택 + 데이터·라벨 품질 관리  
📚 **참고 문헌:** Ultralytics / AutoAugment / RandAugment / AugMix

<p align="center">
  <img width="1405" height="96" alt="image" src="https://github.com/user-attachments/assets/004dcfec-e0cd-4f37-89a5-d7a177afa9ed" />
  <img width="1416" height="545" alt="image" src="https://github.com/user-attachments/assets/3afac4c6-8766-479f-a8b4-948e1998fe7c" />
</p>

---

<div align="center">
  
  ## 👥 Team Contributions (가설 → 행동 → 결과 → 교훈)

</div>

### 🧠 김진욱 — *클래스 불균형 & 모델 선택*
- **단순 오버샘플링:** 희귀 클래스 ↑ 기대했으나 **개선 미미 (mAP50–95 = 0.8837)**  
- **Sticker(객체만 학습):** 문맥 상실로 **성능 하락 (≈ 0.771)**  
- **최종 세팅:** YOLOv12m / `imgsz=960` / `epochs=10` / `AdamW(auto)`  
  → 로컬 `mAP50–95 = 0.588`, Kaggle `0.993` (**1위**)  
> 🎯 **교훈:** 단순 “데이터 수 늘리기”보다 **문맥 유지 + 모델 선택**이 핵심.

---

### 👩‍💻 진수경 — *불균형·문맥 보존 재현 & 검증*
- Sticker 실패 원인(문맥 상실) 분석 → **문맥 유지형 접근**으로 전환  
- 실험 재현성 확보(파라미터·데이터 버전 관리)

---

### ⚙️ 함건희 — *훈련 스케줄링·증강·사이즈 비교*
- **Epoch 10→30:** `0.74 → 0.83`, **50ep에서도 0.83 (임계점 도달)**  
- **Rotation 과다:** 성능 하락 / **TTA:** 유의미한 개선 없음  
- **YOLO size:** `n(10→50ep: 0.84→0.91)`, `s(10→50ep: 0.88→0.92)`  
> 💡 **교훈:** “과한 변형”은 독, **합리적 epoch·사이즈**가 핵심.

---

### 🧩 이현석 — *Pseudo-labeling·CIoU 보정·랜덤서치*
- **라벨 보강:** `0.965 → 0.976 (+0.011)`  
- **CIoU 보정:** 중심·비율 고려 → `0.983 (+0.007)`  
- **랜덤서치:** 2,187 조합 중 최고 `0.9259` (통계적 유의성 낮음)  
> 💡 **교훈:** **라벨 품질 보정이 최우선**, 증강은 **보수적으로**.

---

### 🧪 박병현 — *선명도·배포 동선*
- **CLAHE + Unsharp:** 색 변형·그림자 부작용 → **CLAHE 단독 적용**  
- **Inference 단계:** 자동 선명도 옵션 추가(`infer.py` 내부, 원본 미수정)  
- **YOLOv12s:** `0.98532`, **YOLOv12 XL:** 자원 한계로 중단  
> 💡 **교훈:** 자원 한계 환경에서는 **경량 + 정돈 데이터**가 최선.

---

### 🧰 오형주 — *문서·코드 유지보수*
- 코드 정리, 문서화, 실험 서포트 기여

---

<div align="center">
  
  ## 📊 실험 요약 표

</div>

| 트랙 | 시도 | 핵심 결과 | 결론 |
|------|------|-----------|------|
| 라벨 누락 보정 | pseudo-label → CIoU 보정 | `0.965 → 0.983` | **라벨 품질이 최우선** |
| bbox 겹침 완화 | 겹침 해결 | `0.96520 → 0.96650` | 정합성↑로 **소폭 개선** |
| 선명도(Sharpen/CLAHE) | 과적용 | `0.99004 → 0.98532` | **과도한 전처리 역효과** |
| 클래스 불균형 | 단순 복제·Sticker | 개선 미미/하락 | **문맥 유지형 접근 필요** |
| 모델·훈련 | YOLOv12m, epoch 최적 | `Kaggle 0.993 (1위)` | **강한 모델 + 데이터 품질** |

