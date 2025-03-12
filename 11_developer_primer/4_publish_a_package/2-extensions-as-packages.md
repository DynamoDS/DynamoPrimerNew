# 將延伸當作套件

### 將延伸當作套件 <a href="#extensions-as-packages" id="extensions-as-packages"></a>

### 概述 <a href="#overview" id="overview"></a>

Dynamo 延伸可以部署到 Package Manager，就像一般 Dynamo 節點資源庫一樣。如果安裝的套件包含視圖延伸，在 Dynamo 載入的執行時期會載入該延伸。您可以查看 Dynamo 主控台，以確認延伸已正確載入。

### 套件結構 <a href="#package-structure" id="package-structure"></a>

延伸套件的結構與一般套件的結構相同，其中包含...

```
C:\Users\User\AppData\Roaming\Dynamo\Dynamo Core\2.1\packages\Sample View Extension
│   pkg.json
├───bin
│       SampleViewExtension.dll
├───dyf
└───extra
        SampleViewExtension_ViewExtensionDefinition.xml
```

假設您已建置延伸，您 (至少) 會有一個 .NET 組合和一個資訊清單檔案。組合應包含實作 `IViewExtension` 或 `IExtension` 的類別。資訊清單 .XML 檔案會告訴 Dynamo 要實體化哪個類別，以啟動您的延伸。為了讓 Package Manager 能正確找到延伸，資訊清單檔案應準確對應組合位置和命名。

將任何組合檔放在 `bin` 資料夾中，將資訊清單檔案放在 `extra` 資料夾中。此資料夾中也可以放置任何其他資產。

範例資訊清單 .XML 檔案：

```
<ViewExtensionDefinition>
  <AssemblyPath>..\bin\MyViewExtension.dll</AssemblyPath>
  <TypeName>MyViewExtension.MyViewExtension</TypeName>
</ViewExtensionDefinition>
```

### 上傳 <a href="#uploading" id="uploading"></a>

一旦您有包含上述子目錄的資料夾，即可推送 (上傳) 到 Package Manager。需要注意的一點是，您目前無法從 Dynamo Sandbox 發佈套件。這表示您需要使用 Dynamo Revit。在 Dynamo Revit 內瀏覽到「套件」=>「發佈新套件」。這會提示使用者登入要與套件關聯的 Autodesk 帳戶。

此時，您應該會在一般的發佈套件視窗中，您將在其中輸入有關套件/延伸的所有必要欄位。還有一個**非常重要**的額外步驟，您必須確保沒有任何組合檔被標記為節點資源庫。您可以在已匯入的檔案 (上面建立的套件資料夾) 上按一下右鍵確認。此時會顯示一個關聯式功能表，讓您勾選 (或取消勾選) 此選項。應取消勾選所有延伸組合。

![發佈套件](images/ViewExtension_Search.png)

在公開發佈之前，請您務必先在本端發佈，以確保所有內容都如預期般運作。確認之後，您就可以選取「發佈」讓套件上線。

### 提取 <a href="#pulling" id="pulling"></a>

若要確認套件已成功上傳，您應該能夠透過在發佈步驟指定的名稱和關鍵字搜尋到套件。最後，請務必注意，您需要重新啟動 Dynamo，相同的延伸才能正常運作。這些延伸通常需要指定 Dynamo 啟動時的參數。

![搜尋套件](images/ViewExtension_Search.jpg)
