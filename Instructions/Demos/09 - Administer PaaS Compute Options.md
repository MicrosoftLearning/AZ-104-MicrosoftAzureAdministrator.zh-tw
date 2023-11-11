---
demo:
  title: 示範 09：管理員ister PaaS 計算選項
  module: Administer PaaS Compute Options
---

# 09 - 管理員ister PaaS 計算選項

## 設定 Azure App Service 方案

在此示範中，我們將建立和使用 Azure App Service 方案。

**參考 ** ： [ 管理 App Service 方案 - Azure App 服務](https://docs.microsoft.com/azure/app-service/app-service-plan-manage)

**參考 ** ： [ 在 Azure App 服務 中相應增加應用程式](https://learn.microsoft.com/azure/app-service/manage-scale-up)

**參考 ** ： [ 自動調整Azure App 服務](https://learn.microsoft.com/azure/app-service/manage-automatic-scaling?tabs=azure-portal)

1. 使用 Azure 入口網站。 

1. 搜尋並選取  ** [App Service 方案 ** ]。

1. 建立簡單的 App Service 方案。 討論選取 Windows 或 Linux 的需求。 立即或後續步驟中討論定價方案。 

1. 部署新的 App Service 方案。 

1. 檢閱相應 ** 增加 （App Service 方案） ** 刀鋒視窗。 討論開發/測試和 ** ** 生產 ** 計畫之間的差異 ** 。 檢閱功能清單。 

1. 檢閱相應 ** 放大 （App Service 方案） ** 刀鋒視窗。 檢閱手動 ** 與 ** 規則型 ** 之間的差異 ** 。 

## 設定 Azure 應用程式服務

在此示範中，我們將建立一個執行 Docker 容器的新 Web 應用程式。 容器會顯示歡迎訊息。

**參考 ** ： [ 建立 Web 應用程式](https://learn.microsoft.com/training/modules/host-a-web-app-with-azure-app-service/3-exercise-create-a-web-app-in-the-azure-portal?pivots=csharp)

在這項工作中，我們將建立Azure App 服務 Web 應用程式。

1. 使用 Azure 入口網站。 

1. 搜尋並選取  ** [應用程式服務 ** ]。

1. **建立 ** ** Web 應用程式 ** 。

    - 發佈： ** 程式碼 ** 。 檢閱其他選項。
    - 執行時間堆疊： ** .Net ** 。 檢閱其他選項。
    - 作業系統： ** Linux**

1. **選取 [免費 F1 ** 服務方案]。

1. **檢閱 + 建立 ** Web 應用程式。 等候資源部署。

1. 在 [概觀] ** 頁面上，確定 ** [狀態 ** ] 為 ** [正在執行 ** ]。 **

1. **選取 URL ** ，並確定預設預留位置頁面會載入。

1. 當您有時間時，請流覽 ** [部署位置] ** 選項。
   
## 設定 Azure 容器執行個體

在此示範中，我們會使用 Azure 入口網站中的 Azure 容器執行個體 （ACI） 來建立、設定及部署容器。 ACI 應用程式會顯示具有公用 Microsoft Hello World 影像的靜態 HTML 頁面。 

**參考 ** ： [ 快速入門 - 將 Docker 容器部署至容器實例](https://learn.microsoft.com/en-us/azure/container-instances/container-instances-quickstart-portal)

1. 使用 Azure 入口網站。

1. 搜尋並選取  ** [容器實例 ** ]。

1. **建立 ** 新的容器實例。 

1. 填入 [資源群組 ** ] 和 ** [ ** 容器名稱 ** ]。 

1. 討論影像 ** 來源 ** 選項。 使用 ** 快速入門映射 ** 。

1. 針對 ** 容器映射 ** ，請使用 ** mcr.microsoft.com/azuredocs/aci-helloworld:latest （Linux） ** 。 此範例 Linux 映射會封裝以 Node.js 撰寫的小型 Web 應用程式，以提供靜態 HTML 頁面。

1. 在 [ ** 網路] ** 頁面上，指定 ** 容器的 DNS 名稱標籤 ** 。 

1. 將所有其他設定保留為預設值，然後選取 [ ** 檢閱 + 建立 ** ]。

1. 等候資源部署。

1. 在資源的 [ ** 概觀 ** ] 頁面上，確定 [狀態 ** ] 為 ** [ ** 正在執行 ** ]。

1. 流覽至 ** 容器實例的 FQDN ** ，並確定歡迎頁面會顯示。 

**注意 ** ：若要避免額外的成本，請刪除資源。 

## 設定 Azure Container Apps

在此示範中，我們將建立及使用 Azure Container Apps。 

**參考 ** ：快速入門： [ 使用 Azure 入口網站 部署您的第一個容器應用程式](https://learn.microsoft.com/azure/container-apps/quickstart-portal)

1. 搜尋並選取 ** [容器應用程式 ** ]。

1. 完成專案詳細資料 ** ** 並建立容器應用程式 ** 環境 ** 。

1. **檢閱並建立 ** 容器應用程式。

1. 使用 [ ** 應用程式 URL] ** 連結來檢視您的應用程式。

1. 確認瀏覽器會顯示歡迎 ** 使用 Azure Container Apps ** 訊息。 






