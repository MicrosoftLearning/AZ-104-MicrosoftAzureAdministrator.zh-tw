---
lab:
  title: 02a - 管理訂閱與 RBAC
  module: Module 02 - Governance and Compliance
ms.openlocfilehash: 14b37fcd923ad1b45c83c3a6c41889db3869ed40
ms.sourcegitcommit: 6df80c7697689bcee3616cdd665da0a38cdce6cb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2022
ms.locfileid: "146587436"
---
# <a name="lab-02a---manage-subscriptions-and-rbac"></a>實驗 02a - 管理訂用帳戶與 RBAC
# <a name="student-lab-manual"></a>學員實驗手冊

## <a name="lab-requirements"></a>實驗需求

此實驗需要權限來建立 Azure Active Directory (Azure AD) 使用者、建立自訂 Azure 角色型存取控制 (RBAC) 角色，並將這些角色指派給 Azure AD 使用者。 並非所有實驗主控者都提供這項功能。 請要求講師提供此實驗的可用性。

## <a name="lab-scenario"></a>實驗案例

為了改善 Contoso 中的 Azure 資源管理，您必須負責實作下列功能：

- 建立包含所有 Contoso Azure 訂用帳戶的管理群組

- 授與權限，以將管理群組中所有訂用帳戶的支援要求提交至指定的 Azure Active Directory 使用者。 該使用者的權限需限制為只能： 

    - 建立支援要求票證
    - 檢視資源群組 

## <a name="objectives"></a>目標

在此實驗中，您將會：

+ 工作 1：實作管理群組
+ 工作 2：建立自訂 RBAC 角色 
+ 工作 3：指派 RBAC 角色


## <a name="estimated-timing-30-minutes"></a>預估時間：30 分鐘

## <a name="architecture-diagram"></a>架構圖

![image](../media/lab02a.png)


## <a name="instructions"></a>指示

### <a name="exercise-1"></a>練習 1

#### <a name="task-1-implement-management-groups"></a>工作 1：實作管理群組

在此工作中，您將建立及設定管理群組。 

1. 登入 [**Azure 入口網站**](http://portal.azure.com)。

1. 搜尋並選取 [管理群組] 以瀏覽至 [管理群組] 刀鋒視窗。

1. 檢閱 [管理群組] 刀鋒視窗頂端的訊息。 如果您看到訊息指出 **您已註冊為目錄管理員，但沒有存取根管理群組的必要權限**，請依照下列步驟順序執行：

    1. 在 Azure 入口網站中，搜尋並選取 [Azure Active Directory]。
    
    1.  在顯示您 Azure Active Directory 租用戶屬性的刀鋒視窗上，於垂直功能表左側的 [管理] 區段中選取 [屬性]。
    
    1.  在 Azure Active Directory 租用戶的 [屬性] 刀鋒視窗中，於 [Azure 資源的存取管理] 區段中，選取 [是]，然後選取 [儲存]。
    
    1.  瀏覽回 [管理群組] 刀鋒視窗，然後選取 [重新整理]。

1. 在 [管理群組] 刀鋒視窗上，按一下 [+ 建立]。

    >**注意**：如果您先前尚未建立管理群組，請選取 [開始使用管理群組]

1. 使用下列設定建立管理群組：

    | 設定 | 值 |
    | --- | --- |
    | 管理群組識別碼 | **az104-02-mg1** |
    | 管理群組顯示名稱 | **az104-02-mg1** |

1. 在管理群組清單中，按一下代表新建立管理群組的專案。

1. 在 [az104-02-mg1] 刀鋒視窗上，按一下 [訂用帳戶]。 

1. 在 [az104-02-mg1 \| 訂用帳戶] 刀鋒視窗上，按一下 [+ 新增]，在 [新增訂用帳戶] 刀鋒視窗的 [訂用帳戶] 下拉式清單中，選取您在此實驗中使用的訂用帳戶，然後按一下 [儲存]。

    >**注意**：在 [az104-02-mg1 \| 訂用帳戶] 刀鋒視窗上，將 Azure 訂用帳戶的識別碼複製到剪貼簿。 您將在下一個工作中需要它。

#### <a name="task-2-create-custom-rbac-roles"></a>工作 2：建立自訂 RBAC 角色

在此工作中，您將建立自訂 RBAC 角色的定義。

1. 從實驗電腦，在記事本中開啟 **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** 檔案，並檢閱其內容：

   ```json
   {
      "Name": "Support Request Contributor (Custom)",
      "IsCustom": true,
      "Description": "Allows to create support requests",
      "Actions": [
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
      ],
      "NotActions": [
      ],
      "AssignableScopes": [
          "/providers/Microsoft.Management/managementGroups/az104-02-mg1",
          "/subscriptions/SUBSCRIPTION_ID"
      ]
   }
   ```
    > **注意**：如果您不確定檔案儲存在本機的實驗環境中，請詢問講師。

1. 將 JSON 檔案中的 `SUBSCRIPTION_ID` 預留位置取代為您複製到剪貼簿中的訂用帳戶識別碼，並儲存變更。

1. 在 Azure 入口網站中，按一下搜尋文字方塊右側的工具列圖示，以開啟 [Cloud Shell] 窗格。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。 

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現 **您未掛接任何儲存體** 訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。 

1. 在 [Cloud Shell] 窗格的工具列中，按一下 [上傳/下載檔案] 圖示，在下拉式功能表中，按一下 [上傳]，並將檔案 **\\Allfiles\\Labs\\02\\az104-02a-customRoleDefinition.json** 上傳至 Cloud Shell 主目錄。

1. 從 [Cloud Shell] 窗格中，執行下列命令以建立自訂角色定義：

   ```powershell
   New-AzRoleDefinition -InputFile $HOME/az104-02a-customRoleDefinition.json
   ```

1. 關閉 [Cloud Shell] 窗格。

#### <a name="task-3-assign-rbac-roles"></a>工作 3：指派 RBAC 角色

在此工作中，您將建立 Azure Active Directory 使用者、將您在上一個工作中建立的 RBAC 角色指派給該使用者，並確認使用者可以執行 RBAC 角色定義中指定的工作。

1. 在Azure 入口網站中，搜尋並選取 [Azure Active Directory]，在 [Azure Active Directory] 刀鋒視窗中，按一下 [使用者]，然後按一下 [+ 新增使用者]。

1. 使用下列設定建立新使用者 (讓其他設定保持預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 使用者名稱 | **az104-02-aaduser1**|
    | 名稱 | **az104-02-aaduser1**|
    | 讓我建立密碼 | 已啟用 |
    | 初始密碼 | **提供安全的密碼** |

    >**注意**：將完整的 **使用者名稱** **複製到剪貼簿**。 之後在此實驗中會用到。

1. 在 Azure 入口網站中，瀏覽回到 **az104-02-mg1** 管理群組，並顯示其 **詳細資料**。

1. 按一下 [存取控制 (IAM)]，按一下 [+ 新增]，接著按一下 [新增角色指派]，然後將 **[支援要求參與者] (自訂)** 角色指派給新建立的使用者帳戶。

1. 開啟 **InPrivate** 瀏覽器視窗並使用新建立的使用者帳戶登入 [Azure 入口網站](https://portal.azure.com)。 當系統提示您更新密碼時，請變更使用者的密碼。

    >**注意**：您可以貼上剪貼簿的內容，而不是輸入使用者名稱。

1. 在 [InPrivate] 瀏覽器視窗的 [Azure 入口網站] 中，搜尋並選取 [資源群組]，以確認 az104-02-aaduser1 使用者可以看到所有資源群組。

1. 在 [InPrivate] 瀏覽器視窗的 [Azure 入口網站] 中，搜尋並選取 [所有資源]，以確認 az104-02-aaduser1 使用者無法看到任何資源。

1. 在 [InPrivate] 瀏覽器視窗的 [Azure 入口網站] 中，搜尋並選取 [說明 + 支援]，然後按一下 [+ 建立支援要求]。 

1. 在 [InPrivate]瀏覽器視窗中，於 [說明+支援 - 新增支援要求] 刀鋒視窗的 [問題解說/摘要] 索引標籤上，在 [摘要] 欄位中輸入 [服務和訂用帳戶限制]，然後選取 [服務和訂用帳戶限制 (配額)] 問題類型。 請注意，您在此實驗中使用的訂用帳戶會列在 [訂用帳戶] 下拉式清單中。

    >**注意**：您在 [訂用帳戶] 下拉式清單中使用此實驗中的訂用帳戶，表示您使用的帳戶具有建立訂用帳戶特定支援要求所需的權限。

    >**注意**：如果您沒有看到 [服務和訂用帳戶限制 (配額)] 選項，請從 Azure 入口網站登出並重新登入。

1. 請勿繼續建立支援要求。 請改為從 Azure 入口網站作為 az104-02-aaduser1 使用者登出，然後關閉 InPrivate 瀏覽器視窗。

#### <a name="task-4-clean-up-resources"></a>工作 4：清理資源

   >**注意**：請記得移除您不再使用的任何新建立的 Azure 資源。 移除未使用的資源可確保您不會看到非預期的費用，不過，在此實驗中建立的資源不會產生額外費用。

   >**注意**：如果無法立即移除實驗資源，請不要擔心。 有時候資源具有相依性，需要經過較長的時間才能刪除。 這是監視資源使用量的常見系統管理員工作，因此只需定期檢閱入口網站中的資源，查看清除的運作情況便可。

1. 在Azure 入口網站中，搜尋並選取 [Azure Active Directory]，在 [Azure Active Directory] 刀鋒視窗中，按一下 [使用者]。

1. 在 [使用者 - 所有使用者] 刀鋒視窗中，按一下 [az104-02-aaduser1]。

1. 在 [az104-02-aaduser1 - 設定檔] 刀鋒視窗上，複製 [物件識別碼] 屬性的值。

1. 在 Azure 入口網站中，在 [Cloud Shell] 內起始 [PowerShell] 工作階段。

1. 從 [Cloud Shell] 窗格中，執行下列命令來移除自訂角色定義的指派， (將預留位置 `[object_ID]` 取代為您稍早在此工作中複製的 **az104-02-aaduser1** Azure Active Directory 使用者帳戶 **物件識別碼** 屬性值)：

   ```powershell
   
   $scope = (Get-AzRoleDefinition -Name 'Support Request Contributor (Custom)').AssignableScopes[0]

   Remove-AzRoleAssignment -ObjectId '[object_ID]' -RoleDefinitionName 'Support Request Contributor (Custom)' -Scope $scope
   ```

1. 從 [Cloud Shell] 窗格中，執行下列命令以移除自訂角色定義：

   ```powershell
   Remove-AzRoleDefinition -Name 'Support Request Contributor (Custom)' -Force
   ```

1. 在 Azure 入口網站中，瀏覽回到 **Azure Active Directory** 的 [使用者 - 所有使用者] 刀鋒視窗，然後刪除 **az104-02-aaduser1** 使用者帳戶。

1. 在 Azure 入口網站中，瀏覽回到 [管理群組] 刀鋒視窗。 

1. 在 [管理群組] 刀鋒視窗上，選取 **az104-02-mg1** 管理群組下訂用帳戶旁的 **省略號** 圖示，然後選取 [移動] 將訂用帳戶移至 [租用戶根管理群組]。

   >**注意**：除非您在執行此實驗之前建立自訂管理群組階層，否則目標管理群組可能是 **租用戶根管理群組**。
   
1. 選取 [重新整理] 以確認訂用帳戶已成功移至 [租用戶根管理群組]。

1. 瀏覽回 [管理群組] 刀鋒視窗，按一下 **az104-02-mg1** 管理群組右邊的 **省略號** 圖示，然後按一下 [刪除]。

#### <a name="review"></a>檢閱

在此實驗中，您已：

- 實作管理群組
- 建立自訂 RBAC 角色 
- 指派 RBAC 角色
