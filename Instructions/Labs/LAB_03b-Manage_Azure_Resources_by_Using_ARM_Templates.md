---
lab:
  title: 實驗 03：使用 Azure Resource Manager 範本管理 Azure 資源
  module: Administer Azure Resources
---

# 實驗 03 - 使用 Azure Resource Manager 範本管理 Azure 資源

## 實驗室簡介

在此實驗室中，您會了解如何自動化資源部署。 您會了解 Azure Resource Manager 範本和 Bicep 範本。 您會了解部署範本的不同方式。 

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中的功能可用性。 您可以變更區域，但這些步驟是使用**美國東部**撰寫而成。 

## 預估時間：50 分鐘

## 互動式實驗室模擬

您可能會發現部分互動式實驗室模擬對本主題十分有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬與此實驗室之間有所差異，但許多核心概念都相同。 Azure 訂用帳戶並非必要項。 

+ [使用 Azure Resource Manager 範本管理 Azure 資源](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%205)。 使用範本檢閱、建立及部署受控磁碟。
  
+ [使用範本建立虛擬機器](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209)。 使用快速入門範本部署虛擬機器。
  
## 實驗案例

您的小組想要查看自動化及簡化資源部署的方式。 貴組織正在尋找減少系統管理負荷、降低人為錯誤並增加一致性的方法。  

## 架構圖

![工作的圖表。](../media/az104-lab03-architecture.png)

## 作業技能

+ 工作 1：建立 Azure Resource Manager 範本。
+ 工作 2：編輯 Azure Resource Manager 範本並重新部署範本。
+ 工作 3：使用 Azure PowerShell 設定 Cloud Shell 並部署範本。
+ 工作 4：使用 CLI 部署範本。 
+ 工作 5：使用 Azure Bicep 部署資源。

## 工作 1：建立 Azure Resource Manager 範本

在此工作中，我們會在 Azure 入口網站中建立受控磁碟。 受控磁碟是專為與虛擬機器搭配使用的儲存體。 部署磁碟之後，您即可匯出可在其他部署中使用的範本。

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

1. 搜尋並選取 `Disks`。 
   
1. 在 [磁碟] 頁面上，選取 **[建立]**。

1. 在 **[建立受控磁碟]** 頁面上，設定磁碟，然後選取 **[確定]**。 
    
    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | *您的訂用帳戶* | 
    | 資源群組 | `az104-rg3` (如有必要，請選取 **[新建]**。)
    | 磁碟名稱 | `az104-disk1` | 
    | 區域 | **美國東部** |
    | 可用性區域 | **不需要基礎結構備援** | 
    | 來源類型 | **None** |
    | 效能 | **標準 HDD** (變更大小) |
    | 大小 | **32 Gib** | 

    >**注意：** 我們正在建立簡單的受控磁碟，以便您可使用範本練習。 Azure 受控磁碟是 Azure 所管理的區塊層級儲存體磁碟區。

1. 請按一下 **[檢閱 + 建立]**，然後選取 **[建立]**。

1. 監視通知 (右上方)，並在部署之後選取 **[移至資源]**。 

1. 在 **[自動化]** 刀鋒視窗中，選取 **[匯出範本]**。 

1. 請花一分鐘的時間檢閱**範本**和**參數**檔案。

1. 請按一下 **[下載]**，然後將範本儲存到本機磁碟機。 這會建立壓縮的壓縮檔。 

1. 使用檔案總管將下載檔案的內容解壓縮到電腦的 **[下載]** 資料夾。 請注意，有兩個 JSON 檔案 (範本和參數)。 

   >**您知道嗎？**  您可以匯出整個資源群組，或只匯出該資源群組內的特定資源。

## 工作 2：編輯 Azure Resource Manager 範本，然後重新部署範本

在此工作中，您會使用下載的範本來部署新的受控磁碟。 此工作會概述如何快速且輕鬆地重複部署。 

1. 在 Azure 入口網站中，搜尋並選取 `Deploy a custom template`。

1. 在 **[自訂部署]** 刀鋒視窗上，請注意可以使用**快速入門範本**。 有許多內建範本，如下拉式功能表所示。 

1. 不要使用快速入門，而是選取 **[在編輯器中建置您自己的範本]**。

1. 在 **[編輯範本]** 刀鋒視窗上，按一下 **[載入檔案]**，然後上傳您下載到本機磁碟的 **template.json** 檔案。

1. 在編輯器窗格中，進行這些變更。

    + 將 **disks_az104_disk1_name** 變更為 `disk_name` (要變更兩個地方)
    + 將 **az104-disk1** 變更為 `az104-disk2` (要變更一個地方)

1. 請注意，這是**標準**磁碟。 位置為 **eastus**。 磁碟大小為 **32GB**。

1. **儲存**您的變更。

1. 別忘了參數檔案。 選取 **[編輯參數]**，按一下 **[載入檔案]**，然後上傳 **parameters.json**。 

1. 進行這項變更，使其與範本檔案相符。

    將 **disks_az104_disk1_name** 為 **disk_name** (要變更一個地方)

1. **儲存**您的變更。 

1. 完成自訂部署設定：

    | 設定 | 值 |
    | --- |--- |
    | 訂用帳戶 | *您的訂用帳戶* |
    | 資源群組 | `az104-rg3` |
    | 區域 | **(美國) 美國東部** |
    | Disk_name | `az104-disk2` |

1. 選取 [檢閱 + 建立]，然後選取 [建立]。

1. 選取 [前往資源]  。 確認 **az104-disk2** 已建立。

1. 在 **[概觀]** 刀鋒視窗上，選取資源群組 **az104-rg3**。 您現在應該有兩個磁碟。
   
1. 在 **[設定]** 區段中，按一下 **[部署]**。

    >**注意：** 所有部署詳細資料都會記錄在資源群組中。 在使用範本進行大規模作業之前，最好先檢閱前幾個範本型部署，以確保成功。

1. 選取部署並檢閱 **[輸入]** 和 **[範本]** 刀鋒視窗的內容。

## 工作 3：使用 PowerShell 設定 Cloud Shell 並部署範本 

在這項工作中，您會使用 Azure Cloud Shell 和 Azure PowerShell。 Azure Cloud Shell 是可經由瀏覽器存取的已驗證互動式終端，用於管理 Azure 資源。 其可讓您彈性地從 Bash 或 PowerShell 中，選擇最適合您工作方式的殼層體驗。 在這項工作中，您會使用 PowerShell 來部署範本。 

1. 選取 Azure 入口網站右上方的 **Cloud Shell** 圖示。 或者，您可以直接瀏覽至 `https://shell.azure.com`。

   ![Cloud Shell 圖示的螢幕擷取畫面。](../media/az104-lab03-cloudshell-icon.png)

1. 當系統提示您選取 **Bash** 或 **PowerShell** 時，請選取 **PowerShell**。 

    >**您知道嗎？**  如果您主要使用 Linux 系統，Bash (CLI) 會讓您感覺較熟悉。 如果您主要使用 Windows 系統，Azure PowerShell 會讓您感覺較熟悉。 

1. 在 [ **開始使用]** 畫面上，選取 **[掛接記憶體帳戶**]，選取您的 **記憶體帳戶訂**用帳戶，然後選取 [ **套用**]。

1. 選取 **[我想建立記憶體帳戶** ]，然後 **選取 [下一步**]。 完成建立 **記憶體帳戶** 資訊。 
    
    | 設定 | 值 |
    |  -- | -- |
    | 資源群組 | **az104-rg3** |
    | 區域 | *選取您的區域* | 
    | 儲存體帳戶 (新建) | *必須全域唯一，長度在 3 到 24 個字元之間，而且只能使用數字和小寫字母* |
    | 檔案共用 (新建) | `fs-cloudshell` |

1. 完成時，請選取 [ **建立**]。

    >佈建儲存體需要幾分鐘的時間。

1. 選取 [ **設定** ] （頂端列），然後 **移至傳統版本**。

1. 選取 [ **上傳/下載檔案** ] 圖示 （頂端列），然後選取 [ **上傳**]。

1. 從下載**目錄上傳範本和參數檔案**。 

1. 選取編輯器 **（大括弧）** 圖示，然後流覽至導航窗格中左側的範本 JSON 檔案。

1. 進行變更。 例如，將磁碟名稱變更為 **az104-disk3**。 使用 **Ctrl+S** 儲存變更。 

    >**注意**：您可以將範本部署的目標設為資源群組、訂用帳戶、管理群組或租用戶。 視部署的範圍而定，您可以使用不同的命令。

1. 若要部署至資源群組，請使用 **New-AzResourceGroupDeployment**。

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg3 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. 請確定命令完成，且 ProvisioningState 為 **[成功]**。

1. 確認磁碟已建立。

   ```powershell
   Get-AzDisk
   ```
   
## 工作 4：使用 CLI 部署範本 

1. 繼續在 **Cloud Shell** 中 選取 **Bash**。 **確認**您的選擇。

1. 確認您的檔案可在 Cloud Shell 儲存體中使用。 如果您已完成先前的工作，則範本檔案應該可供使用。 

    ```sh
    ls
    ```

1. 選取 **[編輯器]** (大括弧) 圖示，然後瀏覽至 JSON 檔案範本。

1. 進行變更。 例如，將磁碟名稱變更為 **az104-disk4**。 使用 **Ctrl+S** 儲存變更。 

    >**注意**：您可以將範本部署的目標設為資源群組、訂用帳戶、管理群組或租用戶。 視部署的範圍而定，您可以使用不同的命令。

1. 若要部署至資源群組，請使用 **az deployment group create**。

    ```sh
    az deployment group create --resource-group az104-rg3 --template-file template.json --parameters parameters.json
    ```
    
1. 請確定命令完成，且 ProvisioningState 為 **[成功]**。

1. 確認磁碟已建立。

     ```sh
     az disk list --output table
     ```
   
## 工作 5：使用 Azure Bicep 部署資源

在此工作中，您會使用 Bicep 檔案來部署受控磁碟。 Bicep 是依據 ARM 範本建置的宣告式自動化工具。

1. **找出 \Allfiles\Lab03\azuredeploydisk.bicep** 檔案。

1. 繼續在 **Bash** 工作階段的 **Cloud Shell** 中工作。

1. 選取 [ **管理檔案** ]，然後將 **Bicep 檔案上傳** 至 Cloud Shell。 

1. 按兩下 [ **編輯器** ]，並在出現提示 **時確認** 切換至傳統CloudShell。

1. **選取 azuredeploydisk.bicep** 檔案 

1. 花一分鐘的時間閱讀 Bicep 範本檔案。 請注意磁碟資源的定義方式。 
   
1. 進行下列變更：

    + 將 **managedDiskName** 值第 4 行變更為 Disk4。
    + 將 **sku 名稱** 值第 26 行變更為 StandardSSD_LRS。
    + 將 **diskSizeinGiB** 值變更為第 7 行，變更為 32。

    >**注意：** 實驗室檔案中提供已完成的 Bicep 範本。
    
1. 使用 **Ctrl + S** 儲存變更。

1. 現在請部署範本。

    ```
    az deployment group create --resource-group az104-rg3 --template-file azuredeploydisk.bicep
    ```

1. 確認磁碟已建立。

    ```sh
    az disk list --output table
    ```

    >**注意：** 您已成功部署五個受控磁碟，每個的部署方式都不同。 做得好！

## 清除您的資源

如果您使用**自己的訂用帳戶**，請花點時間刪除實驗室資源。 如此可確保釋出資源，並將成本降到最低。 刪除實驗室資源的最簡單方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站中，選取資源群組，選取 [刪除資源群組]****，[輸入資源群組名稱]****，然後按一下 [刪除]****。
+ 使用 Azure PowerShell `Remove-AzResourceGroup -Name resourceGroupName`。
+ 使用 CLI `az group delete --name resourceGroupName`。

## 利用 Copilot 延伸學習

Copilot 可協助您了解如何使用 Azure 指令碼工具。 Copilot 也可在實驗室中未涵蓋的區域或您需要更多資訊的地方提供協助。 開啟 Edge 瀏覽器，然後選擇 [Copilot] (右上方)，或瀏覽至 *copilot.microsoft.com*。 請花幾分鐘的時間嘗試下列提示。

+ Azure Resource Manager 範本檔案的格式為何？ 請使用範例說明每個元件。 
+ 如何使用現有的 Azure Resource Manager 範本？
+ 將 Azure Resource Manager 範本和 Azure Bicep 範本進行比較和對比。 


## 透過自學型訓練深入了解

+ [使用 JSON ARM 範本部署 Azure 基礎結構](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)。 使用 Visual Studio Code 撰寫 JSON Azure Resource Manager 範本 (ARM 範本)，一致又可靠地將基礎結構部署至 Azure。
+ [檢閱適用於 Azure Cloud Shell 的功能和工具](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/)。 Cloud Shell 功能和工具。 
+ [使用 Windows PowerShell 管理 Azure 資源](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/)。 本課程模組說明如何安裝雲端服務管理所需的模組，並使用 PowerShell 命令對 Azure 虛擬機器、Azure 訂用帳戶和 Azure 儲存體帳戶等雲端資源執行簡單的管理工作。
+ [Bash 簡介](https://learn.microsoft.com/training/modules/bash-introduction/)。 使用 Bash 來管理 IT 基礎結構。
+ [建立您的第一個 Bicep 範本](https://learn.microsoft.com/training/modules/build-first-bicep-template/)。 請在 Bicep 範本中定義 Azure 資源。 改善部署的一致性和可靠性、減少所需的手動工作，並將部署縮放到整個環境。 您的範本將可透過使用參數、變數、運算式和模組，而變得有彈性且可重複使用。

## 重要心得

恭喜您完成此實驗室。 以下是本實驗室的主要重點。 

+ Azure Resource Manager 範本可讓您將解決方案中的所有資源作為一個群組進行部署、管理和監視，而無需分別處理這些資源。
+ Azure Resource Manager 範本是 JavaScript 物件標記法 (JSON) 檔案，可讓您以宣告方式 (而非使用指令碼) 來管理基礎結構。
+ 您可使用包含參數值的單獨 JSON 檔案，而不是將參數作為範本中的內嵌值傳遞。
+ Azure Resource Manager 範本可以透過各種方式部署，包括 Azure 入口網站、Azure PowerShell 和 CLI。
+ Bicep 是 Azure Resource Manager 範本的替代項。 Bicep 會使用宣告式語法來部署 Azure 資源。
+ Bicep 提供簡潔的語法、可靠的類型安全，並支援程式碼重複使用。 Bicep 能夠為您在 Azure 中的基礎結構即程式碼解決方案，提供第一級的製作體驗。


