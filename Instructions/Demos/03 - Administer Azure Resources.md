---
demo:
  title: 示範 03：管理 Azure 資源
  module: Administer Administer Azure Resources
---
# 03 - 管理 Azure 資源

## 示範 -- Azure 入口網站

在此示範中，我們將探索Azure 入口網站。

**參考**：[管理Azure 入口網站設定和喜好設定](https://docs.microsoft.com/azure/azure-portal/set-preferences)

**參考**：[在Azure 入口網站中建立儀表板](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

**參考**：[如何建立Azure 支援要求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

1. 存取 Azure 入口網站。

1. 選取頂端橫幅上的 **[支援&疑難排解** ] 圖示。 檢閱 **支援資源** 連結。 

1. 選取頂端橫幅上的 [設定]**** 圖示。  檢 **閱外觀 + 啟動檢視** 設定。 

1. 使用左側功能表，然後選取 [ **儀表板**]。 使用**圖格庫****編輯**儀表板。 討論自訂選項。 

1. 詢問學生是否有關于如何使用Azure 入口網站的任何問題。 

## 示範 -- Cloud Shell

在此示範中，我們將使用 Cloud Shell 進行實驗。

**參考**：[Azure Cloud Shell快速入門](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

**設定 Cloud Shell**

1.  存取 **Azure 入口網站**。

1.  按一下頂端橫幅上的 **Cloud Shell**   icon。

1.  在 [歡迎使用殼層] 頁面上，注意 Bash 或 PowerShell 的選取項目。  選取 **[PowerShell**]。

1.  討論 Azure Cloud Shell如何要求 Azure 檔案共用才能保存檔案。 如有必要，請設定儲存體共用。 

**使用 Azure PowerShell 和 Bash 進行實驗**

1. 確定已選取 **PowerShell** 殼層，並嘗試幾個命令。 例如，**Get-AzSubscription**   和**Get-AzResourceGroup**。

1. 顯示自動完成的運作方式。 顯示如何清除畫面， **cls**。 

1. 確定已選取 **Bash 殼層** ，並嘗試幾個命令。 例如，**az account list**   和**az resource list**。

1. 詢問學生是否有任何關於使用 PowerShell 或 Bash 命令的問題。 

**使用 Cloud Shell 編輯器進行實驗， (選擇性) **

1. 若要使用雲端編輯器，請選取 **大括弧** 圖示。

1. 從左側瀏覽窗格中選取檔案。  例如， **.profile**。

1. 請注意，在編輯器頂端橫幅上，針對 [設定] (文字大小和字型) 與 [上傳/下載檔案] 的選取項目。

1. 請注意，省略號 (** \. ..**) 最右邊的 **[儲存**]、[**關閉編輯器**] 和 [**開啟檔案**]。

1. 如您有時間進行實驗，然後 **關閉**   [雲端編輯器]。

1. 關閉 Cloud Shell。

## 示範 -- 快速入門範本

在此示範中，我們將探索快速入門範本。

**參考**：[教學課程 - 建立&部署範本 - Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

1. 從流覽至 [Azure 快速入門範本資源庫](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager)開始。 請注意，有 JSON 和 Bicep 範例。 

1. 詢問學生是否有任何感興趣的特定範本。 如果沒有，請選取範本。 例如，使用標籤   範本[部署簡單的 Windows VM](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/)。

1. 討論 [ **部署至 Azure]**   按鈕如何讓您直接透過Azure 入口網站部署範本。

1. **部署** JSON 範本，並討論如何編輯範本和參數檔案。 檢閱檔案的用途。 當您有時間時，請檢閱語法。 

1. 返回程式碼範例庫，並找出 Bicep 範本。 例如， [建立建立標準儲存體帳戶](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/)。 

1. **部署** Bicep 範本，並討論如何編輯範本和參數檔案。 當您有時間時，請檢閱語法。 
