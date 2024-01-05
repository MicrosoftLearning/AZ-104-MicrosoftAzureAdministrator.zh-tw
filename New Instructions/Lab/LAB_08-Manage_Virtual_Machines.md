---
lab:
  title: 實驗室 08：管理 虛擬機器
  module: Administer Virtual Machines
---

# 實驗室 08 – 管理虛擬機器

## 實驗室簡介

在此實驗室中，您將比較虛擬機的手動調整與虛擬機的自動調整。 您將瞭解如何設定及調整單一虛擬機的大小。 您將瞭解如何建立虛擬機擴展集並設定自動調整。

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟是使用美國東部撰寫。

## 預估時間：50 分鐘

## 實驗案例

您的組織想要探索部署和設定 Azure 虛擬機。 首先，您必須判斷使用 Azure 虛擬機器時可以實作的不同計算和儲存體復原與延展性選項。 接下來，您需要調查使用 Azure 虛擬機器擴展集時可用的計算和儲存體復原和延展性選項。

## 互動式實驗室模擬

您可能會發現互動式實驗室模擬對本主題很有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。

+ [在入口網站中](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%201)建立虛擬機。 建立虛擬機、連線並安裝 Web 伺服器角色。 
+ [使用範本](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%209)部署虛擬機。 探索快速入門資源庫並找出虛擬機範本。 部署範本並驗證部署。
+ [使用 PowerShell](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2010) 建立虛擬機。 使用 Azure PowerShell 部署虛擬機。 檢閱 Azure Advisor 建議。 
+ [使用 CLI](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%2011) 建立虛擬機。 使用 CLI 來部署虛擬機。 檢閱 Azure Advisor 建議。 

## 工作

+ 工作 1：使用 Azure 入口網站 部署區域復原的 Azure 虛擬機
+ 工作 2：管理虛擬機的計算和記憶體調整
+ 工作 3：實作 Azure 虛擬機器擴展集
+ 工作 4：調整 Azure 虛擬機器擴展集
+ 工作 5：使用 Azure PowerShell 建立虛擬機（選項 1）
+ 工作 6：使用 CLI 建立虛擬機 （選項 2） 




## 工作 1 和 2：Azure 虛擬機器 架構圖表

![架構工作的圖表。](../media/az104-lab08a-architecture-diagram.png)

## 工作 1：使用 Azure 入口網站 部署區域復原的 Azure 虛擬機

在這項工作中，您將使用 Azure 入口網站，將兩部 Azure 虛擬機部署到不同的可用性區域。 可用性區域為虛擬機提供最高層級的運行時間 SLA，且為 99.99%。 若要達到此 SLA，您必須在不同的可用性區域中部署至少兩部虛擬機。

1. 登入 Azure 入口網站 - `https://portal.azure.com`

1. 搜尋並選取 `Virtual machines` [虛擬機 **] 刀鋒**視窗上的 **[+ 建立**]，然後在下**拉式清單中選取 [+ Azure 虛擬機**]。

1. 在 [**建立虛擬機 **] 刀鋒視窗的 **[基本]** 索引標籤上，於 [可用性區域 **] 下拉功能表中**，將複選標記放在 [區域 2 **] 旁**。 這應該同時 **選取區域 1** 和 **區域 2**。

    >**注意**：這會在選取的區域部署兩部虛擬機，每個區域中各一部。 您可以達到99.99%運行時間 SLA，因為您至少有兩部 VM 分散到至少兩個區域。 在您可能只需要一部 VM 的情況下，最佳做法是仍將 VM 部署至區域，以確保磁碟和對應的資源位於相同的區域中。

1. 在 [基本] 索引標籤上，使用下列設定來完成字段（讓其他人保留預設值）：

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | Azure 訂用帳戶的名稱 |
    | 資源群組 |  **az104-rg8** （如有必要，請按兩下 [ **新建**] |
    | 虛擬機名稱 | `az104-vm1` 和 `az104-vm2` （選取這兩個可用性區域之後，請選取 **[VM 名稱] 字段下的 [編輯名稱** ]。 |
    | 區域 | **美國東部** |
    | 可用性選項 | **可用性區域** |
    | 可用性區域 | **區域 1， 2** （閱讀有關使用虛擬機擴展集的注意事項） |
    | 安全性類型 | **標準** |
    | 映像 | **Ubuntu Server 20.04 LTS - x64 Gen2** |
    | Azure Spot 執行個體 | **unchecked** |
    | 大小 | **標準 D2s v3** |
    | 驗證類型 | **密碼** |
    | 使用者名稱 | `localadmin` |
    | 密碼 | **提供安全的密碼** |
    | 公用輸入連接埠 | **無** |
    | 您要使用現有的 Windows Server 授權嗎？ | **未選取** |

    ![建立 VM 頁面的螢幕快照。](../media/az104-lab08-create-vm.png)

1. 按一下 [下一步：**磁碟 >]** ，在 [建立虛擬機器] 刀鋒視窗的 [磁碟] 索引標籤上，指定下列設定 (其他設定請保留預設值)：

    | 設定 | 值 |
    | --- | --- |
    | OS 磁碟類型 | **進階 SSD** |
    | 启用超级磁盘兼容性 | **未選取** |

1. 按 **[下一步]：網络>** 採用預設值，但未提供負載平衡器。 
   
    |負載平衡選項 | **沒有** |
    
1. 按一下 [下一步：**管理 >]** ，然後在 [建立虛擬機器] 刀鋒視窗的 [管理] 索引標籤上，指定下列設定 (將其他設定保留為預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 修補程式協調流程選項 | **Azure 已協調** |  

1. 按一下 [Next: Monitoring >] \(下一步：監視 >\)，然後在 [建立虛擬機器] 刀鋒視窗的 [監視] 索引標籤上，指定下列設定 (將其他設定保留為預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 開機診斷 | **停用** |

1. 按一下 [下一步：**進階 >]** ，在 [建立虛擬機器] 刀鋒視窗的 [進階] 索引標籤上，檢閱可用的設定而不修改其中任何設定，然後按一下 [檢閱 + 建立]。

1. 在 [檢閱 + 建立] 刀鋒視窗上，按一下 [建立]。

    >**注意：** 監視 **通知** 訊息，並等候部署完成。 

## 工作 2：管理虛擬機的計算和記憶體調整

在這項工作中，您會將虛擬機的大小調整為不同的 SKU，以調整其計算。 Azure 提供 VM 大小選取的彈性，讓您可以調整 VM 一段時間，如果 VM 需要更多（或更少）的計算和記憶體配置。 此概念延伸至磁碟，您可以在其中修改磁碟的效能，或增加配置的容量。

1. 在 Azure 入口網站 中，搜尋並選取 **az104-vm1**。

1. 在 **az104-vm1** 虛擬機刀鋒視窗上，按兩下 **[大小** ]，並將虛擬機大小設定為 **[DS1_v2** ]，然後按兩下 [ **重設大小]**

    >**注意**：如果無法使用**標準 DS1_v2**，請選擇另一個大小。

    ![調整虛擬機大小的螢幕快照。](../media/az104-lab08-resize-vm.png)

1. 在 **[az104-vm1** 虛擬機] 刀鋒視窗上，按兩下 **[磁碟**]，在 [數據磁碟 **] 下**，按兩下 [**+ 建立] 並連結新的磁碟**。

1. 使用下列設定建立受控磁碟 (將其他設定保持預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 磁碟名稱 | `vm1-disk1` |
    | 儲存體類型 | **標準 HDD** |
    | 大小 (GiB) | `32` |

1. 按一下 **套用**。

1. 建立磁碟之後，按兩下 [ **中斷連結**]，然後按兩下 [ **套用**]。
    
    >**注意**：您可能需要向右卷動，才能看到 *卸離* 圖示。

1. 從 Azure 入口網站 搜尋並選取 `Disks`。

1. 從磁碟清單中，選取 **vm1-disk1** 物件。

1. 從 vm1-disk1，選取 [ **大小 + 效能**]。

1. 從 [大小 + 效能] 中，將記憶體類型設定為 **[標準 SSD**]，然後按兩下 [ **儲存**]。

    >**注意**：您無法在連接磁碟或 VM 執行時變更磁碟的儲存類型。 

1. 流覽回 **az104-vm1** 虛擬機，然後選取 [ **磁碟**]。

1. 確認磁碟現在是 **標準 HDD**。

## 工作 3 和 4：Azure 虛擬機器擴展集 架構圖表

![架構工作的圖表。](../media/az104-lab08b-architecture-diagram.png)

## 工作 3：實作 Azure 虛擬機器擴展集

在這項工作中，您將跨可用性區域部署 Azure 虛擬機擴展集。 使用個別 VM 時，如果您的應用程式需要額外的計算，則需要其他自動化來部署和設定其他 VM。 VM 擴展集可讓您設定可讓擴展集自動相應增加或減少集合中 VM 數目的計量或條件，以減少自動化的系統管理負荷。

1. 在 Azure 入口網站 中，搜尋並選取 `Virtual machine scale sets` [虛擬機擴展集 **] 刀鋒視窗上的 **[**+ 建立**]。

1. 在 [**建立虛擬機擴展集 **] 刀鋒視窗的 **[基本]** 索引卷標上，指定下列設定（讓其他人保留預設值），然後按 **[下一步：Spot >**：

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | Azure 訂用帳戶的名稱  |
    | 資源群組 | **az104-rg8**  |
    | [虛擬機器擴展集名稱] | `vmss1` |
    | 區域 | **美國** 東部（或您附近的區域） |
    | 可用性區域 | **區域 1、2、3** |
    | 協調流程模式 | **Uniform** |
    | 安全性類型 | **標準** | 
    | 映像 | **Windows Server 2019 Datacenter - x64 Gen2** |
    | 使用 Azure 現成 VM 折扣執行 | **未選取** |
    | 大小 | **標準 D2s_v3** |
    | 使用者名稱 | `localadmin` |
    | 密碼 | **提供安全的密碼**  |
    | 已經有 Windows Server 授權了嗎? | **未選取** |

    >**注意**：如需支援將Windows虛擬機器部署至可用性區域的 Azure 區域清單，請參閱[什麼是 Azure 中的可用性區域？](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

    ![[建立 vmss] 頁面的螢幕快照。 ](../media/az104-lab08-create-vmss.png)

1. 在 [ **現成]** 索引標籤上，接受預設值，然後選取 **[下一步：磁盘>**]。

1. 在 [ **磁碟** ] 索引標籤上，接受預設值，然後按 **[下一步：網络>**。

1. 在 [**網络] 索引**標籤上，按兩下 [虛擬網络] 文字框中下方的** **[**建立虛擬網络**] 連結，並使用下列設定建立新的虛擬網路（讓其他人保留預設值）。  完成後，選取 [確定]****。 

    | 設定 | 值 |
    | --- | --- |
    | 名稱 | `vmss-vnet` |
    | 位址範圍 | `10.82.0.0/20` |
    | 子網路名稱 | `subnet0` |
    | 子網路範圍 | `10.82.0.0/24` |

1. 在 [ **網络] 索引** 標籤中，按兩下 **網路介面專案右邊的 [編輯網路介面** ] 圖示。

1. 在 [編輯網路介面] 刀鋒視窗的 [NIC 網路安全性群組] 區段中，按一下 [進階]，然後按一下 [設定網路安全性群組] 下拉式清單底下的 [新建]。

1. 在 [建立網路安全性群組] 刀鋒視窗中指定下列設定 (將其他設定保留為預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 名稱 | **vmss1-nsg** |

1. 按一下 [新增輸入規則]，並使用下列設定新增輸入安全性規則 (將其他設定保留為預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 來源 | **任何** |
    | 來源連接埠範圍 | * |
    | Destination | **任何** |
    | 服務 | **HTTP** |
    | 動作 | **允許** |
    | 優先順序 | **1010** |
    | 名稱 | `allow-http` |

1. 按一下 [新增]，然後在 [建立網路安全性群組] 刀鋒視窗上，按一下 [確定]。

1. 在 [ **編輯網络介面]** 刀鋒視窗的 **[公用IP 位址** ] 區段中，按兩下 **[已啟用]** ，然後按兩下 [ **確定**]。

1. 在 [網络] 索引**標籤的 ****[負載平衡**] 區段底下，指定下列專案（讓其他人保留預設值）。

    | 設定 | 值 |
    | --- | --- |
    | 負載平衡選項 | **Azure Load Balancer** |
    | 選取負載平衡器 | **建立負載平衡器** |
    
1.  在 [建立負載平衡器] 頁面上，指定負載平衡器名稱並採用預設設定。 完成時按一下 [建立]，然後前往**下一步：縮放 >** 。
    
    | 設定 | 值 |
    | --- | --- |
    | 負載平衡器名稱 | `vmss-lb` |

1. 在 [ **調整]** 索引標籤上，指定下列設定（讓其他人保留預設值），然後按 **[下一步：管理>**：

    | 設定 | 值 |
    | --- | --- |
    | 初始執行個體計數 | `2` |
    | 調整原則 | **手動** |

1. 在 [ **管理]** 索引標籤上，指定下列設定（讓其他人保留預設值）：

    | 設定 | 值 |
    | --- | --- |
    | 開機診斷 | **停用** |
    
1. 按一下 [下一步:**健全狀況 >]** :

1. 在 [ **健全狀況]** 索引標籤上，檢閱預設設定而不進行任何變更，然後按 **[下一步：進階] >**。

1. 在 [ **進階]** 索引標籤上，按兩下 [ **檢閱 + 建立**]。

1. 在 [ **檢閱 + 建立]** 索引標籤上，確定通過驗證，然後按兩下 [ **建立**]。

    >**注意**：等候虛擬機器擴展集部署完成。 這大約需要 5 分鐘的時間。


## 工作 4：調整 Azure 虛擬機器擴展集

在這項工作中，您會使用自定義調整規則來調整虛擬機擴展集。

1. 選取 **[移至資源** ] 或搜尋 ，然後選取 **vmss1** 擴展集。

1. 從擴展集視窗左側的功能表中選擇 **[調整** ]。

1. 請注意， **調整模式** 可以根據 **計量** 或 **調整為特定實例計數**進行調整。 在具有少量 VM 實例的擴展集中，增加或減少實例計數可能最好。 在具有大量 VM 實例的擴展集中，根據計量進行調整可能更合適。

1. 選取 [ **自定義自動調整**] 按鈕。 然後選取 [新增規則]。 

### 相應放大規則

1. 讓我們建立相應放大規則，以自動增加 VM 實例的數目。 當平均 CPU 負載在 10 分鐘的期間內大於 70% 時，此規則就會相應放大。 當規則觸發時，VM 實例數目會增加 20%。 選取項目之後，按兩下 [ **新增** ]。 

    | 設定 | 值 |
    | --- | --- |
    | 計量來源 | **目前資源 （vmss1）** |
    | 計量命名空間 | **虛擬機器主機** |
    | 度量名稱 | **CPU** 百分比 （檢閱其他選擇） |
    | 運算子 | **大於** |
    | 用於觸發調整動作的計量閾值 | **70** |
    | 持續時間 (分鐘) | **10** |
    | 時間粒紋統計資料 | **平均** |
    | 作業 | **增加百分比 （** 檢閱其他選擇） |
    | 緩和時間 (分鐘) | **5** |
    | 百分比 | **20** |
    
    ![調整新增規則頁面的螢幕快照。](../media/az104-lab08-scale-rule.png)

### 調整規則

1. 在晚上或週末，需求可能會減少，因此請務必建立規則的規模。

1. 讓我們建立一個規則，以減少擴展集中的 VM 實例數目。 當平均 CPU 負載在 10 分鐘期間低於 30% 時，實例數目應該會降低。 當規則觸發時，VM 實例數目會減少 20%。 調整設定，然後選取 [ **新增**]。

    | 設定 | 值 |
    | --- | --- |
    | 運算子 | **小於** |
    | 臨界值 | **30** |
    | 作業 | **減少百分比（** 檢閱您的其他選擇） |
    | 執行個體計數 | **20** |

### 設定實例限制

1. 套用自動調整規則時，實例限制可確保您不會相應放大到實例數目上限，或相應縮小到實例數目下限。

1. **規則之後的 **[調整**] 頁面上會顯示實例限制**。 

    | 設定 | 值 |
    | --- | --- |
    | 最小值 | **2** |
    | 最大值 | **10** |
    | 預設 | **2** |

1. 請務必儲存**** 變更

1. 在 **[vmss1]** 頁面上，選取 [ **實例**]。 這是您要監視虛擬機實例數目的位置。 

## 工作 5：使用 Azure PowerShell 建立虛擬機（選項 1）

1. 登入 Azure 入口網站 - `https://portal.azure.com`

1. 使用功能表來啟動 **Cloud Shell** 工作階段。 或者，直接瀏覽至 `https://shell.azure.com`。

1. 如有必要，請設定 Cloud Shell。 請務必選取 **PowerShell**。

1. 執行下列命令以建立虛擬機。 出現提示時，請提供 VM 的使用者名稱和密碼。 當您等候查看 [New-AzVM](https://learn.microsoft.com/powershell/module/az.compute/new-azvm?view=azps-11.1.0) 命令參考，以取得與建立虛擬機相關聯的所有參數。

    ```powershell
    New-AzVm `
    -ResourceGroupName 'az104-rg8' `
    -Name 'myPSVM' `
    -Location 'East US' `
    -Image 'Win2019Datacenter' `
    -Zone '1' `
    -Size 'Standard_D2s_v3' 
    -Credential '(Get-Credential)' `

1. Once the command completes, use **Get-AzVM** to list the virtual machines in your resource group. 

    ```powershell
    Get-AzVM `
    -ResourceGroupName 'az104-rg8'
    -Status

1. Verify your new virtual machine is listed and the **Status** is **Running**.

1. Use **Stop-AzVM** to deallocate your virtual machine. Type **Yes** to confirm. 

    ```
    Stop-AzVM `
    -ResourceGroupName 'az104-rg8'
    -Name 'myPSVM' `

1. 使用 **Get-AzVM** 搭配 **-Status** 參數來確認機器已 **解除分配**。

    >**你知道嗎？** 當您使用 Azure 停止虛擬機時，狀態會 *解除*分配。 這表示任何非靜態公用IP都已發行，而您停止支付 VM 的計算成本。

## 工作 6：使用 CLI 建立虛擬機 （選項 2）

1. 登入 Azure 入口網站 - `https://portal.azure.com`

1. 使用功能表來啟動 **Cloud Shell** 工作階段。 或者，直接瀏覽至 `https://shell.azure.com`。

1. 如有必要，請設定 Cloud Shell。 請務必選取 **Bash**。

1. 執行下列命令以建立虛擬機。 出現提示時，請提供 VM 的使用者名稱和密碼。 當您等候時， [請查看 az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) 命令參考，以取得與建立虛擬機相關聯的所有參數。


    ```sh
    az vm create --name myCLIVM --resource-group az104-rg8 --image Ubuntu2204 --admin-username localadmin --generate-ssh-keys 
    
1. Once the command completes, use **az vm show** to verify your machine was created.

    ```sh
    az vm show --name  myCLIVM --resource-group az104-rg8 --show-details

1. Verify the **powerState** is **VM Running**.

1. Use **az vm deallocate** to deallocate your virtual machine. Type **Yes** to confirm. 

    ```sh
    az vm deallocate --resource-group az104-rg8 --name myCLIVM

1. Use **az vm show** to ensure the **powerState** is **VM deallocated**.

    >**Did you know?** When you use Azure to stop your virtual machine, the status is *deallocated*. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ Azure virtual machines are on-demand, scalable computing resources.
+ Configuring Azure virtual machines includes choosing an operating system, size, storage and networking settings. 
+ Azure Virtual Machine Scale Sets let you create and manage a group of load balanced VMs.
+ The virtual machines in a Virtual Machine Scale Set are created from the same image and configuration. 
+ In a Virtual Machine Scale Set the number of VM instances can automatically increase or decrease in response to demand or a defined schedule.

## Cleanup your resources

If you are working with your own subscription take a minute to delete the lab resources. This will ensure resources are freed up and cost is minimized. The easiest way to delete the lab resources is to delete the lab resource group. 

+ In the Azure portal, select the resource group, select **Delete the resource group**, **Enter resource group name**, and then click **Delete**.
+ Using Azure PowerShell, `Remove-AzResourceGroup -Name resourceGroupName`.
+ Using the CLI, `az group delete --name resourceGroupName`.


