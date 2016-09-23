---
title: "MIM 2016 の展開 |Microsoft Identity Manager"
description: "Microsoft Identity Manager 2016 を展開するためのすべての手順を、ポータルを構成する環境を準備するところから説明します。"
keywords: 
author: kgremban
manager: femila
ms.date: 09/07/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: fa0af422-b5e9-4599-9d9b-cb6c18ea07f9
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 40dbec941eb2f0b1a01de0f47d44e01717aaca21
ms.openlocfilehash: 77dae279f9078c55abf342a8956aaf77c62773d5


---

# MIM 2016 の展開
このセクションの記事では、これまでに FIM または MIM が展開されていない新規のサーバー上に、エンド ユーザー セルフ サービス シナリオのために Microsoft Identity Manager (MIM) 2016 を展開するための手順を提供します。

> [!NOTE]
> このセクションで説明する展開トポロジは、MIM 初心者の学習のみを目的としています。  運用展開のトポロジの詳細については、[容量計画ガイド](/microsoft-identity-manager/plan-design/capacity-planning-guide)を参照してください。  運用規模または運用用途の場合、MIM を展開する前にこのドキュメントを確認することをお勧めします。

Privileged Access Management のシナリオは、専用拠点のフォレスト環境が必要な点が、他の MIM シナリオとは異なる展開方法です。  Privileged Identity Management 用に MIM を展開する方法については、「[Privileged Access Management の MIM 環境を構成する](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam)」をご覧ください。

MIM 2016 を展開するためのプロセスは、その前身である FIM 2010 R2 のプロセスによく似ています。 FIM のドキュメントを参照する必要がある場合は、「[Forefront Identity Manager 2010 R2 Deployment Guide](https://technet.microsoft.com/library/jj134310)」(Forefront Identity Manager 2010 R2 展開ガイド) をご覧ください。

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



<!--HONumber=Sep16_HO2-->


