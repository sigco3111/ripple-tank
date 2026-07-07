# 🌊 ripple-tank

> WebGL/Canvas 기반 물결 파동 간섭(Ripple Tank) 시뮬레이션

마우스로 클릭하는 지점에서 물결이 퍼져나가고, 벽이나 장애물에 부딪히면 반사(Reflection)되며, 두 물결이 만날 때 보강/상쇄 간섭이 일어나는 과정을 고해상도 높이 맵(Height Map) 알고리즘으로 시뮬레이션합니다. 물 밑의 바닥 타일이 굴절되어 보이는 효과까지 리얼하게 구현합니다.

---

## 🤖 생성 정보 (Attribution)

이 프로젝트의 코드는 아래 모델과 프롬프트를 이용해 **자동으로 생성**되었습니다.

| 항목 | 값 |
|---|---|
| **모델** | minimax-M3 |
| **실행 환경** | OpenCode CLI |
| **저장소** | [`sigco3111/ripple-tank`](https://github.com/sigco3111/ripple-tank) |
| **라이선스** | MIT |
| **의존성** | 없음 (Vanilla JS, 단일 HTML) |

### 📝 사용된 프롬프트 (원문)

```
마우스로 클릭하는 지점에서 물결이 퍼져나가고, 벽이나 장애물에 부딪히면 반사(Reflection)되며,
두 물결이 만날 때 보강/상쇄 간섭이 일어나는 과정을 고해상도 높이 맵(Height Map) 알고리즘으로
시뮬레이션하여 물 밑의 바닥 타일이 굴절되어 보이는 효과까지 리얼하게 구현해줘.
Implementation Advice: Use WebGL or Canvas. The "Ripple" effect is often done by
swapping two buffers (current and previous state) and applying a damping factor.
Render the result as a refraction displacement map over the background image.
모든 의존관계의 코드를 하나의 HTML에 담는 형태로 코드 작성.
```

---

## 📂 프로젝트 구조

```
ripple-tank/
├── index.html      # 단일 HTML (모든 코드 포함) — OpenCode에서 작성 예정
└── README.md       # 한국어 (기본)
```

---

## 🚀 실행 방법 (Quick Start)

### 방법 1: 그냥 브라우저로 열기
```bash
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

### 방법 2: 로컬 서버 (권장)
```bash
python3 -m http.server 8000
# → http://localhost:8000
```

---

## 📜 License

MIT © 2026 sigco3111

---

## 🙏 Acknowledgments

이 프로젝트는 **minimax-M3** 모델과 **OpenCode CLI** 환경에서 생성되었습니다. 프롬프트 엔지니어링과 디자인 결정은 저장소 소유자가 직접 수행했습니다.

- **코딩미션 참조 페이지**: [cokac.com — 코드깎는노인](https://cokac.com/list/announcement/24)