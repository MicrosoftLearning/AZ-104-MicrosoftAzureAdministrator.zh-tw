---
lab:
  title: 實驗室 01：管理 Microsoft Entra ID 身分識別
  module: Administer Identity
---

# 實驗室 01 - 管理 Microsoft Entra ID 身分識別

## 實驗室簡介

這是 Azure 管理員 istrators 系列實驗室中的第一個。 在此實驗室中，您將了解使用者和群組。 使用者和群組是身分識別解決方案的基本建置組塊。 您也熟悉基本系統管理員工具。 這些工具包括 Azure 入口網站、Cloud Shell、Azure PowerShell 和命令行介面 （CLI）。 此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟會顯示在美國東部。

## 預估時間：40 分鐘

## 實驗案例

您的組織正在建置新的實驗室環境，以預先測試應用程式和服務。  一些工程師正被雇用來管理實驗室環境，包括虛擬機。 為了允許工程師使用 Microsoft Entra ID 進行驗證，您已負責布建使用者和組帳戶。 若要將系統管理額外負荷降到最低，應該根據職稱自動更新群組的成員資格。 您也需要知道如何刪除使用者，以防止工程師離開組織之後存取。

## 互動式實驗室模擬

您可能會發現互動式實驗室模擬對本主題很有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。 

+ [管理 Entra ID Identities](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201)*。 建立和設定使用者並指派給群組。 建立 Azure 租使用者並管理來賓帳戶。 

## 架構圖
![實驗室 01 架構的圖表。](../media/az104-lab01-user-and-groups2.png)

## 工作

+ 工作 1：熟悉 Azure 入口網站。
+ 工作 2：建立資源群組。
+ 工作 3：熟悉用戶帳戶。
+ 工作 4：建立群組並新增成員。
+ 工作 5：熟悉 Cloud Shell。
+ 工作 6：使用 Azure PowerShell 練習。
+ 工作 7：使用 Bash 殼層練習。

## 工作 1：熟悉 Azure 入口網站

在這項工作中，您已熟悉 Azure 入口網站。 「Azure 入口網站」是統一的 Web 式主控台，提供有別於命令列工具的替代方案。 透過 Azure 入口網站，您可使用圖形化使用者介面來管理 Azure 訂用帳戶。 您可以在入口網站中建置、管理及監視從簡單 Web 應用程式到複雜雲端部署的所有專案。 

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

1. 選取左上方功能表圖示，開始您的 Azure 入口網站 導覽。 

   + 選取 [ **首頁** ] 以檢視最近的服務和資源。 您也可以建立我的最愛。 
   + 針對自定義檢視選取 **[儀錶板** ]。 [](https://learn.microsoft.com/zure/azure-portal/azure-portal-dashboards)儀錶板是 Azure 入口網站 中雲端資源的專注且有組織的檢視。 使用儀錶板作為工作區，您可以在其中監視資源並快速啟動日常作業的工作。
   + 選取 **[所有服務** ] 以檢視 Azure 服務的分類清單。

1. 您可以使用入口網站頂端中心的搜尋方塊，更 **快速地搜尋資源、服務和檔** 。 搜尋方塊提供服務或資源的自動完成和建議。 例如，請嘗試 `virt` 並注意建議的相符專案。

1. 在頂端功能表欄右側，選取 **設定** 圖示。 設定 可讓您自定義入口網站外觀。

1. 最後，右上角是您的用戶帳戶資訊。
   
## 工作 2：建立新的資源群組

在這項工作中，您會建立新的資源群組。 資源群組是相關資源的群組（例如專案、部門或應用程式的所有資源）。 針對本課程中的每個實驗室，您都會建立資源群組。 
    
1. 在 Azure 入口網站中，搜尋並選取 [資源群組]。
   
1. 在 [ **資源群組]** 刀鋒視窗上，按兩下 [ **+ 建立**]，並提供必要的資訊。 

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶名稱 | 訂用帳戶 |
    | 資源群組名稱 | `az104-rg1` |
    | Location | 您的區域 |
    
1. 按一下 [檢閱 + 建立]  ，然後按一下 [建立]  。

    >**注意**：等候資源群組部署。 **使用通知**圖示 （右上方） 來追蹤部署的進度。

1. 回到 [資源群組]**** 刀鋒視窗，重新整理頁面，並確認新的資源群組出現在資源群組清單中。

    ![資源群組清單的螢幕快照。](../media/az104-lab01-create-resource-group.png)

## 工作 3：熟悉用戶帳戶。

在這項工作中，您會熟悉用戶帳戶和配置檔。 您也會檢視群組成員資格。

1. 在 Azure 入口網站 中，搜尋並選取 `Microsoft Entra ID`。

1. 在 [ **管理]** 區段中，選取 [ **使用者]** 刀鋒視窗。 

1. 從頂端功能表中選取 **[新增使用者** ]。 請注意 [建立新使用者 **] 和 **[邀請外部使用者**] 的選項**。 建立使用者超出此實驗室的範圍。 

1. **搜尋** 並選取您的用戶帳戶。 您的用戶帳戶信息會顯示在入口網站的右上角。 

1. 選取 [ **屬性]** 索引標籤，並檢閱可提供給用戶帳戶的所有配置檔資訊。 



## 工作 4：建立群組並新增成員

在此工作中，您會建立群組。 群組會用於用戶帳戶或裝置。 某些群組具有靜態指派的成員。 某些群組具有動態指派的成員。 動態群組會根據用戶帳戶或裝置的屬性自動更新。 靜態群組需要更多的系統管理額外負荷（系統管理員必須手動新增和移除成員）。

1. 在 Azure 入口網站 中，搜尋並選取 [**群組**]。

1. 請注意成員資格類型 **、**來源**和**類型**等**群組資訊。 另請注意，群組中的成員數目。 

1. 選取 **[+ 新增群組** ]，然後建立新的群組。 

    | 設定 | 值 |
    | --- | --- |
    | 群組類型 | **安全性** |
    | 群組名稱 | `IT Lab Administrators` （ 如果這個名稱無法使用，請調整名稱 ） |
    | 群組描述 | `Administrators that manage the IT lab` |
    | 成員資格類型 | **已指派** |

    >**注意**：您的 **成員資格類型** 下拉式清單可能會呈現灰色。您可以在此從指派的群組切換至動態群組。 這需要 Entra ID 進階版 P1 或 P2 授權。

    ![建立指派群組的螢幕快照。](../media/az104-lab01-create-assigned-group.png)

1. 按一下 [未選取任何成員]。

1. 從 [ **新增成員]** 刀鋒視窗中，搜尋您的用戶帳戶。 **選取** 要新增至群組的用戶帳戶。 

1. 按兩下 **[建立** ] 以完成群組的建立。 

## 工作 5：熟悉 Cloud Shell。

在這項工作中，您會使用 Azure Cloud Shell。 Azure Cloud Shell 是一個互動式、已驗證、可存取瀏覽器的終端機，可用來管理 Azure 資源。 其可讓您彈性地從 Bash 或 PowerShell 中，選擇最適合您工作方式的殼層體驗。 

1. 選取 **Azure 入口網站右上方的 Cloud Shell** 圖示。 或者，您可以直接巡覽至 `https://shell.azure.com`。

1. 當系統提示您選取 Bash 或 PowerShell 時，請選擇 **[PowerShell****]。**** ** Bash 會在下一個工作中使用。

    >**你知道嗎？**  如果您大多使用Linux系統，Azure CLI會感覺更自然。 如果您大多使用 Windows 系統，Azure PowerShell 會感覺更自然。 

1. 在 [ **您沒有掛接存儲設備]** 畫面上，選取 [ **顯示進階設定** ]，並提供必要資訊。 完成時，請選取 [ **建立記憶體**]。 

    | 設定 | 值 |
    |  -- | -- |
    | 資源群組 | **建立新資源群組** |
    | 儲存體 帳戶 （建立新帳戶，使用全域唯一名稱 （例如： cloudshellstoragemystorage） | **cloudshellxxxxxxx** |
    | 檔案分享 （新增） | **shellstorage** |

    >**注意：** 如果您在託管的實驗室環境中工作，則必須在每次建立新的實驗室環境時設定 Cloud Shell 記憶體。

    >**注意：** 工作 6 可讓您練習 Azure PowerShell。 工作 7 可讓您練習 CLI。 您可以執行這兩項工作，或只執行您最感興趣的工作。 

## 工作 6：使用 Azure PowerShell 練習

在這項工作中，您會使用 Cloud Shell 內的 Azure PowerShell 會話來建立資源群組和 Azure AD 群組。

    >**Note:** Use the arrow keys to move through the command history. Use the tab key to autocomplete commands and parameters.

1. 繼續在 Cloud Shell 中工作。 隨時使用 **cls** 清除命令視窗。

1. Azure PowerShell 會*針對 Cmdlet 使用動詞-** 名詞*格式。 例如，要建立新資源群組的 Cmdlet 是 **New-AzResourceGroup**。 若要檢視如何使用 Cmdlet，請執行 Get-Help 命令。

   ```powershell
   Get-Help New-AzResourceGroup -detailed
   ```
1. 若要從 Cloud Shell 內的 PowerShell 工作階段建立資源群組，請執行下列命令。 請注意，以美元符號 （$） 開頭的命令會建立可在後續命令中使用的變數。 請確定您收到成功訊息。 

   ```powershell
   $location = 'eastus'
   $rgName = 'az104-rg-ps'
   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. 若要擷取新建立之資源群組的屬性，請執行下列命令：

   ```powershell
   Get-AzResourceGroup -Name $rgName
   ```

1. 現在，讓我們嘗試建立新的 Azure 群組。

   ```powershell
   Get-Help New-AzureADGroup -detailed
   ```

1. 使用說明中的範例，請嘗試這些命令。 請注意，您必須先連線到 Azure AD。

   ```powershell
   Connect-AzureAD 
   New-AzureADGroup -DisplayName "MyPSgroup" -MailEnabled $false -SecurityEnabled $true -MailNickName "MyPSgroup"
   ```

1. 返回 Azure 入口網站。 確認您有新的資源群組和新的 Azure 群組。 您可能需要重新整理頁面。 

## 工作 7：使用 Bash 殼層練習

在這項工作中，您會使用 Cloud Shell 內的 Azure CLI 工作階段來建立資源群組和 Azure 群組。

1. 在 Cloud Shell 中繼續。 使用下拉式清單切換至 **Bash**。

    >**注意：** 使用箭頭鍵來移動命令歷程記錄。 使用索引標籤索引鍵來自動完成命令和參數。 

1. Azure CLI 使用容易閱讀的語法。 例如，若要與資源群組互動，命令是 **az group**。  

   ```sh
   az group --help
   ```

1. 建立**** 選項看起來很有希望。 請注意，大寫名稱會建立您可以在後續命令中參考的變數。 

   ```sh
   RGNAME='az104-rg1-cli'
   LOCATION='eastus'
   az group create --name $RGNAME --location $LOCATION
   ```
   
1. 若要確認並擷取新建立之資源群組的屬性，請嘗試 **show** 命令。 

   ```sh
   az group show --name $RGNAME
   ```
1. 現在，讓我們使用說明來深入瞭解如何建立 Azure 群組。

    ```sh
    az ad group --help
    ```

1. **建立** 群組並 **列出** 要驗證的群組。

   ```sh
   az ad group create --display-name MyCLIgroup --mail-nickname MyCLIgroup
   az ad group list
   ```

1. 返回 Azure 入口網站。 確認您有新的資源群組和新的 Azure 群組。 您可能需要重新整理頁面。   
    
## 檢閱實驗室的要點

恭喜您完成實驗室。 以下是此實驗室的一些主要方法：

+ Azure 入口網站 是開始建立和管理 Azure 資源的好方法。 管理員 istrators 可以自定義入口網站並共用儀錶板。
+ 資源群組是將相關資源分組的方式。 您可以針對專案、部門或應用程式使用資源群組。 這可讓您輕鬆地管理及監視一組相關資源。 
+ Microsoft Entra ID 中有不同類型的使用者帳戶。 每個用戶帳戶類型都有預期工作範圍特有的存取層級。 
+ 將帳戶群組在一起的相關用戶或裝置。 群組成員資格可以靜態或動態指派。 
+ Cloud Shell 是一個互動式且已驗證的終端機，可用來管理 Azure 資源。 Cloud Shell 提供 Bash 或 Azure PowerShell 的存取權。
+ Azure PowerShell 和 Bash 提供腳本方式來建立資源。 

## 清除您的資源

如果您使用自己的訂用帳戶，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站 中，選取資源群組、選取 **[刪除資源群組**]、**輸入資源組名**，然後按兩下 [**刪除**]。

+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName`。

+ 使用 CLI， `az group delete --name resourceGroupName`。

