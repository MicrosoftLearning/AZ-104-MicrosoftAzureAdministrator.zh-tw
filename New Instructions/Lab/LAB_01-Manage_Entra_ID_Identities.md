---
lab:
  title: 實驗室 01：管理 Microsoft Entra ID 身分識別
  module: Administer Identity
---

# 實驗室 01 - 管理 Microsoft Entra ID 身分識別

## 實驗室簡介

這是 Azure 管理員istrators 系列實驗室中的第一個。 在此實驗室中，您將瞭解使用者和群組。 使用者和群組是身分識別解決方案的基本建置組塊。 您也熟悉基本系統管理員工具。 

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟會顯示在美國 ** 東部 ** 。

## 預估時間：40 分鐘

## 實驗案例

您的組織正在建置新的實驗室環境，以預先測試應用程式和服務。  一些工程師正被雇用來管理實驗室環境，包括虛擬機器。 為了允許工程師使用 Microsoft Entra ID 進行驗證，您已負責布建使用者和群組帳戶。 若要將系統管理額外負荷降到最低，應該根據職稱自動更新群組的成員資格。 您也需要知道如何刪除使用者，以防止工程師離開組織之後存取。

## 互動式實驗室模擬

您可能會發現互動式實驗室模擬對本主題很有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。 

+ [管理 Entra ID Identities ](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%201) 。 建立和設定使用者並指派給群組。 建立 Azure 租使用者並管理來賓帳戶。 

## 架構圖
![實驗室 01 架構的圖表。](../media/az104-lab01-user-and-groups2.png)

## 工作

+ 工作 1：建立資源群組。
+ 工作 2：建立及設定使用者帳戶。
+ 工作 3：建立群組並新增成員。
+ 工作 4：熟悉 Cloud Shell。
+ 工作 5：使用 Azure PowerShell 練習。
+ 工作 6：使用 Bash 殼層練習。

## 工作 1：建立資源群組

在此工作中，您會建立資源群組。 資源群組是相關資源的群組。 例如，專案、部門或應用程式的所有資源。 

>**注意： ** 針對本課程中的每個實驗室，您將建立新的資源群組。 這可讓您快速找出及管理實驗室資源。 

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

    >**注意： ** 所有實驗室都會使用Azure 入口網站。 如果您不熟悉 Azure，請在頂端搜尋方塊中輸入 `Quickstart Center` 。 然後幾分鐘，觀看 ** Azure 入口網站 ** 影片中的開始使用。 即使您之前曾使用過入口網站，您也會找到流覽和自訂介面的一些秘訣和訣竅。 
   
1. 在Azure 入口網站中，搜尋並選取 `Resource groups` 。
   
1. 在 [ ** 資源群組] ** 刀鋒視窗上，按一下 [ ** + 建立 ** ]，並提供必要的資訊。 

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶名稱 | 訂用帳戶 |
    | 資源群組名稱 | `az104-rg1` |
    | Location | **美國東部** |

    >**注意： ** 所有實驗室都使用 ** 美國 ** 東部。 **觀看快速入門中心 ** 選取 ** 最佳區域影片，以瞭解選取區域 ** 時要考慮什麼。  
    
1. 按一下 [檢閱 + 建立]  ，然後按一下 [建立]  。

    >**注意 ** ：等候資源群組部署。 **使用通知 ** 圖示（右上方）來追蹤部署的進度。

1. 選取 ** [移至資源 ** ]，重新整理頁面，並確認新的資源群組會出現在資源群組清單中。

    ![資源群組清單的螢幕擷取畫面。](../media/az104-lab01-create-resource-group.png)

## 工作 2：建立及設定使用者帳戶

在這項工作中，您將建立及設定使用者帳戶。 使用者帳戶會儲存使用者資料，例如名稱、部門、位置和連絡人資訊。

1. 繼續Azure 入口網站。 

1. 搜尋並選取 `Microsoft Entra ID`。

1. Microsoft Entra ID 是 Azure 的雲端式身分識別和存取管理解決方案。 請花幾分鐘的時間熟悉窗格中所列的一些功能。 

   + **管理員單位 ** 可讓您將使用者、群組或裝置分組成單一可管理單位。
   + **** 授權可讓您執行像是 gte 試用版或購買授權、管理您擁有的授權，以及將授權指派給使用者和群組等工作。
   + **自助式密碼重設 ** 可讓您的使用者隨時從任何位置管理其任何裝置的密碼。

1. 選取 ** [使用者 ** ]，然後在 [ ** 新增使用者 ** ] 下拉式清單中選取 ** [建立新使用者 ** ]。 請注意邀請和外部使用者的選取 ** 專案 ** 。 

1. 使用下列設定建立新使用者（將其他人保留為預設值）。 在 [ ** 屬性] ** 索引標籤上，請注意使用者帳戶中包含的所有不同類型的資訊。 

    | 設定 | 值 |
    | --- | --- |
    | 使用者主體名稱 | `az104-user1` |
    | Display name | `az104-user1` |
    | 自動產生密碼 | de-select |
    | 初始密碼 | **提供安全的密碼** |
    | 職稱 （屬性索引標籤） | `Cloud Administrator` |
    | 部門 （屬性索引標籤） | `IT` |
    | 使用位置 （屬性索引標籤） | **美國** |

1. 完成檢閱之後，請選取 [檢閱 + 建立 ** ]，然後 ** 選取 ** [建立 ** ]。

>**注意： ** 您不太可能個別建立使用者帳戶。 您知道貴組織如何規劃建立和管理使用者帳戶？

### 工作 4：建立群組並新增成員

在這項工作中，您會建立群群組帳戶。 群組帳戶可以包含使用者帳戶或裝置。 這是將成員指派給群組的兩種基本方式：靜態和動態方式。 靜態群組需要系統管理員手動新增和移除成員。  動態群組會根據使用者帳戶或裝置的屬性自動更新。 例如，職稱。 

1. 在Azure 入口網站中，搜尋並選取 `Groups` 。

1. 選取 ** [+ 新增群組 ** ]，然後建立新的群組。 

    | 設定 | 值 |
    | --- | --- |
    | 群組類型 | **安全性** |
    | 群組名稱 | `IT Lab Administrators` （如果這個名稱無法使用，請調整名稱） |
    | 群組描述 | `Administrators that manage the IT lab` |
    | 成員資格類型 | **已指派** |

    >**注意 ** ：您的 ** 成員資格類型 ** 下拉式清單可能會呈現灰色。如果您有 Entra ID 進階版 P1 或 P2 授權，您可以在其中選取動態群組。 

    ![建立指派群組的螢幕擷取畫面。](../media/az104-lab01-create-assigned-group.png)

1. 按一下 [未選取任何成員]。

1. 從 [ ** 新增成員] ** 刀鋒視窗中，搜尋並選取 ** az104-user1 ** ，並將其新增至群組。 

1. 按一下 [ ** 建立] ** 以部署群組。

1. 請花幾分鐘的時間熟悉其他群組設定。

   + **到期 ** 可讓您設定天數的群組存留期。 群組必須由擁有者更新。
   + **命名原則 ** 可讓您設定封鎖字組，並將前置詞或尾碼新增至組名。

>**注意： ** 您可能會管理大量群組。 您的組織是否有建立群組和新增成員的計畫？

## 工作 5：熟悉 Cloud Shell。

在這項工作中，您會使用 Azure Cloud Shell。 Azure Cloud Shell 是一個互動式、已驗證、可存取瀏覽器的終端機，可用來管理 Azure 資源。 其可讓您彈性地從 Bash 或 PowerShell 中，選擇最適合您工作方式的殼層體驗。 您將在此課程中經常使用此工具。 

1. 選取 ** Azure 入口網站右上方的 Cloud Shell ** 圖示。 或者，您可以直接巡覽至 `https://shell.azure.com` 。

1. 當系統提示您選取 Bash 或 PowerShell 時，請選取 ** [PowerShell ** ** ]。 ** ** ** 

    >**你知道嗎？**  如果您大多使用 Linux 系統，Azure CLI 會感覺更自然。 如果您大多使用 Windows 系統，Azure PowerShell 會感覺更自然。 

1. 在 [ ** 您沒有掛接存放裝置] ** 畫面上，選取 [ ** 顯示進階設定 ** ]，並提供必要資訊。 完成時，請選取 [ ** 建立儲存體 ** ]。 

    | 設定 | 值 |
    |  -- | -- |
    | 資源群組 | **建立新資源群組** |
    | 儲存體帳戶 （建立新帳戶，使用全域唯一名稱 （例如： cloudshellstoragemystorage）） | **cloudshellxxxxxxx** |
    | 檔案共用 （新建） | **shellstorage** |

    >**注意： ** 工作 6 可讓您練習 Azure PowerShell。 工作 7 可讓您練習 CLI。 您可以執行這兩項工作，或只執行您最感興趣的工作。 

## 工作 6：使用 Azure PowerShell 練習

在這項工作中，您會使用 Cloud Shell 內的 Azure PowerShell 會話來建立資源群組和 Azure AD 群組。 您可以在整個課程中使用 Azure PowerShell 腳本。 

    >**Note:** Use the arrow keys to move through the command history. Use the tab key to autocomplete commands and parameters.

1. 繼續在 Cloud Shell 中工作。 隨時使用 ** cls ** 清除命令視窗。

1. Azure PowerShell 會 * 針對 Cmdlet 使用動詞 * * - 名詞 * 格式。 例如，要建立新資源群組的 Cmdlet 是 ** New-AzResourceGroup ** 。 若要檢視如何使用 Cmdlet，請執行 Get-Help 命令。

   ```powershell
   Get-Help New-AzResourceGroup -detailed
   ```
1. 若要從 Cloud Shell 內的 PowerShell 會話建立資源群組，請執行下列命令。 請注意，以貨幣符號 （$） 開頭的命令會建立可在後續命令中使用的變數。 請確定您收到成功訊息。 

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

在這項工作中，您會使用 Cloud Shell 內的 Azure CLI 會話來建立資源群組和 Azure 群組。 您可以在整個課程中使用 Azure CLI 腳本。 

1. 在 Cloud Shell 中繼續。 使用下拉式清單切換至 ** Bash ** 。

    >**注意： ** 使用方向鍵來移動命令歷程記錄。 使用索引標籤索引鍵來自動完成命令和參數。 

1. Azure CLI 使用容易閱讀的語法。 例如，若要與資源群組互動，命令是 ** az group ** 。  

   ```sh
   az group --help
   ```

1. 建立 ** ** 選項看起來很有希望。 請注意，大寫名稱會建立您可以在後續命令中參考的變數。 

   ```sh
   RGNAME='az104-rg1-cli'
   LOCATION='eastus'
   az group create --name $RGNAME --location $LOCATION
   ```
   
1. 若要確認並擷取新建立之資源群組的屬性，請嘗試 ** show ** 命令。 

   ```sh
   az group show --name $RGNAME
   ```
1. 現在，讓我們使用說明來深入瞭解如何建立 Azure 群組。

    ```sh
    az ad group --help
    ```

1. **建立 ** 群組並 ** 列出 ** 要驗證的群組。

   ```sh
   az ad group create --display-name MyCLIgroup --mail-nickname MyCLIgroup
   az ad group list
   ```

1. 返回 Azure 入口網站。 確認您有新的資源群組和新的 Azure 群組。 您可能需要重新整理頁面。   
    
## 重要心得

恭喜您完成實驗室。 以下是此實驗室的一些主要方法：

+ Azure 入口網站是開始建立和管理 Azure 資源的好方法。 管理員istrators 可以自訂入口網站並共用儀表板。
+ 資源群組是將相關資源分組的方式。 您可以針對專案、部門或應用程式使用資源群組。 這可讓您輕鬆地管理及監視一組相關資源。 
+ Microsoft Entra ID 中有不同類型的使用者帳戶。 每個使用者帳戶類型都有預期工作範圍特有的存取層級。 
+ 將帳戶群組在一起的相關使用者或裝置。 群組成員資格可以靜態或動態指派。 
+ Cloud Shell 是一個互動式且已驗證的終端機，可用來管理 Azure 資源。 Cloud Shell 提供 Bash 或 Azure PowerShell 的存取權。
+ Azure PowerShell 和 Bash 提供腳本方式來建立資源。 

## 清除您的資源

如果您使用自己的訂用帳戶，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在Azure 入口網站中，選取資源群組、選取 ** [刪除資源群組 ** ]、 ** 輸入資源組名 ** ，然後按一下 [ ** 刪除 ** ]。
+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName` 。
+ 使用 CLI， `az group delete --name resourceGroupName` 。

