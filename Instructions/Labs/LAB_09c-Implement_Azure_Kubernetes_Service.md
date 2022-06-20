---
lab:
  title: 09c - 實作 Azure Kubernetes Service
  module: Module 09 - Serverless Computing
ms.openlocfilehash: 42e43fa916e61988df87b3188fba59ab7b57652e
ms.sourcegitcommit: dd61587ee547d5efa09ad0a63c0b2af272ee1e55
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/31/2022
ms.locfileid: "145198102"
---
# <a name="lab-09c---implement-azure-kubernetes-service"></a>實驗 09c - 實作 Azure Kubernetes Service
# <a name="student-lab-manual"></a>學員實驗手冊

## <a name="lab-scenario"></a>實驗案例

Contoso 有許多多層式應用程式，不適合使用 Azure 容器執行個體來執行。 為了判斷這些應用程式是否可以容器化工作負載的形式執行，您想要使用 Kubernetes 作為容器協調器進行評估。 為了進一步將管理負擔降到最低，您想要測試 Azure Kubernetes Service，包括其簡化的部署體驗和調整功能。

## <a name="objectives"></a>目標

在此實驗中，您將會：

+ 工作 1：註冊 Microsoft.Kubernetes 和 Microsoft.KubernetesConfiguration 資源提供者。
+ 工作 2：部署 Azure Kubernetes Service 叢集
+ 工作 3：將 Pod 部署至 Azure Kubernetes Service 叢集
+ 工作 4：調整 Azure Kubernetes 服務叢集中的容器化工作負載

## <a name="estimated-timing-40-minutes"></a>預估時間：40 分鐘

## <a name="architecture-diagram"></a>架構圖

![image](../media/lab09c.png)

## <a name="instructions"></a>指示

### <a name="exercise-1"></a>練習 1

#### <a name="task-1-register-the-microsoftkubernetes-and-microsoftkubernetesconfiguration-resource-providers"></a>工作 1：註冊 Microsoft.Kubernetes 和 Microsoft.KubernetesConfiguration 資源提供者。

在此工作中，您將註冊部署 Azure Kubernetes Services 叢集所需的資源提供者。

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示，開啟 **Azure Cloud Shell**。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現 **您未掛接任何儲存體** 訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。

1. 從 [Cloud Shell] 窗格中，執行下列命令來註冊 Microsoft.Kubernetes 和 Microsoft.KubernetesConfiguration 資源提供者。

   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Kubernetes

   Register-AzResourceProvider -ProviderNamespace Microsoft.KubernetesConfiguration
   ```

1. 關閉 [Cloud Shell] 窗格。

#### <a name="task-2-deploy-an-azure-kubernetes-service-cluster"></a>工作 2：部署 Azure Kubernetes Service 叢集

在這項工作中，您將使用 Azure 入口網站來部署 Azure Kubernetes Services 叢集。

1. 在Azure 入口網站中，搜尋尋找 **Kubernetes 服務**，然後在 [Kubernetes 服務] 刀鋒視窗中，按一下 [+ 建立]，然後按一下 [+ 建立 Kubernetes 叢集]。

1. 在 [建立 Kubernetes 叢集] 刀鋒視窗的 [基本] 索引標籤上，指定下列設定 (讓其他設定保留預設值)：

    | 設定 | 值 |
    | ---- | ---- |
    | 訂用帳戶 | 您在此實驗中使用的 Azure 訂用帳戶名稱 |
    | 資源群組 | 新資源群組 **az104-09c-rg1** 的名稱 |
    | Kubernetes 叢集名稱 | **az104-9c-aks1** |
    | 區域 | 您可以佈建 Kubernetes 叢集的區功能變數名稱 |
    | 可用性區域 | **無** (取消核取所有方塊) |
    | Kubernetes 版本 | 接受預設 |
    | 節點大小 | 接受預設 |
    | 缩放方法 | **手動** |
    | 節點計數 | **1** |

1. 按一下 下一步：**節點集區>** ，然後在 建立 Kubernetes 叢集 刀鋒視窗的 節點集區 索引標籤上，指定下列設定 (讓其他設定保留預設值)：

    | 設定 | 值 |
    | ---- | ---- |
    | 啟用虛擬節點 | [已停用] (預設值) |

1. 按一下 [下一步：**存取] >** ，然後在 [建立 Kubernetes 叢集] 刀鋒視窗的 [存取] 索引標籤上，指定下列設定 (讓其他設定保留預設值)：

    | 設定 | 值 |
    | ---- | ---- |
    | 驗證方法 | **系統指派的受控識別** (預設值 - 沒有變更) | 
    | 角色型存取控制 (RBAC) | **Enabled** |

1. 按一下 [下一步：**網路] >** ，然後在 [建立 Kubernetes 叢集] 刀鋒視窗的 [網路] 索引標籤上，指定下列設定 (讓其他設定保留預設值)：

    | 設定 | 值 |
    | ---- | ---- |
    | 網路組態 | **kubenet** |
    | DNS 名稱首碼 | 任何有效的全域唯一 DNS 主機名稱 |

1. 按一下 [下一步：**整合 >** ]，在 [建立 Kubernetes 叢集] 刀鋒視窗的 [整合] 索引標籤上，將[容器監視] 設定為 [已停用]，按一下 [檢閱 + 建立]，確定通過驗證，然後按一下 [建立]。

    >**注意**：在生產案例中，您會想要啟用監視。 在此情況下會停用監視，因為實驗室中未涵蓋監視。

    >**注意**：等待部署完成。 該操作需要約 10 分鐘。

#### <a name="task-3-deploy-pods-into-the-azure-kubernetes-service-cluster"></a>工作 3：將 Pod 部署至 Azure Kubernetes Service 叢集

在這項工作中，您會將 Pod 部署到Azure Kubernetes Service 叢集。

1. 在 [部署] 刀鋒視窗上，按一下 [移至資源] 連結。

1. 在 [az104-9c-aks1 Kubernetes service] 刀鋒視窗的 [設定] 區段中，按一下 [節點集區]。

1. 在 [az104-9c-aks1 - 節點集區] 刀鋒視窗上，確認叢集包含單一集區與一個節點。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示，開啟 **Azure Cloud Shell**。

1. 將 **Azure Cloud Shell** 切換至 **Bash** (黑色背景)。

1. 從 [Cloud Shell] 窗格中，執行下列命令以擷取認證，藉此存取 AKS 叢集：

    ```sh
    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER
    ```

1. 從 [Cloud Shell] 窗格中，執行下列命令來驗證 AKS 叢集的連線能力：

    ```sh
    kubectl get nodes
    ```

1. 在 [Cloud Shell] 窗格中，檢閱輸出，並確認叢集目前包含的一個節點正在報告 [就緒] 狀態。

1. 從 [Cloud Shell] 窗格中，執行下列命令以從 Docker Hub 部署 **nginx** 映像：

    ```sh
    kubectl create deployment nginx-deployment --image=nginx
    ```

    > **注意**：輸入部署名稱時，請務必使用小寫字母 (nginx-deployment)

1. 從 [Cloud Shell] 窗格中，執行下列命令以確認已建立 Kubernetes Pod：

    ```sh
    kubectl get pods
    ```

1. 從 [Cloud Shell] 窗格中，執行下列命令來識別部署的狀態：

    ```sh
    kubectl get deployment
    ```

1. 從 [Cloud Shell] 窗格中，執行下列命令，讓 Pod 可從網際網路使用：

    ```sh
    kubectl expose deployment nginx-deployment --port=80 --type=LoadBalancer
    ```

1. 從 [Cloud Shell] 窗格中，執行下列命令來識別是否已佈建公用 IP 位址：

    ```sh
    kubectl get service
    ```

1. 重新執行命令，直到 **nginx 部署** 項目在 **EXTERNAL-IP** 資料行中的值從 **\<pending\>** 變更為公用 IP 位址為止。 請記下 **nginx-deployment** 的 **EXTERNAL-IP** 資料行中的公用 IP 位址。

1. 開啟瀏覽器視窗，並瀏覽至您在上一個步驟中取得的 IP 位址。 確認瀏覽器頁面會顯示 **Welcome to nginx！** 回應。

#### <a name="task-4-scale-containerized-workloads-in-the-azure-kubernetes-service-cluster"></a>工作 4：調整 Azure Kubernetes 服務叢集中的容器化工作負載

在此工作中，您會水平調整 Pod 數目，然後調整叢集節點數目。

1. 從 [Cloud Shell] 窗格中，執行下列命令，將 Pod 數目增加為 2 來調整部署：

    ```sh

    RESOURCE_GROUP='az104-09c-rg1'

    AKS_CLUSTER='az104-9c-aks1'

    kubectl scale --replicas=2 deployment/nginx-deployment
    ```

1. 從 [Cloud Shell] 窗格中，執行下列命令來驗證調整部署的結果：

    ```sh
    kubectl get pods
    ```

    > **注意**：檢閱命令的輸出，並確認 Pod 數目增加至 2。

1. 從 [Cloud Shell] 窗格中，執行下列命令，將節點數目增加到 2 來相應放大叢集：

    ```sh
    az aks scale --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER --node-count 2
    ```

    > **注意**：等候其他節點的佈建完成。 這大約需要 3 分鐘。 如果失敗，請重新執行 `az aks scale` 命令。

1. 從 [Cloud Shell] 窗格中，執行下列命令來驗證調整叢集的結果：

    ```sh
    kubectl get nodes
    ```

    > **注意**：檢閱命令的輸出，並確認 Pod 數目增加至 2。

1. 從 [Cloud Shell] 窗格中，執行下列命令來調整部署：

    ```sh
    kubectl scale --replicas=10 deployment/nginx-deployment
    ```

1. 從 [Cloud Shell] 窗格中，執行下列命令來驗證調整部署的結果：

    ```sh
    kubectl get pods
    ```

    > **注意**：檢閱命令的輸出，並確認 Pod 數目增加至 10。

1. 從 [Cloud Shell] 窗格中，執行下列命令以檢閱跨叢集節點的 Pod 散發：

    ```sh
    kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name
    ```

    > **注意**：檢閱命令的輸出，並確認 Pod 分散到這兩個節點。

1. 從 [Cloud Shell] 窗格中，執行下列命令來刪除部署：

    ```sh
    kubectl delete deployment nginx-deployment
    ```

1. 關閉 [Cloud Shell] 窗格。

#### <a name="clean-up-resources"></a>清除資源

>**注意**：請記得移除您不再使用的任何新建立的 Azure 資源。 移除未使用的資源可確保您不會看到非預期的費用。

>**注意**：如果無法立即移除實驗資源，請不要擔心。 有時候資源具有相依性，需要經過很長的時間才能刪除。 這是監視資源使用量的常見系統管理員工作，因此只需定期檢閱入口網站中的資源，查看清除的運作情況便可。 

1. 在 Azure 入口網站中，在 [Cloud Shell] 窗格內開啟 [Bash] Shell 工作階段。

1. 執行下列命令，列出在本課程模組的任何實驗中建立的所有資源群組：

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].name" --output tsv
   ```

1. 執行下列命令，刪除您在本課程模組的任何實驗中建立的所有資源群組：

   ```sh
   az group list --query "[?starts_with(name,'az104-09c')].[name]" --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
   ```

    >**注意**：此命令會以非同步方式執行 (由 --nowait 參數決定)，所以您將可以在相同的 Bash 工作階段中，立即執行另一個 Azure CLI 命令，但這需要幾分鐘才會移除資源群組。

#### <a name="review"></a>檢閱

在此實驗中，您已：

+ 部署 Azure Kubernetes Service 叢集
+ 將 Pod 部署至 Azure Kubernetes Service 叢集
+ 調整 Azure Kubernetes 服務叢集中的容器化工作負載
