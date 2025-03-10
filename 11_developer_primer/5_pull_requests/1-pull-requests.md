# 拉取请求

Dynamo 依赖于其社区的创造力和活跃度，Dynamo 团队会鼓励参与者探索各种可能性、测试创意以及加入社区以提供反馈。尽管鼓励创新，但仅当变更会使 Dynamo 更易于使用并满足本文档中定义的准则时，才会合并这些变更。将不会合并好处狭隘的变更。

#### 拉取请求要求 <a href="#pull-request-expectations" id="pull-request-expectations"></a>

Dynamo 团队要求拉取请求遵循以下准则：

* 遵循我们的[编码标准](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)和[节点命名标准](https://github.com/DynamoDS/Dynamo/wiki/Naming-Standards)
* 添加新功能时包括单位测试
* 修复错误时，添加将亮显如何终止当前行为的单位测试
* 使讨论集中于一个问题。如果出现新主题或相关主题，则创建新问题。

以及一些有关禁止行为的准则：

* 大量拉取请求让我们大吃一惊。反之，请提出问题并发起讨论，以便我们能够在您投入大量时间之前就方向达成共识。
* 提交您未写入的代码。如果您发现您认为非常适合添加到 Dynamo 的代码，请提出问题并发起讨论，然后再继续操作。
* 提交将更改许可相关文件或标头的 PR。如果您认为它们有问题，请提出问题，我们很乐意讨论。
* 生成 API 附加功能，而无需先提出问题并与我们讨论。

#### 填写拉取请求模板 <a href="#filling-out-the-pull-request-template" id="filling-out-the-pull-request-template"></a>

提交拉取请求时，请使用[默认 PR 模板](https://github.com/DynamoDS/Dynamo/blob/master/.github/PULL\_REQUEST\_TEMPLATE.md)。在提交您的 PR 之前，请确保清楚地描述了目的，并且所有声明均可断言为真：

* 代码库在此 PR 之后的状态更佳
* 根据[标准](https://github.com/DynamoDS/Dynamo/wiki/Coding-Standards)进行记录
* 此 PR 包含的测试级别是合适的
* 面向用户的字符串（如果有）将提取到 `*.resx` 文件中
* 所有测试都使用自助服务 CI 通过
* UI 更改的快照（如果有）
* 对 API 的更改遵循[语义版本控制](https://github.com/DynamoDS/Dynamo/wiki/Dynamo-Versions)，并记录在 [API 更改](https://github.com/DynamoDS/Dynamo/wiki/API-Changes)文档中。

Dynamo 团队将为您的拉取请求指定合适的审阅者。

#### 拉取请求审阅过程 <a href="#pull-request-review-process" id="pull-request-review-process"></a>

提交拉取请求后，您可能需要在审阅过程中继续参与。请注意以下审阅标准：

* Dynamo 团队每月召开一次会议，以审阅从最旧到最新的拉取请求。
* 如果审阅后的拉取请求需要所有者进行更改，则 PR 的所有者有 30 天的时间做出响应。如果 PR 在下一次会议之前未看到有任何活动，则它将由团队关闭，或者根据其效用由团队中的某人接管。
* 拉取请求应使用 Dynamo 的默认 PR 模板
* 将不会审阅未完全填写 Dynamo PR 模板且所有声明均已满足的拉取请求。

#### 精选 Dynamo Revit 提交 <a href="#cherry-picking-dynamo-revit-commits" id="cherry-picking-dynamo-revit-commits"></a>

由于市面上提供有多个版本的 Revit，因此您可能需要将所做更改精选给特定的 DynamoRevit 发行分支机构，以便其他版本的 Revit 可以拾取新功能。在审阅过程中，参与者会负责将其已审阅的提交精选给 Dynamo 团队指定的其他 DynamoRevit 分支机构。
