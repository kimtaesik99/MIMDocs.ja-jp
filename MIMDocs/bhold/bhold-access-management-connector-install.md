---
title: "BHOLD Access Management Connector のインストール | Microsoft Docs"
description: "BHOLD コネクタ モジュールは、データの初期同期と実行中同期をサポートします"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
# <a name="access-management-connector-installation"></a>Access Management Connector のインストール

BHOLD Suite の Access Management Connector モジュールは、BHOLD へのデータの初期同期と実行中同期の両方をサポートします。 Access Management Connector は Microsoft Identity Manager (MIM) 同期サービスと連携して、BHOLD Core データベース、FIM 2010 メタバース、ターゲット アプリケーションと ID ストアの間でデータを移動します。 Access Management Connector モジュールをインストールしたら、BHOLD と MIM の間のデータ フローを制御する FIM 管理エージェントを作成できます。

## <a name="access-management-connector-software-requirements"></a>Access Management Connector のソフトウェア要件

Access Management Connector モジュールをインストールする前に、Microsoft .NET Framework 4 をインストールする必要があります。 .NET Framework 4 およびインストール手順の詳細については、[Microsoft .NET のホーム ページ](http://www.microsoft.com/net)を参照してください。
Access Management Connector は、MIM の FIM 同期サービスが実行されているコンピューターにインストールする必要があります。

## <a name="access-management-connector-setup"></a>Access Management Connector のセットアップ

Access Management Control モジュールをインストールするには、Domain Admins グループのメンバーとしてログオンし、次のファイルをダウンロードして、BHOLD FIM 統合モジュールをインストールするサーバーで管理者として実行します。

- AccessManagementConnector.msi

管理者としてプログラム ファイルを実行するには、そのファイルを右クリックし、**[管理者として実行]** をクリックします。

## <a name="next-steps"></a>次のステップ

- [BHOLD FIM 統合のインストール](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) ロールのエンドユーザー セルフサービスを有効にするために、BHOLD FIM 統合モジュールをインストールします
- [BHOLD インストール ガイド](bhold-installation-guide.md)
- [BHOLD 開発者用リファレンス](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD のバージョン履歴](../reference/version-bhold-history.md)