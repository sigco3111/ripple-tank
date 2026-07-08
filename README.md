# 🌀 ripple-tank

> WebGL2 기반 파동 간섭(Ripple Tank) 시뮬레이션 — 고해상도 ping-pong height map 알고리즘

마우스로 클릭하는 지점에서 파동이 퍼져나가고, 장애물에 부딪히면 반사(Neumann 경계)되며, 두 ripple이 만나면 보강/상쇄 간섭이 자연 발생합니다. RGBA32F ping-pong 텍스처(기본 1024×1024)에서 파동방정식을 풀고, 굴절·반사·수심 기반 색조·움직이는 caustics까지 한 번에 처리해 60fps로 그립니다. 페이지 로드마다 달라지는 시드 기반 자연 호반 바닥 위에, 동일한 시드는 항상 동일한 배치를 보장합니다.

[🇰🇷 한국어 (기본)](#) · [🇺🇸 English](./README.en.md)

---

## 🎬 라이브 데모

> **👉 [https://ripple-tank.vercel.app/](https://ripple-tank.vercel.app/)** — 브라우저에서 바로 실행 (WebGL2, 60fps)

| | |
|---|---|
| ![Demo](https://img.shields.io/badge/Live-Demo-7C3AED?style=for-the-badge&logo=vercel&logoColor=white) | [![Repo](https://img.shields.io/badge/GitHub-sigco3111%2Fripple--tank-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/sigco3111/ripple-tank) |
| ![Status](https://img.shields.io/badge/Status-Live-22C55E?style=flat-square) | ![Stack](https://img.shields.io/badge/Stack-WebGL2-FF6F00?style=flat-square&logo=webgl&logoColor=white) |
| ![License](https://img.shields.io/badge/License-MIT-F1C40F?style=flat-square) | ![Deps](https://img.shields.io/badge/Dependencies-0-9CA3AF?style=flat-square) |

### 🎮 빠른 사용법
1. 위 데모 링크 클릭 → 브라우저에서 페이지 열기
2. **클릭** — 해당 위치에서 ripple 발생
3. **클릭 + 드래그** — 드래그 거리에 비례해 진폭 1.0×~1.5× 증폭
4. **🎲 Randomize** — 새 시드로 장애물 배치 + 호반 바닥 재생성
5. **Damping 슬라이더** — 0.950 (급속 감쇠) ↔ 0.999 (거의 영구)

---

## 🤖 생성 정보

이 프로젝트의 코드는 아래 모델과 프롬프트를 이용해 **자동으로 생성**되었습니다.

| 항목 | 값 |
|---|---|
| **모델** | minimax-M3 |
| **실행 환경** | OpenCode CLI |
| **저장소** | [`sigco3111/ripple-tank`](https://github.com/sigco3111/ripple-tank) |
| **라이선스** | MIT |
| **의존성** | 없음 (단일 HTML + 인라인 WebGL2 셰이더) |

### 📝 사용된 프롬프트 (원문)

```
WebGL2를 사용해서 클릭하는 지점에서 파동이 퍼져나가고, 장애물에 부딪히면
반사되며, 두 파동이 만나면 보강/상쇄 간섭이 일어나는 Ripple Tank 시뮬레이션을
구현해줘. RGBA32F ping-pong 텍스처에 이산화된 2D 파동방정식을 풀고,
굴절(refraction)·반사(specular)·수심 기반 색조·움직이는 caustics 효과로
자연스러운 수면처럼 보이게 해줘. 호반 바닥은 FBM 모래 + 능선 잔물결 +
Voronoi 자갈 + 큰 바위 + grain 노이즈로 절차적 생성하고, 페이지 로드 시
mulberry32 PRNG로 5-9개 장애물(circle/ellipse/polygon/wall)을 랜덤 배치해줘.
?seed=N 또는 ?seed=문자열 로 같은 시드는 항상 같은 배치를 보장하고,
?test=1 은 결정론적 baseline (damping=0.970, seed=0) 으로 회귀 테스트용.
모든 의존관계의 코드를 하나의 HTML에 담는 형태로 작성.
```

---

## ✨ 주요 특징

- 🌊 **파동방정식 솔버** — 이산화된 2D wave equation, `r²=0.5` (CFL 한계 안정)
- 🔁 **Ping-Pong 텍스처** — `RGBA32F` 두 장을 번갈아 렌더 타겟, 1024×1024 기본
- 🧱 **Neumann 경계 반사** — 별도 boundary 텍스처 + 셰이더 `mix(neighbor, center, mask)`
- 💧 **자연스러운 굴절** — 그라디언트 → 노멀 → UV 변위 + chromatic aberration
- ✨ **이동 caustics** — 두 표류 FBM 필드를 inverted blend로 자연 광 패턴 생성
- 🏖️ **절차적 호반 바닥** — FBM 모래 + 능선 + Voronoi 자갈 + 큰 바위 + grain
- 🎲 **시드 기반 결정성** — `?seed=N` 또는 `?seed=문자열` (FNV-1a 해시)
- 🧪 **테스트 모드** — `?test=1` (damping=0.970, seed=0, 결정론적 baseline)
- 📦 **단일 HTML** — 외부 의존성 0개, CDN·폰트·import 없음
- 🔒 **온디바이스** — 모든 렌더링·물리가 브라우저 안에서 처리됨

---

## 🚀 실행 방법

### 방법 1: 그냥 브라우저로 열기 (가장 간단)
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

### 방법 3: 라이브 데모 (Vercel)
배포 후 URL이 발급되면 별도 설치 없이 바로 확인 가능합니다.
👉 https://ripple-tank.vercel.app/

---

## 🎮 조작법

| 입력 | 효과 |
|---|---|
| **클릭** | 해당 위치에 ripple 발생 |
| **클릭 + 드래그** | 드래그 거리에 비례해 amplitude 1.0×~1.5× 증폭 |
| **🎲 Randomize 버튼** | 새 시드 → 새 지형 배치 + 호반 바닥 재생성 + URL 갱신 |
| **Damping 슬라이더** | 0.950 (급속 감쇠) ↔ 0.999 (거의 영구) |
| **Pause 버튼** | 시뮬레이션 정지, 마지막 프레임 유지 |
| **Reset 버튼** | height field 초기화 (장애물은 유지) |

### URL 파라미터

| 파라미터 | 효과 |
|---|---|
| `?seed=42` | 특정 시드로 지형 결정 (정수) |
| `?seed=hello` | 문자열 시드 — FNV-1a 해시로 결정 (재현 가능 공유) |
| `?test=1` | 결정론적 모드 (damping=0.970, ambient warp=0, seed=0) — 회귀 테스트용 |
| `?res=512` | 시뮬레이션 해상도를 512×512로 낮춤 (저사양 GPU) |
| `?res=2048` | 2048×2048로 높임 (고사양 GPU) |
| `?warp=0` | ambient warp 비활성화 |

기본값은 damping=0.985, sim=1024×1024, warp=0.0015.

---

## 🛠️ 기술 스택

| 영역 | 사용 기술 |
|---|---|
| **렌더링** | WebGL2 (GLSL ES 3.00) |
| **시뮬레이션** | RGBA32F ping-pong FBO (이산 파동방정식) |
| **경계 처리** | Neumann boundary texture + 셰이더 mix |
| **굴절** | 그라디언트 → 노멀 → UV 변위 + chromatic aberration |
| **반사** | Blinn-Phong (perturbed half-vector) |
| **수심 색조** | Beer-Lambert 식 |
| **호반 바닥** | 절차적 FBM + Voronoi + 능선 + grain |
| **의존성** | 없음 (Vanilla JS + 인라인 GLSL) |

---

## 📂 프로젝트 구조

```
ripple-tank/
├── index.html               # 단일 HTML (HTML + CSS + JS + GLSL 모두 인라인)
├── README.md                # 한국어 (기본)
├── README.en.md             # English (옵션)
├── docs/                    # 검증 스크린샷
│   ├── screenshot-pond-hero.png
│   ├── screenshot-pond-rest.png
│   ├── screenshot-pond-variant.png
│   ├── screenshot-propagation.png
│   ├── screenshot-interference.png
│   ├── screenshot-refraction.png
│   ├── screenshot-random-terrain.png
│   └── screenshot-rest.png
└── .gitignore               # oh-my-opencode 내부 상태 제외
```

**외부 의존성: 0개.** CDN 없음, 외부 폰트 없음, 외부 JS/CSS 없음, `fetch`/`import`/`Worker` 없음. `file://`로 직접 열어도 동작.

---

## 🎨 디자인 결정

브레인스토밍 단계에서 내린 결정 4가지:

| 결정 포인트 | 선택 | 이유 |
|---|---|---|
| **렌더 백엔드** | WebGL2 + RGBA32F ping-pong | CPU 메쉬 격자보다 1024²도 60fps 유지, GPGPU 패러다임 그대로 활용 |
| **경계 처리** | Neumann `mix(neighbor, center, mask)` | 별도 분기 없이 SIMD화 가능, 모든 도형 타입에서 일관 |
| **시드 시스템** | mulberry32 PRNG + FNV-1a 해시 | 정수/문자열 모두 결정성 보장, 회귀 테스트 가능 |
| **호반 베이크** | 1024² RGBA8 한 번 베이크 + 시드 변형 | 렌더 비용 0, 동일 시드 동일 배치, 그리드 패턴 제거 |

### 직접 커스터마이즈하고 싶다면

`index.html` 내 `RENDER_FS` / `SIM_FS` 상단에서 다음 상수를 조정하면 분위기를 바꿀 수 있어요:

```glsl
// RENDER_FS 상단
uniform float u_refractStrength;   // 굴절 강도 (기본 0.045)
uniform float u_aberration;        // 색수차 (기본 0.0025)
uniform float u_staticWarpAmp;     // 정지 시 ambient warp (기본 0.0015)
uniform float u_tileScale;         // 타일 개수 / 짧은 변 (기본 14.0)

// SIM_FS / JS
const damping = 0.985;             // 0.950~0.999 슬라이더
const clickAmplitude = 0.45;       // 클릭 impulse 진폭
const SIM_SIZE = 1024;             // 512 / 1024 / 2048
```

고급 사용자용: `generateObstacles(seed)` 함수를 수정해 장애물 분포를 바꿔보거나, `TILE_FS`의 FBM 파라미터를 조정해 호반 바닥 텍스처를 완전히 다른 톤으로 바꿀 수도 있어요.

---

## 🧠 동작 원리

### 1. Wave Equation (Discrete)
```
new_h = (h_left + h_right + h_up + h_down) * 0.5 - h_prev
new_h *= damping
```
이것은 이산화된 2D 파동방정식의 simplest stable form (`r² = 0.5`, CFL 한계). `h_prev` 채널이 텍스처의 G 채널에 저장되어 매 프레임 자연스럽게 시뮬레이션됩니다.

### 2. Ping-Pong 텍스처
- `stateA`, `stateB` 두 개의 `RGBA32F` 텍스처를 번갈아 렌더 타겟으로 사용
- R = 현재 높이, G = 이전 높이
- 한 프레임: `stateA` 읽어서 → `stateB`에 쓰기 → swap
- 다음 프레임: 방금 쓴 `stateB`를 읽음

### 3. 장애물 반사 (Neumann Boundary)
- 별도 `boundaryTex` (R 채널 = obstacle mask)
- 시뮬레이션 셰이더에서 4방향 이웃 샘플 시, 이웃이 obstacle이면 **현재 셀의 높이로 대체** (`mix(neighbor, center, mask)`)
- 이 방식이 자연스러운 반사를 만들어냄 — 브랜칭 없이 컴파일러가 SIMD화 가능
- 외곽 1-texel도 obstacle로 칠해 캔버스 가장자리 반사 처리

### 4. 임펄스 주입 (Method C)
- 클릭 시 Gaussian 임펄스를 `uniform vec4 u_impulses[16]` 으로 셰이더에 전달
- 한 프레임에 최대 16개까지 동시 처리 가능
- 별도 FBO나 blend state 없이 시뮬 패스 안에서 처리

### 5. 굴절 렌더링
```glsl
gradient:  dx = h_right - h_left,  dy = h_up - h_down
normal:    N = normalize(vec3(-dx, 1.0, -dy))
refract:   uv_offset = clamp((dx, dy) * 0.045, ±0.04)
           tile_color = texture(floorTex, baseUV + uv_offset * [1+ca, 1, 1-ca])
specular:  spec = pow(dot(N, H), 80)
tint:      depth_t = clamp(-h * 1.4 + 0.5, 0, 1)
           color = mix(shallow_tint, deep_tint, depth_t) * tile + spec
```
- **Chromatic aberration**: R/G/B 채널을 서로 다른 굴절 오프셋으로 샘플
- **Beer-Lambert 수심**: trough(깊은 물) = 짙은 청색, peak(얕은 물) = 밝은 청록색
- **Edge vignette**: 캔버스 가장자리 어둡게 → 수조 깊이감

### 6. 절차적 자연 호반 바닥
- `TILE_FS` 셰이더가 한 번에 1024² RGBA8 텍스처로 베이크 (시드 기반 변형)
- 구성: FBM 모래 + 능선 잔물결 + Voronoi 자갈 + 큰 바위 + 미세 grain
- 그리드 패턴 없음 — `u_seed` uniform이 배치 변형

### 7. 움직이는 Caustics
- `RENDER_FS`에서 매 프레임 두 FBM 필드를 표류시킴 (`u_time` 기반)
- `caustic = pow(1 - abs(c1 - c2) * 1.8, 4) * 0.35` — 역차로 밝은 띠 생성
- `depthT`로 가중 — trough에서 강하게, peak에서 약하게

### 8. 랜덤 지형 생성
- `mulberry32` PRNG로 결정성 보장
- `generateObstacles(seed)`: 5-9개 장애물 (circle 50%, ellipse 25%, polygon 25%) + 30% 확률로 wall
- 장애물 간 거리 + 가장자리 마진 검증으로 겹침 방지
- `applySeed(seed)` → `OBSTACLES` 재생성 + boundary 다시 그리기 + floor 재베이크 + URL 업데이트

---

## 🔬 검증

Playwright headless Chromium으로 11개 시나리오 검증 완료 (모두 PASS):

| ID | 시나리오 | 증거 |
|---|---|---|
| S1 | 페이지 로드 → 호반 바닥 + caustics + 랜덤 장애물 + UI 표시 | 스크린샷 + 콘솔 에러 0개 |
| S2 | 클릭 시 ripple 발생 + 시간에 따라 propagation | 스크린샷 (pre / mid / late) |
| S3 | `__ripple_randomize()` 3회 → 매번 다른 seed와 다른 배치 | 스크린샷 + state 변화 |
| S4 | `?seed=42` → 같은 seed는 항상 같은 배치 (결정성) | 같은 시드 두 번 동일 |
| S5 | 다른 seed → 다른 배치 | seed 99 vs 42 vs 12345 비교 |
| S6 | 문자열 seed (`?seed=hello`) → FNV-1a 해시, 결정성 보장 | 두 번 동일 결과 |
| S7 | `?test=1` → seed=0 (테스트 모드 결정성) | state.seed === 0 |
| S8 | 두 ripple 간섭 | 클릭 2회 → 간섭 패턴 |
| S9 | 타일 굴절 | pre/peak 클로즈업 — 타일이 휘어짐 |
| S10 | Wave propagation | 클릭 후 2초 → 동심원 확장 |
| S11 | 시드=0 호반 baseline | `docs/screenshot-pond-rest.png` |

`window.__ripple_state()` inspector, `__ripple_click`, `__ripple_reset`, `__ripple_seed(n|'s')`, `__ripple_randomize()` 프로그래매틱 훅 사용 가능.

---

## 📜 License

MIT © 2026 sigco3111

---

## 🙏 Acknowledgments

이 프로젝트는 **minimax-M3** 모델과 **OpenCode CLI** 환경에서 생성되었습니다. 프롬프트 엔지니어링과 디자인 결정은 저장소 소유자가 직접 수행했습니다.

- **코딩미션 참조 페이지**: [cokac.com — 코드깎는노인](https://cokac.com/list/announcement/24)
- **Wave equation 참고**: Hugo Elias archive, GPU Gems 2 Ch. 44, mysimulator.uk
- **WebGL ripples 참고**: jquery.ripples, m-ender/webgl-ripples