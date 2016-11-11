---
title: "手順 3. SQL の構成"
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
ms.openlocfilehash: 375a34e5255c90559fc0ffb3a80fc7c92ebd27a2


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



<!--HONumber=Nov16_HO2-->


