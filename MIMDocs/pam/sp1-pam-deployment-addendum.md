---
title: "補遺"
description: "本記事は、スクリプトを使用した PAM の展開に関するドキュメントの補遺です。 PRIV ドメインと CORP ドメインの構成、検証時のクライアントのセットアップ、およびサポートの要請方法の詳細について説明します。"
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
ms.openlocfilehash: f69fe68dc63323c0945a4902e34ea8153f938c02
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="pam-deployment-scripts-addendum"></a>PAM 展開スクリプトの補遺:

## <a name="addendum-1-setting-up-the-priv-domain"></a>補遺 1 PRIV ドメインのセットアップ

圧縮ファイルを $env:SYSTEMDRIVE\PAM に解凍した後、PRIV フォレストの詳細を指定するよう PAMDeploymentConfig.xml を編集します。 DNSName、NetbiosName、DC 名、データベース/ログのパスと Sysvol のパスを更新してください。 Domain と ForestMode も更新してください。 Windows Server Technical Preview 5 をテストする場合は、DomainMode と ForestMode を WinThreshold に設定してください。

1. PRIV ドメイン管理者として DC にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. メニュー オプション 9 を選択 (Priv フォレストのセットアップ)


DC は、完了した後に自動的に再起動します。 ディレクトリ サービス復元モード (DSRM) の管理者パスワードは、次の条件に一致する必要があります。

  * パスワードの長さは最小で 15 文字である
  * パスワードには、小文字が 1 文字以上含まれている
  * パスワードには、大文字が 1 文字以上含まれている
  * パスワードには、数字または特殊文字が 1 文字以上含まれている

## <a name="addendum-2-setting-up-the-corp-domain"></a>補遺 2 CORP ドメインのセットアップ

PAM を使い始めたばかりでテスト環境をセットアップする場合、スクリプトでも CORP ドメインを構成できます。 圧縮ファイルを $env:SYSTEMDRIVE\PAM フォルダーに解凍した後、PAMDeploymentConfig.xml を編集し、CORP フォレストの詳細を追加します。 DNSName、NetbiosName、DC 名、データベース/ログのパス、および Sysvol のパスを更新します。 機能レベルが Windows Server 2012 R2 以上である必要があります。

1. 管理者として CORP ドメイン DC にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. import-module .\PAMDeployment.ps1
5. メニュー オプション 10 を選択 (CORP フォレストのセットアップ)

ドメイン コントローラーは、完了後に自動的に再起動します。

## <a name="addendum-3-setting-up-a-corp-client-to-do-the-validation"></a>補遺 3 CORP クライアントをセットアップし、検証を行う

構成ファイルの ClientBinaryLocation は、setup.exe が格納されている場所を指す必要があります。
ローカル管理者としてクライアントにログインし、管理者特権の PowerShell ウィンドウで次のコマンドを実行します。

1. cd $env:path: SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. メニュー オプション 7 を選択 (MIM PAM クライアント セットアップ)


マシンがドメインに参加していない場合は、ドメインへの参加を実行する CORP 管理者の資格情報を求められます。 ドメインへの参加後にコンピューターを再起動する必要があります。 ローカル管理者としてもう一度クライアントにログインし、管理者特権の PowerShell ウィンドウで次のコマンドを実行します。

1. cd $env:path: SYSTEMDRIVE\PAM
2. Import-module .\PAMDeployment.ps1
3. メニュー オプション 7 を選択 (MIM PAM クライアント セットアップ)

上の手順 8 に進みます。

## <a name="addendum-4-if-something-goes-wrong"></a>補遺 4 問題が発生した場合

すべてのスクリプト ログは、%AppData%\MIMPAMInstall に保存されます。 フォルダーを Zip ファイルに圧縮して、操作およびエラーの詳細と共に電子メールで [mim2016@microsoft.com](mailto:mim2016@microsoft.com) に送信してください。
