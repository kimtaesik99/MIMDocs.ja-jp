---
title: "手順 5. PAM のインストール/構成"
description: "スクリプトによって、Privileged Identity Manager で管理する既存の ID または新規の ID を使用して CORP ドメインを準備する"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 09/26/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: a5d86991f1579f292d7d303148422cef746d008a


---
# 手順 5. PAM のインストール/構成

ドメインに参加している PAMServer の場合は、MIMAdmin として、それ以外の場合は、ローカル管理者としてログインします。
1. 管理者として PowerShell を実行
2. cd $env:path: SYSTEMDRIVE\PAM
3. .\PAMDeploymnet.ps1
4. メニュー オプション 5 を選択 (MIM PAM セットアップ)

>[!NOTE] コンピューターがまだ PRIV ドメインに参加していない場合は、資格情報を求められます。 ドメインへの参加後、コンピューターが再起動します。

PAMServer が再起動した後は、MIMAdmin アカウントでコンピューターに再度ログインします。

1. 管理者として PowerShell を実行
2. cd $env:path: SYSTEMDRIVE\PAM
3. .\PAMDeployment.ps1
4. メニュー オプション 5 を選択 (MIM PAM セットアップ)

  要求された場合は、MIM 監視アカウント、MIM コンポーネント アカウント、MIM MA アカウント、MIM サービス アカウント、MIM 管理アカウントおよび SharePoint アカウントのパスワードを入力します。
  インストールが完了すると、コンピューターが再起動します。



<!--HONumber=Sep16_HO4-->


