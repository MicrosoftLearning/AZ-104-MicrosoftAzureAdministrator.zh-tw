---
lab:
  title: 實驗室 03c：使用 Azure PowerShell 管理 Azure 資源（選擇性）
  module: Administer Azure Resources
---

# 實驗室 03c - 使用 Azure PowerShell 管理 Azure 資源
# 學生實驗室手冊

## 實驗案例

您和您的小組已探索 Azure 入口網站 中的基本 Azure 系統管理功能，例如布建資源，並根據資源群組加以組織。 您也探索了 Azure Resource Manager 範本。 接下來，您想要使用 Azure PowerShell 嘗試對等的工作。 在此實驗室中，您將利用 Azure Cloud Shell 中提供的 PowerShell 環境。

**注意：** **[互動式實驗室模擬](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)** (英文) 可供您以自己的步調完成此實驗室。 您可能會發現互動式模擬與託管實驗室之間稍有差異，但所示範的核心概念與想法均相同。 

## 目標

在此實驗中，您將會：

+ 工作 1：在 Azure Cloud Shell 中啟動 PowerShell 工作階段
+ 工作 2：使用 Azure PowerShell 建立資源群組和 Azure 受控磁碟
+ 工作 3：使用 Azure PowerShell 設定受控磁碟

## 預計時間：20 分鐘

## 架構圖

![架構的圖表。](../media/az104-lab03c-architecture-diagram.png)

### 指示

## 練習 1

## 工作 1：在 Azure Cloud Shell 中啟動 PowerShell 工作階段

在這項工作中，您將在 Cloud Shell 中開啟 PowerShell 工作階段。 Cloud Shell 內建於 Azure 入口網站，可讓您直接在入口網站中執行 PowerShell 或 Azure CLI 命令，而不需要安裝任何專案。 記憶體帳戶會保留命令歷程記錄，並提供儲存腳本的位置。

1. 在入口網站中，按一下 Azure 入口網站右上角的圖示，開啟 [Azure Cloud Shell] 。 或者，直接瀏覽至 `https://shell.azure.com`。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。 

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現**您未掛接任何儲存體**訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。 

1. 如果出現系統提示，請按一下 [建立儲存體] ，並等候 Azure Cloud Shell 窗格顯示。 

1. 確認在 [Cloud Shell] 窗格左上角的下拉式功能表中有顯示 [PowerShell] 。

## 工作 2：使用 Azure PowerShell 建立資源群組和 Azure 受控磁碟

在這項工作中，您將使用 Cloud Shell 內的 Azure PowerShell 會話來建立資源群組和 Azure 受控磁碟。 此工作旨在示範 Cloud Shell 和 Azure PowerShell 如何用來編寫一般工作的腳本。 接著，您可以使用 Azure 自動化、Logic Apps 或其他自動化工具來觸發腳本。

1. PowerShell 會針對 Cmdlet 使用*動詞-** 名詞*格式。 例如，要建立新資源群組的 Cmdlet 是 **New-AzResourceGroup**。 若要檢視如何使用 Cmdlet，請執行下列命令：

   ```powershell
   Get-Help New-AzResourceGroup
   ```


1. 若要從 Cloud Shell 內的 PowerShell 工作階段建立資源群組，請執行下列命令。 請注意，以美元符號 （$） 開頭的命令會建立可在後續命令中使用的變數。

   ```powershell
   $location = 'eastus'

   $rgName = 'az104-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
   >**注意**：如果 **az104-rg1** 已經存在，請在 $rgName** 參數中使用**不同的名稱。 

   ![建立資源群組的螢幕快照。 ](../media/az104-lab03c-createrg.png)

1. 若要擷取新建立之資源群組的屬性，請執行下列命令：

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```

1. 類似於建立新的資源群組，若要建立新的磁碟，請使用 **New-AzDisk** Cmdlet。 若要檢視如何使用 Cmdlet，請執行下列命令：

   ```powershell
   Get-Help New-AzDisk
   ```

1. 若要建立名為 **az104-disk1** 的新受控磁碟，請執行下列命令：

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -SkuName Standard_LRS

   $diskName = 'az104-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

   ![建立磁碟的螢幕快照。 ](../media/az104-lab03c-createdisk.png)

1. 若要擷取新建立磁碟的屬性，請執行下列命令：

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

## 工作 3：使用 Azure PowerShell 設定受控磁碟

在這項工作中，您將使用 Cloud Shell 內的 Azure PowerShell 會話來設定 Azure 受控磁碟。 此工作旨在示範 Cloud Shell 和 Azure PowerShell 如何用來編寫一般工作的腳本。

>**你知道嗎？**  您也可以使用 Azure 自動化、Logic Apps 或其他自動化工具來執行腳本。

1. 若要將 Azure 受控磁碟的大小增加為 **64 GB**，請從 Cloud Shell 中的 PowerShell 工作階段執行下列程式碼：

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. 若要確認變更生效，請執行下列命令：

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. 若要將目前的 SKU 確認為 **Standard_LRS**，請執行下列命令：

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

   ![更新 SKU 的螢幕快照。](../media/az104-lab03c-updatesku.png)

1. 若要將磁碟效能 SKU 變更為 **進階版_LRS**，請從 Cloud Shell 內的 PowerShell 會話執行下列命令：

   ```powershell
   New-AzDiskUpdateConfig -SkuName Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. 若要確認變更生效，請執行下列命令：

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

   ![更新 sku final 的螢幕快照。](../media/az104-lab03c-updatesku2.png)

>**您注意到了嗎？** PowerShell Cmdlet 會使用常見的動詞命令，例如 Get **、**New** 和 **Update****。 從 Get** 開始的 **Cmdlet 通常會擷取設定的相關信息。 從 New** 開始的 **Cmdlet 通常會建立新的資源。 從 Update** 開始的 **Cmdlet 通常會設定現有的資源。

## 檢閱

恭喜！ 您已成功啟動 Azure Cloud Shell 工作階段、使用 PowerShell 建立資源群組、使用 PowerShell 建立受控磁碟，以及使用 PowerShell 設定受控磁碟。
