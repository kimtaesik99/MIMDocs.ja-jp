---
title: "手順 3. SQL の構成"
description: "この記事は、スクリプトを使用した Privileged Identity Manager の構成手順に関するシリーズ記事の 3 番目であり、SQL Server の構成手順について説明します。"
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
ms.openlocfilehash: 93ae9f198d73d21ae966fe3c3b22e47435bd5608
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="step-3-configuring-sql"></a>手順 3. SQL の構成

>[!div class="step-by-step"]
[«手順 2](sp1-step2-configuring-corp-domain.md)
[手順 4 »](sp1-step4-configuring-sharepoint.md)

次の手順を行う前に、SQL Server 2012 SP1 以降または SQL Server 2014 を使用していることを確認します。 ドメインに参加しているサーバーの場合は、MIMAdmin アカウントを使用して、それ以外の場合は、ローカル管理者としてログインします。
1. 管理者として PowerShell を実行
2. cd $env:path: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. メニュー オプション 3 を選択 (SQL Server セットアップ)

  サーバーがまだ PRIV ドメインに参加していない場合は、資格情報の入力を求められ、入力後サーバーはドメインに参加します。
  ドメインへの参加後、コンピューターが再起動します。 正常に再起動したら、MIMAdmin アカウントを使用してサーバーにログオンします。

1. 管理者として PowerShell を実行
2. cd $env:path: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. メニュー オプション 3 を選択 (SQL Server セットアップ)

要求された場合は、MIMAdmin のサービス アカウントのパスワードを入力し、インストールを続行します。 完了したら、手順 4 に進みます。

>[!div class="step-by-step"]
[«手順 2](sp1-step2-configuring-corp-domain.md)
[手順 4 »](sp1-step4-configuring-sharepoint.md)
