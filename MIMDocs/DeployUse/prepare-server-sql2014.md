---
title: "SQL サーバーの構成 | Microsoft Docs"
description: "MIM 2016 インストールの準備で SQL Server 2014 をインストールします。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 4ecb80282591ae5e4e52124637ab0e2edf0809b3


---

# <a name="set-up-an-identity-management-server-sql-server-2014"></a>ID 管理サーバー: SQL Server 2014 のセットアップ

>[!div class="step-by-step"]
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 たとえば、
> - ドメイン コントローラー名 - **mimservername**
> - ドメイン名 - **contoso**
> - パスワード - **Pass@word1**

## <a name="install-sql-server-2014-standard-edition"></a>**SQL Server 2014 Standard Edition** をインストールします。

1. ドメイン管理者として **PowerShell** を起動します。

2. SQL Server セットアップ プログラムがあるディレクトリに移動します。

3. 次のコマンドを入力します。

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"
    ```

>[!div class="step-by-step"]  
[« Windows Server 2012 R2](prepare-server-ws2012r2.md)
[SharePoint »](prepare-server-sharepoint.md)



<!--HONumber=Nov16_HO2-->


