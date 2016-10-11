---
title: "手順 6. PAM 信頼のセットアップ"
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
ms.openlocfilehash: 46afda513e849e457f5f3644a46f244161467e50


---

# PAM 信頼のセットアップ

**PRIV のみの環境に必須ではありません。** MIMAdmin アカウントを使用して PAMServer にログインします。

1. MIMAdmin アカウントを使用して PAMServer にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. メニュー オプション 6 (PAM の信頼のセットアップ) を選択

  要求された場合は、CORP 管理者アカウントの資格情報を入力します。 資格情報を入力した後、信頼関係が確立され、構成が完了します。



<!--HONumber=Sep16_HO4-->


