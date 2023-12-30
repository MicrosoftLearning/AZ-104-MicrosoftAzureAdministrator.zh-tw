---
lab:
  title: 實驗室 03：使用 Azure Resource Manager 範本管理 Azure 資源
  module: Administer Azure Resources
---

# 實驗室 03 - 使用 Azure Resource Manager 範本管理 Azure 資源

## 實驗室簡介

在此實驗室中，您將瞭解如何使用 Azure Resource Manager 範本來定義您的資源基礎結構。 範本可確保一致性，並可讓您一次建立多個資源。 您將瞭解如何使用 Azure 入口網站、Azure PowerShell 或 CLI 來部署範本。 

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟是使用 ** 美國 ** 東部撰寫。 

## 預估時間：40 分鐘

## 互動式實驗室模擬

您可能會發現互動式實驗室模擬對本主題很有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。 

+ [使用範本 ](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209) 建立虛擬機器。 使用快速入門範本部署虛擬機器。
  
+ [使用 Azure Resource Manager 範本 ](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205) 管理 Azure 資源。 使用範本檢閱、建立及部署受控磁片。

## 實驗案例

您的小組已探索基本的 Azure 系統管理功能，例如布建資源，並根據資源群組加以組織。 接下來，您的小組想要查看自動化和簡化部署的方式。 組織通常會尋找自動化，以減少系統管理額外負荷、減少人為錯誤或增加一致性，以及讓系統管理員能夠處理更複雜的或創造性的工作。

## 架構圖

![工作的圖表。](../media/az104-lab03b-architecture-diagram.png)

## 工作

+ 工作 1：建立 Azure Resource Manager 範本以部署 Azure 受控磁片。
+ 工作 2：編輯 Azure Resource Manager 範本，然後使用範本建立 Azure 受控磁片。
+ 工作 3：檢閱受控磁片的 Azure Resource Manager 範本型部署。
+ 工作 4：使用 Azure Bicep 部署受控磁片。
+ 工作 5：使用 Azure PowerShell 部署範本（選項 1）。
+ 工作 5：使用 CLI 部署範本（選項 2）。 


## 工作 1：建立 Azure Resource Manager 範本以部署 Azure 受控磁片

在這項工作中，您會使用Azure 入口網站來產生 Azure Resource Manager 範本。 然後，您可以下載範本，以在未來的部署中使用。 計畫部署數百或數千個磁片的組織可以利用一或多個範本來協助自動化部署。 

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

1. 搜尋並選取 `Disks`。

1. 在 [磁片] 頁面上，選取 [ ** 建立 ** ]。

1. 在 [建立受控磁片] 頁面上，使用下列資訊來建立磁片。
    
    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | *訂用帳戶* | 
    | 資源群組 | `az104-rg3` （如有必要，請選取 **建立 new ** 。）
    | 磁碟名稱 | `az104-disk1` | 
    | 區域 | **美國東部** |
    | 可用性區域 | **不需要基礎結構備援** | 
    | 來源類型 | **None** |
    | 大小 | **32 Gb** | 
    | 效能 | **標準 HDD** |

1. 按一下 [ ** 檢閱 + 建立 ** * ] 一次 * 。 請勿 ** ** 部署資源。

1. 驗證之後，按一下 [ ** 下載自動化 ** 範本] （頁面底部）。

1. 檢閱範本中顯示的資訊。 檢閱 [範本 ** ] 和 ** [ ** 參數 ** ] 索引標籤。

1. 按一下 [ ** 下載 ** ]，並將範本儲存至您的電腦。

1. 將下載檔案的內容解壓縮到 ** 您電腦上的 [下載 ** ] 資料夾中。 請注意，有兩個 JSON 檔案（範本和參數）。 

1. 關閉所有 [檔案總管] 視窗。

1. 在Azure 入口網站中，取消受控磁片的部署。

## 工作 2：編輯 Azure Resource Manager 範本，然後使用範本建立 Azure 受控磁片

在這項工作中，您會使用您建立的範本來部署新的受控磁片。 此工作概述具有範本型部署的一般程式，讓您能夠快速且輕鬆地重複部署。 如果您需要變更參數或兩個參數，您未來可以輕鬆地修改範本。

1. 在Azure 入口網站中，搜尋並選取 `Deploy a custom template` 。

1. 在 [ ** 自訂部署] ** 刀鋒視窗上，請注意可以使用 ** 快速入門範本 ** 。 有許多內建範本。 選取任何範本將提供簡短的描述。

1. 不要使用快速入門，而是在編輯器 ** 中選取 ** [建置您自己的範本]。

1. 在 [編輯範本] 刀鋒視窗上，按一下 [載入檔案]，然後上傳您在上一個工作中下載的 **template.json** 檔案。

1. 在編輯器窗格中，移除下列幾行。 請務必移除右括弧和逗號。 

   ```
    "sourceResourceId": {
            "type": "string"
    },
   ```
   
   ```
      "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

    >**注意**：這些參數不適用於目前的部署，因此需要移除。 SourceResourceId、sourceUri、osType 和 hyperVGeneration 參數適用于從現有的 VHD 檔案建立 Azure 磁片。

1. **儲存**變更。

1. 返回 [自訂部署] 刀鋒視窗，然後按一下 [編輯參數]。 

1. 在 [編輯參數] 刀鋒視窗上，按一下 [載入檔案]，並上傳您在上一個工作中下載的 **parameters.json** 檔案，然後 [儲存] 變更。

1. 回到 [自訂部署] 刀鋒視窗，指定下列設定：

    | 設定 | 值 |
    | --- |--- |
    | 訂用帳戶 | *訂用帳戶* |
    | 資源群組 | `az104-rg3` |
    | 區域 | **美國東部** |
    | 磁碟名稱 | `az104-disk1` |
    | Location | 您在上一個工作中所記錄的位置參數值 |
    | SKU | **Standard_LRS** |
    | 磁碟大小 (GB) | **32** |
    | 建立選項 | **empty** |
    | 磁碟加密集類型 | **EncryptionAtRestWithPlatformKey** |
    | 資料存取驗證模式 | 無 |
    | 網路存取原則 | **AllowAll** |
    | 公用網路存取 | 停用 |

1. 選取 [檢閱 + 建立]，然後選取 [建立]。

1. 確認部署已成功完成。

## 工作 3：檢閱以 Azure Resource Manager 範本為基礎的受控磁片部署

在這項工作中，您會確認部署已順利完成。 所有先前的部署都會記錄在部署的目標資源群組中。 此檢閱會顯示部署時間和長度的詳細資料，這在疑難排解時會很有説明。 最好先檢閱前幾個以範本為基礎的部署，以確保在使用範本進行大規模作業之前成功。

1. 在 Azure 入口網站中，搜尋並選取 [資源群組]。 

1. 在資源群組清單中，按一下 ** az104-rg3 ** 。

1. 在 [az104-rg3 ** 資源群組] 刀鋒視窗的 ** [設定] ** 區段中，按一下 ** [部署 ** ]。 **

1. **從 az104-rg3 - Deployments ** 刀鋒視窗中，按一下部署清單中的第一個專案，並檢閱 [輸入 ** ] 和 ** [範本 ** ] 刀鋒視窗的內容 ** 。

1. 在 Azure 入口網站中，搜尋並選取 [磁碟]****。

1. 請注意，您的受控磁片已建立。

    >**注意： ** 您也可以從命令列部署範本。 工作 4，選項 1，示範如何使用 PowerShell。 工作 5，選項 2，示範如何使用 CLI。

## 工作 3：使用 Azure Bicep 部署資源

在這項工作中，您將使用 Bicep 檔案，將儲存體帳戶部署至您的資源群組。 Bicep 是一種宣告式自動化工具，建置在 ARM 範本上，但更容易閱讀和使用。

1. 開啟 Cloud Shell ** Bash ** 會話。 如有必要，請使用 [ ** 進階 ** ] 連結來設定儲存體。 

1. 選取 Cloud Shell 功能表列中的 ** [上傳/下載檔案 ** ] 圖示。 這是由具有向上和向下箭號的檔圖示表示。

1. 選取**上傳**。 找出 \Allfiles\Lab03 目錄，然後選取 Bicep 範本檔案 ** azuredeploy.bicep ** 。

1. 若要確認檔案已上傳，請選取 Cloud Shell 功能表中的雙括弧圖示，以開啟內建編輯器。

1. 檔案應該列在左側導覽樹狀結構中。 選取它以驗證其內容。

   ![Cloud Shell 編輯器中 bicep 檔案的螢幕擷取畫面。](../media/az104-lab03d-editor.png)

1. 請花一分鐘的時間讀取 bicep 範本檔案。 請注意如何定義儲存體 （stg） 資源。 請注意如何設定參數和允許的值。
   
1. 若要將 Bicep 檔案部署至 ** az104-rg3 ** 資源群組，請執行下列命令：

   ```sh
   az deployment group create --resource-group az104-rg3 --template-file azuredeploy.bicep
   ```

1. 當系統提示您輸入儲存體字串值時，請輸入 `az104` 。 範本檔案會使用您的字串搭配隨機產生的字串，以建立唯一的部署名稱。

1. 關閉 Cloud Shell 並返回完整的 Azure 入口網站。 

1. 搜尋並選取 ** [儲存體帳戶 ** ]。 確認已在 az104-rg3 ** 資源群組中 ** 建立名為 ** az104 ** 的儲存體帳戶。

## 工作 5. 使用 Azure PowerShell 部署範本（選項 1）。

1. 開啟 Cloud Shell，然後選取 ** [PowerShell ** ]。

1. 如有必要，請使用進 ** 階 ** 設定來建立 Cloud Shell 的磁片儲存體。

1. 在 Cloud Shell 中，使用 ** 上傳 ** 圖示來上傳範本和參數檔案。 您必須個別上傳每個檔案。

1. 確認您的檔案可在 Cloud Shell 儲存體中使用。

    ```powershell
    dir
    ```

1. 在 Cloud Shell 中 ** ，選取編輯器 ** 圖示並流覽至參數 JSON 檔案。

1. 進行變更。 例如，將磁碟名稱變更為 **az104-disk2**。 

    >**注意**：您可以將範本部署的目標設為資源群組、訂用帳戶、管理群組或租使用者。 根據部署的範圍，您可以使用不同的命令。

1. 若要部署至資源群組，請使用 **New-AzResourceGroupDeployment**。

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. 請確定命令完成，且 ProvisioningState 為 **Succeeded**。
   
## 工作 6：使用 CLI 部署樣本 （選項 2）

1. 開啟 Cloud Shell，然後選取 **[Bash**]。

1. 如有必要，請使用進 **階** 設定來建立 Cloud Shell 的磁碟記憶體。

1. 在 Cloud Shell 中，使用 **上傳** 圖示來上傳範本和參數檔案。 您必須個別上傳每個檔案。

1. 確認您的檔案可在 Cloud Shell 記憶體中使用。

    ```sh
    dir
    ```

1. 在 Cloud Shell 中 **，選取編輯器** 圖示並瀏覽至參數 JSON 檔案。

1. 進行變更。 例如，將磁碟名稱變更為 **az104-disk2**。 

    >**注意**：您可以將範本部署的目標設為資源群組、訂用帳戶、管理群組或租使用者。 根據部署的範圍，您可以使用不同的命令。

1. 若要部署至資源群組，請使用 **az deployment group create**。

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
1. 請確定命令完成，且 ProvisioningState 為 **Succeeded**。



## 檢閱實驗室的要點

恭喜您完成實驗室。 以下是此實驗室的主要外賣。 

+ Azure Resource Manager 範本可讓您將解決方案的所有資源部署、管理及監視為群組，而不是個別處理這些資源。
+ Azure Resource Manager 範本是 JavaScript 物件表示法 （JSON） 檔案，可讓您以宣告方式而非使用腳本來管理基礎結構。
+ 您可以使用包含參數值的個別 JSON 檔案，而不是在範本中傳遞參數作為內嵌值。
+ Azure Resource Manager 範本可以透過各種方式部署，包括 Azure 入口網站、Azure PowerShell 和 CLI。

## 清除您的資源

如果您使用自己的訂用帳戶，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站 中，選取資源群組、選取 **[刪除資源群組**]、**輸入資源組名**，然後按兩下 [**刪除**]。

+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName`。

+ 使用 CLI， `az group delete --name resourceGroupName`。
