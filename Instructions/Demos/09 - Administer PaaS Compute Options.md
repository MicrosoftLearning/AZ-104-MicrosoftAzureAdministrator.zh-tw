---
demo:
  title: 示範 09：管理 PaaS 計算選項
  module: Administer PaaS Compute Options
---

# 09 - 管理 PaaS 計算選項

## 設定 Azure App Service 方案

在此示範中，我們將建立和使用 Azure App Service 方案。

**參考**：[管理App Service方案 - Azure App 服務](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

**參考**：[在 Azure App 服務 中相應增加應用程式](https://learn.microsoft.com/azure/app-service/manage-scale-up)

**參考**：[自動調整Azure App 服務](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. 使用 Azure 入口網站。 

1. 搜尋並選取 **App Service方案**。

1. 建立簡單的App Service計畫。 討論選取 Windows 或 Linux 的需求。 立即或後續步驟討論定價方案。 

1. 部署新的 App Service 方案。 

1. 檢閱 [**相應增加 (App Service方案) **] 刀鋒視窗。 討論**開發/測試與****生產**方案之間的差異。 檢閱功能清單。 

1. 檢閱 [**相應放大 (App Service方案) **] 刀鋒視窗。 檢閱 **Manual** 和 **Rule=based**之間的差異。 

## 設定 Azure 應用程式服務

在此示範中，我們將建立一個執行 Docker 容器的新 Web 應用程式。  容器將顯示歡迎訊息。

**參考**： [建立 Web 應用程式](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

在此工作中，我們將建立Azure App 服務 Web 應用程式。

1. 使用 Azure 入口網站。 

1. 搜尋並選取 **[應用程式服務**]。

1. **建立** **Web 應用程式**。

    - 發佈： **程式碼**。 檢閱其他選項。
    - 執行時間堆疊： **.Net**。 檢閱其他選項。
    - 作業系統： **Linux**

1. 選取 **[免費 F1** 服務方案]。

1. **檢閱 + 建立** Web 應用程式。 等候資源部署。

1. 在 [ **概觀]** 頁面上，確定 **[狀態** ] 為 [ **正在執行**]。

1. 選取 **URL** ，並確定預設預留位置頁面會載入。

1. 當您有時間時，請探索 **[部署位置]** 選項。 
## 設定 Azure 容器執行個體

在此示範中，我們會從 Azure 入口網站使用 Azure 容器執行個體 (ACI) 來建立、設定及部署容器。 ACI 應用程式會顯示具有公用 Microsoft Hello World 影像的靜態 HTML 頁面。 

[快速入門 - 將 Docker 容器部署至容器實例](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. 使用 Azure 入口網站。

1. 搜尋並選取 **[容器實例**]。

1. **建立** 新的容器實例。 

1. 填入 **[資源群組** ] 和 [ **容器名稱**]。 

1. 討論 **影像來源** 選項。 使用 **快速入門映射**。

1. 針對 **容器映射** ，請使用 **mcr.microsoft.com/azuredocs/aci-helloworld:latest (Linux) **。 此範例 Linux 映像會封裝以 Node.js 撰寫並提供靜態 HTML 網頁的小型 Web 應用程式。

1. 在 [網路]  頁面上，為您的容器指定 [DNS 名稱標籤]  。 

1. 將所有其他設定保留預設值，然後選取 [檢閱 + 建立]。

1. 等候資源部署。

1. 在重新執行的 [ **概** 觀] 頁面上，確定 **[狀態** ] 為 **[正在執行**]。

1. 流覽至容器實例的 **FQDN** ，並確定歡迎頁面隨即顯示。 

**注意**：若要避免額外的成本，請刪除資源。 
