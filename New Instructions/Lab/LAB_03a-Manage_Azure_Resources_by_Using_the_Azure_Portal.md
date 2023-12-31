---
lab:
  title: 實驗室 03a：使用 Azure 入口網站管理 Azure 資源
  module: Administer Azure Resources
---

# 實驗 03a - 使用 Azure 入口網站管理 Azure 資源
# 學員實驗手冊

## 實驗案例

您必須探索與布建資源相關聯的基本 Azure 系統管理功能，並加以組織。  您想要瞭解 Azure 中最常見的方法，以組織資源。 您也想要瞭解如何移動資源。 最後，您想要測試保護磁碟資源免於意外刪除，同時仍允許修改效能特性和大小。

**注意：** **[互動式實驗室模擬](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%204)** (英文) 可供您以自己的步調完成此實驗室。 您可能會發現互動式模擬與託管實驗室之間稍有差異，但所示範的核心概念與想法均相同。 

## 目標

在此實驗中，我們將：

+ 工作 1：建立資源群組、建立資源，並將資源部署到資源群組
+ 工作 2：在資源群組之間移動資源
+ 工作 3：實作和測試資源鎖定

## 預估時間：30 分鐘

## 架構圖

![圖表實驗室 03a 架構](../media/az104-lab03a-architecture-diagram.png)

### 指示

## 練習 1

## 工作 1：建立資源群組，並將資源部署至資源群組

在這項工作中，您將使用 Azure 入口網站 來建立受控磁碟。 在下一個工作中，我們會將此磁碟移至另一個資源群組。 

1. 登入 [**Azure 入口網站**](http://portal.azure.com)。

1. 在 Azure 入口網站中，搜尋並選取 [磁碟]，按一下 [+ 建立] 並指定下列設定：

    |設定|值|
    |---|---|
    |訂用帳戶| Azure 訂用帳戶的名稱  |
    |資源群組| 選取 **建立新的** 資源群組 - `az104-rg3a`|
    |磁碟名稱| `az104-disk1` |
    |區域| **(美國) 美國東部** |
    |可用性區域| **不需要基礎結構備援** |
    |來源類型| **None** |

1. 選取 **[變更大小]** ，然後選擇 **[標準 HDD** ] 和 **[32 GiB**]。 完成時選取 [ **確定** ]。 

    ![建立磁碟頁面的螢幕快照。](../media/az104-lab03a-createdisk1.png)

1. 按一下 [檢閱 + 建立]，然後按一下 [建立]。

    >**注意**：等候磁碟建立。 此作業所需的時間應該不到一分鐘。

## 工作 2：在資源群組之間移動資源 

在此工作中，我們會將您在上一個工作中建立的磁碟資源移至新的資源群組。 個別資源，例如磁碟，只能放在單一資源群組中。 組織通常會在資源群組層級套用角色型訪問控制、標記或 Azure 原則。 如果資源重新配置至另一個專案，如果您想要利用資源群組的現有組態，您可能需要將資源移至不同的資源群組（例如，已設定角色型訪問控制和標記），或打算將原始資源群組解除委任。

1. 搜尋並選取 [資源群組]。 

1. 在 [ **資源群組]** 刀鋒視窗中，按兩下代表 **您在上一個工作中建立的 az104-rg3a** 資源群組的專案。

1. 從資源群組的 [ **概觀]** 刀鋒視窗中，在資源群組資源清單中，選取代表新建立磁碟的專案。

1. 使用工具列省略號 （...） 選取 **[移動**]，然後在下拉式清單中選取 [ **移至另一個資源群組**]。 請注意，您選擇移至另一個訂用帳戶或區域。 

    ![移動資源的螢幕快照。](../media/az104-lab03a-moverg.png)

1. 在 [ **資源群組]** 文字框中，按兩下 [ **新建** ]，然後在文字框中輸入 `az104-rg3move` 。 選取 [ **確定** ] 以建立新的資源群組。

1. 在 [ **要移動** 的資源] 索引標籤上，確認 **[成功** ] 是驗證狀態。 驗證可能需要一分鐘的時間才能完成。 同時，請檢閱 [支援的移動作業](https://learn.microsoft.com/azure/azure-resource-manager/management/move-support-resources)清單。 選取 [下一步]  繼續進行。 

1. 在 [檢閱] 索引標籤上，選取 [我了解我必須先更新移動之資源所關聯的工具與指令碼，這些工具與指令碼才能使用新的資源識別碼] 核取方塊，然後按一下 [移動]。

    >**注意**：請勿等候移動完成，而是繼續進行下一個工作。 這大約需要 10 分鐘。 您可以藉由監視來源或目標資源群組的活動記錄項目，來判斷作業已完成。 完成下一項工作後，請重新造訪此步驟。

## 工作 3：實作資源鎖定

在此工作中，您會將資源鎖定套用至包含磁碟資源的 Azure 資源群組。 資源鎖定可以防止對資源進行任何類型的變更，例如磁碟的大小。 或者，資源鎖定可以防止意外刪除。 這兩種鎖定類型為 *刪除* 鎖定和 *只讀* 鎖定。

### 建立新的受控磁碟。 

1. 在 Azure 入口網站中，搜尋並選取 [磁碟]，按一下 [+ 建立] 並指定下列設定：

    |設定|值|
    |---|---|
    |訂用帳戶| 您在此實驗中使用的訂閱名稱 |
    |資源群組| 按兩下 **[建立新的** 資源群組]，並將其命名為 `az104-rg3` |
    |磁碟名稱| `az104-disk2` |
    |區域| 您在此實驗中建立其他資源群組的 Azure 區域功能變數名稱 |
    |可用性區域| **不需要基礎結構備援** |
    |來源類型| **None** |

1. 選取 **[變更大小]** ，然後選擇 **[標準 HDD** ] 和 **[32 GiB**]。 完成時選取 [ **確定** ]。 

    ![設定磁碟類型和大小的螢幕快照。](,./media/az104-lab03a-createdisk2.png)

1. 按一下 [檢閱 + 建立]，然後按一下 [建立]。

1. 等候磁碟部署，然後選取 **[移至資源**]。

### 鎖定資源群組，以便無法選取資源。 

1. 從 [概 **觀]** 刀鋒視窗中，選取您的資源群組。

1. 在 [設定] 區**段中，選取 **[鎖定]**，然後選取 [**+ 新增**]。** 指定下列設定，然後選取 [ **確定**]。 

    |設定|值|
    |---|---|
    |鎖定名稱| `az104-rglock` |
    |鎖定類型| **刪除** |

    ![資源鎖定畫面的螢幕快照。](../media/az104-lab03a-deletelock.png)

1. 從 [資源群組 **概觀]** 刀鋒視窗中，選取受控磁碟，然後在工具欄中選取 [ **刪除** ]。 

1. 在 [ **刪除資源]** 頁面上的 **[確認刪除** ] 文本框中，輸入 `delete` 並按兩下 [ **刪除**]。 確認您的 **刪除確認**。 

1. 您應該會看到錯誤訊息，通知您刪除作業失敗。

    >**注意**：這是預期的。 訊息指出資源群組有鎖定。 

    ![錯誤訊息的螢幕快照。](../media/az104-lab03a-deleteerror.png)

1. **選取 az104-disk2** 資源。 

1. 在 **[設定]** 區段中，按兩下 [**大小 + 效能**]。 變更為 **進階版 SSD** 和 **64 GiB**。 按一下 [儲存]**** 來套用變更。

1. 從 [概 **觀]** 刀鋒視窗中，確認您可以變更磁碟大小。 

    >**注意**：這是預期的。 資源群組層級鎖定僅適用於刪除作業。 您可以編輯資源設定。 

## 檢閱

恭喜！ 您已成功建立資源群組和資源、將資源移至另一個群組，並實作資源鎖定。