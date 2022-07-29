---
lab:
  title: 07 - 管理 Azure 儲存體
  module: Module 07 - Azure Storage
ms.openlocfilehash: 34b6dba73d87731df935f80a1b5909e44075e871
ms.sourcegitcommit: 6df80c7697689bcee3616cdd665da0a38cdce6cb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2022
ms.locfileid: "146587463"
---
# <a name="lab-07---manage-azure-storage"></a>實驗室 07 - 管理 Azure 儲存體
# <a name="student-lab-manual"></a>學生實驗手冊

## <a name="lab-scenario"></a>實驗案例

您必須評估使用 Azure 儲存體來儲存目前位於內部部署資料存放區中的檔案。 雖然其中大多數檔案並不會經常存取，但有一些例外狀況。 您想要將較不常存取的檔案放在較低價格的儲存層中，以將儲存成本降到最低。 您也計畫探索 Azure 儲存體提供的不同保護機制，包括網路存取、驗證、授權和複寫。 最後，您想要判斷將 Azure 檔案儲存體服務用來裝載內部部署檔案共用可能的限度。

## <a name="objectives"></a>目標

在此實驗中，您將會：

+ 工作 1：佈建實驗環境
+ 工作 2：建立並設定 Azure 儲存體帳戶
+ 工作 3：管理 Blob 儲存體
+ 工作 4：管理 Azure 儲存體的驗證和授權
+ 工作 5：建立和設定 Azure 檔案儲存體共用
+ 工作 6：管理 Azure 儲存體的網路存取

## <a name="estimated-timing-40-minutes"></a>預估時間：40 分鐘

## <a name="architecture-diagram"></a>架構圖

![image](../media/lab07.png)


## <a name="instructions"></a>指示

### <a name="exercise-1"></a>練習 1

#### <a name="task-1-provision-the-lab-environment"></a>工作 1：佈建實驗環境

在此工作中，您將部署本實驗稍後要使用的 Azure 虛擬機器。

1. 登入 [Azure 入口網站](https://portal.azure.com)。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示以開啟 **Azure Cloud Shell**。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。

    >**注意**：如果這是您第一次啟動 **Cloud Shell**，而且出現 **您未掛接任何儲存體** 訊息，請選取您在此實驗中使用的訂用帳戶，並按一下 [建立儲存體]。

1. 在 [Cloud Shell] 窗格的工具列中，按一下 [上傳/下載檔案] 圖示，在下拉式功能表中，按一下 [上傳]，並將檔案 **\\Allfiles\\Labs\\07\\az104-07-vm-template.json** 和 **\\Allfiles\\Labs\\07\\az104-07-vm-parameters.json** 上傳至 Cloud Shell 主目錄。

1. 編輯您剛才上傳的 **參數** 檔案，並變更密碼。 如果您需要在 Shell 中編輯檔案的協助，請向講師尋求協助。 最佳做法便是秘密 (例如密碼)，這應會在 Key Vault 中儲存時提供更佳的安全性。 

1. 從 [Cloud Shell] 窗格中，執行下列命令來建立裝載虛擬機器的資源群組 (將 '[Azure_region]' 預留位置取代為您想要部署 Azure 虛擬機器的 Azure 區域名稱)

    >**注意**：若要列出 Azure 區域的名稱，請執行 `(Get-AzLocation).Location`
    >**請注意**：下列的每個命令都應個別輸入

    ```powershell
    $location = '[Azure_region]'
    ```
  
    ```powershell
     $rgName = 'az104-07-rg0'
    ```

    ```powershell
    New-AzResourceGroup -Name $rgName -Location $location
    ```
    
1. 從 Cloud Shell 窗格中，執行下列命令以使用上傳的範本和參數檔案來部署虛擬機器：

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-07-vm-template.json `
      -TemplateParameterFile $HOME/az104-07-vm-parameters.json `
      -AsJob
   ```

    >**注意**：不需等待部署完成，請直接進行下一項工作。

    >**注意**：如果您收到指出 VM 大小無法使用的錯誤，請向講師尋求協助，並嘗試下列步驟。
    > 1. 按一下 CloudShell 中的 `{}` 按鈕，從左側側邊列選取 [az104-07-vm-parameters.json]，並記下 `vmSize` 參數值。
    > 1. 檢查部署 'az104-04-rg1' 資源群組的位置。 您可以在 CloudShell 中執行 `az group show -n az104-04-rg1 --query location` 以取得位置。
    > 1. 在 CloudShell 中執行 `az vm list-skus --location <Replace with your location> -o table --query "[? contains(name,'Standard_D2s')].name"`。
    > 1. 將 `vmSize` 參數的值取代為您剛才執行命令所傳回的其中一個值。
    > 1. 現在再次執行 `New-AzResourceGroupDeployment` 命令來重新部署範本。 您可以按幾次向上按鈕，藉此顯示次執行的命令。

1. 關閉 [Cloud Shell] 窗格。

#### <a name="task-2-create-and-configure-azure-storage-accounts"></a>工作 2：建立並設定 Azure 儲存體帳戶

在這項工作中，您將建立及設定 Azure 儲存體帳戶。

1. 在 Azure 入口網站中，搜尋並選取 [儲存體帳戶]，然後按一下 [+ 建立]。

1. 在 [建立儲存體帳戶] 窗格的 [基本] 索引標籤上，指定下列設定 (將其他設定保留為預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 訂用帳戶 | 您要在此實驗室中使用的 Azure 訂用帳戶名稱 |
    | 資源群組 | **新** 資源群組 **az104-07-rg1** 的名稱 |
    | 儲存體帳戶名稱 | 長度介於 3 到 24 個字元之間，由字母和數字組成的任何全域唯一名稱 |
    | 區域 | 您可以在其中建立 Azure 儲存體帳戶的 Azure 區域名稱  |
    | 效能 | **標準** |
    | 備援性 | **異地備援儲存體 (GRS)** |

1. 按一下 **下一步：進階 >** ，在 建立儲存體帳戶 刀鋒視窗的 進階 索引標籤上，檢閱可用的選項，接受預設設定，並按一下 **下一步：網路 >** 。

1. 在 [建立儲存體帳戶] 刀鋒視窗的 [網路] 索引標籤上，檢閱可用的選項，接受預設選項 [在所有網路啟用公用存取]，然後按一下 [下一步: 資料保護>]。

1. 在 [建立儲存體帳戶] 刀鋒視窗的 [資料保護] 索引標籤上，檢閱可用的選項，接受預設值，按一下 [檢閱 + 建立]，等候驗證程式完成，然後按一下 [建立]。

    >**注意**：等候儲存體帳戶建立完成。 這應該大約需要 2 分鐘的時間。

1. 在部署刀鋒視窗上，按一下 [前往資源] 以顯示 [Azure 儲存體帳戶] 刀鋒視窗。

1. 在 [儲存體帳戶] 刀鋒視窗的 [資料管理] 區段中，按一下 [異地複寫]，並記下次要位置。 

1. 在 [儲存體帳戶] 刀鋒視窗的 [設定] 區段中，選取 [組態]，在 [複寫] 下拉式清單中選取 [本機備援儲存體 (LRS)] 並儲存變更。

1. 切換回 [異地複寫] 刀鋒視窗，並請注意，此時儲存體帳戶只有主要位置。

1. 再次顯示儲存體帳戶的 [組態] 刀鋒視窗，將 [Blob 存取層 (預設)] 設定為 [非經常性]，然後儲存變更。

    > **注意**：非經常性存取層適合不常存取的資料。

#### <a name="task-3-manage-blob-storage"></a>工作 3：管理 Blob 儲存體

在這個工作中，您將建立一個 Blob 容器，並將 Blob 檔案上傳至其中。

1. 在 [儲存體帳戶] 刀鋒視窗上的 [資料儲存體] 區段中，按一下 [容器]。

1. 按一下 [+ 容器]，並使用下列設定建立容器：

    | 設定 | 值 |
    | --- | --- |
    | 名稱 | **az104-07-container**  |
    | 公用存取層級 | **私人 (沒有匿名存取)** |

1. 在容器清單中，按一下 **az104-07-container**，然後按一下 [上傳]。

1. 瀏覽至實驗室電腦上的 **\\Allfiles\\Labs\\07\\LICENSE**，然後按一下 [開啟]。

1. 在 [上傳 Blob] 刀鋒視窗上，展開 [進階] 區段並指定下列設定 (將其他設定保留預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 驗證類型 | **帳戶金鑰**  |
    | Blob 類型 | **區塊 Blob** |
    | 區塊大小 | **4 MB** |
    | 存取層 | **經常性** |
    | 上傳到資料夾 | **licenses** |

    > **注意**：您可以設定個別 Blob 的存取層。

1. 按一下 [上傳] 。

    > **注意**：請注意，上傳會自動建立名為 **licenses** 的子資料夾。

1. 回到 [az104-07-container] 刀鋒視窗，按一下 [licenses]，然後按一下 [LICENSE]。

1. 在 [licenses/LICENSE] 刀鋒視窗上，檢閱可用的選項。

    > **注意**：您可以選擇下載 Blob、變更其存取層 (目前設定為 [經常性])、取得租用，這會將其租用狀態變更為 [鎖定] (目前設定為 [已解除鎖定])，並保護 Blob 不受修改或刪除，以及藉由指定任意索引鍵和值組來指派自訂中繼資料。 您也可以直接在 Azure 入口網站介面內 **編輯** 檔案，而無需先下載。 您也可以建立快照集，以及產生 SAS 權杖 (下一個工作將會探索此方法)。

#### <a name="task-4-manage-authentication-and-authorization-for-azure-storage"></a>工作 4：管理 Azure 儲存體的驗證和授權

在此工作中，您將設定 Azure 儲存體的驗證和授權。

1. 在 [licenses/LICENSE] 刀鋒視窗的 [概觀] 索引標籤上，按一下 [URL] 項目旁邊的 [複製到剪貼簿] 按鈕。

1. 使用 InPrivate 模式開啟另一個瀏覽器視窗，並瀏覽至您在上一個步驟中複製的 URL。

1. 您應該會看到 XML 格式的訊息，指出 **ResourceNotFound** 或 **PublicAccessNotPermitted**。

    > **注意**：這是預期的結果，因為您建立的容器已將公用存取層級設定為 [私人 (無匿名存取)]。

1. 關閉 InPrivate 模式瀏覽器視窗，返回瀏覽器視窗，其中顯示Azure 儲存體容器的 [licenses/LICENSE] 刀鋒視窗，然後切換至 [產生 SAS] 索引標籤。

1. 在 [licenses/LICENSE] 刀鋒視窗的 [產生 SAS] 索引標籤上，指定下列設定 (將其他設定保留預設值)：

    | 設定 | 值 |
    | --- | --- |
    | 簽署金鑰 | **金鑰 1** |
    | 權限 | **讀取** |
    | 開始日期 | 昨天的日期 |
    | 開始時間 | 目前時間 |
    | 到期日 | 明天的日期 |
    | 過期時間 | 目前時間 |
    | 允許的 IP 位址 | 保留空白 |
    

1. 按一下 [產生 SAS 權杖與 URL]。

1. 按一下 [Blob SAS URL] 項目旁邊的 [複製至剪貼簿] 按鈕。

1. 使用 InPrivate 模式開啟另一個瀏覽器視窗，並瀏覽至您在上一個步驟中複製的 URL。

    > **注意**：如果您使用 Microsoft Edge，您應該會看到 **MIT 授權 (MIT)** 頁面。 如果您使用 Chrome、Microsoft Edge (Chromium) 或 Firefox，應該能夠下載檔案並使用記事本來檢視檔案的內容。

    > **注意**：這是預期的結果，因為現在您的存取權會根據新產生的 SAS 權杖獲得授權。

    > **注意**：儲存 Blob SAS URL。 之後在此實驗中會用到。

1. 關閉 InPrivate 模式瀏覽器視窗，返回瀏覽器視窗，其中顯示Azure 儲存體容器的 [licenses/LICENSE] 刀鋒視窗，然後在此瀏覽回到 [az104-07-container] 刀鋒視窗。

1. 按一下 [驗證方法] 標籤旁的 [切換為 Azure AD 使用者帳戶] 連結。

    > **注意**：當您變更驗證方法時可能會看到錯誤 (錯誤為 *「您無權使用您的 Azure AD 使用者帳戶列出資料」* )。 這是預期的結果。  

    > **注意**：目前，您沒有變更驗證方法的權限。

1. 在 [az104-07-container] 刀鋒視窗中，按一下 [存取控制 (IAM)]。

1. 在 [檢查存取權] 索引標籤上，按一下 [新增角色指派]。

1. 在 [新增角色指派] 刀鋒視窗中，指定下列設定：

    | 設定 | 值 |
    | --- | --- |
    | 角色 | **儲存體 Blob 資料擁有者** |
    | 存取權指派對象 | **使用者、群組或服務主體** |
    | 成員 | 使用者帳戶的名稱 |

1. 按一下 [檢閱 + 指派]，接著按一下 [檢閱 + 指派]，然後返回 [az104-07-container] 容器的 [概觀] 刀鋒視窗，並確認您可以將驗證方法變更為 [(切換為 Azure AD 使用者帳戶)]。

    > **注意**：變更可能需要大約 5 分鐘才會生效。

#### <a name="task-5-create-and-configure-an-azure-files-shares"></a>工作 5：建立和設定 Azure 檔案儲存體共用

在此工作中，您將建立和設定 Azure 檔案儲存體共用。

> **注意**：開始這項工作之前，請確認您在此實驗第一個工作中佈建的虛擬機器正在執行。

1. 在 Azure 入口網站中，瀏覽回到您在此實驗第一個工作中建立的儲存體帳戶刀鋒視窗，然後在 [資料儲存體] 區段中，按一下 [檔案共用]。

1. 按一下 [+ 檔案共用]，並使用下列設定建立檔案共用：

    | 設定 | 值 |
    | --- | --- |
    | 名稱 | **az104-07-share** |

1. 按一下新建立的檔案共用，然後按一下 [連線]。

1. 在 [連線] 刀鋒視窗上，確定已選取 [Windows] 索引標籤。 您會在下方找到含有指令碼的灰色文字方塊，將滑鼠停留在該方塊右下角的頁面圖示上，然後按一下 [複製至剪貼簿]。

1. 在 Azure 入口網站中，搜尋並選取 [虛擬機器]，然後在虛擬機器清單中，按一下 [az104-07-vm0]。

1. 在 [az104-07-vm0] 刀鋒視窗上的 [作業] 區段中，按一下 [執行命令]。

1. 在 [az104-07-vm0 - 執行命令] 刀鋒視窗上，按一下 [RunPowerShellScript]。

1. 在 [執行命令指令碼] 刀鋒視窗上，將您稍早在這項工作中複製的指令碼貼到 [PowerShell 指令碼] 窗格中，然後按一下 [執行]。

1. 確認指令碼已成功完成。

1. 以下列指令碼取代 [PowerShell 指令碼] 窗格的內容，然後按一下 [執行]：

   ```powershell
   New-Item -Type Directory -Path 'Z:\az104-07-folder'

   New-Item -Type File -Path 'Z:\az104-07-folder\az-104-07-file.txt'
   ```

1. 確認指令碼已成功完成。

1. 瀏覽回到 [az104-07-share] 檔案共用刀鋒視窗，按一下 [重新整理]，並確認 **az104-07-folder** 出現在資料夾清單中。

1. 按一下 [az104-07-folder]，並確認 **az104-07-file.txt** 出現在檔案清單中。

#### <a name="task-6-manage-network-access-for-azure-storage"></a>工作 6：管理 Azure 儲存體的網路存取

在此工作中，您將設定 Azure 儲存體的網路存取。

1. 在 Azure 入口網站中，瀏覽回到您在此實驗第一個工作中建立的儲存體帳戶刀鋒視窗，然後在 [安全性 + 網路] 區段中，按一下 [網路]，然後按一下 [防火牆與虛擬網路]。

1. 按一下 [已從選取的虛擬網路和 IP 位址啟用] 選項，然後檢閱啟用此選項後可用的組態設定。

    > **注意**：您可以透過這些設定使用服務端點，組態虛擬網路指定子網路上的 Azure 虛擬機器與儲存體帳戶之間的直接連線。

1. 按一下 [新增您的用戶端 IP 位址] 核取方塊，然後儲存變更。

1. 使用 InPrivate 模式開啟另一個瀏覽器視窗，並瀏覽至您在上一個工作中產生的 Blob SAS URL。

    > **注意**：如果您沒有記錄工作 4 的 SAS URL，您應該產生具有相同組態的新 URL。 請使用工作 4 步驟 4-6 作為產生新 Blob SAS URL 的指南。 

1. 您應該會看到 [MIT 授權 (MIT)] 頁面的內容。

    > **注意**：這是預期的結果，因為您是從用戶端 IP 位址進行連線。

1. 關閉 InPrivate 模式瀏覽器視窗，返回顯示 Azure 儲存體帳戶 [網路] 刀鋒視窗的瀏覽器視窗。

1. 在 Azure 入口網站中，按一下 Azure 入口網站右上角的圖示以開啟 **Azure Cloud Shell**。

1. 當系統提示您選取 [Bash] 或 [PowerShell] 時，請選取 [PowerShell]。

1. 從 [Cloud Shell] 窗格中執行下列命令，以嘗試從儲存體帳戶的 [az104-07-container] 容器下載 LICENSE Blob (將 `[blob SAS URL]` 預留位置取代為您在上一個工作中產生的 Blob SAS URL)：

   ```powershell
   Invoke-WebRequest -URI '[blob SAS URL]'
   ```
1. 確認下載嘗試失敗。

    > **注意**：您應該會收到訊息表示 **AuthorizationFailure：此要求未獲授權執行此作業**。 這是預期的結果，因為您進行連線的 IP 位址指派給裝載 Cloud Shell 執行個體的 Azure VM。

1. 關閉 [Cloud Shell] 窗格。

#### <a name="clean-up-resources"></a>清除資源

>**注意**：請記得移除您不再使用的任何新建立的 Azure 資源。 移除未使用的資源可確保您不會看到非預期的費用。

>**注意**：如果無法立即移除實驗資源，請不要擔心。 有時候資源具有相依性，需要經過很長的時間才能刪除。 這是監視資源使用量的常見系統管理員工作，因此只需定期檢閱入口網站中的資源，查看清除的運作情況便可。 您也可以嘗試刪除資源所在的資源群組。 這是快速的系統管理員捷徑。 如果您有疑慮，請諮詢講師。

1. 在 Azure 入口網站中，在 [Cloud Shell] 窗格內開啟 [PowerShell] 工作階段。

1. 執行下列命令，列出在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*'
   ```

1. 執行下列命令，刪除您在本課程模組的任何實驗中建立的所有資源群組：

   ```powershell
   Get-AzResourceGroup -Name 'az104-07*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**注意**：此命令以非同步方式執行 (由 --AsJob 參數決定)，所以您隨後能夠在相同 PowerShell 工作階段內立即執行另一個 PowerShell 命令，但需要經過幾分鐘後，才會實際移除資源群組。

#### <a name="review"></a>檢閱

在此實驗中，您已：

- 佈建實驗環境
- 建立和設定 Azure 儲存體帳戶
- 管理 Blob 儲存體
- 管理 Azure 儲存體的驗證和授權
- 建立和設定 Azure 檔案儲存體共用
- 管理 Azure 儲存體的網路存取
