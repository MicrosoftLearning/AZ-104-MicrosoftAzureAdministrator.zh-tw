---
lab:
  title: 實驗室 03d：使用 Azure CLI 管理 Azure 資源
  module: Administer Azure Resources
---

# 實驗室 03d - 使用 Azure CLI 管理 Azure 資源
# 學生實驗手冊

## 實驗案例

現在，您已探索與佈建資源相關聯的基本 Azure 管理功能，並使用 Azure 入口網站、Azure Resource Manager 範本和 Azure PowerShell 根據資源群組加以組織，您必須使用 Azure CLI 來執行對等的工作。 為了避免安裝 Azure CLI，您將利用 Azure Cloud Shell 中提供的 Bash 環境。

**注意：** **[互動式實驗室模擬](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%207)** (英文) 可供您以自己的步調完成此實驗室。 您可能會發現互動式模擬與託管實驗室之間稍有差異，但所示範的核心概念與想法均相同。 

## 目標

在此實驗中，您將會：

+ 工作 1：在 Azure Cloud Shell 中啟動 Bash 工作階段
+ 工作 2：使用 Azure CLI 建立資源群組和 Azure 受控磁碟
+ 工作 3：使用 Azure CLI 設定受控磁碟

## 預估時間：20 分鐘

## 架構圖

![image](../media/lab03d.png)

### 指示

## 練習 1

## 工作 1：在 Azure Cloud Shell 中啟動 Bash 工作階段

在這項工作中，您將在 Cloud Shell中開啟 Bash 工作階段。 

1. 在入口網站中，按一下 Azure 入口網站右上角的圖示，開啟 **Azure Cloud Shell**。

1. 如果系統提示您選取 **Bash** 或 **PowerShell**，請選取 **Bash**。 

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現**您未掛接任何儲存體**訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。 

1. 如果出現系統提示，請按一下 [建立儲存體]，並等候 Azure Cloud Shell 窗格顯示。 

1. 確認在 [Cloud Shell] 窗格左上角的下拉式功能表中有顯示 [Bash]。

## 工作 2：使用 Azure CLI 建立資源群組和 Azure 受控磁碟

在此工作中，您將使用 Azure CLI 工作階段在 Cloud Shell 內建立資源群組和 Azure 受控磁碟。

1. 若要在上一個實驗室所建立 **az104-03c-rg1** 資源群組的相同 Azure 區域中建立資源群組，請從 Cloud Shell 內的 Bash 工作階段執行下列程式碼：

   ```sh
   LOCATION=$(az group show --name 'az104-03c-rg1' --query location --out tsv)

   RGNAME='az104-03d-rg1'

   az group create --name $RGNAME --location $LOCATION
   ```
1. 若要擷取新建立資源群組的屬性，請執行下列程式碼：

   ```sh
   az group show --name $RGNAME
   ```
1. 若要使用本課程模組先前實驗室中所建立的受控磁碟相同特性建立新的受控磁碟，請從 Cloud Shell 內的 Bash 工作階段執行下列程式碼：

   ```sh
   DISKNAME='az104-03d-disk1'

   az disk create \
   --resource-group $RGNAME \
   --name $DISKNAME \
   --sku 'Standard_LRS' \
   --size-gb 32
   ```
    >**注意**：使用多行語法時，請確定每一行結尾都是反斜線 (`\`)，且每行的開頭沒有前置空格。

1. 若要擷取新建立磁碟的屬性，請執行下列程式碼：

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME
   ```

## 工作 3：使用 Azure CLI 設定受控磁碟

在這項工作中，您將使用 Azure CLI 工作階段在 Cloud Shell 內管理 Azure 受控磁碟的設定。 

1. 若要將 Azure 受控磁碟的大小增加為 **64 GB**，請從 Cloud Shell 中的 Bash 工作階段執行下列程式碼：

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --size-gb 64
   ```

1. 若要確認變更是否生效，請執行下列程式碼：

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query diskSizeGb
   ```

1. 若要將磁碟效能 SKU 變更為 **Premium_LRS**，請從 Cloud Shell 內的 Bash 工作階段執行下列程式碼：

   ```sh
   az disk update --resource-group $RGNAME --name $DISKNAME --sku 'Premium_LRS'
   ```

1. 若要確認變更是否生效，請執行下列程式碼：

   ```sh
   az disk show --resource-group $RGNAME --name $DISKNAME --query sku
   ```

## 清除資源

 > **注意**：請記得移除您不再使用的任何新建立的 Azure 資源。 移除未使用的資源可確保您不會看到非預期的費用。

 > **注意**：如果無法立即移除實驗資源，請不要擔心。 有時候資源具有相依性，需要經過很長的時間才能刪除。 這是監視資源使用量的常見系統管理員工作，因此只需定期檢閱入口網站中的資源，查看清除的運作情況便可。 

1. 在 Azure 入口網站中，在 [Cloud Shell] 窗格內開啟 [Bash] Shell 工作階段。

1. 執行下列命令，列出在本課程模組的任何實驗中建立的所有資源群組：

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].name" --output tsv
   ```

1. 執行下列命令，刪除您在本課程模組的任何實驗中建立的所有資源群組：

   ```sh
   az group list --query "[?starts_with(name,'az104-03')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >**注意**：此命令會以非同步方式執行 (由 --nowait 參數決定)，所以您將可以在相同的 Bash 工作階段中，立即執行另一個 Azure CLI 命令，但這需要幾分鐘才會移除資源群組。

## 檢閱

在此實驗中，您已：

- 在 Azure Cloud Shell 中啟動 Bash 工作階段
- 使用 Azure CLI 建立資源群組和 Azure 受控磁碟
- 使用 Azure CLI 設定受控磁碟
