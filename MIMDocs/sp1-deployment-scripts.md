---
title: "MIM2016 SP1 PAM 展開スクリプト"
description: "このページは、スクリプトを使用した Privileged Identity Manager の構成に関するシリーズ記事の一部です。 環境の前提条件の一覧が記載されています。"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: ae8f6a87f57c95e073b40d3cda944c71f1bf7247
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2017
---
# <a name="mim2016-sp1-pam-deployment-scripts"></a>MIM2016 SP1 PAM 展開スクリプト

この Service Pack には、PAM の展開を容易にする一連の展開スクリプトが導入されています。 これらのスクリプトは、ダウンロード センターで入手できます。 スクリプトを使用する前に、次の前提条件が使用中の環境に当てはまることを確認してください。

重要な前提条件:
1. すべてのコンピューターのオペレーティング システムが、Windows Server 2012 R2 以上です。 Windows Server 2016 Technical Preview 5 を試す場合は、TP5 ビルドを使用して PRIV ドメイン コント ローラーをインストールする必要があります。
2. DNS は、ドメイン コントローラーとコンポーネントのサーバー間での名前解決が自動となるように構成する必要があります。
3. インストール バイナリは、SQL、SharePoint、および MIM のインストールで指定したサーバーのローカルで使用可能にする必要があります。
4. この環境には、次の 3 つ専用 (物理または仮想) コンピューターがあり、別々に CORPDC、PRIVDC、PAMSERVER を実行しています。
5. 検証オプション用に、この手順を実行する専用のクライアント コンピューターが存在するものとします。

>[!NOTE]
>スクリプトの実行に関する問題が発生した場合は、ログの確認が必要になる場合があります。 すべてのスクリプト ログは、%AppData%\MIMPAMInstall に保存されます。 フォルダーを Zip ファイルに圧縮して、操作およびエラーの詳細と共に、お客様のサポート ケースに追加してください。

PAM 展開スクリプトを使用する準備ができたら、 「[スクリプトを使用した PAM の構成](./pam/sp1-pam-configure-using-scripts.md)」に進んでください。
