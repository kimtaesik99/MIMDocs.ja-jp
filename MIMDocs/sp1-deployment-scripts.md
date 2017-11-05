---
title: "MIM2016 SP1 PAM 展開スクリプト"
description: "このページは、スクリプトを使用した Privileged Identity Manager の構成に関するシリーズ記事の一部です。 環境の前提条件の一覧が記載されています。"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/17/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 77a222c0a36f4e244a5114eddfc0edadb168d1cd
ms.sourcegitcommit: 06add1a636720f74bc0c0f25b4100b19f1bd31da
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM 展開スクリプト

この Service Pack には、PAM の展開を容易にする一連の展開スクリプトが導入されています。 これらのスクリプトは、ダウンロード センターで入手できます。 スクリプトの使用を試みる前に、次の要件が満たされていることを確認してください。

1. すべてのサーバーのオペレーティング システムが、Windows Server 2012 R2 以上である必要があります。
2. ドメイン コントローラーとコンポーネント サーバーの間の名前解決が有効であるように、DNS が構成されている必要があります。
3. インストール バイナリは、SQL、SharePoint、および MIM のインストールで指定したサーバーのローカルで使用可能にする必要があります。
4. この環境には、次の 3 つ専用 (物理または仮想) コンピューターがあり、別々に CORPDC、PRIVDC、PAMSERVER を実行しています。
5. 検証オプションの場合は、専用のワークステーションが必要です。

>[!NOTE]
>スクリプトの実行に関する問題が発生した場合は、ログの確認が必要になる場合があります。 すべてのスクリプト ログは、%AppData%\MIMPAMInstall に保存されます。 フォルダーを Zip ファイルに圧縮して、操作およびエラーの詳細と共に、お客様のサポート ケースに追加してください。

PAM 展開スクリプトを使用する準備ができたら、 「[スクリプトを使用した PAM の構成](./pam/sp1-pam-configure-using-scripts.md)」に進んでください。
