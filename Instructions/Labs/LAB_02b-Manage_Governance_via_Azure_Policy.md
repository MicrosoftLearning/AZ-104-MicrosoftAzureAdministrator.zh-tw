---
lab:
  title: 實驗室 02b：透過 Azure 原則管理治理
  module: Administer Governance and Compliance
---

# 實驗 02b - 透過 Azure 原則管理治理

## 實驗室簡介

在此實驗室中，您將了解如何實作組織的治理方案。 您瞭解 Azure 原則如何確保整個組織強制執行作業決策。 您將了解如何使用資源標記來改善報告。 

此實驗室需要 Azure 訂閱。 您的訂用帳戶類型可能會影響此實驗室中的功能可用性。 您可以變更區域，但這些步驟是使用**美國東部**作為區域來撰寫。 

## 預估時間：30 分鐘

## 實驗案例

您組織的雲端使用量在過去一年裡已大幅成長。 在最近的稽核期間，您已探索到大量沒有已定義擁有者、專案或成本中心的資源。 為了改善組織中 Azure 資源的管理，您決定實作下列功能：

- 套用資源標籤以將重要的中繼資料附加至 Azure 資源

- 使用 Azure 原則強制使用新資源的資源標籤

- 使用資源標籤更新現有的資源

- 使用資源鎖定來保護已設定的資源

## 互動式實驗室模擬

>**注意**：先前提供的實驗室模擬已淘汰。

## 架構圖

![工作結構的圖表。](../media/az104-lab02b-architecture.png)

## 作業技能

+ 工作 1：透過 Azure 入口網站建立和指派標籤。
+ 工作 2：透過 Azure 原則強制執行標籤。
+ 工作 3：透過 Azure 原則套用標籤。
+ 工作 4：設定及測試資源鎖定。 

## 工作 1：透過 Azure 入口網站指派標籤

在此工作中，您將透過 Azure 入口網站建立標籤，並將其指派給 Azure 資源群組。 標籤是治理策略的重要元件，如 Microsoft Well-Architected Framework 和雲端採用架構中所述。 標籤可讓您快速識別資源擁有者、終止日期、群組連絡人，以及貴組織認為重要的其他名稱/值組。 針對這項工作，您會指派識別資源成本中心的標記。 

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。
      
1. 搜尋並選取 `Resource groups`。

1. 從 [資源群組] 中，選取 [+ 建立]****。

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶名稱 | 訂用帳戶 |
    | 資源群組名稱 | `az104-rg2` |
    | Location | **美國東部** |

    >**注意：** 針對本課程中的每個實驗室，您將建立新的資源群組。 這可讓您快速找出及管理實驗室資源。 

1. 選取 **[下一步** ] 並移至 [ **卷標** ] 索引標籤。提供新標籤上。

    | 設定 | 值 |
    | --- | --- |
    | 名稱 | 成本中心 |
    | 值 | 000 |

1. 選取 [檢閱 + 建立]，然後選取 [建立]。

## 工作 2：透過 Azure 原則強制執行標記

在這項工作中，您會將內建的*需要標籤及其資源值*原則指派給資源群組，並評估結果。 Azure 原則可用來強制執行設定，在此情況下會對 Azure 資源進行治理。 

1. 在 Azure 入口網站中，搜尋並選取 `Policy`。 

1. 在 [製作]**** 刀鋒視窗中，選取 [定義]****。 請花點時間瀏覽可供您使用的[內建原則定義](https://learn.microsoft.com/azure/governance/policy/samples/built-in-policies)清單。 請注意，您也可以搜尋定義。

    ![原則定義的螢幕擷取畫面。](../media/az104-lab02b-policytags.png)

1. 搜尋 `Require a tag and its value on resources` 內建原則。 選取原則，並花一分鐘時間檢閱定義。 

1. 選取 [指派原則]****。

1. 按一下省略符號按鈕並選取下列值來指定 [範圍]****。 完成時，按一下 [選取]****。 

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | *您的訂用帳戶* |
    | 資源群組 | **az104-rg2** |

    >**注意**：您可以在管理群組、訂用帳戶或資源群組層級上指派原則。 您也可以選擇指定排除專案，例如個別訂用帳戶、資源群組或資源。 在此案例中，我們想要在資源群組中的所有資源上標記。

1. 藉由指定下列設定來設定指派的**基本**屬性 (讓其他設定保留預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 指派名稱 | 「需要成本中心標籤及其資源上的值 |
    | 描述 | `Require Cost Center tag and its value on all resources in the resource group`|
    | 強制執行原則 | 啟用 |

    >**注意**：[指派名稱] 會自動填入您選取的原則名稱，但您可加以變更。 [描述]**** 是選擇性的。 請注意，您可以隨時停用原則。 

1. 按 [下一步]****，並將 [參數]**** 設定為下列值：

    | 設定 | 值 |
    | --- | --- |
    | 標記名稱 | `Cost Center` |
    | 標記值 | `000` |

1. 按一下 [下一步]，然後檢閱 [補救] 索引標籤。保留 [建立受控識別] 核取方塊取消勾選。 

1. 按一下 [檢閱 + 建立]，然後按一下 [建立]。

    >**注意**：現在，您會嘗試在資源群組中建立 Azure 儲存體帳戶，以確認新的原則指派生效。 您將建立儲存體帳戶，而不需要新增必要的標籤。 
    
    >**注意**：原則可能需要 5 到 10 分鐘才會生效。

1. 在入口網站中，搜尋並選取 `Storage Accounts`，然後選取 [+ 建立]****。 

1. 在 [建立儲存體帳戶]**** 刀鋒視窗的 [基本]**** 索引標籤上，完成設定。

    | 設定 | 值 |
    | --- | --- |
    | 資源群組 | **az104-rg2** |
    | 儲存體帳戶名稱 | *介於 3 到 24 個小寫字母和數位之間的任何全域唯一組合，從字母開始* |

1. 選取 [檢閱]****，然後按一下 [建立]****。

1. 您應該會收到**驗證失敗**訊息。 檢視訊息以找出失敗的原因。 確認錯誤訊息指出原則是否不允許資源部署。 

    ![不允許原則錯誤的螢幕擷取畫面。](../media/az104-lab02b-policyerror.png) 

>**注意**：按兩下 [ **原始錯誤]** 索引標籤，您可以找到更多有關錯誤的詳細數據，包括角色定義 **的名稱需要標籤及其資源**值。 部署失敗，因為您嘗試建立的儲存體帳戶沒有名為**成本中心**的標籤，且其值設定為**預設**。

## 工作 3：透過 Azure 原則套用標籤

在這項工作中，我們將使用新原則定義來補救任何不符合規範的資源。 在此案例中，我們會讓資源群組的任何子資源繼承資源群組上定義的**成本中心**標籤。

1. 在 Azure 入口網站中，搜尋並選取 `Policy`。 

1. 在 [撰寫] 區段中，按一下 [指派]。 

1. 在工作分派清單中，按兩下資料列中的省略號圖示，代表 **[需要標籤] 及其資源** 原則指派的值，並使用 **[刪除指派** ] 選單項來刪除指派。

1. 按一下 [指派原則]，然後按一下省略號按鈕並選取下列值來指定 [範圍]：

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | 您的 Azure 訂用帳戶 |
    | 資源群組 | `az104-rg2` |

1. 若要指定**原則定義**，請按一下省略符號按鈕，然後搜尋並選取 `Inherit a tag from the resource group if missing`。

1. 選取 [新增]****，然後設定指派的其餘 [基本]**** 屬性。

    | 設定 | 值 |
    | --- | --- |
    | 指派名稱 | `Inherit the Cost Center tag and its value 000 from the resource group if missing` |
    | 描述 | `Inherit the Cost Center tag and its value 000 from the resource group if missing` |
    | 強制執行原則 | 已啟用 |

1. 按 [下一步]****，並將 [參數]**** 設定為下列值：

    | 設定 | 值 |
    | --- | --- |
    | 標記名稱 | `Cost Center` |

1. 按一下 [下一步]，然後在 [補救] 索引標籤上，設定下列設定 (保留其他項目為預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 建立補救工作 | 已啟用 |
    | 要補救的原則 | **從資源群組繼承標籤 (若遺漏)** |

    >**注意**：此原則定義包含 **Modify** 效果。 因此，需要受控識別。 

    ![原則補救頁面的螢幕擷取畫面。 ](../media/az104-lab02b-policyremediation.png) 

1. 按一下 [檢閱 + 建立]，然後按一下 [建立]。

    >**注意**：為了確認新的原則指派生效，您會在相同的資源群組中建立另一個 Azure 儲存體帳戶，而不明確新增必要的標籤。 
    
    >**注意**：原則可能需要 5 到 10 分鐘才會生效。

1. 搜尋並選取 `Storage Account` ，然後按兩下 [ **+ 建立**]。 

1. 在 [建立儲存體帳戶] 刀鋒視窗的 [基本] 索引標籤上，確認您使用的是已套用原則的資源群組，並指定下列設定 (讓其他設定保留預設值)，然後按一下 [檢閱]：

    | 設定 | 值 |
    | --- | --- |
    | 儲存體帳戶名稱 | *介於 3 到 24 個小寫字母和數位之間的任何全域唯一組合，從字母開始* |

1. 請確認這次通過驗證，然後按一下 [建立]。

1. 佈建新的儲存體帳戶之後，請按一下 [移至資源]****。

1. 在 [標籤]**** 刀鋒視窗上，請注意，具有值 **000** 的標籤**成本中心**已自動指派給資源。

    >**您知道嗎？** 如果您在入口網站中搜尋並選取 [標籤]****，則可以檢視具有特定標籤的資源。 

## 工作 4：設定及測試資源鎖定

在這項工作中，您會設定及測試資源鎖定。 鎖定可防止刪除或修改資源。 

1. 搜尋並選取您的資源群組。
   
1. 在 [設定]**** 刀鋒視窗中，選取 [鎖定]****。

1. 選取 [新增]**** 並完成資源鎖定資訊。 完成後，選取 [確定]****。 

    | 設定 | 值 |
    | --- | --- |
    | 鎖定名稱 | `rg-lock` |
    | 鎖定類型 | **刪除** (請注意唯讀的選取項目) |
    
1. 瀏覽至資源群組 [概觀]**** 刀鋒視窗，然後選取 [刪除資源群組]****。

1. 在 [輸入資源群組名稱以確認刪除]**** 文字方塊中，提供資源群組名稱 `az104-rg2`。 請注意，您可以複製並貼上資源群組名稱。 

1. 請注意警告：刪除此資源群組及其相依資源是永久性動作且無法復原。 選取 **[刪除]**。

1. 您應該會收到拒絕刪除的通知。 

    ![無法刪除訊息的螢幕擷取畫面。](../media/az104-lab02b-failuretodelete.png) 

    >**注意：** 如果您想要刪除資源群組，則必須移除鎖定。 
    
## 清除您的資源

如果您使用**自己的訂用帳戶**，請花點時間刪除實驗室資源。 如此可確保釋出資源，並將成本降到最低。 刪除實驗室資源的最簡單方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站中，選取資源群組，選取 [刪除資源群組]****，[輸入資源群組名稱]****，然後按一下 [刪除]****。
+ 使用 Azure PowerShell `Remove-AzResourceGroup -Name resourceGroupName`。
+ 使用 CLI `az group delete --name resourceGroupName`。

## 利用 Copilot 延伸學習
Copilot 可協助您了解如何使用 Azure 指令碼工具。 Copilot 也可在實驗室中未涵蓋的區域或您需要更多資訊的地方提供協助。 開啟 Edge 瀏覽器，然後選擇 [Copilot] (右上方)，或瀏覽至 *copilot.microsoft.com*。 請花幾分鐘的時間嘗試下列提示。
+ 在資源群組上新增和刪除資源鎖定的 Azure PowerShell 和 CLI 命令為何？
+ 列表化 Azure 原則與 Azure RBAC 之間的差異，包括範例。
+ 強制執行 Azure 原則並補救不符合規範資源的步驟為何？
+ 如何取得具有特定標籤的 Azure 資源報告？

## 透過自學型訓練深入了解

+ [設計企業治理策略](https://learn.microsoft.com/training/modules/enterprise-governance/)。 使用 RBAC 和 Azure 原則來限制對 Azure 解決方案的存取權，並判斷哪個方法適合您的安全性目標。

## 重要心得

恭喜您完成此實驗室。 以下是此實驗室的主要重點。 

+ Azure 標籤是包含索引鍵/值組的中繼資料。 標籤會描述您環境中的特定資源。 特別是，在 Azure 中標記可讓您以邏輯方式標記資源。
+ Azure 原則可建立資源的慣例。 原則定義會描述資源合規性條件，以及符合條件時所要採取的效果。 條件會與必要值比較資源屬性欄位或值。 有許多內建原則定義，且您可以自訂原則。 
+ Azure 原則補救工作功能可用來根據定義和指派，將資源帶入合規性。 不符合 修改 或 deployIfNotExist 定義指派的資源，可以使用補救工作進入合規性。
+ 您可以在訂用帳戶、資源群組或資源上設定資源鎖定。 鎖定可保護資源免於意外使用者刪除和修改。 鎖定會覆寫使用者的任何權限。
+ Azure 原則是預先部署的安全性做法。 RBAC 和資源鎖定是部署後的安全性做法。

