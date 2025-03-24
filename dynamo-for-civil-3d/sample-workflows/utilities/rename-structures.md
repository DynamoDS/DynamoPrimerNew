# 구조물 이름 바꾸기

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption></figcaption></figure>

파이프 및 구조물을 관망에 추가할 때 Civil 3D는 템플릿을 사용하여 자동으로 이름을 지정합니다. 일반적으로 초기 배치 시에는 이 정도면 충분하지만 향후 설계가 진행됨에 따라 불가피하게 이름을 변경해야 할 수도 있습니다. 또한 가장 먼 다운스트림 구조물에서 시작하여 관로 내에서 순차적으로 구조물의 이름을 지정하거나 로컬 에이전시의 데이터 스키마에 맞는 이름 지정 패턴을 따르는 등 다양한 이름 지정 패턴이 필요할 수 있습니다. 이 예에서는 Dynamo를 사용하여 모든 유형의 이름 지정 전략을 정의하고 일관되게 적용하는 방법을 보여 줍니다.

## 목표

> :dart: 선형의 측점 표시를 기반으로 관망 구조물의 이름을 순서대로 바꿉니다.

## 주요 개념

> * 경계 상자 작업
> * **List.FilterByBoolMask** 노드를 사용하여 데이터 필터링
> * **List.SortByKey** 노드를 사용하여 데이터 정렬
> * 문자열 생성 및 수정

## 버전 호환성

{% hint style="success" %} 이 그래프는 **Civil 3D 2020** 이상 버전에서 실행됩니다. {% endhint %}

## 데이터세트

먼저 아래의 샘플 파일을 다운로드한 다음 DWG 파일과 Dynamo 그래프를 엽니다.

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dyn" %}

{% file src="../../../.gitbook/assets/Utilities_RenameStructures.dwg" %}

## 해결 방법

이 그래프의 논리에 대한 개요는 다음과 같습니다.

> 1. 도면층별 구조물 선택
> 2. 구조물 위치 가져오기
> 3. 간격띄우기를 기준으로 구조물을 필터링한 다음 측점을 기준으로 정렬
> 4. 새 이름 생성
> 5. 구조물 이름 바꾸기

그럼 시작하겠습니다!

### 구조물 선택

가장 먼저 해야 할 일은 작업할 모든 구조물을 선택하는 것입니다. 특정 도면층에서 모든 객체를 선택하기만 하면 되므로, 다른 관망에서 구조물을 선택할 수 있습니다(동일한 도면층을 공유한다고 가정할 때).

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectStructures (1).png" alt=""><figcaption><p>지정된 도면층에서 구조물 선택</p></figcaption></figure>

> 1. 이 노드를 사용하면 구조물과 동일한 도면층을 공유할 수 있는, 원하지 않는 객체 유형을 실수로 검색하지 않을 수 있습니다.

### 구조물 위치 가져오기

이제 구조물이 준비되었으므로 공간에서 구조물의 위치를 파악하여 위치에 따라 구조물을 정렬해야 합니다. 이를 위해 각 객체의 경계 상자를 활용하겠습니다. 객체의 **경계 상자** 는 객체의 기하학적 범위를 완전히 포함하는 최소 크기의 상자입니다. 경계 상자의 중심을 계산하면 구조물 삽입점에 대한 근사치를 꽤 정확하게 구할 수 있습니다.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GetPosition.png" alt=""><figcaption><p>경계 상자를 사용하여 각 구조물의 대략적인 삽입점 가져오기</p></figcaption></figure>

이러한 점을 사용하여 선택한 선형을 기준으로 구조물의 측점 및 간격띄우기를 구할 것입니다.

<div>

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SelectAlignment.png" alt="" width="268"><figcaption></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StationOffset.png" alt="" width="306"><figcaption></figcaption></figure>

</div>

### 필터 및 정렬

여기서부터 작업이 조금 까다로워지기 시작합니다. 이 단계에서는 도면층에 지정한 모든 구조물의 큰 리스트가 있고 이를 정렬할 선형을 선택했습니다. 문제는 리스트에 이름을 바꾸고 싶지 않은 구조물이 있을 수 있다는 것입니다. 예를 들어, 그러한 구조물은 관심 있는 특정 실행의 일부가 아닐 수 있습니다.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_StructureIllustration.png" alt="" width="555"><figcaption></figcaption></figure>

> 1. 선택한 선형
> 2. 이름을 바꾸려는 구조물
> 3. 무시해야 하는 구조물

따라서 선형에서 특정 간격띄우기보다 큰 구조물은 고려하지 않도록 구조물 리스트를 필터링해야 합니다. 이 작업은 **List.FilterByBoolMask** 노드를 사용하여 수행하는 것이 가장 좋습니다. 구조물 리스트를 필터링한 후 **List.SortByKey** 노드를 사용하여 측점 값을 기준으로 구조물을 정렬합니다.

{% hint style="info" %} 리스트 작업을 처음 해보는 경우 [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") 섹션을 참조하십시오. {% endhint %}

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_FilterAndSort.png" alt=""><figcaption><p>구조 필터링 및 정렬</p></figcaption></figure>

> 1. 구조물의 간격띄우기가 임계값보다 작은지 확인
> 2. Null 값을 _false_ 로 대치
> 3. 구조물 및 측점 리스트 필터링
> 4. 측점별로 구조물 정렬

### 새 이름 생성

마지막으로 해야 할 작업은 구조물의 이름을 새로 생성하는 것입니다. 사용할 형식은 `<alignment name>-STRC-<number>` 입니다. 원하는 경우 숫자를 0으로 채울 수 있는 몇 가지 추가 노드가 있습니다(예: "1" 대신 "01").

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_GenerateNames.png" alt=""><figcaption><p>새 구조물 이름 생성</p></figcaption></figure>

### 구조물 이름 바꾸기

마지막으로, 구조물 이름을 바꾸겠습니다.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_SetName.png" alt="" width="289"><figcaption><p>구조물의 이름 설정</p></figcaption></figure>

### 결과

다음은 **Dynamo 플레이어** 를 사용하여 그래프를 실행하는 예입니다.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Player.gif" alt=""><figcaption><p>Dynamo 플레이어를 사용하여 그래프를 실행하고 Civil 3D에서 결과 확인</p></figcaption></figure>

{% hint style="info" %} Dynamo 플레이어를 처음 사용하는 경우 [dynamo-player.md](../../dynamo-player.md "mention") 섹션을 참조하십시오. {% endhint %}

> :tada: 작업을 완료했습니다!

### 보너스: Dynamo에서 시각화

최종 결과 대신 그래프의 중간 출력을 시각화하기 위해 Dynamo의 3D 배경 미리보기를 활용하는 것이 도움이 될 수 있습니다. 한 가지 쉬운 방법은 구조물의 경계 상자를 보여주는 것입니다. 또한 이 특정 데이터세트에는 문서에 코리더가 있으므로 코리더 형상선 형상을 Dynamo로 가져와서 구조물이 공간에서 어디에 위치하는지에 대한 컨텍스트를 제공할 수 있습니다. 코리더가 없는 데이터세트에서 그래프를 사용하는 경우 이러한 노드는 아무 작업도 수행하지 않습니다.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Visualize (2).png" alt=""><figcaption><p>구조물 및 코리더 형상선에 대한 형상 시각화</p></figcaption></figure>

이제 간격띄우기를 기준으로 구조를 필터링하는 프로세스가 작동하는 방식을 더 잘 이해할 수 있습니다.

<figure><img src="../../../.gitbook/assets/Utilities_RenameStructures_Dynamo (1).gif" alt=""><figcaption><p>Dynamo에서 선형 간격띄우기 임계값을 조정하고 영향을 받는 구조물 시각화</p></figcaption></figure>

## 아이디어

다음은 이 그래프의 기능을 확장하는 방법에 대한 몇 가지 아이디어입니다.

{% hint style="info" %} 특정 선형을 선택하는 대신 **가장 가까운 선형** 을 기준으로 구조물의 이름을 바꿉니다. {% endhint %}

{% hint style="info" %} 구조물 외에 **파이프의 이름을 바꿉니다**. {% endhint %}

{% hint style="info" %} 해당 실행을 기준으로 구조물의 **도면층을 설정** 합니다.  {% endhint %}
