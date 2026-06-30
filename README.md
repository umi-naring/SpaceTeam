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
**Unreal Engine 5.3**과 **C++**으로 개발 된 게임이며

스테이지가 지날수록 더 강해지고 많아지는 **드론**들을 상대로 살아남으며
팀원들이 **운전**, **사격**, **수집** 이 셋의 역할을 나누어
**인공위성 폐기물**과 **드론**으로부터 광물을 얻어 목표 액수를 채우고 **다음 스테이지**로 넘어가는 게임입니다.

___
## **2. 기술 스택**

___
## **3. 팀원 및 역할**

___
## **4. 세부 구현**

### **4-1. 소행성 시스템**

GameplayTag 기반의 상태 관리를 통해 복잡한 캐릭터 상태를 처리합니다:

Tag예시

Survivor.Status.Injured: 부상 상태
Survivor.Status.Dying: 빈사 상태
Survivor.Status.Captured.Hook: 갈고리 걸림 상태
Survivor.Status.Captured.Killer: 살인마에게 잡힘 상태
상태 전환에 대한 로직은 다음 세가지를 활용하였습니다.

GameplayAbility Trigger
GameplayTagEvent
AttributeValueChangeDelegates
---
### **4-2. 인공위성 폐기물 생성**

관련 파일:

DBDCharacterSubsystem.cpp
PoolEntry_ScratchMark.cpp
DBDObjectPoolComponent.cpp
설계 방향
생존자가 달릴 때마다 생성되는 발자국은 매 틱마다 엑터의 생성과 삭제를 반복해야 합니다. 또한, 발자국은 **살인마(Killer)**에게만 보여야 하므로 모든 클라이언트에 동기화할 필요가 없습니다.

따라서 DBDCharacterSubsystem을 통해 살인마 클라이언트에서만 독립적으로 발자국 생성하고, 오브젝트 풀링으로 메모리 비용을 최적화했습니다.



### **4-3. 드론 AI**

관련 파일:

DBDCharacterSubsystem.cpp
DBDCharacter.cpp
설계 방향
오라(Aura)는 모든 플레이어에게 보이는 것이 아니라, 특정 퍽(Perk)을 장착한 생존자 클라이언트에서만 보여야 합니다. 이를 구현하기 위해 UDBDCharacterSubsystem이 클라이언트 전용 로직을 수행합니다.

오라는 조건이 만족될 경우에만 보이므로 모든 생존자의 상태를 주기적으로 점검해야 합니다. 이를 최적화 하기 위해 관련 퍽이 있는 플레이어의 클라이언트에서만 점검합니다.

주요 함수
// UDBDCharacterSubsystem.h
void EnableSurvivorAuraWithDistanceAndTag(
    UObject* AuraInstigator, 
    ADBDCharacter* EffectOwner, 
    float Distance,
    FGameplayTagContainer RequiredTags,
    FGameplayTagContainer BlockedTags
);
활용 사례
Bond 퍽: Bond 퍽을 든 생존자 화면에서만 10m 내 다른 생존자의 오라가 보임
Empathy 퍽: Empathy 퍽을 든 생존자 화면에서만 20m 내 부상당한 생존자의 오라가 보임
동작 시퀀스

### **4-4. RPC 네트워크 시스템**

관련 파일:

PerkComponent.cpp
JMS/Perk/ 7개 퍽 구현
설계 방향
다형성을 활용해 베이스 구조의 변경 없이 OnServerSideInitialized, OnOwnerClientSideInitialized 함수 오버라이드 만으로 여러가지 퍽을 만들 수 있게 하였습니다. 만들어진 퍽은 데이터테이블에 담아 체계적으로 관리하였습니다.

구현된 퍽 목록
퍽 이름	효과	구현 방식
Sprint Burst	질주 시작 시 3초간 이동속도 150% + 탈진 효과(광역 쿨타임 역할)	GameplayTagEvent 트리거 -> GameplayEffect 적용
Self Care	치료 도구 없이 자가 치료 가능	GameplayAbility 부여
Botany Knowledge	치료 속도 33% 증가	GameplayEffect 적용
Adrenaline	탈출구 개방 시 즉시 건강상태 1단계 회복 + 3초간 이동속도 150% + 탈진 효과(광역 쿨타임 역할)	Delegate가입, GameplayEffect 적용
Bond	10m 내 동료 오라 표시	캐릭터 서브시스템 사용
Empathy	20m 내 부상 동료 오라 표시	캐릭터 서브시스템 사용
Prove Thyself	주변 생존자 수 만큼 광역 버프(발전기 수리 속도 증가)	GameplayEffect레벨 + CurveTable로 버프 수치 적용, 캐릭터 서브시스템으로 거리 연산
퍽 클래스 구조

5. Gameplay Ability System (GAS) 적용
관련 파일:

생존자 GAS 관련 클래스 구현
AbilitySystemComponent 상속 구조

주요 GameplayAbility 예시
1) 기본 행동 어빌리티
GA_Survivor_Move: 이동 처리
GA_Survivor_Crouch: 웅크리기
GA_Survivor_Sprint: 질주
2) 상호작용 어빌리티
GA_Survivor_RepairGenerator: 발전기 수리
GA_Survivor_HealOther: 동료 치료
GA_SelfCare: SelfCare퍽에 의해 부여되는 어빌리티
GA_Survivor_Rescue: 갈고리 구출
GA_Survivor_OpenExitDoor: 탈출구 개방
GA_Survivor_PickUpItem: 아이템 습득
3) 패시브 어빌리티
GA_Survivor_Dying: 빈사 상태 처리
GA_Survivor_HookedIn: 갈고리 걸림 상태
GA_Survivor_CapturedByKiller: 킬러에게 잡힘
GA_Survivor_Escape: 탈출 처리

AttributeSet 설계
USurvivorAttributeSet는 생존자의 핵심 능력치를 정의합니다:

Attribute	설명	복제 여부
MovementSpeed	기본 이동 속도	-
SprintSpeed	질주 속도	-
HealProgress	치료 진행도 (0.0 ~ 1.0)	-
DyingHP	빈사 상태 HP	-
네트워크 동기화를 통해 서버의 Attribute 변경이 모든 클라이언트에 자동 반영됩니다.

GameplayTag 활용
Tag 기반 시스템으로 복잡한 조건부 로직을 단순화했습니다:

Survivor.Status.Normal
Survivor.Status.Injured
Survivor.Status.Dying
Survivor.Status.Captured.Hook
Survivor.Ability.Interaction.RepairGenerator
Survivor.Ability.Interaction.HealOther
Survivor.Status.Sprinting
Interactable.Object.Generator
Interactable.Character.Survivor
Interactable.Object.Hook
6. 아이템 시스템
관련 파일:

SurvivorItem.cpp
설계 방향
아이템은 떨어뜨리면 다른 생존자가 주울 수 있는 독립적인 액터입니다. 따라서 단일 책임 원칙을 지킬 수 있도록 멤버를 구성하였습니다. 애드온을 통해 아이템을 강화할 수 있고 이 또한 독립적인 컴포넌트 클래스입니다.

아이템 종류
아이템	기능
Medkit	자가 치료 또는 동료 치료 속도 증가
Toolbox	발전기 수리 속도 증가, 갈고리 파괴
Firecracker	폭발 후 주변에 실명 태그 부여
클래스 구조

7. 상호작용 시스템
관련 파일:

InteractorComponent.cpp
설계 방향:
상호작용은 인터페이스 끼리 신호를 주고받고, 세부 기능은 각 클래스 별로 구현하여 의존성 역전 원칙을 지키도록 의도하였습니다. InteractorComponent는 신호를 주는 쪽의 컴포넌트로, 서버에서만 동작하고, 별도의 충돌 채널을 사용합니다.

주기적으로 주변을 탐색하여 플레이어에게 GameplayTag를 통해 미리 정보를 표시하여 주고, 탐색 주기를 Tick대신 타이머로 관리하여 연산 효율을 높였습니다.

상호작용 흐름

8. 유틸리티 클래스
설계 방향
전역적으로 사용 가능한 GAS 관련 유틸리티, 애니메이션 동기화 연산 관련 유틸리티, 디버그 유틸리티를 구현하여 개발 효율을 높였습니다.

관련 파일:

DBDBlueprintFunctionLibrary.cpp
DBDDebugHelper.cpp
DBDBlueprintFunctionLibrary
블루프린트에서 사용 가능한 유틸리티 함수 제공:

GAS 관련 헬퍼 함수
애니메이션 시작 전 메시 소켓 기반 위치 계산 함수
DBDDebugHelper
개발 중 디버깅을 위한 시각화 도구:

PIE창, 로그 창 모두 메시지 출력
NetMode 검사 메시지 출력
가변 인자 Printf 스크린 출력
Server, Client 별 디버그 메시지
