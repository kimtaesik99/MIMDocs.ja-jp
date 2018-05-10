---
title: Microsoft Identity Manager 2016 の展開に必要な手順 | Microsoft Docs
description: Microsoft Identity Manager 2016 を展開するためのすべての手順を、ポータルを構成する環境を準備するところから説明します。
keywords: ''
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3460436682054acf5e9e1b186c3fa39faaa40a43
ms.sourcegitcommit: 8316fa41f06f137dba0739a8700910116b5575d8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="deploy-microsoft-identity-manager-2016-sp1"></a>Microsoft Identity Manager 2016 SP1 の展開
このセクションの記事では、これまでに FIM または MIM が展開されていない新規のサーバー上に、エンド ユーザー セルフ サービス シナリオのために Microsoft Identity Manager (MIM) 2016 を展開するための手順を提供します。

> [!NOTE]
> このセクションで説明する展開トポロジは、MIM 初心者の学習のみを目的としています。  運用展開のトポロジの詳細については、[容量計画ガイド](capacity-planning-guide.md)を参照してください。  運用規模または運用用途の場合、MIM を展開する前にこのドキュメントを確認することをお勧めします。

Privileged Access Management のシナリオは、専用拠点のフォレスト環境が必要な点が、他の MIM シナリオとは異なる展開方法です。  Privileged Identity Management 用に MIM を展開する方法については、「[Privileged Access Management の MIM 環境を構成する](./pam/configuring-mim-environment-for-pam.md)」をご覧ください。

MIM を展開するためのプロセスは、その前身である FIM 2010 R2 のプロセスによく似ています。 FIM のドキュメントを参照する必要がある場合は、「[Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310)」(Forefront Identity Manager 2010 R2 展開ガイド) をご覧ください。

## <a name="first-prepare-a-domain"></a>1: ドメインを準備する
MIM は Active Directory (AD) と連動するため、次の手順に従って AD ドメイン コントローラーを構成します。
- [ドメインのセットアップ](preparing-domain.md)

## <a name="next-prepare-an-identity-management-servers"></a>次: ID 管理サーバーを準備する
自分のドメインを配置して構成したら、会社の ID 管理サーバーを準備します。 これには、次のセットアップが含まれます。
- [Windows Server 2016](prepare-server-ws2016.md)
- [SQL Server 2016](prepare-server-sql2016.md)
- [SharePoint 2016](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (オプション)

## <a name="finally-install-microsoft-identity-manager-2016-sp1-components"></a>最後: Microsoft Identity Manager 2016 SP1 コンポーネントをインストールする
ドメインとサーバーをセットアップしたら、MIM コンポーネントをインストールし、AD と同期するように構成することができます。
- [MIM 同期サービス](install-mim-sync.md)
- [MIM サービスおよびポータル](install-mim-service-portal.md)
- [Active Directory と MIM サービス データベースを同期する](install-mim-sync-ad-service.md)
- [MIM 2016 以降でサポートされるプラットフォーム](microsoft-identity-manager-2016-supported-platforms.md)
