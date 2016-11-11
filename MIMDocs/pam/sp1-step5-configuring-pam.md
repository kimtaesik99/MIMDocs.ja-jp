---
title: "手順 5. PAM のインストール/構成"
description: "スクリプトによって、Privileged Identity Manager で管理する既存の ID または新規の ID を使用して CORP ドメインを準備する"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: c641865548f753a609ccee8dbf12c329bb6a1c9f


---
# <a name="step-5-installingconfiguring-pam"></a>手順 5. PAM のインストール/構成

>[!div class="step-by-step"]
[«手順 4](sp1-step4-configuring-sharepoint.md)
[手順 6 »](sp1-step6-setup-pam-trust.md)

ドメインに参加している PAMServer の場合は、MIMAdmin として、それ以外の場合は、ローカル管理者としてログインします。
1. 管理者として PowerShell を実行
2. cd $env:path: SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. メニュー オプション 5 を選択 (MIM PAM セットアップ)

>[!NOTE]
>コンピューターがまだ PRIV ドメインに参加していない場合は、資格情報を求められます。 ドメインへの参加後、コンピューターが再起動します。

PAMServer が再起動した後は、MIMAdmin アカウントでコンピューターに再度ログインします。

1. 管理者として PowerShell を実行
2. cd $env:path: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. メニュー オプション 5 を選択 (MIM PAM セットアップ)

  要求された場合は、MIM 監視アカウント、MIM コンポーネント アカウント、MIM MA アカウント、MIM サービス アカウント、MIM 管理アカウントおよび SharePoint アカウントのパスワードを入力します。
  インストールが完了すると、コンピューターが再起動します。

>[!div class="step-by-step"]
[«手順 4](sp1-step4-configuring-sharepoint.md)
[手順 6 »](sp1-step6-setup-pam-trust.md)



<!--HONumber=Nov16_HO2-->


