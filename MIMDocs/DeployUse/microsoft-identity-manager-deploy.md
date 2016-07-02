---
title: "MIM 2016 の展開 |Microsoft Identity Manager"
description: "Microsoft Identity Manager 2016 を展開するためのすべての手順を、ポータルを構成する環境を準備するところから説明します。"
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ca7fdef81eb8a68aff46df528e1989f019f5d2a4
ms.openlocfilehash: a56ead9777f1dad1aa0d214a506cf1242f51e167


---

# MIM 2016 の展開
このセクションの記事では、これまでに FIM または MIM が展開されていない新規のサーバー上に、エンド ユーザー セルフ サービス シナリオのために Microsoft Identity Manager (MIM) 2016 を展開するための手順を提供します。

> [!NOTE]
> このセクションで説明する展開トポロジは、MIM 初心者の学習のみを目的としています。  運用展開のトポロジの詳細については、[容量計画ガイド](/microsoft-identity-manager/plan-design/capacity-planning-guide)を参照してください。  運用規模または運用用途の場合、MIM を展開する前にこのドキュメントを確認することをお勧めします。

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## 1: ドメインを準備する
MIM は Active Directory (AD) と連動するため、次の手順に従って AD ドメイン コントローラーを構成します。
- [ドメインのセットアップ](preparing-domain.md)

## 2: ID 管理サーバーを準備する
自分のドメインを配置して構成したら、会社の ID 管理サーバーを準備します。 これには、次のセットアップが含まれます。
- [Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [SQL Server 2014](prepare-server-sql2014.md)
- [SharePoint](prepare-server-sharepoint.md)
- [Exchange Server](prepare-server-exchange.md) (オプション)

## 3: Microsoft Identity Manager 2016 コンポーネントをインストールする
ドメインとサーバーをセットアップしたら、MIM コンポーネントをインストールし、AD と同期するように構成することができます。
- [MIM 同期サービス](install-mim-sync.md)
- [MIM サービスおよびポータル](install-mim-service-portal.md)
- [Active Directory と MIM サービス データベースを同期する](install-mim-sync-ad-service.md)



<!--HONumber=Jun16_HO4-->


