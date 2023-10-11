# 시작하기

이제 큰 그림에 대해 조금 더 알게 되었으니, 바로 Civil 3D에서 첫 번째 Dynamo 그래프를 작성해 보겠습니다!

{% hint style="info" %}
 이 예는 기본적인 Dynamo 기능을 보여주기 위한 간단한 예입니다. 비어 있는 새 Civil 3D 문서에서 단계별로 작업하는 것이 좋습니다. 
{% endhint %}

## Dynamo 열기

가장 먼저 Civil 3D에서 빈 문서를 엽니다. 빈 문서에서 Civil 3D 리본의 **관리** 탭으로 이동하여 **시각적 프로그래밍** 패널을 찾습니다.

![](<../.gitbook/assets/image (7).png>)

**Dynamo** 버튼을 클릭합니다. 그러면 별도의 창에서 Dynamo가 실행합니다.

{% hint style="info" %}
 **Dynamo와 Dynamo 플레이어 간의 차이점은 무엇입니까?**

Dynamo는 그래프를 작성하고 실행하는 데 사용됩니다. Dynamo 플레이어는 Dynamo에서 열지 않고도 그래프를 실행할 수 있는 간편한 방법입니다.

사용할 준비가 되면 [dynamo-player.md](dynamo-player.md "mention") 섹션으로 이동합니다. 
{% endhint %}

## 새 그래프 시작

Dynamo가 열리면 시작 화면이 표시됩니다. **새로 만들기**를 클릭하여 빈 작업공간을 엽니다.

<figure><img src="../.gitbook/assets/c3d-start.png" alt=""><figcaption><p>Dynamo 시작 화면</p></figcaption></figure>

{% hint style="info" %}
 **샘플은 어떻습니까?**

Dynamo for Civil 3D에는 미리 작성된 몇 가지 그래프가 포함되어 있어 Dynamo를 사용하는 방법에 대한 더 많은 아이디어를 얻을 수 있습니다. 입문서의 [sample-workflows](sample-workflows/ "mention")와 함께, 언젠가는 이 내용을 살펴볼 것을 권장합니다. 
{% endhint %}

## 노드 추가

이제 빈 작업공간이 표시됩니다. Dynamo가 실제로 작동하는 모습을 살펴보겠습니다! 목표는 다음과 같습니다.

>  :dart: **문자를 모형 공간에 삽입할 Dynamo 그래프를 작성합니다.**

아주 간단합니다. 하지만 시작하기 전에 몇 가지 기본 사항을 검토해야 합니다.

Dynamo 그래프의 핵심 빌딩 블록을 **노드**라고 합니다. 노드는 작은 기계와 같아서 데이터를 입력하면 해당 데이터에 대해 몇 가지 작업을 수행한 후 결과를 출력합니다. Dynamo for Civil 3D에는 하나의 노드만으로는 할 수 없는 더 크고 더 나은 작업을 수행하는 **그래프**를 작성할 수 있는, **와이어**로 연결 가능한 노드 **라이브러리**가 있습니다.

{% hint style="info" %}
 **이전에 Dynamo를 사용해 본 적이 없다면 어떻게 해야 합니까?**

일부 내용은 매우 생소할 수 있지만, 괜찮습니다! 다음과 같은 섹션이 도움이 될 것입니다.

[3_user_interface](../3\_user\_interface/ "mention")\
 [4_nodes_and_wires](../4\_nodes\_and\_wires/ "mention")\
 [5_essential_nodes_and_concepts](../5\_essential\_nodes\_and\_concepts/ "mention") 
{% endhint %}

그럼, 그래프를 작성해 보겠습니다. 다음은 필요한 모든 노드의 리스트입니다.

<figure><img src="../.gitbook/assets/c3d-create-text-node-list.png" alt=""><figcaption></figcaption></figure>

라이브러리의 검색 막대에 해당 이름을 입력하거나 캔버스에서 아무 곳이나 마우스 오른쪽 버튼으로 클릭하고 검색하면 이러한 노드를 찾을 수 있습니다.

<figure><img src="../.gitbook/assets/c3d-create-text-node-placement.gif" alt=""><figcaption><p>라이브러리에서 노드를 배치하거나 캔버스에서 마우스 오른쪽 버튼을 클릭하여 노드를 배치할 수 있습니다</p></figcaption></figure>

{% hint style="info" %}
 **어떤 노드를 사용해야 하는지, 그러한 노드를 어디에서 찾을 수 있는지 어떻게 알 수 있습니까?**

라이브러리의 노드는 노드의 기능에 따라 논리적 카테고리로 그룹화됩니다. 더 자세한 내용을 확인하려면 [node-library.md](node-library.md "mention") 섹션을 참조하십시오. 
{% endhint %}

최종 그래프의 모습은 다음과 같습니다.

<figure><img src="../.gitbook/assets/c3d-text-create-final (2).png" alt=""><figcaption><p>완성된 그래프</p></figcaption></figure>

지금까지 수행한 작업을 요약해 보겠습니다.

> 1. 작업할 문서를 선택했습니다. 이 경우(그리고 많은 경우) Civil 3D의 활성 문서에서 작업합니다.
> 2. 문자 객체를 작성해야 하는 대상 블록(이 경우에는 모형 공간)을 정의했습니다.
> 3. _String_ 노드를 사용하여 문자를 배치할 도면층을 지정했습니다.
> 4. 문자가 배치될 위치를 정의하기 위해 _Point.ByCoordinates_ 노드를 사용하여 점을 작성했습니다.
> 5. 두 개의 _Number Slider_ 노드를 사용하여 문자 삽입점의 X 및 Y 좌표를 정의했습니다.
> 6. 또 다른 _String_ 노드를 사용하여 문자 객체의 컨텐츠를 정의했습니다.
> 7. 마지막으로 문자 객체를 작성했습니다.

멋지게 작성된 새 그래프의 결과를 확인해 보겠습니다!

## 결과 보기

Civil 3D로 돌아가서 **모형** 탭이 선택되어 있는지 확인합니다. Dynamo에서 작성한 새 문자 객체가 표시됩니다.

{% hint style="info" %}
 문자가 표시되지 않으면 ZOOM -> EXTENTS 명령을 실행하여 오른쪽 스폿으로 줌해야 할 수 있습니다. 
{% endhint %}

<figure><img src="../.gitbook/assets/c3d-create-text-result.png" alt="" width="413"><figcaption></figcaption></figure>

좋습니다! 이제 문자를 약간 업데이트해 보겠습니다.

Dynamo 그래프로 돌아가서 문자열, 삽입점 좌표 등 몇 가지 입력 값을 변경합니다. Civil 3D에서 문자가 자동으로 업데이트되는 것을 볼 수 있습니다. 또한 입력 포트 중 하나를 분리하면 문자가 제거됩니다. 모든 항목을 다시 연결하면 문자가 다시 생성됩니다. 

<div data-full-width="false">

<figure><img src="../.gitbook/assets/c3d-create-text.gif" alt=""><figcaption><p>완성된 그래프의 실제 모습</p></figcaption></figure>

</div>

{% hint style="info" %}
 **그래프가 실행될 때마다 Dynamo가 새 문자 객체를 삽입하지 않는 이유는 무엇입니까?**

기본적으로 Dynamo는 작성하는 객체를 "기억"합니다. 노드 입력 값을 변경하면 완전히 새로운 객체를 작성하는 대신 Civil 3D의 객체가 업데이트됩니다. 이 동작에 대한 자세한 내용은 [object-binding.md](advanced-topics/object-binding.md "mention") 섹션을 참조하십시오. 
{% endhint %}

> :tada: 작업을 완료했습니다!

## 다음 단계

이 예시는 Dynamo for Civil 3D로 수행할 수 있는 작업의 일부에 불과합니다. 자세히 알아보려면 계속 읽어 보십시오!
