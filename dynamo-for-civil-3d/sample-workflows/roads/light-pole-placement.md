# 등주 배치

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption></figcaption></figure>

Dynamo의 많은 뛰어난 사용 사례 중 하나는 코리더 모형을 따라 개별 객체를 동적으로 배치하는 것입니다. 코리더를 따라 삽입된 조립품과 독립적인 위치에 객체를 배치해야 하는 경우가 자주 있는데, 이는 수동으로 수행하기에는 매우 지루한 작업입니다. 또한 코리더의 수평 또는 수직 형상이 변경되면 상당한 양의 재작업이 필요합니다.

## 목표

> :dart: 코리더를 따라 Excel 파일에 지정된 측점 값에 등주 블록 참조를 배치합니다. 

## 주요 개념

> * 외부 파일에서 데이터 읽기(이 경우 Excel)
> * 사전에서 데이터 구성
> * 좌표계를 사용하여 위치/축척/회전 제어
> * 블록 참조 배치
> * Dynamo에서 형상 시각화

## 버전 호환성

{% hint style="success" %} 이 그래프는 **Civil 3D 2020** 이상 버전에서 실행됩니다. 
{% endhint %} 

## 데이터세트

먼저 아래의 샘플 파일을 다운로드한 다음 DWG 파일과 Dynamo 그래프를 엽니다.

{% hint style="info" %}
 Excel 파일은 Dynamo 그래프와 동일한 디렉토리에 저장하는 것이 가장 좋습니다. 
{% endhint %} 

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dyn" %}

{% file src="../../../.gitbook/assets/Roads_CorridorBlockRefs.dwg" %}

{% file src="../../../.gitbook/assets/LightPoles.xlsx" %}

## 해결 방법

이 그래프의 논리에 대한 개요는 다음과 같습니다.

> 1. Excel 파일을 읽고 Dynamo로 데이터 가져오기
> 2. 지정된 코리더 기준선에서 형상선 가져오기
> 3. 원하는 측점에서 코리더 형상선을 따라 좌표계 생성
> 4. 좌표계를 사용하여 모형 공간에 블록 참조 배치

그럼 시작하겠습니다!

### Excel 데이터 가져오기

이 예제 그래프에서는 Excel 파일을 사용하여 Dynamo가 등주 블록 참조를 배치하는 데 사용할 데이터를 저장하겠습니다. 테이블의 모습은 다음과 같습니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_ExcelFile.png" alt=""><figcaption><p>Excel 파일 테이블 구조</p></figcaption></figure>

{% hint style="info" %}
 외부 파일(예: Excel 파일)에서 데이터를 읽을 때 Dynamo를 사용하는 것은 훌륭한 전략이며, 특히 데이터를 다른 팀 구성원과 공유해야 하는 경우 더 그렇습니다. 
{% endhint %} 

Dynamo로 가져온 Excel 데이터는 다음과 같습니다. 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetExcelData (1).png" alt="" width="548"><figcaption><p>Excel 데이터를 Dynamo로 가져오기</p></figcaption></figure>

이제 데이터가 있으므로 그래프의 나머지 부분에서 사용할 수 있도록 열(_코리더_, _기준선_, _PointCode_ 등)별로 데이터를 분할해야 합니다. 이 작업을 수행하는 일반적인 방법은 **List.GetItemAtIndex** 노드를 사용하고 원하는 각 열의 색인 번호를 지정하는 것입니다. 예를 들어, _코리더_ 열은 색인 0에 있고, _기준선_ 열은 색인 1에 있습니다.

괜찮아 보이시죠? 하지만 이 접근 방식에는 잠재적인 문제가 있습니다. 나중에 Excel 파일의 열 순서가 변경되면 어떻게 됩니까? 아니면 두 열 사이에 새 열이 추가됩니까? 그러면 그래프가 제대로 작동하지 않기 때문에 업데이트해야 합니다. Excel 열 헤더를 _키_ 로, 나머지 데이터를 _값_ 으로 사용하여 데이터를 **사전** 에 넣으면 추후에도 그래프를 사용할 수 있습니다.

{% hint style="info" %}
 사전을 처음 사용하는 경우 [5-5_dictionaries-in-dynamo](../../../5\_essential\_nodes\_and\_concepts/5-5\_dictionaries-in-dynamo/ "mention") 섹션을 참조하십시오. 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dictionary.png" alt=""><figcaption><p>사전에 Excel 데이터 넣기</p></figcaption></figure>

이렇게 하면 Excel에서 열의 순서를 유연하게 변경할 수 있으므로 그래프의 탄력성이 향상됩니다. 열 헤더가 동일하게 유지되는 한, 해당 _키_ (즉, 열 헤더)를 사용하여 사전에서 데이터를 간단히 검색할 수 있으며, 이것이 우리가 다음에 수행할 작업입니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_DictionaryRetrieval.png" alt=""><figcaption><p>사전에서 데이터 검색</p></figcaption></figure>

### 코리더 형상선 가져오기

이제 Excel 데이터를 가져와서 사용할 준비가 되었으므로 해당 데이터를 사용하여 Civil 3D에서 코리더 모형에 대한 몇 가지 정보를 가져와 보겠습니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCorridorFeatureLines.png" alt=""><figcaption></figcaption></figure>

> 1. 이름으로 코리더 모형을 선택합니다.
> 2. 코리더 내에서 특정 기준선을 가져옵니다.
> 3. 기준선 내의 형상선을 점 코드로 가져옵니다.

### 좌표계 생성

이제 Excel 파일에서 지정한 측점 값으로 코리더 형상선을 따라 **좌표계** 를 생성하겠습니다. 이러한 좌표계는 등주 블록 참조의 위치, 회전 및 축척을 정의하는 데 사용됩니다.

{% hint style="info" %}
 좌표계를 처음 사용하는 경우 [2-vectors.md](../../../5\_essential\_nodes\_and\_concepts/5-2\_geometry-for-computational-design/2-vectors.md "mention") 섹션을 참조하십시오. 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetCoordinateSystems (1).png" alt=""><figcaption><p>코리더 형상선을 따라 좌표계 가져오기</p></figcaption></figure>

여기에서는 Code Block을 사용하여 좌표계가 기준선의 어느 쪽에 있는지에 따라 좌표계를 회전합니다. 여러 노드의 시퀀스를 사용하여 이를 달성할 수도 있지만, 이것은 그냥 작성하는 것이 더 쉬운 상황임을 보여주는 좋은 예입니다.

{% hint style="info" %}
 Code Block을 처음 사용하는 경우 [8-1_code-blocks-and-design-script](../../../8\_coding\_in\_dynamo/8-1\_code-blocks-and-design-script/ "mention") 섹션을 참조하십시오. 
{% endhint %} 

### 블록 참조 작성

거의 다 왔습니다! 블록 참조를 실제로 배치하는 데 필요한 모든 정보를 확보했습니다. 가장 먼저 해야 할 일은 Excel 파일의 _BlockName_ 열을 사용하여 원하는 블록 정의를 가져오는 것입니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_GetBlockDefinitions.png" alt=""><figcaption><p>문서에서 원하는 블록 정의 가져오기</p></figcaption></figure>

여기에서 마지막 단계는 블록 참조를 작성하는 것입니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_CreateBlockReferences.png" alt=""><figcaption><p>모형 공간에서 블록 참조 작성</p></figcaption></figure>

### 결과

그래프를 실행하면 코리더를 따라 모형 공간에 새 블록 참조가 표시되는 것을 볼 수 있습니다. 그래프의 실행 모드를 자동으로 설정하고 Excel 파일을 편집하면 블록 참조가 자동으로 업데이트됩니다. 아주 멋진 기능입니다.

{% hint style="info" %}
 [3_user_interface](../../../3\_user\_interface/ "mention") 섹션에서 그래프 실행 모드에 대한 자세한 내용을 확인할 수 있습니다. 
{% endhint %} 

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Excel.gif" alt=""><figcaption><p>Excel 파일을 업데이트하고 Civil 3D에서 빠르게 결과 확인</p></figcaption></figure>

다음은 **Dynamo 플레이어** 를 사용하여 그래프를 실행하는 예입니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Player (1).gif" alt=""><figcaption><p>Dynamo 플레이어를 사용하여 그래프를 실행하고 Civil 3D에서 결과 확인</p></figcaption></figure>

{% hint style="info" %}
 Dynamo 플레이어를 처음 사용하는 경우 [dynamo-player.md](../../dynamo-player.md "mention") 섹션을 참조하십시오. 
{% endhint %} 

> :tada: 작업을 완료했습니다!

### 보너스: Dynamo에서 시각화

Dynamo에서 코리더 형상을 시각화하여 컨텍스트를 제공하면 도움이 될 수 있습니다. 이 특정 모형에는 모형 공간에서 이미 코리더 솔리드가 추출되어 있으므로 해당 솔리드를 Dynamo로 가져오겠습니다. 

하지만 한 가지 더 고려해야 할 사항이 있습니다. 솔리드는 상대적으로 "무거운" 형상 유형이므로, 이 작업을 수행하면 그래프의 속도가 느려집니다. 솔리드를 볼지 여부를 간단하게 _선택_ 할 수 이는 방법이 있다면 좋을 것입니다. 분명한 해답은 **Corridor.GetSolids** 노드의 연결을 해제하는 것이지만, 이렇게 하면 모든 다운스트림 노드에 경고가 발생하여 약간 지저분해집니다. 이 상황에서는 **ScopeIf** 노드가 정말 유용합니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_VisualizeCorridor (1).png" alt=""><figcaption></figcaption></figure>

> 1. **Object.Geometry** 노드의 맨 아래에는 회색 막대가 있습니다. 즉, 노드 미리보기가 꺼져 있으므로(노드를 마우스 오른쪽 버튼으로 클릭하여 액세스 가능) **GeometryColor.ByGeometryColor** 가 배경 미리보기에서 다른 형상과 화면표시 우선순위를 두고 "경쟁"하는 것을 피할 수 있습니다.
> 2. **ScopeIf** 노드를 사용하면 기본적으로 노드의 전체 분기를 선택적으로 실행할 수 있습니다. _test_ 입력이 false이면 **ScopeIf** 노드에 연결된 모든 노드가 실행되지 않습니다.

Dynamo 배경 미리보기의 결과는 다음과 같습니다.

<figure><img src="../../../.gitbook/assets/Roads_CorridorBlockRefs_Dynamo.png" alt=""><figcaption><p>Dynamo에서 코리더 형상 시각화</p></figcaption></figure>

## 아이디어

다음은 이 그래프의 기능을 확장하는 방법에 대한 몇 가지 아이디어입니다.

{% hint style="info" %}
 Excel 파일에 **회전** 열을 추가하고 이를 사용하여 좌표계의 회전을 구동합니다. 
{% endhint %} 

{% hint style="info" %}
 필요한 경우 등주가 코리더 형상선에서 벗어날 수 있도록 Excel 파일에 **수평 또는 수직 간격띄우기** 를 추가합니다. 
{% endhint %} 

{% hint style="info" %}
 측점 값이 있는 Excel 파일을 사용하는 대신, 시작 측점 및 일반적인 간격을 사용하여 **Dynamo에서 직접** 측점 값을 생성합니다. 
{% endhint %} 
