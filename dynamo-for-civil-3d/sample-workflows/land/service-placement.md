# 서비스 배치

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption></figcaption></figure>

일반적인 주택 개발의 엔지니어링 설계에는 위생 하수, 강우 배수, 식수 등 여러 지하 공공 설비와 관련된 작업이 포함됩니다. 이 예에서는 Dynamo를 사용하여 분배 본관에서 지정된 로트(예: 구획)로 서비스 연결을 그릴 수 있는 방법을 보여 줍니다. 일반적으로 모든 로트에서 서비스 연결이 필요하므로 모든 서비스를 배치하는 데 상당히 지루한 작업을 하게 됩니다. Dynamo는 필요한 형상을 정밀하게 자동으로 그리는 것은 물론 현지 에이전시 표준에 맞게 조정할 수 있는 유연한 입력을 제공하여 프로세스 속도를 높일 수 있습니다.

## 목표

> :dart: 수도 서비스 계량기 블록 참조를 로트 선에서 지정된 간격띄우기에 배치하고 각 서비스 연결에 대해 분배 본관에 수직으로 선을 그립니다.

## 주요 개념

> * 사용자 입력을 위한 **객체 선택** 노드 사용
> * 좌표계 관련 작업
> * **Geometry.DistanceTo** 및 **Geometry.ClosestPointTo**와 같은 기하학적 연산 사용
> * 블록 참조 작성
> * 객체 바인딩 설정 제어

## 버전 호환성

{% hint style="success" %} 이 그래프는 **Civil 3D 2020** 이상 버전에서 실행됩니다. {% endhint %}

## 데이터세트

먼저 아래의 샘플 파일을 다운로드한 다음 DWG 파일과 Dynamo 그래프를 엽니다.

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dyn" %}

{% file src="../../../.gitbook/assets/Land_ServicePlacement.dwg" %}

## 해결책

이 그래프의 논리에 대한 개요는 다음과 같습니다.

> 1. 분배 본관에 대한 곡선 형상 가져오기
> 2. 사용자가 선택한 로트 선에 대한 곡선 형상 가져오기(필요한 경우 반전)
> 3. 서비스 계량기에 대한 삽입점 생성
> 4. 서비스 계량기 위치에서 가장 가까운 분배 본관의 점 가져오기
> 5. 모형 공간에서 블록 참조 및 선 작성

그럼 시작하겠습니다!

### 분배 본관 형상 가져오기

첫 번째 단계는 분배 본관에 대한 형상을 Dynamo로 가져오는 것입니다. 개별 선 또는 폴리선을 선택하는 대신, 특정 도면층에 있는 모든 객체를 가져와서 Dynamo PolyCurve로 결합합니다.

{% hint style="info" %} Dynamo 곡선 형상을 처음 사용하는 경우 [4-curves.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/4-curves.md "mention") 섹션을 참조하십시오. {% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_DistributionMain (1).png" alt=""><figcaption><p>Civil 3D에서 객체를 가져와 모든 것을 하나의 PolyCurve에 결합</p></figcaption></figure>

### 로트 선 형상 가져오기

다음으로, 선택한 로트 선의 형상을 Dynamo로 가져와서 작업할 수 있도록 해야 합니다. 이 작업에 적합한 도구는 그래프 사용자가 Civil 3D에서 특정 객체를 선택할 수 있는 **객체 선택** 노드입니다.

또한 발생할 수 있는 잠재적인 문제도 처리해야 합니다. 로트 선에는 시작점과 끝점이 있으며, 이는 로트 선에 방향이 있음을 의미합니다. 그래프가 일관된 결과를 생성하려면 모든 로트 선의 방향이 일관되어야 합니다. 그래프 논리에서 이 조건을 직접 고려할 수 있으므로, 그래프의 탄력성이 향상됩니다. 

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Selection (2).png" alt=""><figcaption><p>로트 선을 선택하고 올바른 방향인지 확인</p></figcaption></figure>

> 1. 로트 선의 시작점과 끝점을 가져옵니다.
> 2. 각 점에서 분배 본관까지의 거리를 측정한 다음 어느 거리가 더 긴지 파악합니다.
> 3. 원하는 결과는 선의 시작점이 분배 본관에 가장 가까운 것입니다. 그렇지 않으면 로트 선의 방향을 반전합니다. 그 외에는 원래 로트 선을 반환합니다.

### 삽입점 생성

이제 서비스 계량기를 어디에 배치할지 결정할 차례입니다. 일반적으로 현지 에이전시 요구 사항에 따라 배치가 결정되므로 다양한 조건에 맞게 변경할 수 있는 입력 값만 제공합니다. 점을 만들기 위한 참조로 로트 선을 따라 **좌표계**를 사용하겠습니다. 이렇게 하면 로트 선의 방향에 관계없이 로트 선을 기준으로 간격띄우기를 쉽게 정의할 수 있습니다.

{% hint style="info" %} 좌표계를 처음 사용하는 경우 [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") 섹션을 참조하십시오. {% endhint %}

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_InsertionPoints.png" alt=""><figcaption><p>서비스 계량기에 대한 삽입점 작성</p></figcaption></figure>

### 핀 연결점 가져오기

이제 서비스 계량기 위치에서 가장 가까운 분배 본관에 점을 확보해야 합니다. 이렇게 하면 모형 공간에 서비스 연결을 그려서 항상 분배 본관에 수직이 되도록 할 수 있습니다. **Geometry.ClosestPointTo** 노드는 완벽한 솔루션입니다.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_GetPerpendicularPoints (1).png" alt="" width="339"><figcaption><p>분배 본관에 수직 점 얻기</p></figcaption></figure>

> 1. 이것은 분배 본관 PolyCurve입니다.
> 2. 이것은 서비스 계량기 삽입점입니다.

### 객체 작성

마지막 단계는 모형 공간에서 실제로 객체를 작성하는 것입니다. 이전에 생성한 삽입점을 사용하여 블록 참조를 생성한 다음 분배 본관에 있는 점을 사용하여 서비스 연결에 대한 선을 그립니다.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_CreateObjects.png" alt=""><figcaption></figcaption></figure>

### 결과

그래프를 실행하면 모형 공간에 새 블록 참조 및 서비스 연결선이 표시됩니다. 일부 입력을 변경하고 모든 항목이 자동으로 업데이트되는지 확인해 보십시오!

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Dynamo (1).gif" alt=""><figcaption><p>Dynamo에서 입력 매개변수를 조정하고 Civil 3D에서 즉시 결과 확인</p></figcaption></figure>

### 보너스: 순차 배치 사용

하나의 로트 선에 객체를 배치한 후 다른 로트 선을 선택하면 객체가 "이동"되는 것을 볼 수 있습니다.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Binding.gif" alt=""><figcaption><p>객체 바인딩이 켜져 있을 때의 동작</p></figcaption></figure>

이는 Dynamo의 기본 동작이고, 대부분의 경우 매우 유용합니다. 그러나 여러 서비스 연결을 순차적으로 배치하고 원래 객체를 수정하는 대신 실행할 때마다 Dynamo가 새 객체를 작성하도록 하고자 할 수 있습니다. 이 동작은 객체 바인딩 설정을 변경하여 제어할 수 있습니다.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>Dynamo의 객체 바인딩 설정</p></figcaption></figure>

{% hint style="info" %} 자세한 내용은 [ object-binding.md](../../advanced-topics/object-binding.md "mention") 섹션을 참조하십시오. {% endhint %}

이 설정을 변경하면 Dynamo가 실행할 때마다 작성하는 객체를 "잊어버립니다". 다음은 **Dynamo 플레이어**를 사용하여 객체 바인딩을 끈 상태에서 그래프를 실행하는 예입니다.

<figure><img src="../../../.gitbook/assets/Land_ServicePlacement_Player (2).gif" alt=""><figcaption><p>Dynamo 플레이어를 사용하여 그래프를 실행하고 Civil 3D에서 결과 확인</p></figcaption></figure>

{% hint style="info" %} Dynamo 플레이어를 처음 사용하는 경우 [dynamo-player.md](../../dynamo-player.md "mention") 섹션을 참조하십시오. {% endhint %}

> :tadda: 작업을 완료했습니다!

## 아이디어

다음은 이 그래프의 기능을 확장하는 방법에 대한 몇 가지 아이디어입니다.

{% hint style="info" %} 각 로트 선을 선택하는 대신 **여러 개의 서비스 연결**을 동시에 배치합니다. {% endhint %}

{% hint style="info" %} 입력을 조정하여 수도 서비스 계량기 대신 **하수 청소구**를 배치합니다. {% endhint %}

{% hint style="info" %} 로트 선의 양쪽이 아닌 특정 쪽에 단일 서비스 연결을 배치할 수 있는 **토글을 추가**합니다. {% endhint %}
