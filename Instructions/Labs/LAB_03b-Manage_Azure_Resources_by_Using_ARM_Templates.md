---
lab:
  title: 03b - 使用 ARM 範本管理 Azure 資源
  module: Module 03 - Azure Administration
ms.openlocfilehash: 602da542fdf20f6b1be637e792ec47daaa0de04b
ms.sourcegitcommit: 8282cbcee5f7cd46bdc73d781c460d6a078049bb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/19/2022
ms.locfileid: "145198079"
---
# <a name="lab-03b---manage-azure-resources-by-using-arm-templates"></a>實驗 03b - 使用 ARM 範本管理 Azure 資源
# <a name="student-lab-manual"></a>學生實驗室手冊

## <a name="lab-scenario"></a>實驗案例
現在您已探索與佈建資源相關聯的基本 Azure 管理功能，並使用 Azure 入口網站根據資源群組加以組織，接著您需要使用 Azure Resource Manager 範本來執行對等的工作。

## <a name="objectives"></a>目標

在此實驗中，您將會：

+ 工作 1：檢閱用於部署 Azure 受控磁碟的 ARM 範本
+ 工作 2：使用 ARM 範本建立 Azure 受控磁碟
+ 工作 3：檢閱受控磁碟的 ARM 範本型部署

## <a name="estimated-timing-20-minutes"></a>預計時間：20 分鐘

## <a name="architecture-diagram"></a>架構圖

![image](../media/lab03b.png)

## <a name="instructions"></a>指示

### <a name="exercise-1"></a>練習 1

#### <a name="task-1-review-an-arm-template-for-deployment-of-an-azure-managed-disk"></a>工作 1：檢閱用於部署 Azure 受控磁碟的 ARM 範本

在此工作中，您將使用 Azure Resource Manager 範本建立 Azure 磁碟資源。

1. 登入 [**Azure 入口網站**](http://portal.azure.com)。

1. 在 Azure 入口網站中，搜尋並選取 [資源群組]。 

1. 在資源群組清單中，按一下 [az104-03a-rg1]。

1. 在 [az104-03a-rg1] 資源群組刀鋒視窗的 [設定] 區段中，按一下 [部署]。

1. 在 [az104-03a-rg1 - 部署] 刀鋒視窗上，按一下部署清單中的第一個項目。

1. 在 **Microsoft.ManagedDisk-* XXXXXXXXX* \| 概觀** 刀鋒視窗上，按一下 [範本]。

    >**注意**：檢閱範本的內容，並注意您可以選擇將其 [下載] 到本機電腦、[新增至程式庫]，或再次 [部署]。

1. 按一下 [下載]，並將包含範本和參數檔案的壓縮檔儲存到實驗室電腦上的 [下載] 資料夾。

1. 在 **Microsoft.ManagedDisk-* XXXXXXXXX* \| 範本** 刀鋒視窗上，按一下 [輸入]。

1. 請注意 **location** 參數的值。 您將在下一個工作中需要它。

1. 將下載檔案的內容解壓縮到實驗室電腦上的 [下載] 資料夾。

    >**注意**：這些檔案也會以 **\\Allfiles\\Labs\\03\\az104-03b-md-template.json** 和 **\\Allfiles\\Labs\\03\\az104-03b-md-parameters.json** 的方式提供
    
1. 關閉所有 [檔案總管] 視窗。

#### <a name="task-2-create-an-azure-managed-disk-by-using-an-arm-template"></a>工作 2：使用 ARM 範本建立 Azure 受控磁碟

1. 在 Azure 入口網站中，搜尋並選取 [部署自訂範本]。

1. 在 [自訂部署] 刀鋒視窗中按一下 [在編輯器中建置您自己的範本]。

1. 在 [編輯範本] 刀鋒視窗上，按一下 [載入檔案]，然後上傳您在上一個工作中下載的 **template.json** 檔案。

1. 在編輯器窗格中，移除下列幾行：

   ```json
   "sourceResourceId": {
       "type": "String"
   },
   "sourceUri": {
       "type": "String"
   },
   "osType": {
       "type": "String"
   },
   ```

   ```json
   "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

   ```json
   "osType": "[parameters('osType')]",
   ```

    >**注意**：這些參數不適用於目前的部署，因此需要移除。 在這當中，sourceResourceId、sourceUri、osType 和 hyperVGeneration 參數適用於從現有的 VHD 檔案建立 Azure 磁碟。

1. **儲存** 變更。

1. 返回 [自訂部署] 刀鋒視窗，然後按一下 [編輯參數]。  

1. 在 [編輯參數] 刀鋒視窗上，按一下 [載入檔案]，並上傳您在上一個工作中下載的 **parameters.json** 檔案，然後 [儲存] 變更。

1. 回到 [自訂部署] 刀鋒視窗，指定下列設定：

    | 設定 | 值 |
    | --- |--- |
    | 訂用帳戶 | *您在此實驗室中使用的 Azure 訂閱名稱* |
    | 資源群組 | **新** 資源群組 **az104-03b-rg1** 的名稱 |
    | 區域 | 您在此實驗室中使用的訂用帳戶中，可用的任何 Azure 區域名稱 |
    | 磁碟名稱 | **az104-03b-disk1** |
    | 位置 | 您在上一個工作中所記錄的位置參數值 |
    | SKU | **Standard_LRS** |
    | 磁碟大小 (GB) | **32** |
    | 建立選項 | **empty** |
    | 磁碟加密集類型 | **EncryptionAtRestWithPlatformKey** |
    | 網路存取原則 | **AllowAll** |

1. 選取 [檢閱 + 建立]，然後選取 [建立]。

1. 確認部署已成功完成。

#### <a name="task-3-review-the-arm-template-based-deployment-of-the-managed-disk"></a>工作 3：檢閱受控磁碟的 ARM 範本型部署

1. 在 Azure 入口網站中，搜尋並選取 [資源群組]。 

1. 在資源群組清單中，按一下 [az104-03b-rg1]。

1. 在 [az104-03b-rg1] 資源群組刀鋒視窗的 [設定] 區段中，按一下 [部署]。

1. 從 [az104-03b-rg1 - 部署] 刀鋒視窗中，按一下部署清單中的第一個項目，並檢閱 [輸入] 和 [範本] 刀鋒視窗的內容。

#### <a name="clean-up-resources"></a>清除資源

   >**注意**：請勿刪除您在此實驗中部署的資源。 您將在本課程模組的下一個實驗中引用這些資源。

#### <a name="review"></a>檢閱

在此實驗中，您已：

- 檢閱用於部署 Azure 受控磁碟的 ARM 範本
- 使用 ARM 範本建立 Azure 受控磁碟
- 檢閱受控磁碟的 ARM 範本型部署
