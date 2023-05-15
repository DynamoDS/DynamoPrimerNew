# Zero-Touch 사례 연구 - 그리드 노드

Visual Studio 프로젝트를 실행한 상태에서 직사각형의 셀 그리드를 만드는 사용자 지정 노드를 빌드하는 방법을 살펴봅니다. 여러 표준 노드를 사용하여 이 노드를 생성할 수 있지만, 이 노드는 Zero-Touch 노드에 쉽게 포함할 수 있는 유용한 도구입니다. 그리드 선과 달리 셀은 중심점을 기준으로 크기를 조정하거나, 모서리 정점을 조회하거나, 면에 내장될 수 있습니다.

이 예에서는 Zero-Touch 노드를 생성할 때 주의해야 할 몇 가지 기능 및 개념에 대해 다룹니다. 사용자 지정 노드를 빌드하여 Dynamo에 추가한 후에는 ‘Zero-Touch로 한 단계 더 나아가기’ 페이지를 검토하여 기본 입력 값, 여러 값 반환, 문서, 객체, Dynamo 형상 유형 사용 및 마이그레이션에 대해 자세히 살펴보십시오.

![직사각형 그리드 그래프](images/cover-image.jpg)

#### 사용자 지정 직사각형 그리드 노드 <a href="#custom-rectangular-grid-node" id="custom-rectangular-grid-node"></a>

그리드 노드 빌드를 시작하려면 새 Visual Studio 클래스 라이브러리 프로젝트를 생성합니다. 프로젝트를 설정하는 방법에 대한 자세한 설명은 시작하기 페이지를 참조하십시오.

![Visual Studio에서 새 프로젝트 생성하기](images/vs-new-project-1.jpg)

![Visual Studio에서 새 프로젝트 구성하기](images/vs-new-project-2.jpg)

> 1. 프로젝트 유형으로 `Class Library`를 선택합니다.
> 2. 프로젝트의 이름을 `CustomNodes`로 지정합니다.

형상을 작성하고 있으므로 적절한 NuGet 패키지를 참조해야 합니다. Nuget Package Manager에서 ZeroTouchLibrary 패키지를 설치합니다. 이 패키지는 `using Autodesk.DesignScript.Geometry;` 문에 필요합니다.

![ZeroTouchLibrary 패키지](images/vs-nugetpackage.jpg)

> 1. ZeroTouchLibrary 패키지를 찾아봅니다.
> 2. Dynamo Studio의 현재 빌드인 1.3에서 이 노드를 사용할 것입니다. 이 버전과 일치하는 패키지 버전을 선택합니다.
> 3. 클래스 파일의 이름도 `Grids.cs`로 변경했습니다.

다음으로, RectangularGrid 메서드가 위치할 네임스페이스 및 클래스를 설정해야 합니다. Dynamo에서 노드의 이름은 메서드 및 클래스 이름에 따라 지정됩니다. 아직 Visual Studio로 복사하지 않아도 됩니다.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
        //The method for creating a rectangular grid will live in here
        }
    }
}
```

> `Autodesk.DesignScript.Geometry;`는 ZeroTouchLibrary 패키지의 ProtoGeometry.dll을 참조하며 `System.Collections.Generic`은 리스트를 작성하는 데 필요합니다.

이제 직사각형을 그리는 메서드를 추가할 수 있습니다. 클래스 파일은 다음과 같아야 하며 Visual Studio로 복사할 수 있습니다.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        public static List<Rectangle> RectangularGrid(int xCount, int yCount)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                }
            }
            return pList;
        }
    }
}
```

프로젝트가 이와 비슷하면 `.dll`을 빌드해 봅니다.

![DLL 빌드하기](images/vs-grids.jpg)

> 1. 빌드 > 솔루션 빌드를 선택합니다.

프로젝트의 `bin` 폴더에 `.dll`이 있는지 확인합니다. 빌드가 성공하면 Dynamo에 `.dll`을 추가할 수 있습니다.

![Dynamo의 사용자 지정 노드](images/RectangularGrid-Dynamo.jpg)

> 1. Dynamo 라이브러리의 사용자 지정 RectangularGrids 노드
> 2. 캔버스의 사용자 지정 노드
> 3. Dynamo에 `.dll`을 추가하기 위한 추가 버튼

#### 사용자 지정 노드 수정 <a href="#custom-node-modifications" id="custom-node-modifications"></a>

위의 예에서는 `RectangularGrids` 메서드 외에 다른 것을 정의하지 않은 매우 간단한 노드를 만들었습니다. 그러나 입력 포트에 대한 툴팁을 작성하거나 표준 Dynamo 노드처럼 노드에 요약을 제공할 수 있습니다. 이러한 기능을 사용자 지정 노드에 추가하면 특히 사용자가 라이브러리에서 기능을 검색하려는 경우 더 쉽게 사용할 수 있습니다.

![입력 툴팁](images/nodemodification.png)

> 1. 기본 입력 값
> 2. xCount 입력에 대한 툴팁

RectangularGrid 노드에는 다음과 같은 기본 기능이 필요합니다. 아래 코드에서 입력 및 출력 포트 설명, 요약 및 기본 입력 값을 추가했습니다.

```
using Autodesk.DesignScript.Geometry;
using System.Collections.Generic;

namespace CustomNodes
{
    public class Grids
    {
        /// <summary>
        /// This method creates a rectangular grid from an X and Y count.
        /// </summary>
        /// <param name="xCount">Number of grid cells in the X direction</param>
        /// <param name="yCount">Number of grid cells in the Y direction</param>
        /// <returns>A list of rectangles</returns>
        /// <search>grid, rectangle</search>
        public static List<Rectangle> RectangularGrid(int xCount = 10, int yCount = 10)
        {
            double x = 0;
            double y = 0;

            var pList = new List<Rectangle>();

            for (int i = 0; i < xCount; i++)
            {
                y++;
                x = 0;
                for (int j = 0; j < yCount; j++)
                {
                    x++;
                    Point pt = Point.ByCoordinates(x, y);
                    Vector vec = Vector.ZAxis();
                    Plane bP = Plane.ByOriginNormal(pt, vec);
                    Rectangle rect = Rectangle.ByWidthLength(bP, 1, 1);
                    pList.Add(rect);
                    Point cPt = rect.Center();
                }
            }
            return pList;
        }
    }
}
```

* `RectangularGrid(int xCount = 10, int yCount = 10)`와 같이 메서드 매개변수에 값을 할당하여 입력 기본값을 지정합니다.
* 앞에 `///`가 오는 XML 문서로 입력 및 출력 툴팁, 검색 키워드 및 요약을 작성합니다.

툴팁을 추가하려면 프로젝트 디렉토리에 xml 파일이 있어야 합니다. 이 옵션을 사용하면 Visual Studio에서 `.xml`을 자동으로 생성할 수 있습니다.

![XML 문서를 사용하도록 설정하기](images/vs-xml.jpg)

> 1. 여기에서 XML 문서 파일을 사용하도록 설정하고 파일 경로를 지정합니다. 그러면 XML 파일이 생성됩니다.

모두 끝났습니다. 여러 표준 기능을 사용하여 새 노드를 만들었습니다. 다음 장에서는 Zero-Touch 노드 개발 및 알아야 할 이슈에 대해 자세히 설명합니다.
