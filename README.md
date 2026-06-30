# **🎮Team Victory(팀 승리호)**

<img width="867" height="503" alt="image" src="https://github.com/user-attachments/assets/ade06f93-1b02-4d01-9501-3911637eb2e1" />
___

## 목차
1. [프로젝트 소개](#1-프로젝트-소개)
3. [기술 스택](#2-기술-스택)
4. [팀원 및 역할](#3-팀원-및-역할)
5. [세부 구현](#4-세부-구현)
   
   4-1. [소행성 시스템](#4-1-소행성-시스템)
   
   4-2. [인공위성 폐기물 생성](#4-2-인공위성-폐기물-생성)
   
   4-3. [드론 AI](#4-3-드론-AI)
   
   4-4. [RPC 네트워크 시스템](#4-4-RPC-네트워크-시스템)
   
___
## **1. 프로젝트 소개**

### **레퍼런스**

* **Lethal Company - 리썰 컴퍼니**
<img width="691" height="416" alt="image" src="https://github.com/user-attachments/assets/575639bb-5ecd-47ed-aa6f-d86f431bfc2e" />

* **R.E.P.O - 레포**
<img width="713" height="417" alt="image" src="https://github.com/user-attachments/assets/5de21382-3f2b-4180-9591-b42fea9c8688" />

### **Team Victory**

이 프로젝트는 **R.E.P.O**와 **Lethal Company**를 참고한 **멀티 플레이 수집 생존형** 팀 프로젝트입니다.


스테이지가 지날수록 더 강해지고 많아지는 **드론**들을 상대로 살아남으며

팀원들이 **운전**, **사격**, **수집** 이 셋의 역할을 나누어

**인공위성 폐기물**과 **드론**으로부터 광물을 얻어 목표 액수를 채우고 **다음 스테이지**로 넘어가는 게임입니다.

___
## **2. 기술 스택**

### **Development**
* **C++**
  
* **Unreal Engine**

### **Cooperation Tools**
* **Zira**
  
* **GitHub**

* **Discord**
  
___
## **3. 세부 구현**

### **3-1. 소행성 시스템**

* **벡터 예측 낙하**: 우주선의 속도 벡터와 플레이어 위치를 기반으로 예측 낙하 지점 계산 → 정확한 타겟팅
- **크기/속도 기반 데미지**: 소행성이 충돌할 때 FTargetInfo의 Size와 Speed에 기반하여 Attack_Damage 값이 적용되는 방식으로 물리적 현실감 표현

### **3-2. 인공위성 폐기물 생성**

* **절차적 생성**: 스폰 포인트 주변에 난수 기반으로 크기/회전축/속도를 동적 생성 → 매 게임마다 다양한 환경
  
### **3-3. 드론 AI**

- **FSM 상태 머신**: Idle → SearchTarget → Chase → Attack 상태 전이 (거리 임계값, LOS 기반 감지)
  <img width="1295" height="362" alt="image" src="https://github.com/user-attachments/assets/693c6a4d-eb3e-4e3f-ab2f-b1ef21859671" />
  
- **궤도 공전**: 드론마다 각 매칭되는 궤도를 부여, IDLE State에서 인공위성 폐기물 주위를 도는 궤도를 쫓아가는 드론
  
- **쿼터니언 틸트**: 궤도 기울기를 쿼터니언으로 동적 변경하고 자전 적용 → 우주 환경에서의 자연스러운 3D 회전

### **3-4. RPC 네트워크 시스템**

- **Reliable RPC**: 데미지/상태 변화 같은 중요 이벤트는 반드시 전달 보장
  
- **Unreliable RPC**: 파티클/사운드 같은 시각 효과는 손실 허용으로 대역폭 최적화
  
- **서버 권한**: 모든 RPC에서 HasAuthority() 검증 → 클라이언트 조작 방지, 게임 로직은 서버에서만 처리
  
- **궤도 파라미터 복제**: 서버의 드론 궤도 파라미터(ChaseCurvePhase, TiltAngleCurrent, AngularSpeedCurrent)를 클라에 복제 → 클라이언트가 동일 로직으로 예측 계산
  
- **다단계 보간**: Lerp(파라미터) → 궤도 계산 → VInterpTo(위치) 3단계 조합으로 20Hz 네트워크를 60fps에서 부드럽게 표현
