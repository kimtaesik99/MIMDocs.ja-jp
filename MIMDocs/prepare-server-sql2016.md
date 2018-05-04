---
title: Microsoft Identity Manager 2016 SP1 向けの SQL Server の構成 | Microsoft Docs
description: MIM 2016 インストールの準備で SQL Server 2016 をインストールします。
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 04/26/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 297df3b3-192e-4ed9-82ed-c95eb5297c84
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: e2006ca2a74f8c974f6019004aeaefdc73069fa6
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-an-identity-management-server-sql-server-2016"></a>ID 管理サーバー: SQL Server 2016 のセットアップ

>[!div class="step-by-step"]
[« Windows Server 2016](prepare-server-ws2016.md)
[SharePoint »](prepare-server-sharepoint.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 次に例を示します。
> - ドメイン コントローラー名 - **corpdc**
> - ドメイン名 - **contoso**
> - MIM サービス サーバー名 - **corpservice**
> - MIM 同期サーバー名 - **corpsync**
> - SQL Server 名 - **corpsql**
> - パスワード - **Pass@word1**

## <a name="install-sql-server-2016-standardenterprise-edition"></a>**SQL Server 2016 Standard/Enterprise Edition** をインストールします

1. ドメイン管理者として **PowerShell** を起動します。

2. SQL Server セットアップ プログラムがあるディレクトリに移動します。

3. 次のコマンドを入力します。

    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="contoso\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="contoso\Administrator"

More info SQL deployment accounts and services can be found [here](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/configure-windows-service-accounts-and-permissions?view=sql-server-2017)
> [!NOTE]
> SSMS is no longer included in SQL 2016 downlaod details can be found [here](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)    ```

>[!div class="step-by-step"]  
[« Windows Server 2016](prepare-server-ws2016.md)
[SharePoint »](prepare-server-sharepoint.md)
