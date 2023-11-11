---
demo:
  title: 示範 07：管理員 ister Azure 儲存體
  module: Administer Azure Storage
---


# 07 - 管理員 ister Azure 儲存體

## 設定儲存體帳戶

在此示範中，我們將建立記憶體帳戶。

**參考**： [建立記憶體帳戶](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal)

1. 使用 Azure 入口網站。

1. 檢閱記憶體帳戶的用途。 
   
1. 搜尋並選取 **[儲存體 帳戶**]。 
 
1. 建立基本記憶體帳戶。 

    - 討論如何命名記憶體帳戶，以及名稱在 Azure 中必須是唯一的需求。 

    - 檢閱不同的記憶體類型。 例如，一般用途 v2。 

    - 檢閱存取層選取專案。 例如，非經常性存取層和經常性存取層。 

    - 其他索引標籤和設定將會涵蓋在其他示範中。 

1. 建立記憶體帳戶，並等候資源部署。 


## 設定 Blob 儲存體

在此示範中，我們將探索 Blob 記憶體。

**注意：** 這些步驟需要記憶體帳戶。

**參考**：快速入門： [上傳、下載及列出 Blob](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)

1. 瀏覽至 Azure 入口網站中的儲存體帳戶。

1. 檢閱 Blob 記憶體的用途。 

1. 建立 Blob 容器。 檢閱容器的存取層級。 例如，私用 （沒有匿名存取權）。 

1. 將 Blob 上傳到該容器。 當您有時間檢閱進階設定時。 例如，Blob 類型和 Blob 大小。 

## 設定儲存體安全性

在此示範中，我們將建立共用存取簽章。

**注意：** 此示範需要記憶體帳戶、具有 Blob 容器和上傳的檔案。

**參考**： [建立記憶體容器的SAS令牌](https://learn.microsoft.com/azure/applied-ai-services/form-recognizer/create-sas-tokens?source=recommendations&view=form-recog-3.0.0)

1. 選取您想要保護的 Blob 或檔案。 

1. 產生共用存取簽章 (SAS)。 檢閱許可權、開始和到期時間，以及允許的通訊協定。

1. 使用SAS URL來確保資源會顯示。 


## 設定 Azure 檔案儲存體 

在此示範中，我們將使用檔案共用和快照集。

**注意：** 這些步驟需要記憶體帳戶。

**參考**： [管理 Azure 檔案共用的快速入門](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-portal?tabs=azure-portal)

1. 檢閱檔案共用的目的。 

1. 存取記憶體帳戶，然後按兩下 [ **檔案**]。

1. 建立檔案共用。 檢閱配額、上傳檔案，以及新增目錄以組織資訊。 

1. 建立檔案共用快照集。 檢閱何時使用快照集，以及快照集與備份的不同方式。 當您有時間時，上傳檔案、擷取快照集、刪除檔案，以及還原快照集。


## 儲存體 工具（選擇性）

在此示範中，我們將檢閱數個常見的 Azure 記憶體工具。 

**參考**：[開始使用 儲存體總管](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)

1. 安裝 儲存體總管 或使用 儲存體 Browser。

1. 檢閱如何流覽和建立記憶體資源。 例如，新增 Blob 容器。 

**參考**：[使用 AzCopy v10 將數據複製到 Azure 儲存體](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=/azure/storage/files/toc.json)

1. 討論使用 AzCopy 的時機。 檢視說明 **azcopy /？**。

1. 向下卷動 **[範例]** 區段。 當您有時間時，請嘗試任何範例。 
    



