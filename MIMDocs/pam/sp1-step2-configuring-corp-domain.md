---
title: "手順 2. CORP ドメインの構成"
description: "この記事では、CORP ドメインの構成に必要な 2 番目の手順について説明します。この手順では、CORPDC への sids.txt のコピー後にスクリプトを実行します。"
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
translationtype: Human Translation
ms.sourcegitcommit: f08b0197341351bd5f33552f26b96132b1356239
ms.openlocfilehash: 8480350f85b3543a32d4db3dbc6a388afcb16352


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



<!--HONumber=Jan17_HO2-->


