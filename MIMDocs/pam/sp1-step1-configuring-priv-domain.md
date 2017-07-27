---
title: "手順 1. Priv ドメインの構成"
description: "スクリプトによって、Privileged Identity Manager で管理する既存の ID または新規の ID を使用して CORP ドメインを準備する"
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
ms.openlocfilehash: 24e91ed2f51206b03bec505fc0d28d25128d2c94
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="step-1-configuring-the-priv-domain"></a>手順 1. Priv ドメインの構成

>[!div class="step-by-step"]
[手順 2 »](sp1-step2-configuring-corp-domain.md)

1. 管理者として PRIVDC にログイン
  * PRIVOnly 環境の場合は、CORPDC にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. .\PAMDeployment.ps1
5. メニュー オプション 1 を選択 (PRIV フォレスト構成)


SQL/SharePoint および MIM の管理に必要なサービス アカウントは、ドメインにまだ存在しない場合には、自動的に作成されます。 スクリプトの実行中にこれらのサービス アカウントの作成に必要なパスワードを入力するように促されます。
PRIV ドメインが、Windows Server 2016 Technical Preview 5 に機能レベルを設定した Windows Server 2016 である場合、スクリプトによって、PAM が必要とするオプションの Active Directory "Privileged Access Management 機能" を有効にするよう求められます。 [はい] を確認して、先に進みます。
Windows Server 2016 よりも前の機能レベルの場合は、追加の構成が実行されないことを示す警告を無視してください。 管理者が Windows Server 2016 に機能レベルを上げた場合は、PAMDeployment.ps1 と PAM フォレスト構成を再実行する必要があります。

>[!NOTE]
>次の手順は、PRIVOnly 構成には必要ありません。

$env:SYSTEMDRIVE\PAM で生成される SIDs.txt を CORPDC の同じフォルダーにコピーします。 この操作は、CORP ユーザーのプロパティを読み取る PRIV ユーザーに対してアクセス許可を設定するために、CORPDC が必要とします。
スクリプトが完了すると、変更を有効にするためにコンピューターを再起動するように求められます。

>[!div class="step-by-step"]
[手順 2 »](sp1-step2-configuring-corp-domain.md)
