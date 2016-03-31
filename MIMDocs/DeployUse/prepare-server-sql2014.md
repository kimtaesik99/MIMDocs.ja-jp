---
title: Id 管理サーバーと #58; を準備します。SQL Server 2014 |Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: kgremban
---
# Id 管理サーバーを準備する: SQL Server 2014

>[! div クラスを「ステップバイ ステップ」=]
[前へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-ws2012r2.html)
**Id 管理サーバーの準備: Windows Server 2012 R2**

> [!NOTE]
> 次に、すべての例で **mimservername** 、ドメイン コント ローラーの名前を表す **contoso** ドメイン名を表すと **Pass@word1** 例パスワードを表します。

## インストール **SQL Server 2014 Standard Edition**

1. ドメイン管理者として **PowerShell** を起動します。

2. SQL Server セットアップ プログラムがあるディレクトリに移動します。

3. 次のコマンドを入力します。

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[! div クラスを「ステップバイ ステップ」=]  
[次へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-sharepoint.html)
**Id 管理サーバーの準備をしています: SharePoint**


<!--HONumber=Mar16_HO3-->


