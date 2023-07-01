---
lab:
  title: 實驗室 03c：使用 Azure PowerShell 管理 Azure 資源
  module: Administer Azure Resources
---

# 實驗室 03c - 使用 Azure PowerShell 管理 Azure 資源
# 學生實驗室手冊

## 實驗案例

現在，您已探索與佈建資源相關聯的基本 Azure 管理功能，並使用 Azure 入口網站、Azure Resource Manager 範本根據資源群組加以組織，您必須使用 Azure PowerShell 來執行對等的工作。 若要避免安裝 Azure PowerShell 模組，您將有效率的調控 Azure Cloud Shell 中提供的 PowerShell 環境。

**注意：** **[互動式實驗室模擬](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%206)** (英文) 可供您以自己的步調完成此實驗室。 您可能會發現互動式模擬與託管實驗室之間稍有差異，但所示範的核心概念與想法均相同。 

## 目標

在此實驗中，您將會：

+ 工作 1：在 Azure Cloud Shell 中啟動 PowerShell 工作階段
+ 工作 2：使用 Azure PowerShell 建立資源群組和 Azure 受控磁碟
+ 工作 3：使用 Azure PowerShell 設定受控磁碟

## 預計時間：20 分鐘

## 架構圖

![image](../media/lab03c.png)

### 指示

> **注意**：針對您所建立的任何虛擬機器或使用者帳戶，一律建立您自己的安全密碼。 如果系統已為您建立虛擬機器，請使用入口網站中的 [重設密碼] 來更新密碼。 

## 練習 1

## 工作 1：在 Azure Cloud Shell 中啟動 PowerShell 工作階段

在這項工作中，您將在 Cloud Shell 中開啟 PowerShell 工作階段。 

1. 在入口網站中，按一下 Azure 入口網站右上角的圖示，開啟 [Azure Cloud Shell] 。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell] 。 

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現**您未掛接任何儲存體**訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。 

1. 如果出現系統提示，請按一下 [建立儲存體] ，並等候 Azure Cloud Shell 窗格顯示。 

1. 確認在 [Cloud Shell] 窗格左上角的下拉式功能表中有顯示 [PowerShell] 。

## 工作 2：使用 Azure PowerShell 建立資源群組和 Azure 受控磁碟

在此工作中，您將使用 Azure PowerShell 工作階段在 Cloud Shell 內建立資源群組和 Azure 受控磁碟

1. 若要在上一個實驗室所建立 **az104-03c-rg1** 資源群組的相同 Azure 區域中建立資源群組，請從 Cloud Shell 內的 PowerShell 工作階段執行下列程式碼：

   ```powershell
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. 若要擷取新建立資源群組的屬性，請執行下列程式碼：

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```
1. 若要使用本課程模組先前實驗室中所建立的受控磁碟相同特性建立新的受控磁碟，請執行下列程式碼：

   ```powershell
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. 若要擷取新建立磁碟的屬性，請執行下列程式碼：

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

## 工作 3：使用 Azure PowerShell 設定受控磁碟

在這項工作中，您將使用 Azure PowerShell 工作階段在 Cloud Shell 內管理 Azure 受控磁碟的設定。 

1. 若要將 Azure 受控磁碟的大小增加為 **64 GB**，請從 Cloud Shell 中的 PowerShell 工作階段執行下列程式碼：

   ```powershell
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. 若要驗證變更是否生效，請執行下列程式碼：

   ```powershell
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. 若要驗證目前的 SKU 是否為 **Standard_LRS**，請執行下列程式碼：

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. 若要將磁碟效能 SKU 變更為 **Premium_LRS**，請從 Cloud Shell 內的 PowerShell 工作階段執行下列程式碼：

   ```powershell
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. 若要驗證變更是否生效，請執行下列程式碼：

   ```powershell
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

## 清除資源

   >**注意**：請勿刪除您在此實驗中部署的資源。 您將在本課程模組的下一個實驗中引用這些資源。

## 檢閱

在本實驗中，您已：

- 在 Azure Cloud Shell 中啟動 PowerShell 工作階段
- 使用 Azure PowerShell 建立資源群組和 Azure 受控磁碟
- 使用 Azure PowerShell 設定受控磁碟
