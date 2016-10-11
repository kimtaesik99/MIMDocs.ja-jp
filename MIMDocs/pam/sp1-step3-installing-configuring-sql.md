---
title: "手順 3. SQL の構成"
description: "スクリプトによって、Privileged Identity Manager で管理する既存の ID または新規の ID を使用して CORP ドメインを準備する"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/27/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a7d456b1c2baf31ef2d7ca801a567cf42eaef52e


---
# 手順 3. SQL の構成

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



<!--HONumber=Sep16_HO4-->


