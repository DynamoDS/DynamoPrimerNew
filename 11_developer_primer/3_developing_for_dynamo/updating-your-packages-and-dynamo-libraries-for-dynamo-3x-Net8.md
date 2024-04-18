# Dynamo 3.x 및 NET8용 패키지 및 Dynamo 라이브러리 업데이트하기

### 소개 <a href="#introduction" id="introduction"></a>

이 섹션에는 그래프, 패키지 및 라이브러리를 Dynamo 3.x로 마이그레이션할 때 발생할 수 있는 문제에 대한 정보가 포함되어 있습니다.

Dynamo 3.0은 주 릴리즈이며 일부 API는 변경되거나 제거되었습니다. Dynamo 3.x의 개발자 또는 사용자가 영향을 받을 가능성이 높은 가장 큰 변화는 .NET8으로의 전환입니다.

.NET은 Dynamo 작성에 사용된 C# 언어의 기반이 되는 런타임입니다. Dynamo는 나머지 Autodesk 에코시스템과 더불어 이 런타임의 최신 버전으로 업데이트되었습니다.

[블로그 게시물](https://dynamobim.org/dynamo-on-net-8/)에서 자세한 내용을 확인할 수 있습니다.
***

### 패키지 호환성 <a href="#package-compatibility" id="package-compatibility"></a>

#### Dynamo 3.x에서 Dynamo 2.x 패키지 사용 
Dynamo 3.x는 이제 .NET8 런타임에서 실행되기 때문에 Dynamo 3.x에서 Dynamo 2.x (*.NET48 사용*) 용으로 작성된 패키지의 작동이 보장되지 않습니다. Dynamo 3.0 미만 버전에서 게시된 패키지를 Dynamo 3.x에서 다운로드하려고 하면 해당 패키지가 이전 버전의 Dynamo에서 제공되었다는 경고가 표시됩니다. 

**이것은 패키지가 작동하지 않는다는 의미가 아닙니다** 단순히 호환성 문제가 발생할 수 있다는 경고이며, 일반적으로 특별히 Dynamo 3.x용으로 작성된 최신 버전이 있는지 확인하는 것이 좋습니다.

패키지 로드 시 Dynamo 로그 파일에 이러한 유형의 경고가 표시될 수도 있습니다. 모든 것이 제대로 작동하는 경우 이 경고를 무시하면 됩니다.

#### Dynamo 2.x에서 Dynamo 3.x 패키지 사용 

Dynamo 3.x (*.Net8 사용*) 용으로 작성된 패키지가 Dynamo 2.x에서 작동할 가능성은 거의 없습니다. 또한 이전 버전을 사용하는 동안 최신 버전 Dynamo용으로 작성된 패키지를 다운로드할 때 경고가 표시됩니다.


