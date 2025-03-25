# 클리어런스 엔벨로프

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption></figcaption></figure>

클리어런스 검증을 위한 운동학적 엔벨로프를 개발하는 것은 레일 설계에서 중요한 부분입니다. 복잡한 코리더 횡단구성요소를 작성하고 관리하는 대신 Dynamo를 사용하여 엔벨로프에 사용할 솔리드를 생성할 수 있습니다.

## 목표

> :dart: 차량 프로파일 블록을 사용하여 코리더를 따라 클리어런스 엔벨로프 3D 솔리드를 생성합니다.

## 주요 개념

> * 코리더 형상선 작업
> * 좌표계 간 형상 변환
> * 로프트를 통한 솔리드 작성
> * 레이싱 설정으로 노드 동작 제어

## 버전 호환성

{% hint style="success" %} 이 그래프는 **Civil 3D 2020** 이상 버전에서 실행됩니다. 
{% endhint %} 

## 데이터세트

먼저 아래의 샘플 파일을 다운로드한 다음 DWG 파일과 Dynamo 그래프를 엽니다.

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dyn" %}

{% file src="../../../.gitbook/assets/Rail_ClearanceEnvelope.dwg" %}

## 해결 방법

이 그래프의 논리에 대한 개요는 다음과 같습니다.

> 1. 지정된 코리더 기준선에서 형상선 가져오기
> 2. 코리더 형상선을 따라 원하는 간격으로 좌표계 생성
> 3. 프로파일 블록 형상을 좌표계로 변환
> 4. 프로파일 사이에 솔리드 로프트
> 5. Civil 3D에서 솔리드 작성

그럼 시작하겠습니다!

### 코리더 데이터 가져오기

첫 번째 단계는 코리더 데이터를 가져오는 것입니다. 이름으로 코리더 모형을 선택하고 코리더 내에서 특정 기준선을 가져온 다음, 기준선 내에서 점 코드로 형상선을 가져옵니다.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_GetCorridorData.png" alt=""><figcaption><p>코리더, 기준선 및 형상선 선택</p></figcaption></figure>

### 좌표계 생성

이제 지정된 시작 측점과 끝 측점 사이의 코리더 형상선을 따라 **좌표계** 를 생성하겠습니다. 이러한 좌표계는 차량 프로파일 블록 형상을 코리더에 정렬하는 데 사용됩니다.

{% hint style="info" %}
 좌표계를 처음 사용하는 경우 [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") 섹션을 참조하십시오. 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_CreateCoordinateSystems.png" alt=""><figcaption><p>코리더 형상선을 따라 좌표계 가져오기</p></figcaption></figure>

> 1. 노드의 오른쪽 아래 모서리에 있는 작은 **XXX** 이(가) 있습니다. 이는 노드의 레이싱 설정이 두 형상선에 대해 동일한 측점 값으로 좌표계를 생성하는 데 필요한 _외적_ 으로 설정되어 있음을 의미합니다.

{% hint style="info" %}
 노드 레이싱을 처음 사용하는 경우 [1-whats-a-list.md](../../../5\_essential\_nodes\_and\_concepts/5-4\_designing-with-lists/1-whats-a-list.md "mention") 섹션을 참조하십시오. 
{% endhint %} 

### 블록 형상 변환

이제 형상선을 따라 차량 프로파일의 어레이를 만들어야 합니다. 이제 **Geometry.Transform** 노드를 사용하여 차량 프로파일 블록 정의에서 형상을 변환하겠습니다. 이는 시각화하기 까다로운 개념이므로, 노드를 살펴보기 전에 어떤 일이 일어날지 보여주는 다음 그래픽을 확인하십시오.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_TransformAnimation.gif" alt=""><figcaption><p>좌표계 간 형상 변환의 시각화</p></figcaption></figure>

따라서 기본적으로 _단일_ 블록 정의에서 Dynamo 형상을 가져와서 이동/회전하는 동시에 형상선을 따라 어레이를 작성합니다. 아주 멋집니다! 다음은 노드 순서의 모습입니다.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Transform.png" alt=""><figcaption></figcaption></figure>

> 1. 이 노든 문서에서 블록 정의를 가져옵니다.
> 2. 이러한 노드는 블록 내 객체의 Dynamo 형상을 가져옵니다.
> 3. 이러한 노드는 기본적으로 형상을 _변환하는_ 좌표계를 정의합니다.
> 4. 마지막으로 이 노드는 형상을 변환하는 실제 작업을 수행합니다.
> 5. 이 노드에서 _가장 긴_ 레이싱을 주목합니다.

우리가 Dynamo에서 얻는 결과는 다음과 같습니다.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Profiles.png" alt=""><figcaption><p>변환 후 차량 프로파일 블록 형상</p></figcaption></figure>

### 솔리드 생성

기쁜 소식을 전해 드립니다! 힘든 작업은 끝났습니다. 이제 프로파일 간에 솔리드를 생성하기만 하면 됩니다. **Solid.ByLoft** 노드를 사용하면 이 작업을 쉽게 수행할 수 있습니다.

<figure><img src="../../../.gitbook/assets/Rail_PlaceTies_SolidByLoft.png" alt="" width="325"><figcaption></figcaption></figure>

결과는 다음과 같습니다. 이 솔리드는 Dynamo 솔리드이므로 여전히 Civil 3D에서 작성해야 합니다.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Dynamo_Solids.png" alt=""><figcaption><p>로프트 후 Dynamo 솔리드</p></figcaption></figure>

### 솔리드를 Civil 3D로 출력

마지막 단계는 생성된 솔리드를 모형 공간으로 출력하는 것입니다. 또한 눈에 잘 띄도록 색상도 지정할 것입니다.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_SolidsToC3D.png" alt=""><figcaption><p>Civil 3D로 솔리드 출력</p></figcaption></figure>

### 결과

다음은 **Dynamo 플레이어** 를 사용하여 그래프를 실행하는 예입니다.

<figure><img src="../../../.gitbook/assets/Rail_ClearanceEnvelope_Player.gif" alt=""><figcaption><p>Dynamo 플레이어를 사용하여 그래프를 실행하고 Civil 3D에서 결과 확인</p></figcaption></figure>

{% hint style="info" %}
 Dynamo 플레이어를 처음 사용하는 경우 [dynamo-player.md](../../dynamo-player.md "mention") 섹션을 참조하십시오. 
{% endhint %} 

> :tada: 작업을 완료했습니다!

## 아이디어

다음은 이 그래프의 기능을 확장하는 방법에 대한 몇 가지 아이디어입니다.

{% hint style="info" %}
 각 트랙에 대해 **다른 측점 범위** 를 별도로 사용할 수 있는 기능을 추가합니다. 
{% endhint %} 

{% hint style="info" %}
 충돌을 개별적으로 분석할 수 있도록 **솔리드를 더 작은 세그먼트로 분할** 합니다. 
{% endhint %} 

{% hint style="info" %}
 엔벨로프 솔리드가 **피쳐와 교차** 하는 확인하고 충돌하는 부분에 색상을 지정합니다. 
{% endhint %} 
