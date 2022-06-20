---
lab:
  title: 09a - 實作 Web 應用程式
  module: Module 09 - Serverless Computing
ms.openlocfilehash: af243b0cfa2b011dd419516139b5200ba349bcb4
ms.sourcegitcommit: c360d3abaa6e09814f051b2568340e80d0d0e953
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/09/2022
ms.locfileid: "145198095"
---
# <a name="lab-09a---implement-web-apps"></a>實驗 09a - 實作 Web 應用程式
# <a name="student-lab-manual"></a>學員實驗手冊

## <a name="lab-scenario"></a>實驗案例

您必須評估 Azure Web 應用程式的使用方式，以裝載 Contoso 的網站，此網站目前裝載於公司內部部署資料中心。 網站會使用 PHP 執行階段堆疊在 Windows 伺服器上執行。 您也需要利用 Azure Web 應用程式部署位置來判斷如何實作 DevOps 做法。

## <a name="objectives"></a>目標

在此實驗中，您將會：

+ 工作 1：建立 Azure Web 應用程式
+ 工作 2：建立預備部署位置
+ 工作 3：設定 Web 應用程式部署設定
+ 工作 4：將程式碼部署至預備部署位置
+ 工作 5：交換預備位置
+ 工作 6：設定並測試 Azure Web 應用程式的自動調整

## <a name="estimated-timing-30-minutes"></a>預估時間：30 分鐘

## <a name="architecture-diagram"></a>架構圖

![image](../media/lab09a.png)

## <a name="instructions"></a>指示

### <a name="exercise-1"></a>練習 1

#### <a name="task-1-create-an-azure-web-app"></a>工作 1：建立 Azure Web 應用程式

在這個工作中，您將建立一個 Azure Web 應用程式。

1. 登入 [**Azure 入口網站**](http://portal.azure.com)。

1. 在Azure 入口網站中，搜尋並選取 [應用程式服務]，然後在 [應用程式服務] 刀鋒視窗上，按一下 [+ 建立]。

1. 在 [建立 Web 應用程式] 刀鋒視窗的 [基本] 索引標籤上，指定下列設定 (其他設定請保留預設值)：

    | 設定 | 值 |
    | --- | ---|
    | 訂用帳戶 | 您在此實驗中使用的 Azure 訂用帳戶名稱 |
    | 資源群組 | 新資源群組 **az104-09a-rg1** 的名稱 |
    | Web 應用程式名稱 | 任何全域唯一名稱 |
    | 發佈 | **程式碼** |
    | 執行階段堆疊 | **PHP 7.4** |
    | 作業系統 | **Windows** |
    | 區域 | 您可以在其中佈建 Azure Web 應用程式 Azure 區域的名稱 |
    | App Service 方案 | 接受預設設定 |

1. 按一下 [檢閱 + 建立]  。 在 [建立 Web 應用程式] 刀鋒視窗的 [檢閱 + 建立] 索引標籤上，確定通過驗證，然後按一下 [建立]。

    >**注意**：請等到建立 Web 應用程式，再繼續進行下一個工作。 這應該大約需要 1 分鐘的時間。

1. 在 [部署] 刀鋒視窗上，按一下 [移至資源]。

#### <a name="task-2-create-a-staging-deployment-slot"></a>工作 2：建立預備部署位置

在此工作中，您將建立預備部署位置。

1. 在新部署 Web 應用程式的刀鋒視窗上，按一下 **URL** 連結，以在新的瀏覽器索引標籤中顯示預設網頁。

1. 關閉新的瀏覽器索引標籤，然後回到 [Azure 入口網站]，在 [Web 應用程式] 刀鋒視窗的 [部署] 區段中，按一下 [部署位置]。

    >**注意**：Web 應用程式此時具有標示為 **PRODUCTION** 的單一部署位置。

1. 按一下 [+ 新增位置]，並使用下列設定新增位置：

    | 設定 | 值 |
    | --- | ---|
    | 名稱 | **staging** |
    | 從下列複製設定 | **不要複製設定**|

1. 回到 Web 應用程式的 [部署位置] 刀鋒視窗上，按一下代表新建立暫存位置的專案。

    >**注意**：這將開啟顯示預備位置屬性的刀鋒視窗。

1. 檢閱預備位置刀鋒視窗，並注意其 URL 與指派給生產位置的 URL 不同。

#### <a name="task-3-configure-web-app-deployment-settings"></a>工作 3：設定 Web 應用程式部署設定

在這項工作中，您將設定 Web 應用程式部署設定。

1. 在 [預備部署位置] 刀鋒視窗的 [部署] 區段中，按一下 [部署中心]，然後選取 [設定] 索引標籤。

    >**注意：** 請確定您位於預備位置刀鋒視窗 (而不是生產位置)。
    
1. 在 [設定] 索引標籤的 [來源] 下拉式清單中，選取 [本機 Git]，然後按一下 [儲存] 按鈕

1. 在 [部署中心] 刀鋒視窗上，將 [Git 複製 URL] 項目複製到記事本。

    >**注意：** 您將在本實驗室的下一個工作中需要 Git 複製 URL 值。

1. 在 [部署中心] 刀鋒視窗上，選取 [本機 Git/FTPS 認證] 索引標籤，在 [使用者範圍] 區段中指定下列設定，然後按一下 [儲存]。

    | 設定 | 值 |
    | --- | ---|
    | 使用者名稱 | 任何全域唯一名稱 (不可包含 `@` 字元) |
    | 密碼 | 符合複雜度需求的任何密碼|

    >**注意：** 密碼長度必須至少為 8 個字元，包含下列三個元素其中兩個：字母、數字和非英數字元。

    >**注意：** 在本實驗的下一個工作中，您將需要這些認證。

#### <a name="task-4-deploy-code-to-the-staging-deployment-slot"></a>工作 4：將程式碼部署至預備部署位置

在這項工作中，您會將程式碼部署至預備部署位置。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示，開啟 **Azure Cloud Shell**。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現 **您未掛接任何儲存體** 訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。

1. 從 [Cloud Shell] 窗格中，執行下列命令以複製包含 Web 應用程式程式碼的遠端存放庫。

   ```powershell
   git clone https://github.com/Azure-Samples/php-docs-hello-world
   ```

1. 從 [Cloud Shell] 窗格中，執行下列命令，將目前位置設定為包含範例 Web 應用程式程式碼的本機存放庫新建複本。

   ```powershell
   Set-Location -Path $HOME/php-docs-hello-world/
   ```

1. 從 [Cloud Shell] 窗格中，執行下列命令以新增遠端 git (請務必將 `[deployment_user_name]` 和 `[git_clone_url]` 預留位置分別取代為您在先前工作中識別的 **部署認證** 使用者名稱和 **Git 複製 URL** 值)：

   ```powershell
   git remote add [deployment_user_name] [git_clone_url]
   ```

    >**注意**：下列 `git remote add` 值不需要符合 **部署認證** 使用者名稱，但必須是唯一的

1. 從 [Cloud Shell] 窗格中，執行下列命令，將範例 Web 應用程式程式碼從本機存放庫推送至 Azure Web 應用程式預備部署位置 (請務必將 `[deployment_user_name]` 預留位置取代為您在先前工作中識別的 **部署認證** 使用者名稱值)：

   ```powershell
   git push [deployment_user_name] master
   ```

1. 如果系統提示您進行驗證，請輸入 (您在先前工作中設定的) `[deployment_user_name]` 和對應密碼。

1. 關閉 [Cloud Shell] 窗格。

1. 在預備位置刀鋒視窗上，按一下 [概觀]，然後按一下 **URL** 連結，在新的瀏覽器索引標籤中顯示預設網頁。

1. 確認瀏覽器頁面會顯示 **Hello World！** 訊息並關閉新的索引標籤。

#### <a name="task-5-swap-the-staging-slots"></a>工作 5：交換預備位置

在這項工作中，您會將預備位置與生產位置交換

1. 瀏覽回到顯示 Web 應用程式生產位置的刀鋒視窗。

1. 在 [部署] 區段中，按一下 [部署位置]，然後按一下 [交換] 工具列圖示。

1. 在 [交換] 刀鋒視窗上，檢閱預設設定，然後按一下 [交換]。

1. 按一下 Web 應用程式的 [生產位置] 刀鋒視窗上的 [概觀]，然後按一下 **URL** 連結，在新的瀏覽器索引標籤中顯示網站首頁。

1. 確認預設網頁已取代為 **Hello World！** 頁面。

#### <a name="task-6-configure-and-test-autoscaling-of-the-azure-web-app"></a>工作 6：設定並測試 Azure Web 應用程式的自動調整

在此工作中，您將設定及測試 Azure Web 應用程式的自動調整。

1. 在顯示 Web 應用程式生產位置的刀鋒視窗中，按一下 [設定] 區段中的 [向外延展 (App Service方案)]。

1. 按一下 [自訂自動調整]。

    >**注意**：您也可以選擇手動調整 Web 應用程式。

1. 保留預設選項 [根據計量調整] 為選取，然後按一下 [+ 新增規則]

1. 在 [調整規則] 刀鋒視窗上，指定下列設定 (將其他設定保留為預設值)：

    | 設定 | 值 |
    | --- |--- |
    | 計量來源 | **目前的資源** |
    | 時間彙總 | **最大值** |
    | 計量命名空間 | **App Service 方案標準計量** |
    | 度量名稱 | **CPU 百分比** |
    | 運算子 | **大於** |
    | 用於觸發調整動作的計量閾值 | **10** |
    | 持續時間 (分鐘) | **1** |
    | 時間粒紋統計資料 | **最大值** |
    | 作業 | **將計數增加** |
    | 執行個體計數 | **1** |
    | 緩和時間 (分鐘) | **5** |

    >**注意**：很明顯地，這些值並不代表實際設定，因為其目的是要儘快觸發自動調整，而不延長等候期間。

1. 按一下 [新增]，回到 [App Service計畫調整] 刀鋒視窗上，指定下列設定 (讓其他設定保留預設值)：

    | 設定 | 值 |
    | --- |--- |
    | 執行個體最小值限制 | **1** |
    | 執行個體最大值限制 | **2** |
    | 執行個體預設值限制 | **1** |

1. 按一下 [檔案] 。

    >**注意**：如果您收到有關未註冊 'microsoft.insights' 資源提供者的錯誤，請在 cloudshell 中執行 `az provider register --namespace 'Microsoft.Insights'`，然後重試儲存自動調整規則。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示，開啟 **Azure Cloud Shell**。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。

1. 從 [Cloud Shell] 窗格中，執行下列命令以識別 Azure Web 應用程式的 URL。

   ```powershell
   $rgName = 'az104-09a-rg1'

   $webapp = Get-AzWebApp -ResourceGroupName $rgName
   ```

1. 從 [Cloud Shell] 窗格中，執行下列命令以啟動和無限迴圈，以將 HTTP 要求傳送至 Web 應用程式：

   ```powershell
   while ($true) { Invoke-WebRequest -Uri $webapp.DefaultHostName }
   ```

1. 將 [Cloud Shell] 窗格最小化 (但不關閉)，然後在 [Web 應用程式] 刀鋒視窗的 [監視] 區段中，按一下 [處理序總管]。

    >**注意**：處理序總管有助於監視執行個體數目及其資源使用率。

1. 監視使用率和執行個體數目幾分鐘。

    >**注意**：您可能需要 **重新整理** 頁面。

1. 一旦您注意到執行個體數目已增加為 2，請重新開啟 [Cloud Shell] 窗格，然後按 **Ctrl+C** 結束指令碼。

1. 關閉 [Cloud Shell] 窗格。

#### <a name="clean-up-resources"></a>清除資源

>**注意**：請記得移除您不再使用的任何新建立的 Azure 資源。 移除未使用的資源可確保您不會看到非預期的費用。

>**注意**：如果無法立即移除實驗資源，請不要擔心。 有時候資源具有相依性，需要經過很長的時間才能刪除。 這是監視資源使用量的常見系統管理員工作，因此只需定期檢閱入口網站中的資源，查看清除的運作情況便可。 

1. 在 Azure 入口網站中，在 [Cloud Shell] 窗格內開啟 [PowerShell] 工作階段。

1. 執行下列命令，列出在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*'
   ```

1. 執行下列命令，刪除您在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-09a*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：此命令以非同步方式執行 (由 --AsJob 參數決定)，所以您隨後能夠在相同 PowerShell 工作階段內立即執行另一個 PowerShell 命令，但需要經過幾分鐘後，才會實際移除資源群組。

#### <a name="review"></a>檢閱

在此實驗中，您已：

+ 建立 Azure Web 應用程式
+ 建立預備部署位置
+ 設定 Web 應用程式部署設定
+ 將程式碼部署至預備部署位置
+ 交換預備位置
+ 設定並測試 Azure Web 應用程式的自動調整
