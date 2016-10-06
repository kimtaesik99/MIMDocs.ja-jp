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
ms.sourcegitcommit: 689c2ef0e4e4a681a398ba7e94fb3def525937ea
ms.openlocfilehash: 4c5cfa92f3111a6d298f586ba547a1eca2502853


---

# セットアップ SID 履歴/SID フィルター

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



<!--HONumber=Sep16_HO4-->


