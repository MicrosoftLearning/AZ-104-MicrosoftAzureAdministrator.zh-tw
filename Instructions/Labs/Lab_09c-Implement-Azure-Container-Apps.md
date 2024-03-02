---
lab:
  title: 實驗室 09c：實作 Azure Container Apps
  module: Administer PaaS Compute Options
---

# 實驗室 09c - 實作 Azure Container Apps

## 實驗室簡介

在此實驗室中，您將瞭解如何實作及部署 Azure Container Apps。

此實驗室需要 Azure 訂用帳戶。 您的訂用帳戶類型可能會影響此實驗室中功能的可用性。 您可以變更區域，但步驟是使用 **美國**東部撰寫。

## 預估時間：15 分鐘

## 實驗案例

您的組織有 Web 應用程式，可在內部部署資料中心的虛擬機上執行。 組織想要將所有應用程式移至雲端，但不想要管理大量的伺服器。 您決定評估 Azure Container Apps。

## 互動式實驗室模擬

本主題沒有互動式實驗室模擬。 

## 作業技能

- 工作 1：建立及設定 Azure 容器應用程式和環境。
- 工作 2：測試及驗證 Azure 容器應用程式的部署。

## 架構圖

![工作的圖表。](../media/az104-lab09b-aca-architecture.png)

## 工作 1：建立及設定 Azure 容器應用程式和環境

Azure Container Apps 會進一步採用受控 Kubernetes 叢集的概念，並進一步管理叢集環境，並在叢集上提供其他受控服務。 不同於 Azure Kubernetes 叢集，您仍必須管理叢集，Azure Container Apps 實例會移除設定 Kubernetes 叢集的一些複雜度。

1. 從 Azure 入口網站 搜尋並選取 `Container Apps`。

1. 從 **[容器應用程式**] 中，選取 [ **建立**]。

1. 使用下列資訊填寫 [基本] 索引標籤上 **的詳細數據** 。*。

    | 設定 | 動作 |
    |---|---|
    | 訂用帳戶 | 選取您的 Azure 訂用帳戶 |
    | 資源群組 | `az104-rg9` |
    | 容器應用程式名稱 |  `my-app` |
    | 區域    | **美國** 東部 （或您附近可用的區域） |
    | Container Apps 環境 | 「保留預設」 |

1. 在 [ **容器]** 索引標籤上，確定 **[使用快速入門映射** ] 已啟用，且快速入門映射已設定為 **Simple hello world 容器**。

1. 選取 [檢閱**並建立]，然後**選取** [建立**]。

    >**注意：** 等候容器應用程式部署。 這需要幾分鐘的時間。 
 
## 工作 2：測試及驗證 Azure 容器應用程式的部署

根據預設，您建立的 Azure 容器應用程式會使用範例 Hello World 應用程式，接受埠 80 上的流量。 Azure Container Apps 將提供應用程式的 DNS 名稱。 複製並流覽至此 URL，以確保應用程式已啟動並執行。

1. 選取 **[移至資源** ] 以檢視新的容器應用程式。

1. 選取 [應用程式 URL *] 旁*的連結，以檢視您的應用程式。

    ![入口網站中 ACA 概觀頁面的螢幕快照。](../media/az104-lab09b-aca-overview.png)

1. 確認您收到 **Azure Container Apps 應用程式的即時** 訊息。
   
## 清除您的資源

如果您使用自己的訂用 **帳戶** ，需要一分鐘的時間才能刪除實驗室資源。 這可確保資源可以釋出，並將成本降到最低。 刪除實驗室資源最簡單的方式是刪除實驗室資源群組。 

+ 在 Azure 入口網站 中，選取資源群組、選取 **[刪除資源群組**]、**輸入資源組名**，然後按兩下 [**刪除**]。
+ 使用 Azure PowerShell， `Remove-AzResourceGroup -Name resourceGroupName`。
+ 使用 CLI， `az group delete --name resourceGroupName`。



## 重要心得

恭喜您完成實驗室。 以下是此實驗室的主要外賣。 

+ Azure Container Apps （ACA） 是無伺服器平臺，可讓您在執行容器化應用程式時維護較少的基礎結構並節省成本。
+ Container Apps 提供伺服器組態、容器協調流程和部署詳細數據。 
+ ACA 上的工作負載通常是長時間執行的進程，例如 Web 應用程式。

## 透過自學型訓練深入了解

+ [在 Azure Container Apps 中設定容器應用程式](https://learn.microsoft.com/training/modules/configure-container-app-azure-container-apps/)。 檢查 Azure Container Apps 的功能，然後著重於如何使用 Azure Container Apps 建立、設定、調整及管理容器應用程式。
     
