---
lab:
  title: 實驗室 09b：實作 Azure 容器執行個體
  module: Administer PaaS Compute Options
---

# 實驗 09b - 實作 Azure 容器執行個體

## 實驗室簡介

在此實驗室中，您將瞭解如何實作和部署 Azure 容器執行個體。

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟是使用 **美國**東部撰寫。

## 預估時間：15 分鐘

## 實驗案例

您的組織有 Web 應用程式，可在內部部署資料中心的虛擬機上執行。 組織想要將所有應用程式移至雲端，但不想要管理大量的伺服器。 您決定評估 Azure 容器執行個體和 Docker。 
## 互動式實驗室模擬

您可能會發現互動式實驗室模擬對本主題很有用。 模擬可讓您以自己的步調點選類似的案例。 互動式模擬和此實驗室之間有差異，但許多核心概念都相同。 不需要 Azure 訂用帳戶。

+ [部署 Azure 容器執行個體](https://mslearn.cloudguides.com/en-us/guides/AZ-900%20Exam%20Guide%20-%20Azure%20Fundamentals%20Exercise%203)。 使用 Azure 容器執行個體 建立、設定及部署 Docker 容器。
  
+ [實作](https://mslabs.cloudguides.com/guides/AZ-104%20Exam%20Guide%20-%20Microsoft%20Azure%20Administrator%20Exercise%2014) Azure 容器執行個體。  使用 Azure 容器執行個體 部署 Docker 映射。 

## 作業技能

- 工作 1：使用 Docker 映射部署 Azure 容器實例。
- 工作 2：測試及驗證 Azure 容器實例的部署。


## 架構圖

![工作的圖表。](../media/az104-lab09b-aci-architecture.png)

## 工作 1：使用 Docker 映射部署 Azure 容器實例

在這項工作中，您將使用 Docker 映像建立簡單的 Web 應用程式。 Docker 是一個平臺，可讓您在稱為容器的隔離環境中封裝和執行應用程式。 Azure 容器執行個體 提供容器映像的計算環境。

1. 登入 **Azure 入口網站** - `https://portal.azure.com`。

1. 在 Azure 入口網站 中，搜尋並選取 `Container instances` ，然後在 [容器實例 **] 刀鋒視窗上**，按兩下 **[+ 建立**]。

1. 在 [建立容器執行個體] 刀鋒視窗的 [基本] 索引標籤上，指定下列設定 (其他設定維持預設值)：

    | 設定 | 值 |
    | ---- | ---- |
    | 訂用帳戶 | 選取您的 Azure 訂用帳戶 |
    | 資源群組 | `az104-rg9`（如有必要，請選取**** 新增） |
    | 容器名稱 | `az104-c1` |
    | 區域 | **美國** 東部 （或您附近可用的區域）|
    | 映像來源 | **快速入門映像** |
    | 映像 | **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux)** |

1. 按 **[下一步]：網络>** 並指定下列設定（讓其他人保留預設值）：

    | 設定 | 值 |
    | --- | --- |
    | DNS 名稱標籤 | 任何有效的全域唯一 DNS 主機名稱 |

    >**注意**：您的容器可以透過 dns-name-label.region.azurecontainer.io 供公眾存取。 如果您收到 **DNS 名稱標籤無法使用**錯誤訊息，請指定其他的值。

1. 按 **[下一步：進階>**，檢閱設定而不進行任何變更。

 1. 按兩下 [ **檢閱 + 建立**]，確定通過驗證，然後選取 [ **建立**]。

    >**注意**：等待部署完成。 這應該需要 2-3 分鐘的時間。

    >**注意**：在您等待的時候，可能會有興趣查看[範例應用程式背後的程式碼](https://github.com/Azure-Samples/aci-helloworld)。 若要檢視程式代碼，請瀏覽 \\應用程式資料夾。

## 工作 2：測試及驗證 Azure 容器實例的部署 

在這項工作中，您會檢閱容器實例的部署。 根據預設，Azure 容器實例可透過埠 80 存取。 部署實例之後，您可以使用您在上一個工作中提供的 DNS 名稱來巡覽至容器。

1. 在 [部署] 刀鋒視窗上，按一下 [移至資源] 連結。

1. 在容器執行個體的 [概觀] 刀鋒視窗上，確認 [狀態] 回報為 [執行中]。

1. 複製容器執行個體 **FQDN** 的值、開啟新的瀏覽器索引標籤，然後瀏覽至對應的 URL。

     ![入口網站中 ACI 概觀頁面的螢幕快照。](../media/az104-lab09b-aci-overview.png)

1. 確認 [歡迎使用 Azure 容器執行個體] 頁面顯示。 重新整理頁面數次，以建立一些記錄專案，然後關閉瀏覽器索引標籤。  

1. 在**容器實例刀鋒視窗的 [設定**] 區段中，按兩下 **[容器**]，然後按兩下 [**記錄**]。

1. 在瀏覽器中顯示應用程式，確認您看到所產生代表 HTTP GET 要求的記錄項目。
   
## 清除您的資源

如果您使用自己的訂用 **帳戶** ，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站 中，選取資源群組、選取 **[刪除資源群組**]、**輸入資源組名**，然後按兩下 [**刪除**]。
+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName`。
+ 使用 CLI， `az group delete --name resourceGroupName`。


## 重要心得

恭喜您完成實驗室。 以下是此實驗室的主要外賣。 

+ Azure 容器執行個體 （ACI） 是一項服務，可讓您在 Microsoft Azure 公用雲端上部署容器。
+ ACI 不需要您布建或管理任何基礎結構。
+ ACI 同時支援 Linux 容器和 Windows 容器。
+ ACI 上的工作負載通常會由某種程式或觸發程序啟動和停止，且通常短期運作。 

## 透過自學型訓練深入了解

+ [在 Azure 容器執行個體中執行容器映像](https://learn.microsoft.com/training/modules/create-run-container-images-azure-container-instances/)。 了解 Azure 容器執行個體如何協助您快速部署容器、如何設定環境變數，以及指定容器重新啟動原則。

    
