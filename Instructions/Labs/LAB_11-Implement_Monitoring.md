---
lab:
  title: 11 - 實作監視
  module: Administer Monitoring
---

# <a name="lab-11---implement-monitoring"></a>實驗 11 - 實作監視
# <a name="student-lab-manual"></a>學生實驗手冊

## <a name="lab-scenario"></a>實驗案例

針對所提供 Azure 資源的效能和設定深入解析，您必須加以評估 Azure 功能，特別是著重於 Azure 虛擬機器。 為了達成此目的，您應要檢查 Azure 監視器的功能，包含 Log Analytics。

**注意：** **[互動式實驗室模擬](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2017)** (英文) 可供您以自己的步調完成此實驗室。 您可能會發現互動式模擬與託管實驗室之間稍有差異，但所示範的核心概念與想法均相同。 

## <a name="objectives"></a>目標

在此實驗中，您將會：

+ 工作 1：佈建實驗室環境
+ 工作 2：註冊 Microsoft.Insights 和 Microsoft.AlertsManagement 資源提供者
+ 工作 3：建立和設定 Azure Log Analytics 工作區和 Azure 自動化的解決方案
+ 工作 4：檢閱 Azure 虛擬機器的預設監視設定
+ 工作 5：設定 Azure 虛擬機器診斷設定
+ 工作 6：檢閱 Azure 監視器功能
+ 工作 7：檢閱 Azure Log Analytics 功能

## <a name="estimated-timing-45-minutes"></a>預估時間：45 分鐘

## <a name="architecture-diagram"></a>架構圖

![image](../media/lab11.png)

## <a name="instructions"></a>指示

### <a name="exercise-1"></a>練習 1

#### <a name="task-1-provision-the-lab-environment"></a>工作 1：佈建實驗室環境

在此工作中，您將部署虛擬機器以用於測試監視案例。

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示以開啟 **Azure Cloud Shell**。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現**您未掛接任何儲存體**訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。

1. 在 [Cloud Shell] 窗格的工具列中，按一下 [上傳/下載檔案] 圖示，在下拉式功能表中，按一下 [上傳]，並將檔案 **\\Allfiles\\Labs\\11\\az104-11-vm-template.json** 和 **\\Allfiles\\Labs\\11\\az104-11-vm-parameters.json** 上傳至 Cloud Shell 主目錄。

1. 編輯您剛才上傳的參數檔案，並變更密碼。 如果您需要在 Shell 中編輯檔案的協助，請向講師尋求協助。 最佳做法便是秘密 (例如密碼)，這應會在 Key Vault 中儲存時提供更佳的安全性。 

1. 從 [Cloud Shell] 窗格中，執行下列命令來建立裝載虛擬機器的資源群組 (將 `[Azure_region]` 預留位置取代為您想要部署 Azure 虛擬機器的 Azure 區域名稱)：

    >**注意**：請務必選擇其中一個區域，其區域必須是列為[工作區對應文件](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)中參考的 **Log Analytics 工作區區域**

   ```powershell
   $location = '[Azure_region]'

   $rgName = 'az104-11-rg0'

   New-AzResourceGroup -Name $rgName -Location $location
   ```

1. 從 [Cloud Shell] 窗格中，執行下列命令以建立第一個虛擬網路，並使用您上傳的範本和參數檔案，將虛擬機器部署到其中：

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-11-vm-template.json `
      -TemplateParameterFile $HOME/az104-11-vm-parameters.json `
      -AsJob
   ```

    >**注意**：不須等待部署完成，請直接進行下一項工作。 此部署應需要大約 3 分鐘的時間。

#### <a name="task-2-register-the-microsoftinsights-and-microsoftalertsmanagement-resource-providers"></a>工作 2：註冊 Microsoft.Insights 和 Microsoft.AlertsManagement 資源提供者。

1. 從 [Cloud Shell] 窗格中，執行下列命令來註冊 Microsoft.Insights 和 Microsoft.AlertsManagement 資源提供者。

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Insights

   Register-AzResourceProvider -ProviderNamespace Microsoft.AlertsManagement
   ```

1. 將 [Cloud Shell] 窗格最小化 (但不關閉)。

#### <a name="task-3-create-and-configure-an-azure-log-analytics-workspace-and-azure-automation-based-solutions"></a>工作 3：建立和設定 Azure Log Analytics 工作區和 Azure 自動化的解決方案

在此工作中，您將建立和設定 Azure Log Analytics 工作區和 Azure 自動化的解決方案

1. 在 Azure 入口網站中，搜尋並選取 [Log Analytics 工作區]，然後在 [Log Analytics 工作區] 刀鋒視窗上，按一下 [+ 建立]。

1. 在 [建立 Log Analytics 工作區] 刀鋒視窗的 [基本] 索引標籤上，輸入下列設定，按一下 [檢閱 + 建立]，然後按一下 [建立]：

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | 您要在此實驗室中使用的 Azure 訂用帳戶名稱 |
    | 資源群組 | 新資源群組 **az104-11-rg1** 的名稱 |
    | Log Analytics 工作區 | 任何唯一的名稱 |
    | 區域 | 您在上一項工作中部署虛擬機器的 Azure 區域名稱 |

    >**注意**：請確定指定與在上一項工作中部署虛擬機器的相同區域。

    >**注意**：等待部署完成。 部署應需要大約 1 分鐘的時間。

1. 在 Azure 入口網站中，搜尋並選取 [自動化帳戶]，然後在 [自動化帳戶] 刀鋒視窗上，按一下 [+ 建立]。

1. 在 [建立自動化帳戶] 刀鋒視窗上，指定下列設定，然後在驗證時按一下 [檢閱 + 建立]，接著按一下 [建立]：

    | 設定 | 值 |
    | --- | --- |
    | 自動化帳戶名稱 | 任何唯一的名稱 |
    | 訂用帳戶 | 您要在此實驗室中使用的 Azure 訂用帳戶名稱 |
    | 資源群組 | **az104-11-rg1** |
    | 區域 | 依據[工作區對應文件](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)決定的 Azure 區域名稱 |

    >**注意**：請確定您根據[工作區對應文件](https://docs.microsoft.com/en-us/azure/automation/how-to/region-mappings)指定 Azure 區域

    >**注意**：等待部署完成。 部署可能需要大約 3 分鐘的時間。

1. 按一下 [前往資源]。

1. 在 [自動化帳戶] 刀鋒視窗的 [組態管理] 區段中，按一下 [詳細目錄]。

1. 在 [詳細目錄] 窗格的 [Log Analytics 工作區] 下拉式清單中，選取您稍早在此工作中建立的 Log Analytics 工作區，然後按一下 [啟用]。

    >**注意**：等候對應的 Log Analytics 解決方案安裝完成。 這大約需要 3 分鐘。

    >**注意**：這也會自動安裝**變更追蹤**解決方案。

1. 在 [自動化帳戶] 刀鋒視窗的 [更新管理] 區段中，按一下 [更新管理]，然後按一下 [啟用]。

    >**注意**：等候安裝完成。 這大約需要 5 分鐘的時間。

#### <a name="task-4-review-default-monitoring-settings-of-azure-virtual-machines"></a>工作 4：檢閱 Azure 虛擬機器的預設監視設定

在此工作中，您將檢閱 Azure 虛擬機器的預設監視設定

1. 在 Azure 入口網站中，搜尋並選取 [虛擬機器]，然後在 [虛擬機器] 刀鋒視窗中，按一下 [az104-11-vm0]。

1. 在 [az104-11-vm0] 刀鋒視窗的 [監視] 區段中，按一下 [計量]。

1. 在 [az104-11-vm0 \| 計量] 刀鋒視窗的預設圖表上，請注意，唯一可用的 [計量命名空間] 是 [虛擬機器主機]。

    >**注意**：這是預期的狀況，因為尚未進行客體層級診斷設定。 不過，您也可以選擇直接從 [計量命名空間] 下拉式清單啟用客體記憶體計量。 您稍後將在此練習中啟用客體記憶體計量。

1. 在 [計量] 下拉式清單中，檢閱可用計量的清單。

    >**注意**：清單包含可從虛擬機器主機收集的 CPU、磁碟和網路相關計量範圍，而無須存取客體層級計量。

1. 在 [計量] 下拉式清單中，選取 [百分比 CPU]，在 [彙總] 下拉式清單中，選取 [平均]，然後檢閱產生的圖表。

#### <a name="task-5-configure-azure-virtual-machine-diagnostic-settings"></a>工作 5：設定 Azure 虛擬機器診斷設定

在此工作中，您將進行 Azure 虛擬機器診斷設定。

1. 在 [az104-11-vm0] 刀鋒視窗的 [監視] 區段中，按一下 [診斷設定]。

1. 在 [az104-11-vm0 \| 診斷設定] 刀鋒視窗的 [概觀] 索引標籤上，按一下 [啟用客體層級監視]。

    >**注意**：等候作業生效。 這大約需要 3 分鐘。

1. 切換至 [az104-11-vm0 \| 診斷設定] 刀鋒視窗的 [效能計數器] 索引標籤，並檢閱可用的計數器。

    >**注意**：根據預設，會啟用 CPU、記憶體、磁碟和網路計數器。 您可以切換至 [自訂] 檢視，以取得更詳細的清單。

1. 切換至 [az104-11-vm0 \| 診斷設定] 刀鋒視窗的 [記錄] 索引標籤，並檢閱可用的事件記錄收集選項。

    >**注意**：根據預設，記錄收集包含應用程式記錄檔和系統記錄檔中的嚴重、錯誤和警告項目，以及安全性記錄檔中的稽核失敗項目。 您也可以在此處切換至 [自訂] 檢視，以取得更詳細的組態設定。

1. 在 [az104-11-vm0] 刀鋒視窗的 [監視] 區段中，按一下 [Log Analytics Agent] \(Log Analytics 代理程式\)，然後按一下 [啟用]。

1. 在 [az104-11-vm0 - 記錄] 刀鋒視窗中，確定您在 [選擇 Log Analytics 工作區] 下拉式清單中已選取稍早在此實驗中建立的 Log Analytics 工作區，然後按一下 [啟用]。

    >**注意**：不須等候作業完成，請直接進行下一個步驟。 作業大約需要 5 分鐘。

1. 在 [az104-11-vm0 \| 記錄] 刀鋒視窗的 [監視] 區段中，按一下 [計量]。

1. 在 [az104-11-vm0 \| 計量] 刀鋒視窗的預設圖表上，請注意，目前除了 [虛擬機器主機] 項目之外，[計量命名空間] 下拉式清單也包含 [客體 (傳統)] 項目。

    >**注意**：這是預期的狀況，因為您已啟用客體層級診斷設定。 您也可以選擇 [啟用新的客體記憶體計量]。

1. 在 [計量命名空間] 下拉式清單中，選取 [客體 (傳統)] 項目。

1. 在 [計量] 下拉式清單中，檢閱可用計量的清單。

    >**注意**：清單只包含依賴主機層級監視時無法使用的其他客體層級計量。

1. 在 [計量] 下拉式清單中，選取 [記憶體\\可用位元組]，在 [彙總] 下拉式清單中，選取 [最大]，然後檢閱產生的圖表。

#### <a name="task-6-review-azure-monitor-functionality"></a>工作 6：檢閱 Azure 監視器功能

1. 在 Azure 入口網站中，搜尋並選取 [監視]，然後在 [監視 \| 概觀] 刀鋒視窗上，按一下 [計量]。

1. 在 [選取範圍] 刀鋒視窗的 [瀏覽] 索引標籤上，瀏覽至 [az104-11-rg0] 資源群組並展開，選取該資源群組內 [az104-11-vm0] 虛擬機器項目旁的核取方塊，然後按一下 [套用]。

    >**注意**：這會提供與 [az104-11-vm0 - 計量] 刀鋒視窗中可用項目的相同檢視和選項。

1. 在 [計量] 下拉式清單中，選取 [百分比 CPU]，在 [彙總] 下拉式清單中，選取 [平均]，然後檢閱產生的圖表。

1. 在 [監視 \| 計量] 刀鋒視窗的 [az104-11-vm0 的平均百分比 CPU] 窗格上，按一下 [新增警示規則]。

    >**注意**：來自客體 (傳統) 計量命名空間的計量不支援從計量建立警示規則。 這可以使用 Azure Resource Manager 範本來完成，如[使用 Windows 虛擬機器的 Resource Manager 範本將客體 OS 計量傳送至 Azure 監視器計量存放區](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/collect-custom-metrics-guestos-resource-manager-vm)文件中所述

1. 在 [建立警示規則] 刀鋒視窗的 [條件] 區段中，按一下現有的條件項目。

1. 在 [設定訊號邏輯] 刀鋒視窗的訊號清單中，於 [警示邏輯] 區段中指定下列設定 (將其他設定保留預設值)，然後按一下 [完成]：

    | 設定 | 值 |
    | --- | --- |
    | 臨界值 | **靜態** |
    | 運算子 | **大於** |
    | 彙總類型 | **平均** |
    | 臨界值 | **2** |
    | 彙總細微性 (期間) | **1 分鐘** |
    | 評估頻率 | **每隔 1 分鐘** |

1. 按一下 [下一步：**動作 >]，在 [建立警示規則] 刀鋒視窗的 [動作群組] 區段中，按一下 [+ 建立動作群組] 按鈕。

1. 在 [建立虛擬網路] 刀鋒視窗的 [基本] 索引標籤上，指定下列設定 (將其他設定保留預設值)，然後選取 [下一步：通知 >]：

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | 您要在此實驗室中使用的 Azure 訂用帳戶名稱 |
    | 資源群組 | **az104-11-rg1** |
    | 動作群組名稱 | **az104-11-ag1** |
    | 顯示名稱 | **az104-11-ag1** |

1. 在 [建立動作群組] 刀鋒視窗的 [通知] 索引標籤上，於 [通知類型] 下拉式清單中選取 [電子郵件/手機簡訊/推播/語音]。 在 [名稱] 文字方塊中，輸入**管理員電子郵件**。 按一下 [編輯詳細資料] (鉛筆) 圖示。

1. 在 [電子郵件/簡訊訊息/推播/語音] 刀鋒視窗上，選取 [電子郵件] 核取方塊，在 [電子郵件] 文字方塊中輸入您的電子郵件地址 (將其他設定保留預設值)，按一下 [確定]，接著返回至 [建立動作群組] 刀鋒視窗的 [通知] 索引標籤上，選取 [下一步：動作 >]。

1. 在 [建立動作群組] 刀鋒視窗的 [動作] 索引標籤上，檢閱 [動作類型] 下拉式清單中的可用項目 (無須進行任何變更)，然後選取 [檢閱 + 建立]。

1. 在 [建立動作群組] 刀鋒視窗的 [檢閱 + 建立] 索引標籤上，選取 [建立]。

1. 接著返回至 [建立警示規則] 刀鋒視窗，按一下 [下一步：詳細資料 >]，並在 [警示規則詳細資料] 區段中，指定下列設定 (將其他設定保留為預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 警示規則名稱 | **高於測試閾值的 CPU 百分比** |
    | 警示規則描述 | **高於測試閾值的 CPU 百分比** |
    | 嚴重性 | **Sev 3** |
    | 在建立時啟用 | **是** |

1. 按一下 [檢閱 + 建立]，然後在 [檢閱 + 建立] 索引標籤上按一下 [建立]。

    >**注意**：計量警示規則最多需要 10 分鐘才會變成作用中狀態。

1. 在 Azure 入口網站中，搜尋並選取 [虛擬機器]，然後在 [虛擬機器] 刀鋒視窗中，按一下 [az104-11-vm0]。

1. 在 [az104-11-vm0] 刀鋒視窗上，按一下 [連線]，在下拉式功能表中，按一下 [RDP]，在 [透過 RDP 連線] 刀鋒視窗上，按一下 [下載 RDP 檔案]，然後遵循提示來啟動遠端桌面工作階段。

    >**注意**：此步驟是指透過遠端桌面從 Windows 電腦進行連線。 在 Mac 電腦上，您可以使用 Mac App Store 的遠端桌面用戶端；而在 Linux 電腦上，您可以使用開放原始碼 RDP 用戶端軟體。

    >**注意**：連線至目標虛擬機器時，您可以忽略任何警告提示。

1. 出現提示時，請使用參數檔案中的 **Student** 使用者名稱和密碼登入。

1. 在遠端桌面工作階段中，按一下 [開始]，展開 [Windows 系統] 資料夾，然後按一下 [命令提示字元]。

1. 從命令提示字元執行下列命令，以觸發 **az104-11-vm0** Azure VM 上的已增加 CPU 使用率：

   ```sh
   for /l %a in (0,0,1) do echo a
   ```

    >**注意**：這會起始無限迴圈，這應增加 CPU 使用率且應高於新建立警示規則的閾值。

1. 將遠端桌面工作階段保持開啟，然後切換回瀏覽器視窗，以顯示實驗室電腦上的 Azure 入口網站。

1. 在 Azure 入口網站中，瀏覽回 [監視] 刀鋒視窗，然後按一下 [警示]。

1. 記下 **Sev 3** 警示的數目，然後按一下 [Sev 3] 資料列。

    >**注意**：您可能需要等候幾分鐘，接著按一下 [重新整理]。

1. 在 [所有警示] 刀鋒視窗上，檢閱產生的警示。

#### <a name="task-7-review-azure-log-analytics-functionality"></a>工作 7：檢閱 Azure Log Analytics 功能

1. 在 Azure 入口網站中，瀏覽回 [監視] 刀鋒視窗，按一下 [記錄]。

    >**注意**：如果這是您第一次存取 Log Analytics，則可能需要按一下 [開始使用]。

1. 如有必要，請按一下 [選取範圍]，在 [選取範圍] 刀鋒視窗上，選取 [最近使用] 索引標籤，選取 [az104-11-vm0]，然後按一下 [套用]。

1. 在查詢視窗中，貼上下列查詢，按一下 [執行]，然後檢閱產生的圖表：

   ```sh
   // Virtual Machine available memory
   // Chart the VM's available memory over the last hour.
   InsightsMetrics
   | where TimeGenerated > ago(1h)
   | where Name == "AvailableMB"
   | project TimeGenerated, Name, Val
   | render timechart
   ```

    > **注意**：查詢不應該有任何錯誤 (由右捲軸上的紅色區塊指示)。 如果查詢無法在未發生錯誤情況下直接從指示貼上，請將查詢程式碼貼到文字編輯器中 (例如記事本)，然後從該處複製並貼到查詢視窗中。


1. 按一下工具列中的 [查詢]，在 [查詢] 窗格上，找出 [追蹤 VM 可用性] 圖格，然後按兩下以填入查詢視窗，接著按一下圖格中的 [執行] 命令按鈕，然後檢閱結果。

1. 在 [新增查詢 1] 索引標籤上，選取 [資料表] 標頭，然後檢閱 [虛擬機器] 區段中的資料表清單。

    >**注意**：數個資料表的名稱會對應至您稍早在此實驗中安裝的解決方案。

1. 將滑鼠停留在 [VMComputer] 項目上，然後按一下 [查看預覽資料] 圖示。

1. 如果有可用的資料，請在 [更新] 窗格中，按一下 [在編輯器中使用]。

    >**注意**：您可能需要等候幾分鐘，更新資料才會可供使用。

#### <a name="clean-up-resources"></a>清除資源

>**注意**：請記得移除您不再使用的任何新建立的 Azure 資源。 移除未使用的資源可確保您不會看到非預期的費用。

>**注意**：如果無法立即移除實驗資源，請不要擔心。 有時候資源具有相依性，需要經過較長的時間才能刪除。 這是監視資源使用量的常見系統管理員工作，因此只需定期檢閱入口網站中的資源，查看清除的運作情況便可。 

1. 在 Azure 入口網站中，在 [Cloud Shell] 窗格內開啟 [PowerShell] 工作階段。

1. 執行下列命令，列出在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*'
   ```

1. 執行下列命令，刪除您在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-11*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：此命令以非同步方式執行 (由 --AsJob 參數決定)，所以您隨後能夠在相同 PowerShell 工作階段內立即執行另一個 PowerShell 命令，但需要經過幾分鐘後，才會實際移除資源群組。

#### <a name="review"></a>檢閱

在此實驗中，您已：

+ 佈建實驗室環境
+ 建立和設定 Azure Log Analytics 工作區和 Azure 自動化的解決方案
+ 檢閱 Azure 虛擬機器的預設監視設定
+ 設定 Azure 虛擬機器診斷設定
+ 檢閱 Azure 監視器功能
+ 檢閱 Azure Log Analytics 功能
