# 점 그룹 관리

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption></figcaption></figure>

Civil 3D에서 COGO 점 및 점 그룹으로 작업하는 것은 많은 필드-마감 프로세스의 핵심 요소입니다. 이 예제에서는 데이터 관리와 관련하여 Dynamo가 매우 유용할 수 있는 잠재적인 사용 사례 하나를 보여드리겠습니다.  

## 목표

> :dart: 고유한 각 COGO 점 설명에 대한 점 그룹을 작성합니다. 

## 주요 개념

> * 리스트 작업
> * **List.GroupByKey** 노드로 유사 객체 그룹화
> * Dynamo 플레이어에 사용자 출력 표시

## 버전 호환성

{% hint style="success" %} 이 그래프는 **Civil 3D 2020** 이상 버전에서 실행됩니다. 
{% endhint %} 

## 데이터세트

먼저 아래의 샘플 파일을 다운로드한 다음 DWG 파일과 Dynamo 그래프를 엽니다.

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dyn" %}

{% file src="../../../.gitbook/assets/Survey_CreatePointGroups.dwg" %}

## 해결 방법

이 그래프의 논리에 대한 개요는 다음과 같습니다.

> 1. 문서의 모든 COGO 점 가져오기
> 2. 설명별로 COGO 점 그룹화
> 3. 점 그룹 작성
> 4. Dynamo 플레이어에 요약 출력

그럼 시작하겠습니다!

### COGO 점 가져오기

첫 번째 단계는 문서의 모든 점 그룹을 가져온 다음 각 그룹 내의 모든 COGO 점을 가져오는 것입니다. 그러면 _내포된 리스트_ 또는 "리스트의 리스트"가 생성되며, **List.Flatten** 노드를 사용하여 모든 것을 단일 리스트로 단순화하면 나중에 작업하기가 더 쉬워집니다.

{% hint style="info" %}
 리스트 작업을 처음 해보는 경우 [2-working-with-lists.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/2-working-with-lists.md "mention") 섹션을 참조하십시오. 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GetPoints.png" alt=""><figcaption><p>모든 점 그룹 및 COGO 점 가져오기 </p></figcaption></figure>

### 설명별로 점 그룹화

이제 모든 COGO 점이 있으므로 해당 설명을 기준으로 점을 그룹으로 분리해야 합니다. 이것이 바로 **List.GroupByKey** 노드의 역할입니다. 이 노드는 기본적으로 동일한 키를 공유하는 모든 항목을 그룹화합니다.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_GroupPoints.png" alt="" width="563"><figcaption><p>설명별로 COGO 점 그룹화</p></figcaption></figure>

### 점 그룹 작성

힘든 작업은 끝났습니다! 마지막 단계는 그룹화된 COGO 점에서 새 Civil 3D 점 그룹을 작성하는 것입니다.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_CreatePointGroups.png" alt="" width="371"><figcaption><p>새 점 그룹 작성</p></figcaption></figure>

### 출력 요약

그래프를 실행할 때, 우리는 형상에 대한 작업을 하지 않을 것이기 때문에 Dynamo 배경 미리보기에 아무것도 표시되지 않습니다. 따라서 그래프가 제대로 실행되는지 확인할 수 있는 유일한 방법은 도구공간을 확인하거나 노드 출력 미리보기를 보는 것입니다. 그러나 **Dynamo 플레이어** 를 사용하여 그래프를 실행하면 작성된 점 그룹에 대한 요약을 출력하여 그래프 결과에 대해 더 많은 피드백을 제공할 수 있습니다. 노드를 마우스 오른쪽 버튼으로 클릭하고 _Is Output_ 으로 설정하기만 하면 됩니다. 이 경우 이름이 변경된 **Watch** 노드를 사용하여 결과를 확인합니다.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Output.png" alt="" width="437"><figcaption><p>노드를 <em>Is Output</em> 으로 설정하면 Dynamo 플레이어 출력에 해당 컨텐츠가 표시됩니다.</p></figcaption></figure>

### 결과

다음은 **Dynamo 플레이어** 를 사용하여 그래프를 실행하는 예입니다.

<figure><img src="../../../.gitbook/assets/Survey_CreatePointGroups_Player.gif" alt=""><figcaption><p>Dynamo 플레이어를 사용하여 그래프를 실행하고 도구공간에서 결과 확인</p></figcaption></figure>

{% hint style="info" %}
 Dynamo 플레이어를 처음 사용하는 경우 [dynamo-player.md](../../dynamo-player.md "mention") 섹션을 참조하십시오. 
{% endhint %} 

> :tada: 작업을 완료했습니다!

## 아이디어

다음은 이 그래프의 기능을 확장하는 방법에 대한 몇 가지 아이디어입니다.

{% hint style="info" %}
 초기 정보 대신 ** 전체 설명** 을 기반으로 점 그룹을 수정합니다. 
{% endhint %} 

{% hint style="info" %}
 선택한 몇 가지 다른 **사전 정의된 카테고리** (예: "Ground shots", "Monuments" 등)로 점을 그룹화합니다. 
{% endhint %} 

{% hint style="info" %}
 특정 그룹의 점에 대한 TIN 지표면을 자동으로 작성합니다. 
{% endhint %} 
