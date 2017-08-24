---
title: "手順 6. PAM 信頼のセットアップ"
description: "スクリプトを使用した PAM 構成の 6 番目の手順です。 このセクションでは、CORP ドメインと PRIV ドメイン間に必要な信頼関係の設定方法について説明します。"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: a24b2a5a196e633379b696ee82e9428dd67454ca
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2017
---
# <a name="step-6-set-up-the-pam-trust"></a>手順 6. PAM 信頼のセットアップ

>[!div class="step-by-step"]
[« 手順 5](sp1-step5-configuring-pam.md)
[手順 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)

**PRIV のみの環境に必須ではありません。** MIMAdmin アカウントを使用して PAMServer にログインします。

1. MIMAdmin アカウントを使用して PAMServer にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. メニュー オプション 6 (PAM の信頼のセットアップ) を選択

  要求された場合は、CORP 管理者アカウントの資格情報を入力します。 資格情報を入力した後、信頼関係が確立され、構成が完了します。

>[!div class="step-by-step"]
[« 手順 5](sp1-step5-configuring-pam.md)
[手順 7 »](sp1-step7-setup-sidhistory-sidfiltering.md)
