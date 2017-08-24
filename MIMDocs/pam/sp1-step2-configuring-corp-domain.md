---
title: "手順 2. CORP ドメインの構成"
description: "この記事では、CORP ドメインの構成に必要な 2 番目の手順について説明します。この手順では、CORPDC への sids.txt のコピー後にスクリプトを実行します。"
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
ms.openlocfilehash: 53f39055f0af4f01b47bf789276092cd93f5c329
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2017
---
# <a name="step-2-configuring-the-corp-domain"></a>手順 2. CORP ドメインの構成

>[!div class="step-by-step"]
[<< 手順 1](sp1-step1-configuring-priv-domain.md)
[手順 3 >>](sp1-step3-installing-configuring-sql.md)

SIDs.txt を CORPDC にコピーした場合は、**PRIVOnly の展開には不要**

1. 管理者として CORPDC にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. メニュー オプション 2 を選択 (CORP フォレスト構成)

>[!div class="step-by-step"]
[<< 手順 1](sp1-step1-configuring-priv-domain.md)
[手順 3 >>](sp1-step3-installing-configuring-sql.md)
