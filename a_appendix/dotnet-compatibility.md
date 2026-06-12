# .NET 호환성

아래 표에는 각 주요 Dynamo 릴리즈 시리즈가 대상으로 하는 .NET 버전이 나와 있습니다. 프로젝트가 호환되는 .NET 버전을 대상으로 해야 하므로 이는 패키지 또는 확장을 빌드할 때와 관련이 있습니다.

| Dynamo 버전 | .NET 버전       |
| -------------- | ------------------ |
| 0.9 – 2.0      | .NET Framework 4.5 |
| 2.1 – 2.6      | .NET Framework 4.7 |
| 2.7 – 2.19     | .NET Framework 4.8 |
| 3.0 – 3.6      | .NET 8             |
| 3.3.2          | .NET 10            |
| 3.7            | .NET 10            |
| 4.0+           | .NET 10            |

{% hint style="info" %} 3.3.2 및 3.7은 4.0 릴리스 후보에서 .NET 10이 백포트된 특별 릴리스입니다. {% endhint %}

패키지를 새 .NET 버전으로 업데이트하는 방법에 대한 지침은 개발자 입문서의 마이그레이션 가이드를 참조하십시오.

* [Dynamo 2.x용 패키지 업데이트](../11\_developer\_primer/3\_developing\_for\_dynamo/6-0-updating-your-packages-and-dynamo-libraries-for-dynamo-2x.md)
* [Dynamo 3.x/.NET 8용 패키지 업데이트](../11\_developer\_primer/3\_developing\_for\_dynamo/6-1-updating-your-packages-and-dynamo-libraries-for-dynamo-3x-Net8.md)
* [Dynamo 4.x/.NET 10용 패키지 업데이트](../11\_developer\_primer/3\_developing\_for\_dynamo/6-2-updating-your-packages-and-dynamo-libraries-for-dynamo-4x.md)
