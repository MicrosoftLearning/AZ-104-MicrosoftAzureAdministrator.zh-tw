---
lab:
  title: 05 - 實作站台間連線能力
  module: Administer Intersite Connectivity
---

# <a name="lab-05---implement-intersite-connectivity"></a>實驗 05 - 實作站台間連線能力
# <a name="student-lab-manual"></a>學生實驗手冊

## <a name="lab-scenario"></a>實驗案例

Contoso 將其位於波士頓、紐約和西雅圖辦公室的資料中心，透過網狀廣域網路連結互相連線，並在這些資料中心之間具備完整連線能力。 您必須實作實驗環境，以反映 Contoso 內部部署網路的拓撲，並驗證其功能性。

**注意：** **[互動式實驗室模擬](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%209)** (英文) 可供您以自己的步調完成此實驗室。 您可能會發現互動式模擬與託管實驗室之間稍有差異，但所示範的核心概念與想法均相同。 

## <a name="objectives"></a>目標

在此實驗中，您將會：

+ 工作 1：佈建實驗環境
+ 工作 2：設定本機和全域虛擬網路對等互連
+ 工作 3：測試站台間連線能力

## <a name="estimated-timing-30-minutes"></a>預估時間：30 分鐘

## <a name="architecture-diagram"></a>架構圖

![image](../media/lab05.png)

### <a name="instructions"></a>指示

#### <a name="task-1-provision-the-lab-environment"></a>工作 1：佈建實驗環境

在這項工作中，您會將三部虛擬機器部署至個別的虛擬網路，其中兩部虛擬機器位於相同的 Azure 區域，而第三部則位於另一個 Azure 區域。

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示以開啟 **Azure Cloud Shell**。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現**您未掛接任何儲存體**訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。

1. 在 [Cloud Shell] 窗格的工具列中，按一下 [上傳/下載檔案] 圖示，在下拉式功能表中，按一下 [上傳]，並將檔案 **\\Allfiles\\Labs\\05\\az104-05-vnetvm-loop-template.json** 和 **\\Allfiles\\Labs\\05\\az104-05-vnetvm-loop-parameters.json** 上傳至 Cloud Shell 主目錄。

1. 編輯您剛才上傳的**參數**檔案，並變更密碼。 如果您需要在 Shell 中編輯檔案的協助，請向講師尋求協助。 最佳做法便是秘密 (例如密碼)，這應會在 Key Vault 中儲存時提供更佳的安全性。 

1. 從 [Cloud Shell] 窗格中執行下列命令，以建立將裝載實驗環境的資源群組。 前兩個虛擬網路和一對虛擬機器將會部署在 [Azure_region_1]。 第三個虛擬網路和第三部虛擬機器將會部署在相同的資源群組，但位於另一個 [Azure_region_2]。 (以您想要部署這些 Azure 虛擬機器的兩個不同 Azure 區域名稱取代 [Azure_region_1] 和 [Azure_region_2] 預留位置，包括方括弧。 例如，$location 1 = 'eastus'。 您可以使用 Get-AzLocation 列出所有位置。)：

   ```powershell
   $location1 = 'eastus'

   $location2 = 'westus'

   $rgName = 'az104-05-rg1'

   New-AzResourceGroup -Name $rgName -Location $location1
   ```

   >**注意**：先前使用的區域已經過測試，且在上次正式檢閱此實驗時已知可運作。 如果您想要使用不同的位置，或這些位置不再運作，您必須識別可以部署至標準 D2Sv3 虛擬機器的兩個不同區域。
   >
   >若要識別 Azure 區域，請從 Cloud Shell 中的 PowerShell 工作階段執行 **(Get-AzLocation).Location**
   >
   >識別出要使用的兩個區域之後，請在 Cloud Shell 中針對每個區域執行下列命令，以確認您可以部署標準 D2Sv3 虛擬機器
   >
   >```az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name" ```
   >
   >如果命令沒有傳回任何結果，則您需要選擇其他區域。 識別出兩個適當的區域之後，您就可以調整上述程式碼區塊中的區域。

1. 從 [Cloud Shell] 窗格中，執行下列命令，使用您上傳的範本和參數檔案，建立三個虛擬網路並將虛擬機器至其中：

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-05-vnetvm-loop-template.json `
      -TemplateParameterFile $HOME/az104-05-vnetvm-loop-parameters.json `
      -location1 $location1 `
      -location2 $location2
   ```

    >**注意**：等候部署完成，再繼續進行下一個步驟。 這應該大約需要 2 分鐘的時間。

1. 關閉 [Cloud Shell] 窗格。

#### <a name="task-2-configure-local-and-global-virtual-network-peering"></a>工作 2：設定本機和全域虛擬網路對等互連

在這項工作中，您將在於先前工作中部署的虛擬網路之間，設定本機和全域對等互連。

1. 在 Azure 入口網站中，搜尋並選取 [虛擬網路]。

1. 檢閱您在上一個工作中建立的虛擬網路，並確認前兩個虛擬網路位於相同的 Azure 區域，且第三個虛擬網路位於不同的 Azure 區域。

    >**注意**：所用於部署三個虛擬網路的範本可確保三個虛擬網路的 IP 位址範圍不會重疊。

1. 在虛擬網路清單中，按一下 [az104-05-vnet0]。

1. 在 [az104-05-vnet0] 虛擬網路刀鋒視窗的 [設定] 區段中，按一下 [對等互連]，然後按一下 [+ 新增]。

1. 使用下列設定新增對等互連 (將其他設定保持預設值)，然後按一下 [新增]：

    | 設定 | 值|
    | --- | --- |
    | 此虛擬網路：對等互連連結名稱 | **az104-05-vnet0_to_az104-05-vnet1** |
    | 此虛擬網路：對遠端虛擬網路的流量 | **允許 (預設)** |
    | 此虛擬網路：從遠端虛擬網路轉送的流量 | **封鎖來自此虛擬網路外部的流量** |
    | 虛擬網路閘道 | **None** |
    | 遠端虛擬網路：對等互連連結名稱 | **az104-05-vnet1_to_az104-05-vnet0** |
    | 虛擬網路部署模型 | **資源管理員** |
    | 我知道我的資源識別碼 | 未選取 |
    | 訂用帳戶 | 您在此實驗中使用的 Azure 訂用帳戶名稱 |
    | 虛擬網路 | **az104-05-vnet1** |
    | 連到遠端虛擬網路的流量 | **允許 (預設)** |
    | 從遠端虛擬網路轉送的流量 | **封鎖來自此虛擬網路外部的流量** |
    | 虛擬網路閘道 | **None** |

    >**注意**：此步驟會建立兩個本機對等互連：一個從 az104-05-vnet0 到 az104-05-vnet1，另一個則從 az104-05-vnet1 到 az104-05-vnet0。

    >**注意**：如果您遇到 Azure 入口網站介面未顯示上一個工作中建立的虛擬網路的問題，則可以從 Cloud Shell 執行下列 PowerShell 命令來設定對等互連：
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet1' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet1.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet0' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. 在 [az104-05-vnet0] 虛擬網路刀鋒視窗的 [設定] 區段中，按一下 [對等互連]，然後按一下 [+ 新增]。

1. 使用下列設定新增對等互連 (將其他設定保持預設值)，然後按一下 [新增]：

    | 設定 | 值|
    | --- | --- |
    | 此虛擬網路：對等互連連結名稱 | **az104-05-vnet0_to_az104-05-vnet2** |
    | 此虛擬網路：對遠端虛擬網路的流量 | **允許 (預設)** |
    | 此虛擬網路：從遠端虛擬網路轉送的流量 | **封鎖來自此虛擬網路外部的流量** |
    | 虛擬網路閘道 | **None** |
    | 遠端虛擬網路：對等互連連結名稱 | **az104-05-vnet2_to_az104-05-vnet0** |
    | 虛擬網路部署模型 | **資源管理員** |
    | 我知道我的資源識別碼 | 未選取 |
    | 訂用帳戶 | 您在此實驗中使用的 Azure 訂用帳戶名稱 |
    | 虛擬網路 | **az104-05-vnet2** |
    | 連到遠端虛擬網路的流量 | **允許 (預設)** |
    | 從遠端虛擬網路轉送的流量 | **封鎖來自此虛擬網路外部的流量** |
    | 虛擬網路閘道 | **None** |

    >**注意**：此步驟會建立兩個本機對等互連：一個從 az104-05-vnet0 到 az104-05-vnet2，另一個則從 az104-05-vnet2 到 az104-05-vnet0。

    >**注意**：如果您遇到 Azure 入口網站介面未顯示上一個工作中建立的虛擬網路的問題，則可以從 Cloud Shell 執行下列 PowerShell 命令來設定對等互連：
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. 瀏覽回到 [虛擬網路] 刀鋒視窗，然後在虛擬網路清單中，按一下 **az104-05-vnet1**。

1. 在 [az104-05-vnet1] 虛擬網路刀鋒視窗的 [設定] 區段中，按一下 [對等互連]，然後按一下 [+ 新增]。

1. 使用下列設定新增對等互連 (將其他設定保持預設值)，然後按一下 [新增]：

    | 設定 | 值|
    | --- | --- |
    | 此虛擬網路：對等互連連結名稱 | **az104-05-vnet1_to_az104-05-vnet2** |
    | 此虛擬網路：對遠端虛擬網路的流量 | **允許 (預設)** |
    | 此虛擬網路：從遠端虛擬網路轉送的流量 | **封鎖來自此虛擬網路外部的流量** |
    | 虛擬網路閘道 | **None** |
    | 遠端虛擬網路：對等互連連結名稱 | **az104-05-vnet2_to_az104-05-vnet1** |
    | 虛擬網路部署模型 | **資源管理員** |
    | 我知道我的資源識別碼 | 未選取 |
    | 訂用帳戶 | 您在此實驗中使用的 Azure 訂用帳戶名稱 |
    | 虛擬網路 | **az104-05-vnet2** |
    | 連到遠端虛擬網路的流量 | **允許 (預設)** |
    | 從遠端虛擬網路轉送的流量 | **封鎖來自此虛擬網路外部的流量** |
    | 虛擬網路閘道 | **None** |

    >**注意**：此步驟會建立兩個本機對等互連：一個從 az104-05-vnet1 到 az104-05-vnet2，另一個則從 az104-05-vnet2 到 az104-05-vnet1。

    >**注意**：如果您遇到 Azure 入口網站介面未顯示上一個工作中建立的虛擬網路的問題，則可以從 Cloud Shell 執行下列 PowerShell 命令來設定對等互連：
    
   ```powershell
   $rgName = 'az104-05-rg1'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 

#### <a name="task-3-test-intersite-connectivity"></a>工作 3：測試站台間連線能力

在這項工作中，您將針對在上一個工作中透過本機和全域對等互連連線的三個虛擬網路，測試虛擬機器之間的連線能力。

1. 在 Azure 入口網站中，搜尋並選取 [虛擬機器]。

1. 在虛擬機器清單中，按一下 **az104-05-vm0**。

1. 在 [az104-05-vm0] 刀鋒視窗上，按一下 [連線]，在下拉式功能表中，按一下 [RDP]，在 [透過 RDP 連線] 刀鋒視窗上，按一下 [下載 RDP 檔案]，然後遵循提示來啟動遠端桌面工作階段。

    >**注意**：此步驟是指透過遠端桌面從 Windows 電腦進行連線。 在 Mac 電腦上，您可以使用 Mac App Store 的遠端桌面用戶端；而在 Linux 電腦上，您可以使用開放原始碼 RDP 用戶端軟體。

    >**注意**：連線至目標虛擬機器時，您可以忽略任何警告提示。

1. 出現提示時，請使用您參數檔案中的 **Student** 使用者名稱和密碼登入。 

1. 在 **az104-05-vm0** 的遠端桌面工作階段中，以滑鼠右鍵按一下 [開始] 按鈕，然後在滑鼠右鍵功能表中，按一下 [Windows PowerShell (管理員)]。

1. 在 [Windows PowerShell 主控台] 視窗中，執行下列命令以測試透過 TCP 通訊埠 3389 與 **az104-05-vm1** 的連線能力 (其私人 IP 位址為 **10.51.0.4**)：

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**注意**：測試會使用 TCP 3389，因為這是作業系統防火牆預設允許的通訊埠。

1. 檢查命令的輸出，並確認連線成功。

1. 在 [Windows PowerShell 主控台] 視窗中，執行下列命令以測試與 **az104-05-vm2** 的連線能力 (其私人 IP 位址為 **10.52.0.4**)：

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

1. 切換回實驗電腦上的 Azure 入口網站，然後瀏覽回到 [虛擬機器] 刀鋒視窗。

1. 在虛擬機器清單中，按一下 **az104-05-vm1**。

1. 在 [az104-05-vm1] 刀鋒視窗上，按一下 [連線]，在下拉式功能表中，按一下 [RDP]，在 [透過 RDP 連線] 刀鋒視窗上，按一下 [下載 RDP 檔案]，然後遵循提示來啟動遠端桌面工作階段。

    >**注意**：此步驟是指透過遠端桌面從 Windows 電腦進行連線。 在 Mac 電腦上，您可以使用 Mac App Store 的遠端桌面用戶端；而在 Linux 電腦上，您可以使用開放原始碼 RDP 用戶端軟體。

    >**注意**：連線至目標虛擬機器時，您可以忽略任何警告提示。

1. 出現提示時，請使用您參數檔案中的 **Student** 使用者名稱和密碼登入。 

1. 在 **az104-05-vm1** 的遠端桌面工作階段中，以滑鼠右鍵按一下 [開始] 按鈕，然後在滑鼠右鍵功能表中，按一下 [Windows PowerShell (管理員)]。

1. 在 [Windows PowerShell 主控台] 視窗中，執行下列命令以測試透過 TCP 通訊埠 3389 與 **az104-05-vm2** 的連線能力 (其私人 IP 位址為 **10.52.0.4**)：

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**注意**：測試會使用 TCP 3389，因為這是作業系統防火牆預設允許的通訊埠。

1. 檢查命令的輸出，並確認連線成功。

#### <a name="clean-up-resources"></a>清除資源

>**注意**：請記得移除您不再使用的任何新建立的 Azure 資源。 移除未使用的資源可確保您不會看到非預期的費用。

>**注意**：如果無法立即移除實驗資源，請不要擔心。 有時候資源具有相依性，需要經過較長的時間才能刪除。 這是監視資源使用量的常見系統管理員工作，因此只需定期檢閱入口網站中的資源，查看清除的運作情況便可。 

1. 在 Azure 入口網站中，在 [Cloud Shell] 窗格內開啟 [PowerShell] 工作階段。

1. 執行下列命令，列出在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*'
   ```

1. 執行下列命令，刪除您在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-05*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：此命令以非同步方式執行 (由 --AsJob 參數決定)，所以您隨後能夠在相同 PowerShell 工作階段內立即執行另一個 PowerShell 命令，但需要經過幾分鐘後，才會實際移除資源群組。

#### <a name="review"></a>檢閱

在此實驗中，您已：

+ 佈建實驗環境
+ 設定本機和全域虛擬網路對等互連
+ 測試站台間連線
