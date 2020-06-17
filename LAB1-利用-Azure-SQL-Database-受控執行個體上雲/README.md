#  LAB 1: 利用 Azure SQL Database 受控執行個體上雲

- 評估資料庫的遷移
  - [任務 1. 在 SqlServer 2008 虛擬機器中設定 TailspinToys 資料庫](#任務-1-在-SqlServer-2008-虛擬機器中設定-TailspinToys-資料庫)
  - [任務 2. 使用 Data Migration Assistant DMA 評估遷移至 Azure SQL Database](#任務-2-使用-Data-Migration-Assistant-DMA-評估遷移至-Azure-SQL-Database)
  - [任務 3. 使用 Data Migration Assistant DMA 評估遷移至 Azure SQL Database Managed Instance](#任務-3-使用-Data-Migration-Assistant-DMA-評估遷移至-Azure-SQL-Database-Managed-Instance)
- 遷移至 Azure SQL Database Managed Instance
  >在此練習中我們將使用在線遷移的方式，以求減少停機時間。
  - [任務 4. 在 SqlServer2008 虛擬機器上建立網路分享](#任務-4-在-SqlServer2008-虛擬機器上建立網路分享)
  - [任務 5. 更改 MSSQLSERVER 服務以在 sqlmiuser 帳戶下執行](#任務-5-更改-MSSQLSERVER-服務以在-sqlmiuser-帳戶下執行)
  - [任務 6. 備份 TailspinToys 資料庫](#任務-6-備份-TailspinToys-資料庫)
  - [任務 7. 檢視 Azure SQL Database Managed Instance 與 SQL Server 2008 虛擬機器連接訊息](#任務-7-檢視-Azure-SQL-Database-Managed-Instance-與-SQL-Server-2008-虛擬機器連接訊息)
  - [任務 8. 創建服務主體](#任務-8-創建服務主體)
  - [任務 9. 建立與執行線上遷移專案](#任務-9-建立與執行線上遷移專案)
  - [任務 10. 執行遷移](#任務-10-執行遷移)
  - [任務 11. 驗證資料庫遷移](#任務-11-驗證資料庫遷移)
- 遷移應用程式至 Azure
  >在此練習中，會將應用程式部署至 App Service，並設定 P2S VPN 連線，以連接 Azure SQL Database Managed Instance 的資料庫
  - [任務 12. 部署網頁應用程式](#任務-12-部署網頁應用程式)
  - [任務 13. 更新網頁應用程式設定](#任務-13-更新網頁應用程式設定)
  - [任務 14. 設定 P2S VPN](#任務-14-設定-P2S-VPN)
  - [任務 15. 設定虛擬網路以整合 App Service](#任務-15-設定虛擬網路以整合-App-Service)
  - [任務 16. 瀏覽網頁應用程式](#任務-16-瀏覽網頁應用程式)



## 任務 1. 在 SqlServer 2008 虛擬機器中設定 TailspinToys 資料庫

### 1. 在 [Azure portal](https://portal.azure.com/) 中點選資源群組

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/azure-services-resource-groups.png" alt="Resource groups is highlighted in the Azure services list." title="Azure services" style="max-width:100%;">



### 2. 選擇 hands-on-lab 資源群組

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-groups.png" alt="Resource groups is selected in the Azure navigation pane, and the &quot;hands-on-lab-SUFFIX&quot; resource group is highlighted." title="Resource groups list" style="max-width:100%;">



### 3. 在資源清單中選擇 SqlServer2008 虛擬機器

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-list-sqlserver2008.png" alt="The SqlServer2008 VM is highlighted in the list of resources." title="Resource list" style="max-width:100%;">



### 4. 在虛擬機器介面中，點選左側的概觀，並點選連接按鈕

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/connect-sqlserver2008.png" alt="The SqlServer2008 VM blade is displayed, with the Connect button highlighted in the top menu." title="Connect to SqlServer2008 VM" style="max-width:100%;">



### 5. 在下拉選單中選擇下載 RDP 檔案，並執行



### 6. 在對話視窗中點選連接按鈕

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/remote-desktop-connection-sql-2008.png" alt="In the Remote Desktop Connection Dialog Box, the Connect button is highlighted." title="Remote Desktop Connection dialog" style="max-width:100%;">



### 7. 輸入 Username 與 Password 遠端登入

- **Username**: `sqlmiuser`
- **Password**: `Password.1234567890`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/rdc-credentials-sql-2008.png" alt="The credentials specified above are entered into the Enter your credentials dialog." title="Enter your credentials" style="max-width:100%;">



### 8. 在對話視窗中點選連接按鈕點選 Yes 按鈕

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/remote-desktop-connection-identity-verification-sqlserver2008.png" alt="In the Remote Desktop Connection dialog box, a warning states that the identity of the remote computer cannot be verified, and asks if you want to continue anyway. At the bottom, the Yes button is circled." title="Remote Desktop Connection dialog" style="max-width:100%;">



### 9. 啟動 Microsoft SQL Server Management Studio 17

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/start-menu-ssms-17.png" alt="SQL Server is entered into the Windows Start menu search box, and Microsoft SQL Server Management Studio 17 is highlighted in the search results." title="Windows start menu search" style="max-width:100%;">



### 10. 使用 SQL Server Authentication 登入

- **Username**: `sqlmiuser`
- **Password**: `Password.1234567890`

<img src="./image/sql-server-connect-to-server.png" alt="SQL Server is entered into the Windows Start menu search box, and Microsoft SQL Server Management Studio 17 is highlighted in the search results." title="Windows start menu search" style="max-width:100%;">



### 11. 點選 New Query 按鈕

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-new-query.png" alt="The New Query button is highlighted in the SSMS toolbar." title="SSMS Toolbar" style="max-width:100%;">



### 12. 執行以下 SQL 指令將復原模式改為 FULL

```sql
USE master;
GO

-- Update the recovery model of the database to FULL and enable Service Broker
ALTER DATABASE TailspinToys SET
RECOVERY FULL,
ENABLE_BROKER WITH ROLLBACK IMMEDIATE;
GO
```



### 13. 執行

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-execute.png" alt="The Execute button is highlighted in the SSMS toolbar." title="SSMS Toolbar" style="max-width:100%;">





## 任務 2. 使用 Data Migration Assistant DMA 評估遷移至 Azure SQL Database

### 1. 啟動 Microsoft Data Migration Assistant

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/windows-start-menu-dma.png" alt="In the Windows Start menu, &quot;data migration&quot; is entered into the search bar, and Microsoft Data Migration Assistant is highlighted in the Windows start menu search results." title="Data Migration Assistant" style="max-width:100%;">



### 2. 新增專案

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-new.png" alt="The new project icon is highlighted in DMA." title="New DMA project" style="max-width:100%;">



### 3. 輸入與選擇必要資訊


- **Project type**: Select **Assessment**.
- **Project name**: Enter `ToAzureSqlDb`
- **Assessment type**: Select **Database Engine**.
- **Source server type**: Select **SQL Server**.
- **Target server type**: Select **Azure SQL Database**.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-new-project-to-azure-sql-db.png" alt="The new project settings for doing a SQL Server to Azure SQL Database migration assessment are entered into the dialog." title="New project settings" style="max-width:100%;">



### 4. 點選 Create 按鈕



### 5. 勾選 Check database compatibility、Check feature parity 並點選 Next

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-options.png" alt="Check database compatibility and check feature parity are checked on the Options screen." title="DMA options" style="max-width:100%;">



### 6. 輸入必要連線資訊

- **Server name**: Select **SQLSERVER2008**.
- **Authentication type**: Select **SQL Server Authentication**.
- **Username**: Enter `WorkshopUser`
- **Password**: Enter `Password.1234567890`
- **Encrypt connection**: Check this box.
- **Trust server certificate**: Check this box.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-connect-to-a-server.png" alt="In the Connect to a server dialog, the values specified above are entered into the appropriate fields." title="Connect to a server" style="max-width:100%;">



### 7. 點選連接



### 8. 勾選 TailspinToys 資料庫，並點選 Add

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-add-sources.png" alt="The TailspinToys box is checked on the Add sources dialog." title="Add sources" style="max-width:100%;">



### 9. 點選 Start Assessment

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-start-assessment-to-azure-sql-db.png" alt="Start assessment" title="Start assessment" style="max-width:100%;">



### 10. 檢視遷移至 *Azure SQL Database* 的評估結果

<img src="https://github.com//microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-feature-parity-service-broker-not-supported.png" alt="Start assessment" title="Start assessment" style="max-width:100%;">

>由結果可知 Azure SQL Database 並不支援兩項在地端 TailspinToys 資料庫中所使用的兩項功能即：<br>1. *跨資料庫查詢*<br>2. *Service Broker*<br>如果非得遷移到 Azure SQL Database 就必須針對資料庫修改。





## 任務 3. 使用 Data Migration Assistant DMA 評估遷移至 Azure SQL Database Managed Instance

透過檢視 Data Migration Assistant 的評估報告後，可得知有兩項功能不支援。但我們有另外一個選擇，就是 *Azure SQL Database Managed Instance* 。請再次使用 Data Migration Assistant 評估遷移至 *Azure SQL Database Managed Instance* 。



### 1. 新增專案

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-new.png" alt="The new project icon is highlighted in DMA." title="New DMA project" style="max-width:100%;">



### 2. 輸入與選擇必要資訊


- **Project type**: Select **Assessment**.
- **Project name**: Enter `ToSqlMi`
- **Assessment type**: Select **Database Engine**.
- **Source server type**: Select **SQL Server**.
- **Target server type**: Select **Azure SQL Database Managed Instance**.



<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-new-project-to-sql-mi.png" alt="The new project settings for doing a SQL Server to Azure SQL Database Managed Instance migration assessment are entered into the dialog." title="New project settings" style="max-width:100%;">



### 3. 點選 Create 按鈕



### 4. 勾選 Check database compatibility、Check feature parity 並點選 Next

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-options.png" alt="Check database compatibility and check feature parity are checked on the Options screen." title="DMA options" style="max-width:100%;">



### 5. 輸入必要連線資訊

- **Server name**: Select **SQLSERVER2008**.
- **Authentication type**: Select **SQL Server Authentication**.
- **Username**: Enter `WorkshopUser`
- **Password**: Enter `Password.1234567890`
- **Encrypt connection**: Check this box.
- **Trust server certificate**: Check this box.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-connect-to-a-server.png" alt="In the Connect to a server dialog, the values specified above are entered into the appropriate fields." title="Connect to a server" style="max-width:100%;">

### 6. 點選連接



### 7. 勾選 TailspinToys 資料庫，並點選 Add

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-add-sources.png" alt="The TailspinToys box is checked on the Add sources dialog." title="Add sources" style="max-width:100%;">



### 8. 點選 Start Assessment

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-start-assessment-to-sql-mi.png" alt="Start assessment" title="Start assessment" style="max-width:100%;">



### 9. 檢視遷移至 *Azure SQL Database Managed Instance* 的評估結果

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-feature-parity-sql-mi.png" alt="For a target platform of Azure SQL Database Managed Instance, no issues are listed." title="Database feature parity" style="max-width:100%;">

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dma-compatibility-issues-sql-mi.png" alt="For a target platform of Azure SQL Database Managed Instance, a message that full-text search has changed, and the list of impacted objects are listed." title="Compatibility issues" style="max-width:100%;">

>由結果可知地端 TailspinToys 資料庫中所使用的功能在 Azure SQL Database Managed Instance 都有支援。<br>除此之外還有提供全文檢索的功能，但這並不會影響 TailspinToys 資料庫遷移到 Azure SQL Database Managed Instance。





## 任務 4. 在 SqlServer2008 虛擬機器上建立網路分享

在此任務中將會在 SqlServer2008 虛擬機器上建立網路分享(SMB)，此分享將被 Data Migration Service 用來將地端 TailspinToys 資料庫遷移至 Azure SQL Database Managed Instance，如此可減少停機時間。



### 1. 在 SqlServer2008 虛擬機器啟動檔案管理員

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/windows-task-bar.png" alt="The Windows Explorer icon is highlighted in the Windows Taskbar." title="Windows Taskbar" style="max-width:100%;">



### 2. 在電腦 Windows (C:) 中新增名為 dms-backups 的資料夾

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/windows-explorer-new-folder.png" alt="In Windows Explorer, Windows (C:) is selected under Computer in the left-hand tree view, and New folder is highlighted in the top menu." title="Windows Explorer" style="max-width:100%;">



### 3. 在名為 dms-backups 的資料夾點擊右鍵，在選單選擇 Share with 與 Specific people

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/windows-explorer-folder-share-with.png" alt="In Windows Explorer, the context menu for the dms-backups folder is displayed, with Share with and Specific people highlighted." title="Windows Explorer" style="max-width:100%;">



### 4. 在對話視窗中確定 sqlmiuser 具備讀寫權限，並點選 Share

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/file-sharing.png" alt="In the File Sharing dialog, the sqlmiuser is highlighted and assigned a permission level of Read/Write." title="File Sharing" style="max-width:100%;">



### 5. 在對話視窗中點選 No, make the network that I am connected to a private network

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/network-discovery-and-file-sharing.png" alt="In the Network discovery and file sharing dialog, No, make the network that I am connected to a private network is highlighted." title="Network discovery and file sharing" style="max-width:100%;">



### 6. 在對話視窗中檢視並記下分享資料夾的路徑 `\\SQLSERVER2008\dms-backups` ，並點選 Done

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/file-sharing-done.png" alt="The Done button is highlighted on the File Sharing dialog." title="File Sharing" style="max-width:100%;">





## 任務 5. 更改 MSSQLSERVER 服務以在 sqlmiuser 帳戶下執行

在此任務中，將使用 SQL Server Configuration Manager 變更 SQL Server (MSSQLSERVER) 的使用帳戶為 `sqlmiuser `，以求 SQL Server service 具備充分的權限。



### 1. 在 SqlServer2008 虛擬機器中啟動 SQL Server Configuration Managed

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/windows-start-sql-configuration-manager.png" alt="In the Windows Start menu, &quot;sql configuration&quot; is entered into the search box, and SQL Server Configuration Manager is highlighted in the search results." title="Windows search" style="max-width:100%;">

> **注意**: 請選擇 **SQL Server Configuration Manager**, 而非 **SQL Server 2017 Configuration Manager**，SQL Server 2017 Configuration Manager 不適用 SQL Server 2008 R2 資料庫。



### 2. 在對話視窗左側選單中選擇 SQL Server Services ，並在右側選單右鍵點擊 SQL Server (MSSQLSERVER) 並選擇 Properties 

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/sql-server-configuration-manager-services.png" alt="SQL Server Services is selected and highlighted in the tree view of the SQL Server Configuration Manager. In the Services pane, SQL Server (MSSQLSERVER) is selected and highlighted. Properties is highlighted in the context menu." title="SQL Server Configuration Manager" style="max-width:100%;">



### 3. 在對話視窗中選擇 This account 並輸入必要資訊

- **Account name**: `sqlmiuser`
- **Password**: `Password.1234567890`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/sql-server-service-properties.png" alt="In the SQL Server (MSSQLSERVER) Properties dialog, This account is selected under Log on as and the sqlmiuser account name and password are entered." title="SQL Server (MSSQLSERVER) Properties" style="max-width:100%;">



### 4. 點選 OK



### 5. 點選 YES

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/confirm-account-change.png" alt="The Yes button is highlighted in the Confirm Account Change dialog." title="Confirm Account Change" style="max-width:100%;">



### 6. 確認登錄身分已變更為 `./sqlmiuser`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/sql-server-service.png" alt="在SQL Server服務列表中，突出顯示了SQL Server（MSSQLSERVER）服務。" title="SQL Server服務" style="max-width:100%;">



### 7. 關閉對話視窗





## 任務 6. 備份 TailspinToys 資料庫




### 1. 啟動 Microsoft SQL Server Management Studio 17

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/start-menu-ssms-17.png" alt="在Windows“開始”菜單搜索框中輸入SQL Server，並且在搜索結果中突出顯示Microsoft SQL Server Management Studio 17。" title="Windows開始菜單搜索" style="max-width:100%;">



### 2. 使用 SQL Server Authentication 登入

- **Username**: `sqlmiuser`
- **Password**: `Password.1234567890`

<img src="./image/sql-server-connect-to-server.png" alt="SQL Server is entered into the Windows Start menu search box, and Microsoft SQL Server Management Studio 17 is highlighted in the search results." title="Windows start menu search" style="max-width:100%;">



### 3. 展開資料庫並在 TailspinToys 資料庫右鍵點擊，在選單中選擇 Tasks，之後選擇 Back up

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-backup.png" alt="In the SSMS Object Explorer, the context menu for the TailspinToys database is displayed, with Tasks and Back Up... highlighted." title="SSMS Backup" style="max-width:100%;">



### 4. 在對話視窗中移除原本的路徑 `C:\TailspinToys.bak`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-back-up-database-general-remove.png" alt="In the General tab of the Back Up Database dialog, C:\TailspinToys.bak is selected, and the Remove button is highlighted under destinations." style="max-width:100%;" class="">



### 5. 加入新的路徑，並將路徑指定為先前建立的分享資料夾，檔案名稱命名為 TailspinToys.bak

- **完整路徑**: `C:\dms-backups\TailspinToys.bak`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-back-up-database-general.png" alt="In the General tab of the Back Up Database dialog, the Add button is highlighted under destinations." title="Back Up Database" style="max-width:100%;">



### 6. 在對話視窗裝點選 `...` 

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-select-backup-destination.png" alt="The Browse button is highlighted in the Select Backup Destination dialog." title="Select Backup Destination" style="max-width:100%;">



### 7. 選擇 `C:\dms-backups` 資料夾並輸入檔名 `TailspinToys.bak`，然後按下 OK

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-locate-database-files.png" alt="In the Select the file pane, the C:\dms-backups folder is selected and highlighted and TailspinToys.bak is entered into the File name field." title="Location Database Files" style="max-width:100%;">



### 8. 點選 OK



### 9. 點選 Media Options 後，選擇 Back up to the existing media set 並勾選 Perform checksum before writing to media

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-back-up-database-media-options.png" alt="In the Back Up Database dialog, the Media Options page is selected, and Overwrite all existing backup sets and Perform checksum before writing to media are selected and highlighted." title="Back Up Database" style="max-width:100%;">



### 10. 點選 OK 開始備份



### 11. 當備份完成你會看到以下訊息，點選 OK

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-backup-complete.png" alt="A dialog displaying a message that the database backup was completed successfully." title="Backup complete" style="max-width:100%;">





## 任務 7. 檢視 Azure SQL Database Managed Instance 與 SQL Server 2008 虛擬機器連接訊息

在此任務中，我們將會使用 Azure Cloud Shell 檢視 Database Migration Service 連接 SqlServer2008 虛擬機器的必要資訊



### 1. 在 [Azure portal](https://portal.azure.com/) 點選 Azure Cloud Shell 圖示

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-icon.png" alt="The Azure Cloud Shell icon is highlighted in the Azure portal's top menu." title="Azure Cloud Shell" style="max-width:100%;">



### 2. 選擇 PowerShell

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-select-powershell.png" alt="In the Welcome to Azure Cloud Shell window, PowerShell is highlighted." title="Azure Cloud Shell" style="max-width:100%;">



### 3. 若無任何儲存體，請新增一個

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-create-storage.png" alt="In the You have no storage mounted dialog, a subscription has been selected, and the Create Storage button is highlighted." title="Azure Cloud Shell" style="max-width:100%;">



### 4. 等待數分鐘後，即可使用 Cloud Shell

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-ps-azure-prompt.png" alt="In the Azure Cloud Shell dialog, a message is displayed that requesting a Cloud Shell succeeded, and the PS Azure prompt is displayed." title="Azure Cloud Shell" style="max-width:100%;">



### 5. 鍵入以下指令，並將 `<your-resource-group-name>` 替換成你的資源群組名稱

```powershell
$resourceGroup = "<your-resource-group-name>"
az vm list-ip-addresses -g $resourceGroup -n SqlServer2008 --output table
```



### 6. 你將會看到以下訊息，請將 `PublicIpAddresses` 底下的 `ipAddress` 記下

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-az-vm-list-ip-addresses.png" alt="The output from the az vm list-ip-addresses command is displayed in the Cloud Shell, and the publicIpAddress for the SqlServer2008 VM is highlighted." title="Azure Cloud Shell" style="max-width:100%;">



### 7. 將 Azure Cloud Shell 保持開啟以進行下一個任務





## 任務 8. 創建服務主體

在此任務中，將使用 Azure Cloud Shell 創建 Azure Active Directory(Azure AD)應用程序和服務主體，以確保 Data Migration Service 有足夠的權限訪問 Azure SQL Database Managed Instance 的 DMS 訪問。將向 hands-on-lab 資源群組和訂閱授予權限。

### 1. 輸入以下指令取得訂閱 ID 

```powershell
az account list --output table
```



### 2. 在輸出的資訊中找到用於此實驗的訂閱ID，並輸入以下指令，請將 `<your-subscription-id>` 替換為訂閱ID

```powershell
$subscriptionId = "<your-subscription-id>"
```



### 3. 輸入以下指令取得訂閱資訊

```powershell
az ad sp create-for-rbac -n "tailspin-toys" --role owner --scopes subscriptions/$subscriptionId/resourceGroups/$resourceGroup
```
<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/azure-cli-create-sp.png" alt="The az ad sp create-for-rbac command is entered into the Cloud Shell, and the output of the command is displayed." title="Azure CLI" style="max-width:100%;">



### 4. 記錄 `appId` 與 `password `



### 5. 在 hands-on-lab 的資源群組中點選左側 Access control (IAM)，並點選 Role assignments，在列表中找到 tailspin-toys

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/rg-hands-on-lab-role-assignments.png" alt="The Role assignments tab is displayed, with tailspin-toys highlighted under OWNER in the list." title="Role assignments" style="max-width:100%;">



### 6. 輸入以下指令在訂閱創建 CONTRIBUTOR 角色

```powershell
az role assignment create --assignee http://tailspin-toys --role contributor
```





## 任務 9. 建立與執行線上遷移專案

### 1. 在 [Azure portal](https://portal.azure.com/) 中點選 hands-on-lab 資源群組，再點選 tailspin-dms

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-group-dms-resource.png" alt="The tailspin-dms Azure Database Migration Service is highlighted in the list of resources in the hands-on-lab-SUFFIX resource group." title="Resources" style="max-width:100%;">



### 2. 在 Azure Database Migration Service 頁面上側點選 New Migration Project.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-add-new-migration-project.png" alt="On the Azure Database Migration Service blade, +New Migration Project is highlighted in the toolbar." title="Azure Database Migration Service New Project" style="max-width:100%;">



### 3. 輸入以下資訊

- **Project name**: 輸入 `OnPremToSqlMi`
- **Source server type**: 選擇 **SQL Server**.
- **Target server type**: 選擇 **Azure SQL Database Managed Instance**.
- **Choose type of activity**: 選擇 **Online data migration** 並選擇 **Save**.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-new-migration-project-blade.png" alt="The New migration project blade is displayed, with the values specified above entered into the appropriate fields." title="New migration project" style="max-width:100%;">



### 4. 點選 Create and run activity



### 5. 在 Select source 選項中輸入資訊

- **Source SQL Server instance name**: 輸入先前紀錄的 SqlServer2008 VM IP, 如 `13.66.228.107`
- **Authentication type**: 選擇SQL驗證
- **Username**: 輸入 `WorkshopUser`
- **Password**: 輸入 `Password.1234567890`
- **Connection properties**: 勾選 Encrypt connection 與 Trust server certificate

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-select-source.png" alt="The Migration Wizard Select source blade is displayed, with the values specified above entered into the appropriate fields." title="Migration Wizard Select source" style="max-width:100%;">





### 6. 選擇 Save

### 7. 在 Select target 選項中輸入資訊

- **Application ID**: 輸入先前記錄之 `appId`
- **Key**: 輸入先前記錄之 `password`
- **Subscription**: 選擇你的訂閱
- **Target Azure SQL Managed Instance**: 選擇 sqlmi
- **SQL Username**: Enter `sqlmiuser`
- **Password**: Enter `Password.1234567890`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-select-target.png" alt="The Migration Wizard Select target blade is displayed, with the values specified above entered into the appropriate fields." title="Migration Wizard Select target" style="max-width:100%;">



### 8. 選擇 Save



### 9. 在 Select databases 選項中選擇 TailspinToys

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-select-databases.png" alt="The Migration Wizard Select databases blade is displayed, with the TailspinToys database selected." title="Migration Wizard Select databases" style="max-width:100%;">



### 10. 選擇 Save



### 11. 在 Configure migration settings 選項中輸入資訊

- **Network share location**: 輸入 SQLSERVER2008 VM 上的分享路徑 `\\SQLSERVER2008\dms-backups`
- **Windows User Azure Database Migration Service impersonates to upload files to Azure Storage**: 輸入 `SQLSERVER2008\sqlmiuser`
- **Password**: Enter `Password.1234567890`
- **Subscription containing storage account**: 選擇訂閱帳號
- **Storage account**: 選擇 **sqlmistore** storage account.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-configure-migration-settings.png" alt="The Migration Wizard Configure migration settings blade is displayed, with the values specified above entered into the appropriate fields." title="Migration Wizard Configure migration settings" style="max-width:100%;">



### 12. 選擇 Save



### 13. 在 Summary 選項中輸入資訊

- **Activity name**: 輸入 TailspinToysMigration.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-migration-summary.png" alt="The Migration Wizard summary blade is displayed, Sql2008ToSqlDatabase is entered into the name field, and Validate my database(s) is selected in the Choose validation option blade, with all three validation options selected." title="Migration Wizard Summary" style="max-width:100%;">



### 14. 選擇 Run migration



### 15. 點選上方 Refresh 以更新狀態

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-status-running.png" alt="On the Migration job blade, the Refresh button is highlighted, and a status of Full backup uploading is displayed and highlighted." title="Migration status" style="max-width:100%;">



### 16. 更新狀態直至 Log shipping in progress

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-status-log-files-uploading.png" alt="In the migration monitoring window, a status of Log shipping in progress is highlighted." title="Migration status" style="max-width:100%;">





## 任務 10. 執行遷移

### 1. 在遷移狀態視窗中選擇 TailspinToys 以獲得更多訊息

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-database-name.png" alt="The TailspinToys database name is highlighted in the migration status window." title="Migration status" style="max-width:100%;">



### 2. 在 TailspinToys 中確認 TailspinToys.bak 狀態為 Restored

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-database-restored.png" alt="On the TailspinToys blade, a status of Restored is highlighted next to the TailspinToys.bak file in the list of active backup files." title="Migration Wizard" style="max-width:100%;">



### 3. 為了檢視資料庫遷移的過程，我們將新增一筆資料



### 4. 在 SqlServer2008 虛擬機器啟動 SSMS 並點選 New Query

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-new-query.png" alt="The New Query button is highlighted in the SSMS toolbar." title="SSMS Toolbar" style="max-width:100%;">



### 5. 將以下指令貼到新的 query 視窗

```sql
USE TailspinToys;
GO

INSERT [dbo].[Game] (Title, Description, Rating, IsOnlineMultiplayer)
VALUES ('Space Adventure', 'Explore the universe with are newest online multiplayer gaming experience. Build custom  rocket ships, and take off for the stars in an infinite open-world adventure.', 'T', 1)
```



### 6. 執行指令新增資料到 Game table

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-execute.png" alt="The Execute button is highlighted in the SSMS toolbar." title="SSMS Toolbar" style="max-width:100%;">



### 7. 新增資料後再次備份，Data Migration Service 會檢查更新，並移至 Migration Service 中，點選 New Query 執行以下指令進行備份
>注意，這次只備份交易紀錄

```sql
USE master;
GO

BACKUP LOG TailspinToys
TO DISK = 'c:\dms-backups\TailspinToysLog.trn'
WITH CHECKSUM
GO
```



### 8. 回到 Azure Portal 中檢視遷移狀態，點選更新按鈕以獲得最新資訊，並可以看到 TailspinToysLog.trn 檔案狀態為已上傳

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-transaction-log-uploaded.png" alt="On the TailspinToys blade, the Refresh button is highlighted. A status of Uploaded is highlighted next to the TailspinToysLog.trn file in the list of active backup files." title="Migration Wizard" style="max-width:100%;">

>若你尚未 TailspinToysLog.trn 請重新整理直至項目出現為止



### 9. 當 TailspinToysLog.trn 檔案上傳後會開始還原，請繼續更新直到狀態為已還原，耗時約數分鐘

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-transaction-log-restored.png" alt="A status of Restored is highlighted next to the TailspinToysLog.trn file in the list of active backup files." title="Migration Wizard" style="max-width:100%;">



### 10. 當 TailspinToysLog.trn 狀態為已還原後，點選 Start Cutover

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-start-cutover.png" alt="The Start Cutover button is displayed." title="DMS Migration Wizard" style="max-width:100%;">



### 11. 在對完成切換的話視窗中檢視資訊，待還原的 Log 數為 `0`，勾選確認並接受

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-complete-cutover-apply.png" alt="In the Complete cutover dialog, a value of 0 is highlighted next to Pending log backups, and the Confirm checkbox is checked." title="Migration Wizard" style="max-width:100%;">



### 12. 在對話視窗下方有進度條，點選 `X` 關閉視窗

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-complete-cutover-completed.png" alt="A status of Completed is displayed in the Complete cutover dialog." title="Migration Wizard" style="max-width:100%;">



### 13. 在頁面中檢視 TailspinToys 還原狀態，持續更新直至已完成

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/dms-migration-wizard-status-complete.png" alt="On the Migration job blade, the status of Completed is highlighted." title="Migration with Completed status" style="max-width:100%;">



### 14. 恭喜你已經將地端 TailspinToys 資料庫遷移至 Azure SQL Managed Instance





## 任務 11. 驗證資料庫遷移

### 1. 請在 [Azure Portal](https://portal.azure.com/) 中開啟 Azure Cloud Shell

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-icon.png" alt="The Azure Cloud Shell icon is highlighted in the Azure portal's top menu." title="Azure Cloud Shell" style="max-width:100%;">



### 2. 點選 PowerShell

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-select-powershell.png" alt="In the Welcome to Azure Cloud Shell window, PowerShell is highlighted." title="Azure Cloud Shell" style="max-width:100%;">



### 3. 經過數分鐘後就可以使用 PowerShell

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-ps-azure-prompt.png" alt="In the Azure Cloud Shell dialog, a message is displayed that requesting a Cloud Shell succeeded, and the PS Azure prompt is displayed." title="Azure Cloud Shell" style="max-width:100%;">



### 4. 輸入以下指令，並將 `<your-resource-group-name>` 替換為你的資源群組名稱

```powershell
$resourceGroup = "<your-resource-group-name>"
az sql mi list --resource-group $resourceGroup
```

### 5. 在 PowerShell 中檢視資料庫位置

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/cloud-shell-az-sql-mi-list-output.png" alt="The output from the az sql mi list command is displayed in the Cloud Shell, and the fullyQualifiedDomainName property and value are highlighted." title="Azure Cloud Shell" style="max-width:100%;">



### 6. SqlServer2008 虛擬機器的 SQL Server Management Studio中，新增連接

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-object-explorer-connect.png" alt="In the SSMS Object Explorer, Connect is highlighted in the menu and Database Engine is highlighted in the Connect context menu." title="SSMS Connect" style="max-width:100%;">



### 7. 在對話視窗中輸入連接資訊

- **Server name**: Enter the fully qualified domain name of your SQL managed instance, which you copied from the Azure Cloud Shell in the previous steps.
- **Authentication**: Select **SQL Server Authentication**.
- **Login**: Enter `sqlmiuser`
- **Password**: Enter `Password.1234567890`
- Check the **Remember password** box.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-connect-to-server-sql-mi.png" alt="The SQL managed instance details specified above are entered into the Connect to Server dialog." title="Connect to Server" style="max-width:100%;">



### 8. 點選 Connect



### 9. 點選 Azure SQL Database Managed Instance 連線中的 TailspinToys 資料庫

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-sql-mi-tailspintoys-database.png" alt="In the SSMS Object Explorer, the SQL MI connection is expanded, and the TailspinToys database is highlighted and selected." title="SSMS Object Explorer" style="max-width:100%;">



### 10. 執行以下命令檢視資料

```sql
USE TailspinToys;
GO

SELECT * FROM Game
```



### 11. 執行命令後可看到後來新增的資料

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/ssms-query-game-table.png" alt="In the new query window, the query above has been entered, and in the results pane, the new Space Adventure game is highlighted." title="SSMS Query" style="max-width:100%;">





## 任務 12. 部署網頁應用程式

### 1. 在 [Azure Portal](https://portal.azure.com/) 中尋找資源群組

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-groups.png" alt="Resource groups is selected in the Azure navigation pane, and the &quot;hands-on-lab-SUFFIX&quot; resource group is highlighted." title="Resource groups list" style="max-width:100%;">



### 2. 在點選 JumpBox 虛擬機器

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-group-resources-jumpbox.png" alt="The list of resources in the hands-on-lab-SUFFIX resource group are displayed, and JumpBox is highlighted." title="JumpBox in resource group list" style="max-width:100%;">



### 3. JumpBox 虛擬機器頁面上方點選連接並選擇 RDP

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/connect-jumpbox.png" alt="The JumpBox VM blade is displayed, with the Connect button highlighted in the top menu." title="Connect to JumpBox VM" style="max-width:100%;">



### 4. 下載 RDP 檔案

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/connect-to-virtual-machine.png" alt="The Connect to virtual machine blade is displayed, and the Download RDP File button is highlighted." title="Connect to virtual machine" style="max-width:100%;">



### 5. 在對話視窗中點選連接按鈕

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/remote-desktop-connection.png" alt="In the Remote Desktop Connection Dialog Box, the Connect button is highlighted." title="Remote Desktop Connection dialog" style="max-width:100%;">



### 6. 輸入 Username 與 Password 遠端登入
- **Username**: `sqlmiuser`
- **Password**: `Password.1234567890`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/rdc-credentials.png" alt="The credentials specified above are entered into the Enter your credentials dialog." title="Enter your credentials" style="max-width:100%;">



### 7. 在對話視窗中點選連接按鈕點選 Yes 按鈕

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/remote-desktop-connection-identity-verification-jumpbox.png" alt="In the Remote Desktop Connection dialog box, a warning states that the identity of the remote computer cannot be verified, and asks if you want to continue anyway. At the bottom, the Yes button is circled." title="Remote Desktop Connection dialog" style="max-width:100%;">



### 8. 開啟檔案管理員

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/windows-2019-start-bar-file-explorer.png" alt="The File Explorer icon is highlighted in the Windows start bar." title="Windows start bar" style="max-width:100%;">



### 9. 在 `C:\hands-on-lab\MCW-Migrating-SQL-databases-to-Azure-master\Hands-on lab\lab-files` 路徑中，雙擊 TailspinToysWeb.sln

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/windows-explorer-tailspintoysweb.png" alt="The folder at the path specified above is displayed, and TailspinToys.sln is highlighted." title="Windows Explorer" style="max-width:100%;">



### 10. 若顯示詢問對話框，點選 Visual Studio 2019 後，點選OK

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-version-selector.png" alt="In the Visual Studio version selector, Visual Studio 2019 is selected and highlighted." title="Visual Studio" style="max-width:100%;">



### 11. 以 Azure account 登入

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-sign-in.png" alt="On the Visual Studio welcome screen, the Sign in button is highlighted." title="Visual Studio" style="max-width:100%;">



### 12. 在對話視窗中勾選 `Ask me for every project in this solution` 並點選OK

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-security-warning.png" alt="A Visual Studio security warning is displayed, and the Ask me for every project in this solution checkbox is unchecked and highlighted." title="Visual Studio" style="max-width:100%;">



### 13. 在 Visual Studio 中，右鍵點選 TailspinToysWeb 專案，並點選 `Publish`

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-project-publish.png" alt="In the Solution Explorer, the context menu for the TailspinToysWeb project is displayed, and Publish is highlighted." title="Visual Studio" style="max-width:100%;">



### 14. 在對話視窗中點選 App Service ，然後選擇 Select Existing ，並點選 Create Profile

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-pick-publish-target.png" alt="In the Pick a publish target dialog, App Service is selected, and Select Existing is selected." title="Visual Studio" style="max-width:100%;">



### 15. 選擇 tailspintoys App Service

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-select-existing-app-service.png" alt="The tailspintoysUNIQUEID App Service is selected in the list of existing App Services." title="Visual Studio" style="max-width:100%;">



### 16. 選擇 OK



### 17. 點選 Publish

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-publish-web-app.png" alt="The Publish button is highlighted on the Publish page in Visual Studio." title="Publish" style="max-width:100%;">



### 18. 當部署完成，可以在 Visual Studio 下方的輸出訊息中看到應用程式網址

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/visual-studio-output-publish-succeeded.png" alt="The Publish Succeeded message is displayed in the Visual Studio Output pane." title="Visual Studio" style="max-width:100%;">



### 19. 當你點選該網址會看到以下錯誤訊息，是因為還沒設定點為連接 Azure SQL Database Managed Instance 

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/web-app-error-screen.png" alt="An error screen is displayed, because the database connection string has not been updated to point to SQL MI in the web app's configuration." title="Web App error" style="max-width:100%;">



## 任務 13. 更新網頁應用程式設定

### 1. 在 [Azure Portal](https://portal.azure.com/) 點選資源群組

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/azure-services-resource-groups.png" alt="Resource groups is highlighted in the Azure services list." title="Azure services" style="max-width:100%;">



### 2. 點選練習的資源群組 

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-groups.png" alt="Resource groups is selected in the Azure navigation pane, and the &quot;hands-on-lab-SUFFIX&quot; resource group is highlighted." title="Resource groups list" style="max-width:100%;" class="">



### 3.  點選 tailspintoys App Service

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-groups.png" alt="Resource groups is selected in the Azure navigation pane, and the &quot;hands-on-lab-SUFFIX&quot; resource group is highlighted." title="Resource groups list" style="max-width:100%;" class="">



### 4.  在左側選單中點選 Configuration

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-configuration-menu.png" alt="The Configuration item is selected under Settings." title="Configuration" style="max-width:100%;">



### 5.  點選 `TailspinToysContext` 右側的設定 icon

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-configuration-connection-strings.png" alt="In the Connection string section, the pencil icon is highlighted to the right of the TailspinToysContext connection string." title="Connection Strings" style="max-width:100%;">



### 6.  將以下連接字串 `<your-sqlmi-host-fqdn-value>` 替換為你的 Azure SQL Database Managed Instance 資料庫位置，作為連線字串

```sql
Server=tcp:<your-sqlmi-host-fqdn-value>,1433;Database=TailspinToys;User ID=sqlmiuser;Password=Password.1234567890;Trusted_Connection=False;Encrypt=True;TrustServerCertificate=True;
```



### 7. 對話視窗中修改連線字串，再次提醒將 `<your-sqlmi-host-fqdn-value>` 替換為你的 Azure SQL Database Managed Instance 資料庫位置

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-configuration-edit-conn-string.png" alt="The your-sqlmi-host-fqdn-value string is highlighted in the connection string." title="Edit Connection String" style="max-width:100%;">

>看起來會像這樣

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-configuration-edit-conn-string-value.png" alt="The updated connection string is displayed, with the fully qualified domain name of SQL MI highlighted within the string." title="Connection string value" style="max-width:100%;">

### 8. 點選 OK



### 9. 將 `TailspinToysReadOnlyContext` 進行相同的設定



### 10. 點選上方的 Save 按鈕




### 11. 點選 Continue

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-restart.png" alt="The prompt warning that the application will be restarted is displayed, and the Continue button is highlighted." title="Restart prompt" style="max-width:100%;">



### 12. 在左側選單中點選 Overview

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-overview-menu-item.png" alt="Overview is highlighted on the left-hand menu for App Service" title="Overview menu item" style="max-width:100%;">



### 13. 瀏覽 URL 會看到以下錯誤訊息，因為 Azure SQL Database Managed Instance 存在隔離的虛擬網路之中

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/web-app-error-screen.png" alt="An error screen is displayed, because the application is unable to connect to SQL MI within its private virtual network." title="Web App error" style="max-width:100%;">




## 任務 14. 設定 P2S VPN

### 1. 在 [Azure Portal](https://portal.azure.com/) 資源群組中點選 hands-on-lab-vnet-gateway

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/resource-group-vnet-gateway.png" alt="The Virtual network gateway resource is highlighted in the list of resources." title="Resources" style="max-width:100%;">



### 2. 在左側選單中點選 Point-to-site configuration，並點選右側的 Configure now

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/virtual-network-gateway-configure-point-to-site.png" alt="Point-to-site configuration is highlighted and selected in the left-hand menu. On the Point-to-site configuration blade, Configure now is highlighted." title="Virtual network gateway" style="max-width:100%;">



### 3. 在 P2S 設定頁面中輸入以下資訊

- **Address pool**: 輸入要設定的 IP 地址範圍，應位於以下其中一項，但不應與虛擬網路使用的範圍重複
  - `10.0.0.0/8` - 這表示IP地址範圍從 10.0.0.0 到 10.255.255.255
  - `172.16.0.0/12` - 這表示IP地址範圍從 172.16.0.0 到 172.31.255.255
  - `192.168.0.0/16` - 這表示IP地址範圍從 192.168.0.0 到 192.168.255.255
- **Tunnel type**: 選擇 **SSTP(SSL)**.
- **Authentication type**: 選擇 **Azure certificate**.

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/virtual-network-gateway-point-to-site-configuration.png" alt="The values specified above are entered into the Point-to-site configuration form." title="Virtual network gateway" style="max-width:100%;">



### 4. 點選 Save 儲存設定





## 任務 15. 設定虛擬網路以整合 App Service

### 1. 在 [Azure Portal](https://portal.azure.com/) 資源群組中點選 tailspintoys App Service

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/rg-app-service.png" alt="The tailspintoys App Service is highlighted in the list of resource group resources." title="Resource group" style="max-width:100%;">



### 2. 在左側選單中點選 Networking ，並點選右側 Click here to configure

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-networking.png" alt="On the App Service blade, Networking is selected in the left-hand menu and Click here to configure is highlighted under VNet Integration." title="App Service" style="max-width:100%;">



### 3. 點選 Add VNet

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-vnet-configuration.png" alt="Add VNet is highlighted on the VNet Configuration blade." title="App Service" style="max-width:100%;">



### 4. 在對話視窗選擇虛擬網路

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-vnet-configuration-add-vnet.png" alt="The hands-on-lab-SUFFIX-vnet** is highlighted." title="App Service" style="max-width:100%;">



### 5. 更新設定需要一些時間，請點選 Refresh 更新狀態，直至 Certificate sync

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-vnet-details.png" alt="The details of the VNet Configuration are displayed. The Certificate Status, Certificates in sync, is highlighted." title="App Service" style="max-width:100%;">

>若收到新增失敗的訊息，請點選 Disconnect 再重新新增





## 任務 16. 瀏覽網頁應用程式

### 1. 在 App Service 概觀頁面中點選 URL 即可瀏覽頁面

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/app-service-url.png" alt="The App service URL is highlighted." title="App service URL" style="max-width:100%;">



### 2. 檢視成果

<img src="https://github.com/microsoft/MCW-Migrating-SQL-databases-to-Azure/raw/master/Hands-on%20lab/media/tailspin-toys-web-app.png" alt="Screenshot of the TailspinToys Operations Web App." title="TailspinToys Web" style="max-width:100%;">

>若收到錯誤訊息，請持續重新整理



### 3. 恭喜！你的應用程式已成功連接 Azure SQL Database Managed Instance 
