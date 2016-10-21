---
title: "手順 7. セットアップ SID 履歴/SID フィルター"
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
ms.sourcegitcommit: bc56b57a06592527bab13aad879ca13466e968b3
ms.openlocfilehash: 4ad5d755de100ad598c6cebd209296edd361e8c5


---

# 手順 7. SID 履歴/SID フィルターのセットアップ

**PRIV のみの環境に必須ではありません。** MIMAdmin アカウントを使用して PAMServer にログインします。

1. 管理者として CORP DC にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. メニュー オプション 8 を選択 (セットアップ SID 履歴/SID フィルター)

正常に実行されると、次のメッセージが表示されます。<br/></br>
SID フィルターの場合: <br/></br>
"信頼で SID をフィルターしないよう設定しています。" または "SID フィルターはこの信頼に対して有効になっていません。" </br></br>
SID 履歴の場合: </br></br>
"この信頼に対して SID 履歴を有効にしています。" または "SID 履歴はこの信頼に対して既に有効になっています。"

>[!div class="step-by-step"]
[« 手順 6](sp1-step6-setup-pam-trust.md)
[手順 8 »](sp1-step8-pam-deployment-verification.md)



<!--HONumber=Oct16_HO1-->


