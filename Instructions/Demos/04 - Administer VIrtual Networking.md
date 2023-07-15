---
demo:
  title: 示範 04：管理虛擬網路
  module: Administer Virtual Networking
---

# 04 - 管理虛擬網路

## 設定虛擬網路

在此示範中，您將建立虛擬網路。

**參考**[：快速入門：建立虛擬網路 - Azure 入口網站](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## 在入口網站中建立虛擬網路

1.  登入 Azure 入口網站並搜尋 **虛擬網路**。

1.  建立虛擬網路，說明您一般的基本設定。 確定已建立至少一個子網。 

1.  確認您的虛擬網路已建立。

## 設定網路安全性群組

在此示範中，您將探索 NSG 和服務端點。

**參考**：[限制對 PaaS 資源的存取 - 教學課程 - Azure 入口網站](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources)

**建立網路安全性群組**

1. 存取 Azure 入口網站。

1. 搜尋並選取 **網路安全性群組**。

1. 建立說明設定的 NSG。 
 
1. 等候新的 NSG 部署。

**探索輸入和輸出規則**

1. 選取新的 NSG。

1. 討論 NSG 如何與子網或網路介面產生關聯。

1. 討論目的輸入和輸出規則。  

1. 檢閱預設輸入和輸出規則。 

1. 建立新的規則，並說明設定。 具體討論服務選取 (，例如 HTTPS) 和優先順序設定。 

## 設定 Azure DNS

在此示範中，您將探索 Azure DNS。

**參考**[：教學課程：裝載網域和子域 - Azure DNS](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns)


**建立 DNS 區域**

1. 存取 Azure 入口網站。

1. 搜尋 **DNS 區域**   服務。

1. 建立 **DNS 區域** 並說明區域的目的。 如需名稱，您可以使用 contoso.internal.com。

1.  等候 DNS 區域建立。 您可能需要 **重新**   整理頁面。

**新增 DNS 記錄集**

**參考**[：教學課程：建立別名記錄以參考區域資源記錄](https://learn.microsoft.com/azure/dns/tutorial-alias-rr)

1. 建立區域之後，請選取 **[+記錄集**]。

1. 使用 [ **類型**   ] 下拉式清單來檢視不同類型的記錄。 檢閱如何使用不同的記錄類型。 請注意，當您選取不同的記錄類型時，記錄資訊如何變更。

1. 建立 **A** 記錄作為範例。 

