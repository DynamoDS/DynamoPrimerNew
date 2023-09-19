# 객체 바인딩

Dynamo for Civil 3D에는 각 노드에서 작성된 객체를 "기억"하는 매우 강력한 메커니즘이 포함되어 있습니다. 이 메커니즘을 **객체 바인딩**이라고 하며, 이를 통해 동일한 문서에서 실행될 때마다 Dynamo 그래프가 일관된 결과를 생성할 수 있습니다. 객체 바인딩은 많은 상황에서 사용하기에 매우 바람직하지만, Dynamo의 동작을 보다 잘 제어하고 싶은 다른 상황도 있을 수 있습니다. 이 섹션에서는 객체 바인딩의 작동 방식과 객체 바인딩을 활용하는 방법을 설명합니다.

## 예

현재 도면층의 모형 공간에 원을 작성하는 이 그래프를 살펴보겠습니다.

<figure><img src="../../.gitbook/assets/c3d-binding-create-circle.png" alt=""><figcaption><p>원을 작성하는 단순한 그래프</p></figcaption></figure>

반지름이 변경되면 어떻게 되는지 확인하십시오.

<figure><img src="../../.gitbook/assets/c3d-binding-change-radius.gif" alt=""><figcaption><p>Dynamo에서 반지름 입력 수정</p></figcaption></figure>

이것이 바로 객체 바인딩이 작동하는 방식입니다. Dynamo의 기본 동작은 반지름 입력이 변경될 때마다 새 원을 작성하는 것이 아니라 원의 반지름을 _수정_하는 것입니다. 이는 그래프가 실행될 때마다 **Object.ByGeometry** 노드가 이 _특정_ 원을 작성한 것을 "기억"하기 때문입니다. 또한 Dynamo는 이 정보를 저장하여 다음번에 Civil 3D 문서를 열고 그래프를 실행할 때 정확히 동일한 동작을 수행합니다.

## 또 다른 예

Dynamo의 기본 객체 바인딩 동작을 변경할 수 있는 예를 살펴보겠습니다. 문자를 원 가운데에 배치하는 그래프를 작성한다고 가정해 보겠습니다. 그러나 이 그래프의 의도는 반복해서 실행하고 선택한 원에 대해 매번 새로운 문자를 배치할 수 있다는 것입니다. 다음은 그래프의 모습입니다.

<figure><img src="../../.gitbook/assets/c3d-binding-create-text.png" alt=""><figcaption><p>선택한 원의 중앙에 문자를 배치하는 단순한 그래프</p></figcaption></figure>

그러나 실제로는 다른 원을 선택하면 이렇게 됩니다.

<figure><img src="../../.gitbook/assets/c3d-binding-select-circle.gif" alt=""><figcaption><p>새 원을 선택할 때 Dynamo의 기본 동작</p></figcaption></figure>

그래프를 실행할 때마다 문자가 삭제되고 다시 생성되는 것처럼 보입니다. 실제로는 선택하는 원에 따라 문자의 위치가 _수정_됩니다. 따라서 다른 위치에 있을 뿐 동일한 문자입니다! 매번 새로운 문자를 작성하려면 바인딩 데이터가 유지되지 않도록 Dynamo의 객체 바인딩 설정을 수정해야 합니다(아래의 [\#binding-settings](object-binding.md#binding-settings "mention") 참조).

<figure><img src="../../.gitbook/assets/Land_ServicePlacement_BindingSettings.png" alt=""><figcaption><p>객체 바인딩 설정</p></figcaption></figure>

이렇게 변경한 후에 원하는 동작을 얻게 됩니다.

<figure><img src="../../.gitbook/assets/c3d-binding-repeat-placement.gif" alt=""><figcaption><p>객체 바인딩을 사용하지 않는 동작</p></figcaption></figure>

## 바인딩 설정

Dynamo for Civil 3D에서는 **Dynamo** 메뉴의 **바인딩 데이터 저장소** 설정을 통해 기본 객체 바인딩 동작을 수정할 수 있습니다.

{% hint style="info" %} 바인딩 데이터 저장소 옵션은 **Civil 3D 2022.1** 이상 버전에서 사용할 수 있습니다. {% endhint %}

<figure><img src="../../.gitbook/assets/c3d-binding-settings (1).png" alt=""><figcaption></figcaption></figure>

모든 옵션은 기본적으로 사용하도록 설정되어 있습니다. 다음은 각 옵션의 기능에 대한 요약입니다.

### 옵션 1: 바인딩 데이터 유지 안 함

이 옵션을 사용하면 Dynamo는 그래프를 마지막으로 실행했을 때 작성한 객체를 "잊어버립니다". 따라서 그래프는 어떤 상황의 어떤 도면에서도 실행될 수 있으며 매번 새로운 개체를 작성합니다.

{% hint style="info" %} **사용해야 하는 경우**

Dynamo가 이전 실행에서 수행한 모든 작업을 "잊어버리고" 매번 새로운 객체를 작성하도록 하려면 이 옵션을 사용합니다. {% endhint %}

### 옵션 2: Dynamo 그래프에 저장

이 옵션을 사용하면 객체 바인딩 메타데이터가 저장될 때 그래프(.dyn 파일)로 직렬화됩니다. 그래프를 닫았다가 다시 열고 **동일한 도면**에서 실행하면 모든 것이 그래프를 닫았을 때와 동일하게 작동합니다. **다른 도면**에서 그래프를 실행하면 바인딩 데이터가 그래프에서 제거되고 새로운 객체가 작성됩니다. 즉, 원본 도면을 열고 그래프를 다시 실행하면 이전 객체와 함께 새로운 객체가 작성됩니다.

{% hint style="info" %} **사용해야 하는 경우**

이 옵션은 Dynamo가 **특정 도면**에서 마지막으로 실행될 때 작성한 객체를 "기억"하도록 하려는 경우에 사용합니다. {% endhint %}

{% hint style="warning" %} 이 옵션은 **특정 도면**과 Dynamo 그래프 간에 1:1 관계를 유지할 수 있는 상황에 가장 적합합니다. 옵션 1과 3은 여러 도면에서 실행되도록 설계된 그래프에 더 적합합니다. {% endhint %}

### 옵션 3: Dynamo 도면에 저장

이 옵션은 객체 바인딩 데이터가 그래프(.dyn 파일) 대신 도면에서 직렬화된다는 점을 제외하고 옵션 2와 유사합니다. 그래프를 닫았다가 다시 열고 **동일한 도면**에서 실행하면 모든 것이 그래프를 닫았을 때와 동일하게 작동합니다. **다른 도면**에서 그래프를 실행하는 경우 그래프가 아닌 도면에 저장되기 때문에 바인딩 데이터는 원래 도면에 그대로 유지됩니다.

{% hint style="info" %} **사용해야 하는 경우**

이 옵션은 **여러 도면**에서 동일한 그래프를 사용하고 Dynamo가 각 도면에서 수행한 작업을 "기억"하도록 하려는 경우에 사용합니다. {% endhint %}

### 옵션 4: Dynamo 플레이어 도면에 저장

이 옵션을 사용할 때 가장 먼저 주목해야 할 점은 기본 Dynamo 인터페이스를 통해 그래프를 실행할 때 그래프가 도면과 상호 작용하는 방식에 영향을 미치지 않는다는 것입니다. 이 옵션은 Dynamo 플레이어를 사용하여 그래프를 실행할 때_만_ 적용됩니다.

{% hint style="info" %} Dynamo 플레이어를 처음 사용하는 경우 [dynamo-player.md](../dynamo-player.md "mention") 섹션을 참조하십시오. {% endhint %}

기본 Dynamo 인터페이스를 사용하여 그래프를 실행한 다음 닫고 Dynamo 플레이어를 사용하여 동일한 그래프를 실행하면 이전에 작성한 객체 위에 새로운 객체가 작성됩니다. 그러나 Dynamo 플레이어에서 그래프가 한 번 실행되면 도면의 객체 바인딩 데이터가 직렬화됩니다. 따라서 Dynamo 플레이어를 통해 그래프를 여러 번 실행하면 새로운 객체가 작성되지 않고 객체가 업데이트됩니다. Dynamo 플레이어를 통해 **다른 도면**에서 그래프를 실행하면 바인딩 데이터는 그래프가 아닌 도면에 저장되기 때문에 원래 도면에 유지됩니다.

{% hint style="info" %} **사용해야 하는 경우**

여러 도면에서 Dynamo 플레이어를 사용하여 그래프를 실행하고 Dynamo 플레이어가 각 도면에서 수행한 작업을 "기억"하도록 하려는 경우에 이 옵션을 사용합니다. {% endhint %}
