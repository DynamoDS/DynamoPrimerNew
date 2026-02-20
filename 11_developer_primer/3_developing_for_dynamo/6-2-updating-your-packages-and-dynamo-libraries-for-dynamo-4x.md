# Dynamo 4.x용 패키지 및 Dynamo 라이브러리 업데이트하기

### 소개 <a href="#introduction" id="introduction"></a>

이 섹션에는 그래프, 패키지 및 라이브러리를 Dynamo 4.x로 마이그레이션할 때 발생할 수 있는 문제에 대한 정보가 포함되어 있습니다. Dynamo 4.0에는 다음과 같은 변경 사항이 포함되어 있습니다.
* 대폭적인 성능 개선
* 안정성 향상 및 버그 수정 업데이트
* 코드베이스 현대화
* 1.x에서 사용 중단으로 표시되었던 API 제거
* .NET 8에서 .NET 10으로의 대규모 런타임 업데이트
* 새로 생성되는 모든 Python 노드의 기본 Python 엔진이 이제 PythonNet3로 변경됨

NET 10으로의 마이그레이션은 2026년 11월 .NET 8 지원 종료에 앞서, Dynamo가*Microsoft의 기술 로드맵과 지속적으로 정렬될 수 있도록 보장합니다.

Dynamo 4.0을 실행하면, 아직 업데이트하지 않은 경우 .NET 10으로 업데이트하라는 메시지가 표시됩니다. 패키지 작성자는 완전한 호환성을 보장하기 위해 프로젝트의 대상 프레임워크를 .NET 10으로 업데이트해야 합니다.

Dynamo 4.0 이상에서 작성된 모든 새 Python 노드는 기본적으로 PythonNet3를 사용합니다. 이전 버전과의 호환성에 대해 걱정할 필요는 없습니다. 여러 버전을 병행 사용하는 환경(예: Revit 또는 Civil 3D 2025/2026)에서는 Dynamo 3.3~3.6에 PythonNet3 Engine 패키지를 설치하면 호환성을 유지할 수 있습니다. 자세한 내용은 [여기](https://dynamobim.org/dynamo-core-4-0-release/)에서 확인할 수 있습니다.

또한 1.x에서 사용 중단으로 표시되었던 API와 노드는 Dynamo 4.0에서 제거되었습니다. [여기에서 변경 사항의 전체 목록](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0)을 참조할 수 있습니다.

### 패키지 호환성 <a href="#package-compatibility" id="package-compatibility"></a>

#### Dynamo 4.x에서 Dynamo 2.x 및 3.x 패키지 사용 
Dynamo 4.x는 이제 .NET 10 런타임에서 실행되기 때문에 Dynamo 4.x에서 Dynamo 2.x(*.NET48 사용*) 및 Dynamo 3.x(*.NET 8 사용*)용으로 빌드된 패키지의 작동이 보장되지 않습니다. Dynamo 4.0 미만 버전에서 게시된 패키지를 Dynamo 4.x에서 다운로드하려고 하면 해당 패키지가 이전 버전의 Dynamo에서 제공되었다는 경고가 표시됩니다.

**이것은 패키지가 동작하지 않는다는 의미가 아닙니다.** 단순히 호환성 문제가 발생할 수 있다는 경고이며, 일반적으로 특별히 Dynamo 4.x용으로 빌드된 최신 버전이 있는지 확인하는 것이 좋습니다.

패키지 로드 시 Dynamo 로그 파일에 이러한 유형의 경고가 표시될 수도 있습니다. 모든 것이 제대로 작동하는 경우 이 경고를 무시하면 됩니다.

#### Dynamo 2.x에서 Dynamo 4.x 패키지 사용 

Dynamo 4.x(*.Net 10 사용*)용으로 빌드된 패키지가 Dynamo 2.x에서 동작할 가능성은 매우 낮습니다. Dynamo 2.x에서 Dynamo 4.x용으로 빌드된 패키지를 설치하려고 할 때도 아래 경고가 표시됩니다.

![패키지 호환성 경고](images/6-2-packages-new-version-compatibility-warning.png)


#### Dynamo 3.x에서 Dynamo 4.x 패키지 사용 

Dynamo 4.x(*.NET 10 사용*)용으로 빌드된 패키지는 패키지에서 사용하는 모든 API가 .NET 8에 존재하는 경우에 한해 Dynamo 3.x에서 동작할 수도 있습니다. 그러나 반드시 동작할 것이라는 보장은 없습니다. Dynamo 3.x에서 Dynamo 4.x용으로 빌드된 패키지를 설치하려고 할 때도 아래 경고가 표시됩니다.

![패키지 호환성 경고](images/6-2-packages-compatibility-warning.png)

#### 패키지 작성자를 위한 모범 사례 
모범 사례는 .csproj를 수정하여 프로젝트를 .NET 8과 .NET 10을 모두 대상으로 멀티 타기팅하는 것입니다.

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
이를 통해 다음을 보장할 수 있습니다.
* 여전히 .NET 8을 사용하는 Revit 호스팅 Dynamo 버전 지원
* .NET 10 기반의 독립 실행형 Dynamo 4.x와의 호환성