# SpaceTeam

# 🚀 프로젝트 승리호 (Project Victory)

> **"우주 폐기물을 수거해 정직원이 되어라!"**  
> 2~4인 멀티플레이어 협동 우주 아케이드 게임 프로젝트

---

## 📌 1. 프로젝트 개요 (Project Overview)

- **게임명**: 승리호 (Project Victory)
- **장르**: 협동 아케이드 (Co-op Arcade)
- **플레이 인원**: 2 ~ 4명 (Network Multiplayer)
- **개발 기간**: 2026.01.05 ~ 2026.03.06 (총 8주)
- **개발 시연**: [🎬 YouTube 시연 영상 보러가기](https://youtu.be/SPshqo9hCD8)

---

## 🛠️ 2. 개발 환경 및 도구 (Tech Stack & Tools)

| 분류 | 사용 도구 / 기술 |
| :--- | :--- |
| **Game Engine** | Unreal Engine 5 |
| **IDE / Language** | Visual Studio / C++ |
| **VCS & 협업** | GitHub, Jira |

<p align="center">
  <img src="input_file_3.png" width="100" alt="Unreal Engine"/>
  <img src="input_file_1.png" width="100" alt="Visual Studio"/>
  <img src="input_file_0.png" width="100" alt="GitHub"/>
  <img src="input_file_2.png" width="100" alt="Jira"/>
</p>

---

## 🎮 3. 게임 소개 (Game Concept & Flow)

### 🌌 핵심 컨셉
승리호는 우주에 떠돌아다니는 폐기물을 수거하여 목표 할당량을 채워 정직원으로 채용되는 것을 목표로 하는 **협동 아케이드 게임**입니다.  
플레이어들은 힘을 합쳐 **우주선 조종, 폐기물 분해 및 회수, 적대 AI 드론 및 소행성 방어**를 병행하며 스테이지를 진행하게 됩니다.

![승리호 프로젝트 개요](input_file_4.png)

### 🔄 메인 플레이 흐름
```text
[게임 실행] ➡️ [타이틀 화면] ➡️ [방 생성 / 매치메이킹] ➡️ [로비]
                                                      ⬇️
[상점 (정산 및 업그레이드)] ⬅️ [우주선 방어 & 폐기물 수거] ⬅️ [Stage 진입]
