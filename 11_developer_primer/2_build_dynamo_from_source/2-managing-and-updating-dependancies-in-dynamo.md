# 管理和更新 Dynamo 中的相依性

### 此 wiki 適用於何種情況

在使用新功能或單純更新現有相依性時，應先評估下列情況，再決定是否將新的相依性引入 Dynamo 儲存庫。

### 考慮事項

1. 新的或更新的相依性的授權為何 - 只有一些開放原始碼授權在沒有與 ADSK 法律部門商談的情況下獲得核准。
   * 解決授權後，請確保將 dep 和版本記錄在內部 wiki 中。
   * 如果授權為 `LGPL`、`GPL` 或 `Apache`，則必須將授權檔複製到 Dynamo 組建的_「Open Source Licenses」_子資料夾中。
   * 如果授權為 `LGPL`，則必須將所有第三方元件的完整原始程式碼及其適當的開放原始碼授權的文字複本上傳到 [www.autodesk.com/lgplsource](https://www.autodesk.com/company/legal-notices-trademarks/open-source-distribution)
2. 如果更新，授權類型是否已從舊版變更？
3. 相依性是否為跨平台？
   * 有原生元件 (例如 `CEFSharp` 或 `ImageMagick`) 嗎？這會讓跨平台部署變得更加困難
   * 是否只有 Windows 參考？如果是，就不該為 DynamoCore 或 Dynamo (模型層) 其他跨平台部分的相依性。
4. 相依性及其所有必需的相依性是否正確捆綁到組建上的 bin 資料夾中？
   * 如果更新，某些檔案是否會因為更新而被移除？此版本的 Dynamo 是否適用於主產品的點版本？如果是，您必須保留舊的二進位檔，直到全球上市年份以支援修補安裝程式。請參閱[這裡](https://github.com/DynamoDS/Dynamo/tree/master/extern/legacy_remove_me)。
5. 相依性或其相依性樹是否與 Dynamo 中其他既有的相依性發生衝突？
6. 相依性或其相依性樹是否與正在整合 Dynamo 之產品 (Revit、Civil 等) 中既有的相依性發生衝突 - **這很重要，因為除非事先完成工作，否則只能在整合時發現這些問題。**
