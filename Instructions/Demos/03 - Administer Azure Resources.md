---
demo:
  title: 示範 03：管理員 註冊 Azure 資源
  module: Administer Administer Azure Resources
---
# 03 - 管理員 註冊 Azure 資源

## 示範 -- Azure 入口網站

在此示範中，我們將探索 Azure 入口網站。

**參考**：[管理 Azure 入口網站 設定和喜好設定](https://docs.microsoft.com/azure/azure-portal/set-preferences)

**參考**：[在 Azure 入口網站 中建立儀錶板](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards)

**參考**：[如何建立 Azure 支援 要求](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)

1. 存取 [Azure 入口網站]。

1. 選取頂端橫幅上的 [ **支援與疑難解答]** 圖示。 檢閱 **支援資源** 連結。 

1. 選取頂端橫幅上的 [設定]**** 圖示。 檢視 **設定** 。 

1. 使用左側功能表，然後選取 [ **儀錶板**]。 **使用**圖格資源庫**編輯**儀錶板。 討論自定義選項。

1. 示範如何搜尋和尋找資源。

1. 使用左上方功能表來找出 **[所有服務**]。 

1. 當您有時間檢閱其他功能時。
   
1. 詢問學生是否有任何問題。

## 示範 -- Cloud Shell

在此示範中，我們將使用 Cloud Shell 進行實驗。

**參考**： [Azure Cloud Shell 快速入門](https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?tabs=azurecli)

**設定 Cloud Shell**

1.   **存取 Azure 入口網站**。

1.   **按兩下頂端橫幅上的 Cloud Shell**  圖示。

1.  在 [歡迎使用殼層] 頁面上，注意 Bash 或 PowerShell 的選取項目。 選取 **[PowerShell**]。

1.  討論 Azure Cloud Shell 如何要求 Azure 檔案共用來保存檔案。 如有必要，請設定記憶體共用。 

**試驗 Azure PowerShell 和 Bash**

1. **請確定已選取 PowerShell 殼**層，並嘗試幾個命令。 例如， **Get-AzSubscription** 和 **Get-AzResourceGroup**。

1. 顯示自動完成的運作方式。 顯示如何清除螢幕 **cls**。 

1. 請確定已 **選取 Bash** 殼層，並嘗試幾個命令。 例如， **az account list** 和 **az resource list**。

1. 詢問學生是否對使用 PowerShell 或 Bash 命令有任何問題。 

**實驗 Cloud Shell 編輯器 （選擇性）**

1. 若要使用雲端編輯器，請選取 **大括弧** 圖示。

1. 從左側瀏覽窗格中選取檔案。 例如， **.profile**。

1. 請注意，在編輯器頂端橫幅上，針對 [設定] (文字大小和字型) 與 [上傳/下載檔案] 的選取項目。

1. 請注意儲存、關閉編輯器**和**開啟檔案**最右邊**** 的省略號 （**\...**）。 **

1. 在有時間時進行實驗，然後 **關閉** 雲端編輯器。

1. 關閉 Cloud Shell。

## 示範 -- 快速入門範本

在此示範中，我們將探索快速入門範本。

**參考：教學**課程 [- 建立和部署範本 - Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/template-tutorial-create-first-template?tabs=azure-powershell)

1. 從流覽至 [Azure 快速入門範本資源庫](https://learn.microsoft.com/en-us/samples/browse/?expanded=azure&products=azure-resource-manager)開始。 請注意，有 JSON 和 Bicep 範例。 

1. 詢問學生是否有任何感興趣的特定範本。 如果沒有，請選取範本。 例如， [使用標籤](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/vm-tags/) 板部署簡單的 Windows VM。

1. 討論 [部署至 Azure ** ] 按鈕如何**讓您直接透過 Azure 入口網站 部署範本。

1. **部署** JSON 範本，並討論如何編輯範本和參數檔案。 檢閱檔案的用途。 當您有時間時，請檢閱語法。 

1. 返回程式代碼範例庫，並找出 Bicep 範本。 例如，[建立建立標準 儲存體 帳戶](https://learn.microsoft.com/en-us/samples/azure/azure-quickstart-templates/storage-account-create/)。 

1. **部署** Bicep 範本，並討論如何編輯範本和參數檔案。 當您有時間時，請檢閱語法。 
