---
title: "手順 2. CORP ドメインの構成"
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
ms.openlocfilehash: b7acc475c505b29559c510fd3fa350fed1c3157b


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



<!--HONumber=Nov16_HO2-->


