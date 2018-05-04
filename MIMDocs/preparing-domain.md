---
title: Microsoft Identity Manager 2016 のドメインのセットアップ | Microsoft Docs
description: MIM 2016 をインストールする前に、Active Directory ドメイン コントローラーを作成する
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/26/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 50345fda-56d7-4b6e-a861-f49ff90a8376
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: ff8d8a6f66212b006e2c17186dc299a5bcf3f68b
ms.sourcegitcommit: 32d9a963a4487a8649210745c97a3254645e8744
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="set-up-a-domain"></a>ドメインのセットアップ

>[!div class="step-by-step"]
[Windows Server 2016 »](prepare-server-ws2016.md)

Microsoft Identity Manger (MIM) は、Active Directory (AD) ドメインと連携します。 ドメインを管理するには、AD のインストールを済ませ、環境内にドメイン コントローラーがあることを確認します。

この記事では、ドメインと MIM を連携させるために必要な手順について説明します。

## <a name="create-user-accounts-and-groups"></a>ユーザー アカウントとグループを作成する

MIM 展開のすべてのコンポーネントには、ドメインでの個別の ID が必要です。  これには MIM サービスと MIM 同期サービス、SharePoint、および SQL などの MIM コンポーネントが含まれます。

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 次に例を示します。
> - ドメイン コントローラー名 - **corpdc**
> - ドメイン名 - **contoso**
> - MIM サービス サーバー名 - **corpservice**
> - MIM 同期サーバー名 - **corpsync**
> - SQL Server 名 - **corpsql**
> - パスワード - **Pass@word1**

1. ドメイン管理者 (*contoso \administrator* など) としてドメイン コントローラーにサインインします。

2. MIM サービス用に次のユーザー アカウントを作成します。 PowerShell を起動し、次の PowerShell スクリプトを入力してドメインを更新します。

    ```PowerShell
    import-module activedirectory
    $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
    New-ADUser –SamAccountName MIMINSTALL –name MIMMA
    Set-ADAccountPassword –identity MIMINSTALL –NewPassword $sp
    Set-ADUser –identity MIMINSTALL –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMMA –name MIMMA
    Set-ADAccountPassword –identity MIMMA –NewPassword $sp
    Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSync –name MIMSync
    Set-ADAccountPassword –identity MIMSync –NewPassword $sp
    Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMService –name MIMService
    Set-ADAccountPassword –identity MIMService –NewPassword $sp
    Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMSSPR –name MIMSSPR
    Set-ADAccountPassword –identity MIMSSPR –NewPassword $sp
    Set-ADUser –identity MIMSSPR –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SharePoint –name SharePoint
    Set-ADAccountPassword –identity SharePoint –NewPassword $sp
    Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName SqlServer –name SqlServer
    Set-ADAccountPassword –identity SqlServer –NewPassword $sp
    Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1
    New-ADUser –SamAccountName BackupAdmin –name BackupAdmin
    Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp
    Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1
    New-ADUser –SamAccountName MIMpool –name BackupAdmin
    Set-ADAccountPassword –identity MIMPool –NewPassword $sp
    Set-ADUser –identity MIMPool –Enabled 1 -PasswordNeverExpires 1
    ```

3.  すべてのグループにセキュリティ グループを作成します。

    ```PowerShell
    New-ADGroup –name MIMSyncAdmins –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncAdmins
    New-ADGroup –name MIMSyncOperators –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncOperators
    New-ADGroup –name MIMSyncJoiners –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncJoiners
    New-ADGroup –name MIMSyncBrowse –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncBrowse
    New-ADGroup –name MIMSyncPasswordReset –GroupCategory Security –GroupScope Global –SamAccountName MIMSyncPasswordReset
    Add-ADGroupMember -identity MIMSyncAdmins -Members Administrator
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMService
    Add-ADGroupmember -identity MIMSyncAdmins -Members MIMInstall
    ```

4.  SPN を追加して、サービス アカウントで Kerberos 認証を有効にする

    ```CMD
    setspn -S http/mim.contoso.com Contoso\mimpool
    setspn -S http/mim Contoso\mimpool
    setspn -S http/passwordreset.contoso.com Contoso\mimsspr
    setspn -S http/passwordregistration.contoso.com Contoso\mimsspr
    setspn -S FIMService/mim.contoso.com Contoso\MIMService
    setspn -S FIMService/corpservice.contoso.com Contoso\MIMService
    ```
5.  設定時に、適切な名前解決のために次の DNS 'A' レコードを追加する必要があります

- corpservice の物理 IP アドレスを指す mim.contoso.com
- corpservice の物理 IP アドレスを指す passwordreset.contoso.com
- corpservice の物理 IP アドレスを指す passwordregistration.contoso.com

>[!div class="step-by-step"]
[Windows Server 2016 »](prepare-server-ws2016.md)
