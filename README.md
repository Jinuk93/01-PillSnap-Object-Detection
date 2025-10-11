<div align="center">

# 💊 Pill Detection Project  
**Detect up to 4 pills per image — Classify & Localize with mAP Evaluation**

📅 **Period:** 2025.09.09 ~ 2025.09.25  
🏆 **Competition:** Bootcamp Kaggle Private Leaderboard  
🔗 **Dataset:** AI Hub 경구약제 이미지 데이터 기반  

</div>

---

## 🧭 Overview

이 프로젝트의 목표는 **하나의 이미지 안에 존재하는 최대 4개의 알약의 이름(클래스)과 위치(바운딩 박스)** 를 정확히 검출하는 것입니다.<br>
여러 모델과 이미지 전처리 기법을 실험하여 최적의 성능을 달성하는 것을 목표로 했습니다.

📈 **평가지표:** `mAP@[0.75:0.95]`  
💡 **주요 과제:** 클래스 불균형 완화, 데이터 정제, 성능 향상 모델링

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
```csv
annotation_id,image_id,category_id,bbox_x,bbox_y,bbox_w,bbox_h,score
1,1,1,156,247,211,456,0.91
2,1,24,498,40,460,474,0.78
3,1,11,579,700,260,473,0.27
