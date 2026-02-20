# Dynamo 4.x 用のパッケージと Dynamo ライブラリを更新する

### はじめに <a href="#introduction" id="introduction"></a>

このセクションでは、グラフ、パッケージ、ライブラリを Dynamo 4.x に移行する際、発生する可能性のある問題について説明します。Dynamo 4.0 では、次の機能が導入されています。
* パフォーマンスの大幅改善
* 安定性の向上とバグ修正のための更新
* codebase の最新化
* 1.x で廃止とマークされていた API の削除
* .NET 8 から .NET 10 への主要なランタイム更新
* PythonNet 3 がすべての新しい Python ノードの既定の Python エンジンになった

.NET 10 の移行作業により、2026 年 11 月の .NET 8 のサポート終了に先立ち、Dynamo が Microsoft のテクノロジー ロードマップとの整合性を維持することが保証されます。

Dynamo 4.0 を起動すると、.NET 10 に更新するように求められます(まだ更新していない場合)。パッケージ作成者は、完全な互換性を確保するために、.NET 10 をターゲットとするようにプロジェクトを更新する必要があります。

Dynamo 4.0+ で作成されたすべての新しい Python ノードは、PythonNet3 で開始されます。下位互換性について心配する必要はありません。マルチバージョンのショップ(Revit や Civil 3D の 2025/2026 など)で作業する場合は、互換性を維持するために Dynamo 3.3 ～ 3.6 に PythonNet3 Engine パッケージをインストールしてください。詳細については、[こちら](https://dynamobim.org/dynamo-core-4-0-release/)を参照してください。

1.x で廃止のマークが付けられていた API とノードは、Dynamo 4.0 で削除されました。変更の完全なリストは、[こちら](https://github.com/DynamoDS/Dynamo/wiki/API-Changes-in-Dynamo-4.0.0)で参照できます。

### パッケージの互換性 <a href="#package-compatibility" id="package-compatibility"></a>

#### Dynamo 2.x および 3.x で Dynamo 4.x パッケージを使用する 
Dynamo 4.x は .NET 10 ランタイムで実行されるようになったため、Dynamo 2.x 用に作成されたパッケージ(*.NET48 を使用*)、および Dynamo 3.x 用に作成されたパッケージ(*.NET 8 を使用*)の Dynamo 4.x での動作は保証されていません。4.0 より前の Dynamo バージョンからパブリッシュされたパッケージを Dynamo 4.x でダウンロードしようとすると、パッケージが旧バージョンの Dynamo からパブリッシュされたという警告が表示されます。

**これは、パッケージが機能しないということではありません。**単に互換性の問題が発生する可能性があるという警告です。通常は、Dynamo 4.x 専用に作成された新しいバージョンがあるかどうかを確認することをお勧めします。

パッケージを読み込む際にも Dynamo ログ ファイルでこのような警告が表示されることがあります。すべてが正しく動作している場合は、警告を無視することができます。

#### Dynamo 2.x で Dynamo 4.x パッケージを使用する 

Dynamo 4.x 用に作成されたパッケージ(*.Net 10 を使用*)が Dynamo 2.x で動作することはほとんどありません。また、Dynamo 4.x 用にビルドされたパッケージを Dynamo 2.x にインストールしようとすると、次の警告が表示されます。

![パッケージの互換性に関する警告](images/6-2-packages-new-version-compatibility-warning.png)


#### Dynamo 3.x で Dynamo 4.x パッケージを使用する 

Dynamo 4.x 用にビルドされたパッケージ(*.NET 10* を使用)は、パッケージで使用されているすべての API が .NET 8 に存在する限り、Dynamo 3.x で動作する可能性があります。ただし、必ずうまく動作するという保証はありません。また、Dynamo 4.x 用にビルドされたパッケージを Dynamo 3.x にインストールしようとすると、次の警告が表示されます。

![パッケージの互換性に関する警告](images/6-2-packages-compatibility-warning.png)

#### パッケージ作成者のためのベスト プラクティス 
ベスト プラクティスは、.csproj を変更して、プロジェクトを .NET 8 と .NET 10 の両方にマルチターゲット化することです。

```
<TargetFrameworks>net8.0;net10.0</TargetFrameworks>
```
これにより、次を実現できます。
* .NET 8 でも Revit でホストされている Dynamo バージョンをサポートする
* .NET 10 でスタンドアロン Dynamo 4.x との互換性を維持する