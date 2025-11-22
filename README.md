# Agentic AI 기반 자율주행 의사결정 시스템 학습 로드맵

본 문서는 Agentic AI 기반 자율주행 의사결정 시스템 연구(석사 논문)를 위해 필요한 기술과
전체 학습 로드맵을 정리한 자료입니다.

## 🚀 전체 로드맵 개요
- **1단계:** 자율주행·비전·기초 AI 이해 (1~2개월)
- **2단계:** LLM·Agentic AI·Vision→Language 기술 (2~3개월)
- **3단계:** Planning·CARLA·통합 시스템 구현 (2~3개월)
- **4단계:** 논문 실험 및 모델 개발 (3~4개월)

---

## 1️⃣ 자율주행 인지(Perception) 기술

### 학습 내용
- YOLOv8/YOLOv10 기반 Object Detection
- Lane Detection (LaneATT, YOLO-Lane)
- Tracking (SORT, DeepSORT)
- 객체 → 거리·상대속도 추정

### 목표
도로 영상을 입력 받아 아래와 같은 JSON 형태의 State 생성:
```json
[
  {"cls": "car", "distance": 14.3, "speed": -2.1},
  {"cls": "pedestrian", "distance": 8.1}
]
