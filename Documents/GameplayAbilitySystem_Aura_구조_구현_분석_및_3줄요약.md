# GameplayAbilitySystem_Aura 구조/구현 분석 및 3줄 요약

대상 저장소: `https://github.com/DruidMech/GameplayAbilitySystem_Aura.git`
로컬 경로: `D:/Github/GameplayAbilitySystem_Aura`
분석 범위: `Source/Aura/*`, `Config/*`, 프로젝트 루트 구조 중심 정적 분석

---

## 1) 한 줄 요약

GameplayAbilitySystem_Aura는 **언리얼에서 GAS를 본격적인 액션 RPG 구조 위에 얹어 둔 대형 학습용/실전형 샘플 프로젝트**입니다.

이 레포의 가치는 단순 GAS 소개를 넘어서,
**능력, 속성, 데미지 계산, UI, 적 AI, 입력, 세이브, 캐릭터 클래스 데이터까지 하나의 ARPG 프레임 안에서 묶어 보여 준다**는 데 있소.

---

## 2) 전체 성격

이 프로젝트는 작은 GAS 입문 예제가 아니오.
구조를 보면 다음 성격이 분명합니다.

- UE 프로젝트 전체를 갖춘 샘플
- GAS 중심 설계
- ARPG형 능력/데미지/적 AI 구조
- UI와 위젯 컨트롤러까지 포함
- 세이브/로드와 체크포인트 흐름까지 고려

즉, 이것은 “GAS 함수 몇 개를 배우는 저장소”가 아니라,
**GAS로 게임 하나의 뼈대를 세우는 법**을 보여 주는 프로젝트에 가깝소.

---

## 3) 전체 구조

핵심 루트:
- `Source/Aura`
- `Config`
- `Content`
- `Data`
- `Plugins`

소스 구조의 중심 축:
- `AbilitySystem`
- `Actor`
- `AI`
- `Character`
- `Game`
- `Input`
- `Interaction`
- `Player`
- `UI`
- `Checkpoint`

이 구조만 봐도,
단순 전투 샘플이 아니라 **게임 전체를 모듈별로 나눈 구조적 프로젝트**라는 점이 드러납니다.

---

## 4) 핵심 구현 포인트

### 4-1. AbilitySystem이 프로젝트의 심장
`AbilitySystem` 폴더 안에 거의 모든 GAS 핵심이 모여 있습니다.

중요한 축:
- `AuraAbilitySystemComponent`
- `AuraAttributeSet`
- `AuraAbilitySystemLibrary`
- `AuraAbilitySystemGlobals`
- `Abilities/*`
- `ExecCalc/*`
- `ModMagCalc/*`
- `Data/*`
- `AbilityTasks/*`
- `AsyncTasks/*`

즉, 이 프로젝트는 GAS를 단순 플러그인 사용 수준이 아니라,
**프로젝트 전체 규칙의 중심 시스템**으로 사용합니다.

### 4-2. 능력 구조가 매우 풍부함
보이는 능력들만 해도:
- `AuraFireBolt`
- `AuraFireBlast`
- `AuraBeamSpell`
- `ArcaneShards`
- `Electrocute`
- `AuraSummonAbility`
- `AuraMeleeAttack`
- `AuraPassiveAbility`

즉, 단발 능력 실험이 아니라,
원거리/근거리/채널링/소환/패시브 등 **능력 유형을 폭넓게 아우르는 샘플**입니다.

### 4-3. 데미지 계산이 Execution Calculation 중심
- `ExecCalc_Damage`
- `MMC_MaxHealth`
- `MMC_MaxMana`

이건 아주 중요하오.
즉, 이 프로젝트는 단순 modifier 누적보다,
**GAS의 Execution / Magnitude Calculation을 실전적으로 쓰는 패턴**을 보여 줍니다.

### 4-4. DataAsset 기반 확장 구조
`AbilitySystem/Data` 안의
- `AbilityInfo`
- `AttributeInfo`
- `CharacterClassInfo`
- `LevelUpInfo`
- `LootTiers`

를 보면, 이 프로젝트는 하드코딩보다
**데이터 기반으로 캐릭터/능력/레벨업/루트 구조를 관리**하려는 방향을 취합니다.

이 점은 실무적으로 매우 중요하오.
게임이 커질수록 능력과 클래스 규칙은 코드보다 데이터로 빠지는 편이 유지보수에 유리하기 때문이오.

### 4-5. UI 구조가 분명함
`UI` 아래에
- `Widget`
- `WidgetController`
- `ViewModel`
- `HUD`

가 분리되어 있습니다.

이는 단순히 위젯 몇 개 띄우는 것이 아니라,
**GAS 상태를 UI에 전달하는 중간 계층을 의식한 구조**라는 뜻입니다.
특히 `OverlayWidgetController`, `SpellMenuWidgetController`, `AttributeMenuWidgetController`는 보기 좋은 포인트요.

### 4-6. Player / Character / AI의 역할 분리
- `AuraCharacter`
- `AuraCharacterBase`
- `AuraEnemy`
- `AuraPlayerController`
- `AuraPlayerState`
- `AuraAIController`
- `BTTask_Attack`
- `BTService_FindNearestPlayer`

즉, 플레이어, 적, AI 행동, GAS 연결이 한 클래스에 다 뭉친 것이 아니라,
**캐릭터/조종자/행동 로직이 비교적 분리된 구조**입니다.

---

## 5) 무엇을 깊게 봐야 하는지

### A. `AuraAbilitySystemComponent`
이 프로젝트에서 가장 먼저 봐야 할 중심축입니다.
능력 부여, 활성화, 상태 전달 등 프로젝트용 ASC 확장이 어떻게 되어 있는지 확인해야 하오.

### B. `AuraAttributeSet`
속성 구조를 어떻게 나눴는지, 데미지/마나/체력/보조 능력치가 어떻게 연결되는지 봐야 합니다.

### C. `ExecCalc_Damage`
실전 GAS에서 가장 가치 있는 부분 중 하나입니다.
단순 수치 변화가 아니라, 데미지를 서버 권위 계산으로 어떻게 처리하는지 이해할 수 있소.

### D. `AbilitySystem/Data/*`
이 프로젝트가 단순 샘플이 아니라 “확장 가능한 구조”인지 보려면 이 폴더가 중요합니다.
특히 `CharacterClassInfo`, `AbilityInfo`, `LevelUpInfo`는 꼭 봐야 하오.

### E. `TargetDataUnderMouse`
입력/타게팅과 GAS를 어떻게 연결하는지 배우기 좋은 부분입니다.
ARPG 스타일 능력 선택 흐름의 실전 포인트요.

### F. `UI/WidgetController/*`
GAS 데이터를 UI에 어떻게 흘려보내는지 보기에 좋습니다.
실제 게임에서 가장 자주 필요한 구조입니다.

### G. `AuraPlayerState`
ASC를 어디에 둘지, PlayerState와 Character의 책임을 어떻게 나눴는지 반드시 확인할 가치가 있소.

---

## 6) 멀티플레이 관점에서의 가치

이 프로젝트는 listen server 튜토리얼처럼 세션 생성부터 가르치는 예제는 아니오.
하지만 멀티플레이 게임플레이 구조 자체는 매우 참고 가치가 큽니다.

특히 중요한 이유:
- GAS는 본질적으로 네트워크 게임플레이 프레임워크요
- ability activation, tags, effects, attributes, cues 모두 replication과 엮입니다
- 따라서 이 프로젝트는 “게임플레이 멀티플레이”를 이해하는 데 큰 도움을 줍니다

즉,
- 세션/호스트 구조는 다른 예제로 잡고
- 실제 전투/상태/능력 동기화는 이 레포로 배우는 식이 좋습니다.

---

## 7) 그대 기획과 연결되는 이유

그대가 찾는 것이 단순 방 생성이 아니라,
나중에 **실제 플레이 규칙이 네트워크 위에서 어떻게 돌아야 하는가**라면 이 레포는 매우 유용하오.

특히 도움이 되는 부분:
- 상태 기반 능력 설계
- 데이터 중심 클래스/레벨업 설계
- UI와 GAS 연결
- AI 적 캐릭터와 능력 시스템 결합
- 데미지 계산 구조

즉, 경영/시뮬레이션 쪽 기획이라 해도,
나중에 스킬/행동/상태가 붙는 순간 이런 사고법이 필요해집니다.

---

## 8) 한계

- 프로젝트 규모가 커서 초심자에게는 무거움
- listen server / 세션 / OSS 자체를 설명하는 레포는 아님
- README가 없고, 구조를 직접 읽어야 해서 진입 장벽이 있음
- ARPG 문맥이 강하므로, 다른 장르에 바로 복붙하긴 어려움

즉, 이 레포는 초급 입문서보다 **중급 이상 구조 참고서**에 가깝습니다.

---

## 9) 활용 추천

추천 용도:
- GAS를 본격적으로 게임 구조에 녹이는 법 배우기
- ARPG형 능력/속성/데미지/UI/AI 구조 참고
- 데이터 기반 캐릭터/능력 확장 패턴 이해

비추천 용도:
- 세션 생성만 빨리 배우기
- 최소 RPC 예제 찾기
- 아주 가벼운 입문용 샘플 찾기

---

## 10) 3줄 요약

1. GameplayAbilitySystem_Aura는 GAS를 실제 ARPG 프로젝트 구조 안에 깊게 녹여 둔 대형 샘플로, 능력·속성·데미지·UI·AI를 함께 보여 줍니다.
2. listen server 입문서라기보다, 네트워크 게임플레이와 데이터 기반 GAS 설계를 배우는 중급 이상 참고서로 가치가 큽니다.
3. 특히 `AuraAbilitySystemComponent`, `AuraAttributeSet`, `ExecCalc_Damage`, `WidgetController`, `Data/*`는 꼭 깊게 볼 만하오.
