# Agentic AI 기반 자율주행 의사결정 시스템 연구  
**Autonomous Driving Decision-Making System using Agentic AI**

본 문서는 *Agentic AI(에이전트형 AI)* 기반 자율주행 의사결정 시스템을 연구하기 위한  
**전체 기술 스택 + 학습 로드맵 + 시스템 개요를 하나로 정리한 통합 문서**입니다.  
석사 논문 준비용으로 작성되었습니다.

---

# 🚗 1. 연구 주제 개요

## 📌 연구 제목(예시)
**“대규모 언어모델 기반 Agentic AI를 활용한 자율주행 차량의 상황 인식 및 단계적 의사결정 시스템 연구”**

## 🎯 연구 목표
Agentic AI(LLM 기반 Reasoning Agent)를 활용하여  
자율주행 차량의 **상황 인식 → 추론 → 행동 결정(Planning)** 을 수행하는 시스템을 개발하는 것.

이는 기존 자율주행 Planning 대비  
- 복잡한 상황에서 유연성 증가  
- 예외상황 대응력 향상  
- 새로운 환경에서도 높은 일반화  
등의 장점을 기대할 수 있음.

---

# 🚦 2. 전체 시스템 구조
[1. Perception]
└─ YOLO / Lane Detection / Tracking
↓
[2. Vision → Language 변환]
└─ 객체·차선 정보 → 자연어 상황 설명
↓
[3. Agentic AI (LLM Planner)]
└─ CoT / ReAct / Tool-Use로 reasoning
↓
[4. Planning & Control]
└─ LLM 행동명령 → 차량 제어 값 변환
↓
[5. CARLA Simulator]
└─ 행동 실험 및 성능 평가


---

# 🔧 3. 핵심 기술 6가지 (논문 수행에 필수)

---

## 3.1. 자율주행 인지(Perception)

### 포함 기술
- Object Detection (YOLOv8 / YOLOv10)
- Lane Detection (LaneATT, YOLO-Lane)
- Tracking (SORT / DeepSORT)
- Distance & Relative Speed Estimation

### 목표 출력(JSON)
```json
[
  {"cls": "car", "distance": 14.3, "speed": -2.1},
  {"cls": "pedestrian", "distance": 8.1}
]

3.2. Vision → Language 상황 설명 생성
필요 개념

템플릿 기반 NLG

VLM 개념(CLIP/BLIP-2 등)

간단한 요약(Summarization)

목표 출력(예)
전방 15m 승용차 1대. 상대속도 -3m/s.
좌측 차선 접근 차량 1대. 보행자 없음.
신호등 녹색. 차선 변경 가능.

3.3. Agentic AI (LLM Reasoning & Planning)
핵심 기술

Chain of Thought(CoT)

ReAct (Reason + Act Loop)

Tool Use

Reflection / Self-Refine

Safety Prompting

LLM 행동 출력 예:
Observation: 앞 차량 12m, 감속 중.
Reasoning: 충돌 위험 증가 → 감속 필요.
Action: {"action": "SLOW_DOWN", "value": 0.2}

3.4. Planning & Vehicle Control
필요한 지식

Kinematic Bicycle Model

Behavior Planning

TTC/TTA 기반 Collision Check

PID / MPC 기본 제어

목적

Agent가 생성한 행동을 실제 차량 제어 명령으로 안전하게 변환.

3.5. CARLA 시뮬레이션 환경
학습 요소

CARLA 설치 및 Python API

Vehicle Control (throttle, brake, steer)

Sensor(Camera/LiDAR) 처리

Traffic Scenario 구성

목표

LLM Agent → CARLA 제어 연결

다양한 도로 상황에서 실험 수행

3.6. LLM ↔ 제어 Action Interface
구현 요소

LLM 출력 parsing

Action 정의(KEEP_LANE, STOP, SLOW_DOWN 등)

Safety Wrapper

control.throttle / steer / brake 연결

📊 4. 실험 및 평가 지표

연구 성능 평가에 필요한 대표 지표:

Collision Rate (%)

Route Completion Rate (%)

Reaction Time

Lane Change Success Rate

Minimum Distance to Obstacles

Unseen Scenario Generalization Score

