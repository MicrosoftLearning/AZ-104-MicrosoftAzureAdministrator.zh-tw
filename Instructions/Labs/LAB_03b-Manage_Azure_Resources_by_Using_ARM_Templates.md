---
lab:
  title: 實驗室 03：使用 Azure Resource Manager 範本管理 Azure 資源
  module: Administer Azure Resources
---

# 實驗室 03 - 使用 Azure Resource Manager 範本管理 Azure 資源

## 實驗室簡介

在此實驗室中，您將瞭解如何自動化資源部署。 您將瞭解 Azure Resource Manager 範本和 Bicep 範本。 您瞭解部署範本的不同方式。 

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟是使用 **美國**東部撰寫。 

## 預估時間：50 分鐘

## 互動式實驗室模擬

您可能會發現互動式實驗室模擬對本主題很有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。 

+ [使用 Azure Resource Manager 樣本](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)管理 Azure 資源。 使用範本檢閱、建立及部署受控磁碟。
  
+ [使用範本](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209)建立虛擬機。 使用快速入門範本部署虛擬機。
  
## 實驗案例

您的小組想要查看自動化及簡化資源部署的方式。 您的組織正在尋找減少系統管理額外負荷、減少人為錯誤並增加一致性的方法。  

## 架構圖

![工作的圖表。](../media/az104-lab03-architecture.png)

## 作業技能

+ 工作 1：建立 Azure Resource Manager 範本。
+ 工作 2：編輯 Azure Resource Manager 範本並重新部署範本。
+ 工作 3：使用 Azure PowerShell 設定 Cloud Shell 並部署範本。
+ 工作 4：使用 CLI 部署範本。 
+ 工作 5：使用 Azure Bicep 部署資源。

## 工作 1：建立 Azure Resource Manager 範本

在這項工作中，我們將在 Azure 入口網站 中建立受控磁碟。 受控磁碟是專為與虛擬機搭配使用的記憶體。 部署磁碟之後，您將匯出可在其他部署中使用的範本。

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

1. 搜尋並選取 `Disks`。

1. 在 [磁碟] 頁面上，選取 [ **建立**]。

1. 在 [ **建立受控磁碟** ] 頁面上，設定磁碟，然後選取 [ **確定**]。 
    
    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | *訂用帳戶* | 
    | 資源群組 | `az104-rg3` （如有必要，請選取 **建立 new**。）
    | 磁碟名稱 | `az104-disk1` | 
    | 區域 | **美國東部** |
    | 可用性區域 | **不需要基礎結構備援** | 
    | 來源類型 | **None** |
    | 效能 | **標準 HDD** （變更大小） |
    | 大小 | **32 Gib** | 

    >**注意：** 我們正在建立簡單的受控磁碟，讓您可以使用範本練習。 Azure 受控磁碟是 Azure 所管理的區塊層級記憶體磁碟區。

1. 按兩下 [ **檢閱 + 建立** ]，然後選取 [ **建立**]。

1. 監視通知（右上方），並在部署之後選取 **[移至資源**]。 

1. 在 [ **自動化** ] 刀鋒視窗中，選取 **[導出範本**]。 

1. 花一分鐘的時間檢閱 **範本** 和 **參數** 檔案。

1. 按兩下 [ **下載** ]，並將範本儲存到本機磁碟驅動器。 這會建立壓縮的壓縮檔案。 

1. 使用 檔案總管 將下載檔的內容解壓縮到**您電腦上的 Downloads** 資料夾中。 請注意，有兩個 JSON 檔案（範本和參數）。 

   >**你知道嗎？**  您可以匯出整個資源群組，或只匯出該資源群組內的特定資源。

## 工作 2：編輯 Azure Resource Manager 範本，然後重新部署範本

在這項工作中，您會使用下載的範本來部署新的受控磁碟。 此工作概述如何快速且輕鬆地重複部署。 

1. 在 Azure 入口網站 中，搜尋並選取 `Deploy a custom template`。

1. 在 [ **自定義部署]** 刀鋒視窗上，請注意可以使用 **快速入門範本**。 有許多內建範本，如下拉功能表所示。 

1. 不要使用快速入門，而是在編輯器**中選取 **[建置您自己的範本]。

1. 在 [**編輯範本]** 刀鋒視窗中，按兩下 **[載入檔案**]，並將您下載的template.json**檔案上傳**至本機磁碟。

1. 在編輯器窗格中，進行這些變更。

    + 將disks_az104_disk1_name**變更**為 `disk_name` （兩個地方要變更）
    + 將 az104-disk1** 變更**為 `az104-disk2` （一個要變更的地方）

1. 請注意，這是 **標準** 磁碟。 位置為 **eastus**。 磁碟大小為 **32 GB**。

1. **儲存**您的變更。

1. 別忘了參數檔案。 選取 [ **編輯參數**]，按兩下 **[載入檔案** ]，然後上傳 **parameters.json**。 

1. 進行這項變更，使其符合範本檔案。

    將disks_az104_disk1_name**變更**為**disk_name**（一個要變更的地方）

1. **儲存**您的變更。 

1. 完成自訂部署設定：

    | 設定 | 值 |
    | --- |--- |
    | 訂用帳戶 | *訂用帳戶* |
    | 資源群組 | `az104-rg3` |
    | 區域 | **（美國）美國東部）** |
    | Disk_name | `az104-disk2` |

1. 選取 [檢閱 + 建立]，然後選取 [建立]。

1. 選取 [前往資源]  。 確認 **已建立 az104-disk2** 。

1. 在 [概 **觀]** 刀鋒視窗中，選取資源群組 **az104-rg3**。 您現在應該有兩個磁碟。
   
1. 在 **[設定]** 區段中，按兩下 [**部署**]。

    >**注意：** 所有部署詳細數據都會記錄在資源群組中。 最好先檢閱前幾個以範本為基礎的部署，以確保在使用範本進行大規模作業之前成功。

1. 選取部署並檢閱 [輸入] 和 **[範本**] 刀鋒視窗的內容 **。**

## 工作 3：使用 Azure PowerShell 設定 Cloud Shell 並部署範本

在這項工作中，您會使用 Azure Cloud Shell 和 Azure PowerShell。 Azure Cloud Shell 是一個互動式、已驗證、可存取瀏覽器的終端機，可用來管理 Azure 資源。 其可讓您彈性地從 Bash 或 PowerShell 中，選擇最適合您工作方式的殼層體驗。 在這項工作中，您會使用PowerShell來部署範本。 

1. 選取 **Azure 入口網站右上方的 Cloud Shell** 圖示。 或者，您可以直接巡覽至 `https://shell.azure.com`。

   ![Cloud Shell 圖示的螢幕快照。](../media/az104-lab03-cloudshell-icon.png)

1. 當系統提示您選取 Bash 或 PowerShell 時，請選擇 **[PowerShell****]。**** ** 

    >**你知道嗎？**  如果您大多使用Linux系統，Bash （CLI） 會感覺更熟悉。 如果您大多使用 Windows 系統，Azure PowerShell 會感覺更熟悉。 

1. 在 [ **您沒有掛接存儲設備]** 畫面上，選取 [ **顯示進階設定** ]，並提供必要資訊。 

    >**注意：** 當您使用 Cloud Shell 時，需要記憶體帳戶和檔案共用。 

    | 設定 | 值 |
    |  -- | -- |
    | 資源群組 | **az104-rg3** |
    | 儲存體 帳戶 （新建） | *長度必須介於 3 到 24 個字元之間，且只能使用數位和小寫字母* |
    | 檔案分享 （新增） | `fs-cloudshell` |

1. 完成時，請選取 [ **建立記憶體**]。 您只需要在第一次使用 Cloud Shell 時執行這項操作。 布建記憶體需要幾分鐘的時間。

1. **使用上傳/下載檔案**圖示，從下載目錄上傳範本和參數檔案。 您必須個別上傳每個檔案。

1. 確認您的檔案可在 Cloud Shell 記憶體中使用。 

    ```powershell
    dir
    ```
    >**注意**：如果您需要，您可以使用 **cls** 清除命令視窗。 您可以使用箭號鍵來移動命令歷程記錄。
   
1. 選取 [ **編輯器** ] （大括弧） 圖示，然後流覽至範本 JSON 檔案。

1. 進行變更。 例如，將磁碟名稱變更為 **az104-disk3**。 使用 **Ctrl +S** 儲存變更。 

    >**注意**：您可以將範本部署的目標設為資源群組、訂用帳戶、管理群組或租使用者。 視部署的範圍而定，您可以使用不同的命令。

1. 若要部署至資源群組，請使用 **New-AzResourceGroupDeployment**。

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. 請確定命令完成，且 ProvisioningState 為 **Succeeded**。

1. 確認已建立磁碟。

   ```powershell
   Get-AzDisk
   ```
   
## 工作 4：使用 CLI 部署範本 

1. 在 Cloud Shell** 中**繼續選取 **Bash**。 **確認** 您的選擇。

1. 確認您的檔案可在 Cloud Shell 記憶體中使用。 如果您已完成先前的工作，則範本檔案應該可供使用。 

    ```sh
    ls
    ```

1. 選取 [ **編輯器** ] （大括弧） 圖示，然後流覽至範本 JSON 檔案。

1. 進行變更。 例如，將磁碟名稱變更為 **az104-disk4**。 使用 **Ctrl +S** 儲存變更。 

    >**注意**：您可以將範本部署的目標設為資源群組、訂用帳戶、管理群組或租使用者。 視部署的範圍而定，您可以使用不同的命令。

1. 若要部署至資源群組，請使用 **az deployment group create**。

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
    
1. 請確定命令完成，且 ProvisioningState 為 **Succeeded**。

1. 確認已建立磁碟。

     ```sh
     az disk list --output table
     ```
   
## 工作 5：使用 Azure Bicep 部署資源

在這項工作中，您將使用 Bicep 檔案來部署受控磁碟。 Bicep 是以ARM樣本為基礎的宣告式自動化工具。

1. 在Bash**工作中**繼續在 **Cloud Shell** 中工作。

1. 找出並下載 **\Allfiles\Lab03\azuredeploydisk.bicep** 檔案。

1. **將 bicep 檔案上傳** 至 Cloud Shell。 

1. 選取 [ **編輯器** ] （大括弧） 圖示，然後瀏覽至檔案。

1. 請花一分鐘的時間讀取 bicep 範本檔案。 請注意如何定義磁碟資源。 
   
1. 進行下列變更：

    + 將 **managedDiskName** 值變更為 `Disk4`。
    + 將 **SKU 名稱** 值變更為 `StandardSSD_LRS`。
    + 將 **diskSizeinGiB** 值變更為 `32`。

1. 使用 **Ctrl +S** 儲存變更。

1. 現在，部署範本。

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```

1. 確認已建立磁碟。

    ```sh
    az disk list --output table
    ```

    >**注意：** 您已以不同的方式成功部署五個受控磁碟。 做得好！

## 清除您的資源

如果您使用自己的訂用 **帳戶** ，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站 中，選取資源群組、選取 **[刪除資源群組**]、**輸入資源組名**，然後按兩下 [**刪除**]。
+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName`。
+ 使用 CLI， `az group delete --name resourceGroupName`。
  
## 重要心得

恭喜您完成實驗室。 以下是此實驗室的主要外賣。 

+ Azure Resource Manager 範本可讓您將解決方案的所有資源部署、管理及監視為群組，而不是個別處理這些資源。
+ Azure Resource Manager 範本是 JavaScript 物件表示法 （JSON） 檔案，可讓您以宣告方式而非使用腳本來管理基礎結構。
+ 您可以使用包含參數值的個別 JSON 檔案，而不是在範本中傳遞參數作為內嵌值。
+ Azure Resource Manager 範本可以透過各種方式部署，包括 Azure 入口網站、Azure PowerShell 和 CLI。
+ Bicep 是 Azure Resource Manager 範本的替代方案。 Bicep 會使用宣告式語法來部署 Azure 資源。 

Bicep 提供簡潔的語法、可靠的類型安全，並支援程式碼重複使用。 Bicep 能夠為您在 Azure 中的基礎結構即程式碼解決方案，提供最佳的製作體驗。

## 透過自學型訓練深入了解

+ [使用 JSON ARM 範本](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)部署 Azure 基礎結構。 使用 Visual Studio Code 撰寫 JSON Azure Resource Manager 範本 (ARM 範本)，一致又可靠地將基礎結構部署至 Azure。
+ [檢閱 Azure Cloud Shell](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/) 的功能和工具。 Cloud Shell 功能和工具。 
+ [使用 Windows PowerShell](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/) 管理 Azure 資源。 本課程模組說明如何安裝雲端服務管理所需的模組，並使用 PowerShell 命令對 Azure 虛擬機器、Azure 訂用帳戶和 Azure 儲存體帳戶等雲端資源執行簡單的管理工作。
+ [Bash](https://learn.microsoft.com/training/modules/bash-introduction/) 簡介。 使用 Bash 來管理 IT 基礎結構。
+ [建置您的第一個 Bicep 範本](https://learn.microsoft.com/training/modules/build-first-bicep-template/)。 請在 Bicep 範本中定義 Azure 資源。 改善部署的一致性和可靠性、減少所需的手動工作，並將部署縮放到整個環境。 您的範本將可透過使用參數、變數、運算式和模組，而變得有彈性且可重複使用。


