---
title: "手順 5. PAM のインストール/構成"
description: "これは、スクリプトを使用した Privileged Identity Manager 構成の 5 番目の手順であり、PAM サーバーでの展開手順について説明します。"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 01/10/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 862f62ab9bac87bcee31c35e249db34740e9fb14
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# 手順 5. PAM のインストール/構成
<a id="step-5-installingconfiguring-pam" class="xliff"></a>

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
