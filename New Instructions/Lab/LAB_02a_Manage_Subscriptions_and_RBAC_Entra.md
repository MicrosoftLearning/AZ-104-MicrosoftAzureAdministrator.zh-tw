---
lab:
  title: 實驗室02a：管理訂用帳戶和 RBAC
  module: Administer Governance and Compliance
---

# 實驗 02a - 管理訂用帳戶與 RBAC

## 實驗室簡介

在此實驗室中，您將瞭解角色型訪問控制。 您將瞭解如何使用許可權和範圍來控制身分識別可以及無法執行的動作。 您也會瞭解如何使用管理群組讓訂用帳戶管理更容易。 

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟是使用 **美國**東部撰寫。 

## 預估時間：30 分鐘

## 實驗案例

為了簡化組織中 Azure 資源的管理，您已負責實作下列功能：

- 建立包含所有 Azure 訂用帳戶的管理群組。

- 授與許可權，將管理群組中所有訂用帳戶的支援要求提交給指定的使用者。 該使用者的權限需限制為只能： 

    - 建立支援要求票證
    - 檢視資源群組

## 互動式實驗室案例

您可能會發現一些互動式實驗室模擬對本主題很有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。 

+ [使用 RBAC](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2014) 管理存取權。 將內建角色指派給使用者，並監視活動記錄。 

+ [管理訂用帳戶和 RBAC](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%202)。 實作管理群組並建立並指派自定義 RBAC 角色。

+ [開啟支援要求](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2022)。 檢閱支援方案選項，然後建立及監視支援要求、技術或計費。

## 架構圖

![實驗室工作的圖表。](../media/az104-lab02a-architecture.png)

## 工作

+ 工作 1：實作管理群組。
+ 工作 2：檢閱並指派內建的 Azure 角色。
+ 工作 3：建立和指派自定義 RBAC 角色。 
+ 工作 4：使用活動記錄監視角色指派。

## 工作 1：實作管理群組

在此工作中，您將建立及設定管理群組。 管理群組可用來以邏輯方式組織訂用帳戶。 訂用帳戶應進行區隔，並允許將 RBAC 和 Azure 原則 指派及繼承給其他管理群組和訂用帳戶。 例如，如果您的組織有歐洲的專用支援小組，您可以將歐洲訂用帳戶組織到管理群組，以提供支援人員對這些訂用帳戶的存取權（而不提供所有訂用帳戶的個別存取權）。 在我們的案例中，技術服務台的每個人都必須跨所有訂用帳戶建立支援要求。 

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

1. 搜尋並選取 [管理群組] 以瀏覽至 [管理群組] 刀鋒視窗。

1. 檢閱 [管理群組] 刀鋒視窗頂端的訊息。 如果您看到訊息指出 **您已註冊為目錄管理員，但沒有存取根管理群組**的必要許可權，請執行下列步驟順序：

    + 在 Azure 入口網站 中，搜尋並選取 **[Microsoft Entra ID**]。
    
    + 在顯示租使用者屬性的刀鋒視窗上，於左側的垂直功能表中，選取 **[管理** ] 區段中的 [ **屬性**]。
    
    + 在租使用者的 [ **屬性]** 刀鋒視窗中，於 **[Azure 資源的** 存取管理] 區段中，選取 [ **是** ]，然後選取 [ **儲存**]。
    
    + 流覽回 [ **管理群組** ] 刀鋒視窗，然後選取 [ **重新整理**]。

1. 在 [管理群組] 刀鋒視窗上，按一下 [+ 建立]。

1. 使用下列設定建立管理群組。 完成時選取 [ **提交** ]。 

    | 設定 | 值 |
    | --- | --- |
    | 管理群組識別碼 | `az104-mg1` |
    | 管理群組顯示名稱 | `az104-mg1` |

1. 在此案例中，所有訂用帳戶現在都會新增至管理群組。 RBAC 接著會套用至管理群組，並限定於技術支援中心。 

## 工作 2：檢閱並指派內建 Azure 角色

在這項工作中，您會將 VM 參與者角色指派給您的用戶帳戶。  

1. 在入口網站中，搜尋 和 **az104-mg1** 管理群組。

1. 選取 [**訪問控制 （IAM）] 刀鋒視窗，然後選取 [**角色]**** 索引標籤。

1. 捲動可用的角色定義。 **檢視**角色以取得許可權 **、**JSON** 和**指派**的詳細資訊**。

1. **從下拉功能表中選取 [+ 新增**]，選取 [**新增角色指派**]。 

1. 在 [新增角色指派]**** 刀鋒視窗上，指定下列設定，並在每個步驟後按一下 [下一步]****：

    | 設定 | 值 |
    | --- | --- |
    | 搜尋索引標籤中的角色 | **虛擬機器參與者** |
    | 指定權 （在[成員] 窗格下 ） | **使用者、群組或服務主體** |
    | 選取 (+選取成員) | *您的使用者帳戶* （顯示在入口網站的右上角 ） |

4. 按兩下 [檢閱 + 指派]**** 以建立角色指派。

    >**注意：** 虛擬機參與者角色可讓您管理虛擬機，但無法存取其操作系統，或管理其連線的虛擬網路和記憶體帳戶。

    >**注意：** 此指派可能實際上不會授與您任何其他授權。 如果您已經有擁有者角色，此角色會包含與參與者角色相關聯的所有許可權。


## 工作 3：建立自定義 RBAC 角色

在這項工作中，您將建立自定義 RBAC 角色。 自定義角色是實作環境最低許可權原則的核心部分。 內建角色可能為您的組織擁有太多許可權。 在此工作中，我們將建立新的角色，並移除不需要的許可權。

### 為技術支援中心使用者建立自定義 RBAC 角色

1. 在入口網站中，搜尋並選取 **az104-mg1** 管理群組。

1. 選取 [**訪問控制 （IAM）] 刀鋒視窗，然後選取 [**角色]**** 索引標籤。

1. 選取 [ **檢查存取權** ] 索引標籤，然後在 [ **建立自定義角色** ] 方塊中，選取 [ **新增**]。

1. 在 [建立自定義角色] 的 [基本] 索引標籤上，提供名稱 `Custom Support Request`。 在 [描述] 欄位中，輸入 `A custom contributor role for support requests.`

1. 在 [基準許可權] 欄位中，選取 [ **複製角色**]。 在 [要複製的角色] 下拉功能表中，選取 [ **支援要求參與者**]。

    ![複製角色的螢幕快照。](../media/az104-lab02a-clone-role.png)

1. 選取 [許可權] 索引**標籤，然後選取 **[+ 排除許可權**]。**

1. 在 [資源提供者搜尋] 字段中，輸入 `.Support` 並選取 **[Microsoft.Support**]。

1. 在許可權清單中，將複選框放在 [其他：註冊支持資源提供者 **] 旁**，然後選取 [**新增**]。 角色應更新為包含此許可權做為 *NotAction*。

1. 選取 [ **可指派的範圍] 索引** 標籤。選取 **訂用帳戶列上的 [刪除]** 圖示。

1. 選取 **[+ 新增可指派的範圍**]。 **選取 az104-mg1** 管理群組，然後按兩下 [**選取**]。

1. 選取 [JSON] 索引**標籤。檢閱角色中自定義的*Actions*、*NotActions* 和 *AssignableScopes* 的** JSON。 

1. **選取 [檢閱 + 建立**]，然後選取 [**建立**]。

    >**注意：** 此時您已建立自定義角色。 下一個步驟是將角色指派給技術支援中心使用者。 

### 身分識別您將用來測試新角色並指派自定義角色的技術支援中心用戶帳戶。 

1. 在 Azure 入口網站 中，搜尋並選取 **[Microsoft Entra ID**]，然後選取 [**使用者**] 刀鋒視窗。

    >**注意**：此工作需要使用者帳戶進行測試。 在此實驗室中， **我們將使用 HelpDesk-user1**。 如有必要****，請花一分鐘來識別測試使用者。 如果您要建立新的使用者，則需要在使用者登入時設定密碼。 

1. 在繼續之前，請確定您有測試帳戶的用戶 **主體名稱** 。 您需要此專案才能登入入口網站。 使用圖示將此資訊複製到剪貼簿。 

1. 在 Azure 入口網站 中，流覽回 **az104-mg1** 管理群組並顯示其詳細數據。

1. 依序按一下 [Access Control (IAM)] \(存取控制 (IAM)\)、[+ 新增] 以及 [Add role assignment] \(新增角色指派\)。 

1. 在 [ **角色]** 索引標籤上，搜尋 `Custom Support Request`。 

    >**注意**：若看不見您的自訂角色，則建立之後最多可能需要 10 分鐘的時間才會顯示自訂角色。

1. 選取 [角色]，然後按一下 [下一步]。 在 [ **成員]** 索引標籤上，按兩下 **[+ 選取成員** ]，然後 **選取** 使用者帳戶 **HelpDesk-user1**。  

1. 選取 [ **檢閱 + 指派** 兩次]。

    >**注意：** 此時，您有具有自定義許可權的技術支援中心用戶帳戶，可建立支援票證。 下一個步驟是測試帳戶。
    
### 測試技術支援中心用戶帳戶，以確保其具有正確的許可權

1. **開啟 InPrivate** 瀏覽器視窗，並使用測試使用者帳戶登入 Azure 入口網站`https://portal.azure.com`。 如果系統提示您更新密碼，請變更用戶的密碼。

    >**注意**：您可以貼上剪貼簿的內容，而不是輸入用戶名稱。

1. 在 **InPrivate** 瀏覽器視窗中，在 [Azure 入口網站] 中，搜尋並選取 **[資源群組**]，以確認技術支援中心使用者可以看到所有資源群組。

1. 在 InPrivate **** 瀏覽器視窗中，在 [Azure 入口網站] 中，搜尋並選取 **[所有資源**] 以確認技術支援中心使用者看不到任何資源。

1. 在 [InPrivate] 瀏覽器視窗的 [Azure 入口網站] 中，搜尋並選取 [說明 + 支援]，然後按一下 [+ 建立支援要求]。 

    >**注意**：許多組織選擇提供所有雲端系統管理員開啟支援案例的存取權。 這可讓系統管理員更快速地解決支援案例。

1. 在 [InPrivate]瀏覽器視窗中，於 [說明+支援 - 新增支援要求] 刀鋒視窗之 [問題描述/摘要] 索引標籤的 [摘要] 欄位中輸入 [服務和訂閱限制]，然後選取 [服務和訂閱帳戶限制 (配額)] 問題類型。 請注意，您在此實驗中使用的訂用帳戶會列在 [訂用帳戶] 下拉式清單中。

    >**注意**：您在 [訂用帳戶] 下拉式清單中使用此實驗中的訂用帳戶，表示您使用的帳戶具有建立訂用帳戶特定支援要求所需的權限。

    >**注意**：如果您沒有看到 [服務和訂用帳戶限制 (配額)] 選項，請從 Azure 入口網站登出並重新登入。

1. 花幾分鐘的時間探索建立 **新的支援要求**，但不要繼續建立支援要求。 請改為從 Azure 入口網站 註銷技術支援中心使用者，並關閉 InPrivate 瀏覽器視窗。

1. 您已完成自定義角色的測試，並檢閱了如何建立支援票證。 

## 工作 4：使用活動記錄監視角色指派

在這項工作中，您會檢視活動記錄檔，以判斷是否有人已建立新的角色。 

1. 返回 **az104-mg1** 資源，然後選取 [ **活動記錄**]。

2. 選取 **[新增篩選**]，選取 [作業 **]，然後選取 **[**建立角色指派**]。

    ![[活動記錄] 頁面的螢幕快照，其中已設定篩選。](../media/az104-lab02a-searchactivitylog.png)

3. 確認活動記錄顯示角色建立活動。 

## 檢閱實驗室的要點

恭喜您完成實驗室。 以下是此實驗室的主要外賣。 

+ 管理群組可用來以邏輯方式組織訂用帳戶。
+ Azure 有許多內建角色。 您可以指派這些角色來控制資源的存取權。
+ 您可以建立新的角色或自定義現有的角色。
+ 角色會以 JSON 格式的檔案定義，並包含 *Actions*、 *NotActions* 和 *AssignableScopes*。
+ 您可以使用活動記錄檔來監視角色指派。 

## 清除您的資源

如果您使用自己的訂用帳戶，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站 中，選取資源群組、選取 **[刪除資源群組**]、**輸入資源組名**，然後按兩下 [**刪除**]。

+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName`。

+ 使用 CLI， `az group delete --name resourceGroupName`。

