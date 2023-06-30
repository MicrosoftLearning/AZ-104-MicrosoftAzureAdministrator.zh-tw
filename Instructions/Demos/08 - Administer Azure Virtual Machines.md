---
demo:
  title: 示範 08：管理 Azure 虛擬機器
  module: Administer Azure Virtual Machines
---


# 08 - 管理 Azure 虛擬機器

## 示範 -- 在入口網站中建立虛擬機器

在此示範中，我們將在入口網站中建立和存取 Azure 虛擬機器。

**參考**

[快速入門 - 在 Azure 入口網站中建立 Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

[快速入門 - 使用 Azure 入口網站建立 Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal)

[使用 Bastion 連線到虛擬機器](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal#connect)

**建立虛擬機器**

**注意：** 這些步驟只涵蓋一些虛擬機器參數。 請隨意探索並涵蓋其他區域。  視您的物件而定，您可以建立 Windows 或 Linux 虛擬機器。

1. 使用 Azure 入口網站。

1. 搜尋 **虛擬機器**。 

1. 建立基本虛擬機器。 檢閱可用性選項、映射和輸入規則。

1. 討論建立安全系統管理員帳戶的重要性。

1. 建立虛擬機器，並等候資源部署。  

**連線至虛擬機器**

1. 有數種方式可以 **連線** 到虛擬機器。 

1. 對於 Windows 伺服器，您可以使用 **RDP**，如教學課程所示。 您可以安裝網頁伺服器並檢視 IIS 歡迎頁面，進一步進行一個步驟。 

1. 針對 Linux 伺服器，您可以 **SSH**，如教學課程所示。 您可以藉由安裝網頁伺服器並檢視作用中的網頁伺服器，進一步進行一個步驟。

1. 針對任一部伺服器，您可以使用 **Bastion** 服務 (教學課程) 。 檢閱為何建議使用 Bastion 給 RDP 或 SSH。 

## 設定虛擬機器可用性

在此示範中，我們將探索虛擬機器調整規模選項。

**參考**

[使用 Azure 入口網站在擴展集中建立虛擬機器](https://learn.microsoft.com/azure/virtual-machine-scale-sets/flexible-virtual-machine-scale-sets-portal)

1. 使用 Azure 入口網站。

1. 搜尋並選取 **[虛擬機器擴展集**]。 

1. 建立**虛擬機器擴展集**。 檢閱虛擬機器擴展集的目的。 檢閱 **統一** 協調流程模式與 **彈性** 協調流程模式之間的差異。 說明您的選取範圍可能會影響您的縮放選項。 

1. 移至 **[調整]** 索引標籤。 

1. 檢閱如何使用 **手動調整**   和**相應縮小原則**。 

1. 變更為 **自訂** 調整原則。 

1. 檢閱 **CPU 閾值 (%) ** 如何用來相應放大和相應放大虛擬機器實例。 

