---
title: "MIM2016 SP1 PAM 展開スクリプト"
description: "このページは、スクリプトを使用した Privileged Identity Manager の構成に関するシリーズ記事の一部です。 環境の前提条件の一覧が記載されています。"
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 12c60e12dc5662ff0313e21bb9180b3709969af6
ms.contentlocale: ja-jp
ms.lasthandoff: 07/10/2017


---

<a id="mim2016-sp1-pam-deployment-scripts" class="xliff"></a>
# MIM2016 SP1 PAM 展開スクリプト

この Service Pack には、PAM の展開を容易にする一連の展開スクリプトが導入されています。 これらのスクリプトは、ダウンロード センターで入手できます。 スクリプトを使用する前に、次の前提条件が使用中の環境に当てはまることを確認してください。

重要な前提条件:
1. すべてのコンピューターのオペレーティング システムが、Windows Server 2012 R2 以上です。 Windows Server 2016 Technical Preview 5 を試す場合は、TP5 ビルドを使用して PRIV ドメイン コント ローラーをインストールする必要があります。
2. DNS は、ドメイン コントローラーとコンポーネントのサーバー間での名前解決が自動となるように構成する必要があります。
3. インストール バイナリは、SQL、SharePoint、および MIM のインストールで指定したサーバーのローカルで使用可能にする必要があります。
4. この環境には、次の 3 つ専用 (物理または仮想) コンピューターがあり、別々に CORPDC、PRIVDC、PAMSERVER を実行しています。
5. 検証オプション用に、この手順を実行する専用のクライアント コンピューターが存在するものとします。

>[!NOTE]
>スクリプトの実行に関する問題が発生した場合は、ログの確認が必要になる場合があります。 すべてのスクリプト ログは、%AppData%\MIMPAMInstall に保存されます。 フォルダーを Zip ファイルに圧縮して、操作およびエラーの詳細と共に電子メールで mim2016@microsoft.com に送信してください。

PAM 展開スクリプトを使用する準備ができたら、 「[スクリプトを使用した PAM の構成](./pam/sp1-pam-configure-using-scripts.md)」に進んでください。

