---
demo:
  title: 示範 07：管理 Azure 儲存體
  module: Administer Azure Storage
---


# 07 - 管理 Azure 儲存體

## 設定儲存體帳戶

在此示範中，我們將建立儲存體帳戶。

**參考**： [建立儲存體帳戶](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. 使用 Azure 入口網站。

1. 檢閱儲存體帳戶的目的。 
   
1. 搜尋並選取 **[儲存體帳戶**]。 
 
1. 建立基本儲存體帳戶。 

    - 討論命名儲存體帳戶的需求，以及名稱在 Azure 中必須是唯一的需求。 

    - 檢閱不同的儲存體類型。 例如，一般用途 v2。 

    - 檢閱存取層選取專案。 例如，非經常性存取層和經常性存取層。 

    - 其他索引標籤和設定將涵蓋在其他示範中。 

1. 建立儲存體帳戶，並等候資源部署。 


## 設定 Blob 儲存體

在此示範中，我們將探索 Blob 儲存體。

**注意：**  這些步驟需要儲存體帳戶。

**參考**[：快速入門：上傳、下載和列出 Blob](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. 瀏覽至 Azure 入口網站中的儲存體帳戶。

1. 檢閱 Blob 儲存體的目的。 

1. 建立 Blob 容器。 檢閱容器的存取層級。 例如，私人 (沒有匿名存取) 。 

1. 將 Blob 上傳至容器。 當您有時間檢閱進階設定時。 例如，Blob 類型和 Blob 大小。 

## 設定 Azure 檔案儲存體 

在此示範中，我們將使用檔案共用和快照集。

**注意：**  這些步驟需要儲存體帳戶。

**參考**： [管理 Azure 檔案共用的快速入門](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. 檢閱檔案共用的目的。 

1. 存取儲存體帳戶，然後按一下 [ **檔案**]。

1. 建立檔案共用。 檢閱配額、上傳檔案，以及新增目錄以組織資訊。 

1. 建立檔案共用快照集。 檢閱何時使用快照集，以及它們與備份有何不同。 您有時間上傳檔案、擷取快照集、刪除檔案，以及還原快照集。 


## 設定儲存體安全性

在此示範中，我們將建立共用存取簽章。

**注意：**  此示範需要儲存體帳戶、Blob 容器和上傳的檔案。

**參考**： [建立儲存體容器的 SAS 權杖](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. 選取您想要保護的 Blob 或檔案。 

1. 產生共用存取簽章 (SAS)。 檢閱許可權、開始和到期時間，以及允許的通訊協定。

1. 使用 SAS URL 來確保資源顯示。 


## 儲存體工具 (選擇性) 

在此示範中，我們將檢閱數個常見的 Azure 儲存體工具。 

**參考**：[開始使用 儲存體總管](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. 安裝儲存體總管或使用儲存體瀏覽器。

1. 檢閱如何流覽和建立儲存體資源。 例如，新增 Blob 容器。 

**參考**： [使用 AzCopy v10 將資料複製到 Azure 儲存體](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. 討論使用 AzCopy 的時機。 檢視說明 **azcopy /？**。

1. 向下捲動 **[範例]**   區段。 當您有時間時，請嘗試任何範例。 
    



