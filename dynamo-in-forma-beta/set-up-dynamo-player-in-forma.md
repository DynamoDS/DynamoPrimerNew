# 在 Forma 中设置 Dynamo Player

要将 Dynamo 与 Forma 结合使用，有两个选项：基于云的 Dynamo 即服务 (DaaS) 或桌面 Dynamo。每个选项都有其好处，具体取决于您要做什么，因此在开始之前，请考虑哪个选项最适合您的需求。但是，请记住，您可以随时在这些选项之间切换。


**Dynamo 即服务与 Dynamo Desktop 比较**

<table><thead><tr><th>Dynamo 即服务</th><th>桌面</th><th data-hidden></th></tr></thead><tbody><tr><td>易于设置</td><td>多步骤安装过程</td><td></td></tr><tr><td>无需安装或打开 Dynamo</td><td>必须打开 Dynamo</td><td></td></tr><tr><td>无法识别已在 Dynamo 中打开的图形</td><td>在 Player 中打开一个在 Dynamo 中打开的图形，只需单击一个按钮即可</td><td></td></tr><tr><td>无法使用 Python</td><td>可以使用 Python</td><td></td></tr><tr><td>只能使用批准的软件包</td><td>可以使用任何软件包</td><td></td></tr></tbody></table>

在此页面上，我们将解释如何设置这两个选项。

### 设置 Dynamo 即服务

Forma Beta 版中的 Dynamo 当前作为早期访问开放 Beta 版提供，这意味着功能和用户界面可能会经常更改。

首先，我们在 Forma 中安装 Dynamo Player。

1. 在 Forma 站点中，转到左侧侧栏中的 **“Extensions”** 然后单击 **“Add extension”**。这将打开 Autodesk App Store。
2. 搜索 Dynamo，然后添加 Dynamo Player Beta 版。阅读免责声明，然后单击 **“Agree”**。

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. 现在，Dynamo Player 在扩展中可用。单击它以将其打开。
4. 现在，可以使用 Dynamo Player了！

### 设置 Dynamo Desktop

要使用 Dynamo Desktop，您将需要 Dynamo，它可以作为独立的沙盒，也可以连接到 Revit 或 Civil 3D。您还需要 DynamoFormaBeta 软件包。

#### Revit

请按照以下说明在 Revit 和 DynamoFormaBeta 软件包中设置 Dynamo。

1. 确保已安装 Revit 2024.1 或更高版本。
2. 转到“管理 > Dynamo”，从 Revit 打开 Dynamo。
3. 在 Dynamo 中，安装 DynamoFormaBeta 软件包。转到“软件包 > 软件包管理器”，然后搜索“DynamoFormaBeta”。
   1. 如果已安装 Revit 2024，请安装 DynamoFormaBeta for 2.x 软件包。
   2. 如果使用的是 Revit 2025，请安装 DynamoFormaBeta 软件包。

#### Civil 3D

请按照以下说明在 Civil 3D 和 DynamoFormaBeta 软件包中设置 Dynamo。

1. 确保已安装 Civil 3D 2024.1 或更高版本。
2. 转到“管理 > Dynamo”，从 Civil 3D 打开 Dynamo。
3. 在 Dynamo 中，安装 DynamoFormaBeta 软件包。转到“软件包 > 软件包管理器”，然后搜索“DynamoFormaBeta”。
   1. 如果您安装了 Civil 3D 2024，请安装 DynamoFormaBeta for 2.x 软件包。
   2. 如果您安装了 Civil 3D 2025，请安装 DynamoFormaBeta 软件包。

#### Dynamo Sandbox

请按照以下说明安装 Dynamo Sandbox 和 DynamoFormaBeta 软件包。

1. 从 [Dynamo 内部版本](https://dynamobuilds.com/)下载 Dynamo 2.18.0 或更高版本。为了获得最佳体验，请选择顶部列出的最稳定的最新版本。
   1. “每日”版本是开发版本，可能包含不完整或正在进行的功能。
2. 使用 [7zip](https://sparanoid.com/lab/7z/) 将 Dynamo 提取到您选择的文件夹。
3. 从 Dynamo 安装文件夹运行 DynamoSandbox.exe。
4. 在 Dynamo 中，安装 DynamoFormaBeta 软件包。转到“软件包 > 软件包管理器”，然后搜索“DynamoFormaBeta”。
   1. 如果您有 Dynamo 2.x，请安装 DynamoFormaBeta for 2.x 软件包。
   2. 如果您有 Dynamo 3.x，请安装 DynamoFormaBeta 软件包。

安装 Dynamo 后，即可将其与 Forma 一起使用。在 Forma 中运行 Dynamo 的桌面选项时，需要打开 Dynamo 才能使用 Dynamo Player 扩展。

#### 在 Forma 中访问 Dynamo Desktop

首先，我们在 Forma 中安装 Dynamo Player。

1. 在 Forma 站点中，转到左侧侧栏中的 **“Extensions”** 然后单击 **“Add extension”**。这将打开 Autodesk App Store。
2. 搜索 Dynamo，然后添加 Dynamo Player Beta 版。阅读免责声明，然后单击 **“Agree”**。

<figure><img src="../.gitbook/assets/install-player.png" alt=""><figcaption></figcaption></figure>

3. 现在，Dynamo Player 在扩展中可用。单击它以将其打开。
4. 在顶部附近，单击“Desktop”以访问 Dynamo Desktop。

<figure><img src="../.gitbook/assets/dynamo-desktop.png" alt=""><figcaption></figcaption></figure>

5. 现在，可以使用 Dynamo Player了！如果已在 Dynamo 中打开一个图形，只需单击 **“Connected graph”** 下的“Open”，即可在 Player 中查看该图形。
