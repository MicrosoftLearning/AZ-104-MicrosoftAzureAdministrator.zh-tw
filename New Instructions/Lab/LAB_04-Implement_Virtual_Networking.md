# 實驗 04 - 實作虛擬網路

## 實驗室簡介

此實驗室是專注於虛擬網路的三個實驗室中的第一個。 在此實驗室中，您將瞭解虛擬網路和子網的基本概念。 您也會瞭解如何使用網路安全組和應用程式安全組保護您的網路。 

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟是使用 **美國** 東部和 **西歐**撰寫。 

## 估計時間：40 分鐘

## 實驗案例 

您的全域組織計劃實作虛擬網路。 這些網路位於美國東部、西歐和東南亞。 立即的目標是要容納所有現有的資源。 不過，組織處於成長階段，並想要確保有額外的成長容量。

**CoreServicesVnet** 虛擬網路會部署在**美國東部**區域。 此虛擬網路的資源數目最多。 網路可透過 VPN 連線連線至內部部署網路。 此網路具有 Web 服務、資料庫和其他系統，這些系統是業務營運的關鍵。 共用服務，例如域控制器和 DNS 位於這裡。 預期會有大量成長，因此此虛擬網路需要大型位址空間。

**ManufacturingVnet** 虛擬網路部署在**西歐**區域，靠近您組織的製造設施位置。 此虛擬網路包含製造設施作業的系統。 組織預期其系統需要大量的內部連線裝置來擷取數據，例如溫度，而且需要可擴充的IP位址空間。

## 互動式實驗室模擬

您可能會發現數個互動式實驗室模擬適合本主題。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。 

+ [建立簡單的虛擬網路](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%204)。 建立具有兩部虛擬機的虛擬網路。 示範虛擬機可以通訊。 

+ [在 Azure](https://mslabs.cloudguides.com/guides/AZ-700%20Lab%20Simulation%20-%20Design%20and%20implement%20a%20virtual%20network%20in%20Azure) 中設計和實作虛擬網路。 建立資源群組，並使用子網建立虛擬網路。  

+ [實作](https://mslabs.cloudguides.com/en-us/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%208)虛擬網路。 建立和設定虛擬網路、部署虛擬機、設定網路安全組，以及設定 Azure DNS。

## 架構圖

![網路配置](../media/az104-lab04-arch-diagram.png)

這些虛擬網路和子網的結構化方式可容納現有的資源，但允許預測的成長。 讓我們建立這些虛擬網路與子網路，為我們的網路基礎結構奠定基礎。

>**您知道嗎？**：最好避免重疊的IP位址範圍，以減少問題並簡化疑難解答。 重迭是整個網路的問題，無論是雲端還是內部部署。 許多組織都會設計全企業IP尋址配置，以避免重疊並規劃未來成長。

## 工作

+ 工作 1：建立資源群組。
+ 工作 2：建立 CoreServicesVnet 虛擬網路和子網路。
+ 工作 3：建立 ManufacturingVnet 虛擬網路和子網路。
+ 工作 4：設定應用程式安全組與網路安全組之間的通訊。 


## 工作 1：建立資源群組

### 為此實驗室中的所有資源建立資源群組。 

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

1. 搜尋並選取 **[資源群組**]，然後選取 [ **+ 建立**]。  

1. 使用這些設定建立資源群組。 

    | **定位字元**         | **選項**                                 | **值**            |
    | --------------- | ------------------------------------------ | -------------------- |
    | 基本概念          | 資源群組                             | `az104-rg4` |
    |                 | 區域                                     | （美國） **美國東部**     |
    | 標籤            | 不需要任何變更                        |                      |
   
1. 完成時，請選取 [檢閱 + 建立]，然後選取 ****[建立**]。**
   
## 工作 2：建立 CoreServicesVnet 虛擬網路和子網路

組織會規劃核心服務的大量成長。 在這項工作中，您會建立虛擬網路和相關聯的子網，以容納現有的資源和計劃成長。

1. 搜尋並選取 **[虛擬網絡**]。

1. 選取 **[虛擬網络] 頁面上的 [建立** ]，然後完成 **[基本]** 和 **[IPv4 位址**]。 

1. 使用下表中的資訊來建立 CoreServicesVnet 虛擬網路。  

    | **定位字元**      | **選項**         | **值**            |
    | ------------ | ------------------ | -------------------- |
    | 基本概念       | 資源群組     | **az104-rg4** |
    |              | 名稱               | `CoreServicesVnet`     |
    |              | 區域             | （美國） **美國東部**         |
    | IP 位址 | IPv4 位址空間 | `10.20.0.0/16` （刪除或覆寫 IP 位址空間）     |


1. 在子網區域中，刪除 **預設** 子網。

1. 選取 **[+ 新增子網**]。 完成每個子網的名稱和地址資訊。 請務必為每個新子網選取 [ **新增** ]。 

    | **子網路**             | **選項**           | **值**              |
    | ---------------------- | -------------------- | ---------------------- |
    | SharedServicesSubnet   | 子網路名稱          | `SharedServicesSubnet`   |
    |                        | 起始位址     | `10.20.10.0`          |
    |                        | 大小                 | `/24` |
    | DatabaseSubnet         | 子網路名稱          | `DatabaseSubnet`         |
    |                        | 起始位址     | `10.20.20.0`        |
    |                        | 大小                 | `/24` |

1. 若要完成建立 CoreServicesVnet 及其相關聯的子網路，請選取 [檢閱 + 建立]****。

1. 確認您的設定已通過驗證，然後選取 [建立]****。

1. 等候虛擬網路部署，然後選取 **[移至資源**]。 

1. 在 [自動化 **] 區**段中，選取 **[導出範本**]，然後等候產生範本。

1. **下載** 範本。

1. 在本機計算機上流覽至 **[下載]** 資料夾，然後 **解壓縮** 所下載 zip 檔案中的所有檔案。 

1. 繼續之前，請確定您有兩個 **檔案 template.json** 和 **parameters.json**。 花一分鐘的時間檢閱檔案和 CoreServicesVnet 的相關信息。 您將使用此範本在下一個工作中建立 ManufacturingVnet。 
 
## 工作 3：建立 ManufacturingVnet 虛擬網路和子網路

在這項工作中，您會建立ManufacturingVnet虛擬網路和相關聯的子網。 組織預期製造業辦公室的成長，因此子網的大小會隨著預期的成長而調整規模。

1. 編輯 Downloads** 資料夾中的**本機 **template.json** 檔案。 如果您使用 Visual Studio Code，請確定您是在信任的視窗中**工作**，而不是在受限制模式**中**工作。 

### 對 ManufacturingVnet 虛擬網路進行變更

1. 將所有出現的 **CoreServicesVnet** 取代為 `ManufacturingVnet`。 

1. 將 eastus** 的所有出現專案**取代為 `westeurope`。 

1. 將所有出現的 **10.20.0.0/16** 取代為 `10.30.0.0/16`。 

### 變更 ManufacturingVnet 子網

1. 將所有出現的 **SharedServicesSubnet** 變更為 `SensorSubnet1`。

1. 將所有出現的 **10.20.10.0/24** 變更為 `10.30.20.0/24`。

1. 將所有出現的 **DatabaseSubnet** 變更為 `SensorSubnet2`。

1. 將所有出現的 **10.20.20.0/24** 變更為 `10.30.21.0/24`。

1. 讀取檔案，並確保所有專案看起來都正確。

1. 請務必**儲存**您的變更。

    >**注意：** 如果這太困難了，最終完成的檔案位於 Lab 04 Downloads 資料夾中。 

## 對 parameters.json 檔案進行變更

1. 編輯本機 **parameters.json** 檔案，並將 CoreServicesVnet** 變更**為 `ManufacturingVnet`。

1. 請確定所有專案看起來都正確，並 **儲存** 您的變更。 

    >**注意：** 您現在可以使用 Azure PowerShell （選項 1） 或 Bash 殼層 （選項 2） 來部署範本。 您選擇的，但只執行一種部署。 

### 使用 Azure Powershell 部署範本 （選項 1）

1. 開啟 Cloud Shell，然後選取 **[PowerShell**]。

1. 如有必要，請使用進 **階** 設定來建立 Cloud Shell 的磁碟記憶體。 詳細步驟位於實驗室 03。 

1. 在 Cloud Shell 中，使用 **上傳** 圖示來上傳範本和參數檔案。 您必須個別上傳每一個。

1. 確認您的檔案可在 Cloud Shell 記憶體中使用。

    ```powershell
    dir
    ```

1. 將範本部署至 az104-rg4 資源群組。

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-rg4 -TemplateFile template.json -TemplateParameterFile parameters.json
    ```
1. 請確定命令完成，且 ProvisioningState 為 **Succeeded**。

    >**注意：** 如果您需要變更檔案，請先確定 **rm** （移除） 舊檔案，再上傳新檔案。 
    


1. 在繼續之前，請返回入口網站，並確定 **已建立 ManufacturingVnet** 虛擬網路和子網。 您可能需要重新 **整理** 虛擬網路頁面。 


### 使用 Bash 部署樣本 （選項 2）


1. 開啟 Cloud Shell，然後選取 **[Bash**]。

1. 如有必要，請使用進 **階** 設定來建立 Cloud Shell 的磁碟記憶體。

1. 在 Cloud Shell 中，使用 **上傳** 圖示來上傳範本和參數檔案。 您必須個別上傳每一個。

1. 確認您的檔案可在 Cloud Shell 記憶體中使用。

    ```bash
    ls
    ```

1. 將範本部署至 az104-rg4 資源群組。

    ```bash
    az deployment group create --resource-group az104-rg4 --template-file template.json --parameters parameters.json
    ```
    
1. 請確定命令完成，且 ProvisioningState 為 **Succeeded**。

1. 返回入口網站，並確定 **已建立ManufacturingVnet** 和相關聯的子網。 您可能需要重新 **整理** 虛擬網路頁面。 
   
## 工作 4：設定應用程式安全組與網路安全組之間的通訊 

在這項工作中，我們會建立應用程式安全組和網路安全組。 NSG 會有允許來自 ASG 流量的輸入安全性規則。 

### 建立應用程式安全組 （ASG）

1. 在 Azure 入口網站 中，搜尋並選取 **[應用程式安全組**]。

1. 按兩下 [ **建立** ]，並提供基本資訊。

    | 設定 | 值 |
    | -- | -- |
    | 訂用帳戶 | *訂用帳戶* |
    | 資源群組 | **az104-rg4** |
    | 名稱 | `asg-web` |
    | 區域 | **(美國) 美國東部**  |

1. 按兩下 [ **檢閱 + 建立** ]，然後在驗證之後按兩下 [ **建立**]。

### 建立網路安全組並將它與 ASG 子網產生關聯

1. 在 Azure 入口網站 中，搜尋並選取 **[網路安全組**]。

1. 選取 **[+ 建立** ]，並在 [ **基本] 索引** 標籤上提供資訊。 

    | 設定 | 值 |
    | -- | -- |
    | 訂用帳戶 | *訂用帳戶* |
    | 資源群組 | **az104-rg4** |
    | 名稱 | `myNSGSecure` |
    | 區域 | **(美國) 美國東部**  |

1. 按兩下 [ **檢閱 + 建立** ]，然後在驗證之後按兩下 [ **建立**]。

1. 建立 NSG 之後，按兩下 **[移至資源**]。

1. 在 **[設定 下**按兩下按一下 [**子網**]，然後按兩下 [**關聯**]。

    | 設定 | 值 |
    | -- | -- |
    | 虛擬網路 | **CoreServicesVnet （az104-rg4）** |
    | 子網路 | **SharedServicesSubnet** |

1. 按兩下 [ **確定** ] 以儲存關聯。

### 設定輸入安全性規則

1. 在 **[設定**] 區域中，選取 [**輸入安全性規則**]。

1. 檢閱預設輸入規則。 請注意，只允許其他虛擬網路和負載平衡器存取。

1. 選取 **+ 新增**。

1. 在 [ **新增輸入埠規則]** 刀鋒視窗上，使用下列資訊來新增輸入埠規則，然後選取 [ **新增**]。

    | 設定 | 值 |
    | -- | -- |
    | 來源 | **any** |
    | 來源連接埠範圍 |  ***** |
    | 目的地 | **應用程式安全組** |
    | 目的地應用程式安全性群組 | **asg-web** |
    | 服務 | **自訂** （請注意您的其他選擇） |
    | 目的地連接埠範圍 | **80,443** |
    | 通訊協定 | **TCP** |
    | 動作 | **允許** |
    | 優先順序 | **100** |
    | 名稱 | **AllowASG** |

1. 建立 NSG 規則之後，請花一分鐘的時間檢閱預設 **的傳出安全性規則**。

## 檢閱實驗室的要點

恭喜您完成實驗室。 以下是此實驗室的主要外賣。 

+ 虛擬網路是雲端中您自己的網路表示法。 
+ 設計虛擬網路時，最好避免重疊的IP位址範圍。 這可減少問題並簡化疑難解答。
+ 子網路是虛擬網路中的一個 IP 位址範圍。 您可以針對組織和安全性，將虛擬網路分成多個子網路。
+ 網路安全組包含允許或拒絕網路流量的安全性規則。 有預設的傳入和傳出規則，您可以自訂您的需求。
+ 應用程式安全組是用來保護具有一般功能的伺服器群組，例如網頁伺服器或資料庫伺服器。 

## 清除您的資源

如果您使用自己的訂用帳戶，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站 中，選取資源群組、選取 **[刪除資源群組**]、**輸入資源組名**，然後按兩下 [**刪除**]。

+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName`。

+ 使用 CLI， `az group delete --name resourceGroupName`。