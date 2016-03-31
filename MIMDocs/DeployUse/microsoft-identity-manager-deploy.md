---
title: MIM を展開する |Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
author: kgremban
---
# MIM の展開
このセクションの記事では、エンド ユーザー セルフ サービス シナリオ FIM または MIM を以前に展開されていない新規のサーバー上の Microsoft Identity Manager (MIM) 2016年を展開するための手順を提供します。

> [!NOTE]
> このセクションで説明する展開トポロジは、MIM 初心者の学習のみを目的としています。   [容量計画ガイド](/MIM/PlanDesign/capacity-planning-guide.html) 運用環境の配置トポロジについて詳しくを説明します。  運用規模または運用用途の場合、MIM を展開する前にこのドキュメントを確認することをお勧めします。

<!---
Comment: Restore after PAM content is included

The privileged access management scenario is deployed differently than other MIM scenarios, as it requires a dedicated bastion forest environment.  If you want to learn more about deploying MIM for Privileged Identity Management, see [Getting Started with Privileged Access Management](privileged-access-management-get-started.md).
--->

## 1: ドメインを準備します。
MIM は Active Directory (AD) で、次の手順を AD ドメイン コント ローラーを構成するために従います。
- [ドメインを準備します。](preparing-domain.md)

## Id 管理サーバーを準備しています。
自分のドメインが存在し、構成されているとは、企業 id 管理サーバーを準備します。 これにより、設定が含まれます。
- [Id 管理サーバーの準備: Windows Server 2012 R2](prepare-server-ws2012r2.md)
- [Id 管理サーバーを準備する: SQL Server 2014](prepare-server-sql2014.md)
- [Id 管理サーバーの準備をしています: SharePoint](prepare-server-sharepoint.md)
- [Id 管理サーバーの準備をしています: Exchange](prepare-server-exchange.md) (省略可能)

## 最後に: インストールの Microsoft Identity Manager 2016 コンポーネント
ドメインとサーバーのセットアップ時に後、は、MIM コンポーネントをインストールし、AD と同期するように構成する準備ができたらします。
- [MIM 同期サービスのインストールを実行する MIM 2016:](install-mim-sync.md)
- [インストールを実行する MIM 2016: MIM サービスおよびポータル](install-mim-service-portal.md)
- [Active Directory と MIM サービスに同期 MIM 2016 をインストールするには。](install-mim-sync-ad-service.md)


<!--HONumber=Mar16_HO3-->


