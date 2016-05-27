---
# required metadata

title: ID 管理サーバー&#58; SQL Server 2014 のセットアップ |Microsoft Identity Manager
description: MIM 2016 インストールの準備で SQL Server 2014 をインストールします。
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# ID 管理サーバー: SQL Server 2014 のセットアップ

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 たとえば、
> - ドメイン コントローラー名 - **mimservername**
> - ドメイン名 - **contoso**
> - パスワード - **Pass@word1**

## **SQL Server 2014 Standard Edition** をインストールします。

1. ドメイン管理者として **PowerShell** を起動します。

2. SQL Server セットアップ プログラムがあるディレクトリに移動します。

3. 次のコマンドを入力します。

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)


<!--HONumber=Apr16_HO3-->


