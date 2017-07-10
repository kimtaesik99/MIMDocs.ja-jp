---
title: "PAM 環境階層モデル | Microsoft Docs"
description: "リスクに対する脆弱性に基づいてシステムを分離する階層モデルについて説明します。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 4c3b43e50403890572e77773191a821cf247269c
ms.contentlocale: ja-jp
ms.lasthandoff: 07/10/2017


---

<a id="tier-model-for-partitioning-administrative-privileges" class="xliff"></a>
# 管理者特権のパーティション分割の階層モデル

今日の脅威に直面した環境では、攻撃者がシステムへのアクセス権を取得するかどうかは問題ではなく、いつそうなるかが問題です。 つまり、社内のセキュリティは強力な境界の防御と同様に重要です。 この記事では、危険度の高いゾーンから高い特権のアクティビティを隔離することによって、特権の昇格から保護するためのセキュリティ モデルについて説明します。 このモデルは、ベスト プラクティスとセキュリティの原則に従いながら、優れたユーザー エクスペリエンスを実現します。

<a id="elevation-of-privilege-in-active-directory-forests" class="xliff"></a>
## Active Directory フォレストにおける特権の昇格

Windows Server Active Directory (AD) フォレストに対して永続的な管理者特権を与えられているユーザー、サービス、アプリケーションのアカウントにより、組織の任務と業務に多大なリスクが発生します。 これらのアカウントは攻撃者のターゲットとされることがあり、侵害されると、攻撃者はドメイン内の他のサーバーやアプリケーションに接続する権限を持つことになります。

階層モデルは、管理するリソースに基づいて管理者の間に区分を設けます。 ユーザー ワークステーションを制御する管理者は、アプリケーションを制御する管理者やエンタープライズ ID を管理する管理者から分離されています。 このモデルについては、「[Securing privileged access reference material (特権を持つアクセスのセキュリティ強化に関する参考資料)](http://aka.ms/tiermodel)」をご覧ください。

<a id="restricting-credential-exposure-with-logon-restrictions" class="xliff"></a>
## ログオン制限による資格情報の公開の制限

管理者アカウントの資格情報が盗難されるリスクを軽減するには、一般に管理作業を変更して、攻撃者への公開を制限する必要があります。 最初の手順として、組織は次を実行することをお勧めします。

- 管理者の資格情報が公開されるホストの数を制限する。
- ロールの権限を最低限必要なものに制限する。
- 標準ユーザーのアクティビティ (電子メールや Web 閲覧など) に使用されるホストで、管理タスクが実行されないようにする。

次の手順は、ログオン制限を実装し、プロセスと作業が階層モデル要件を遵守できるようにすることです。 理想的には、資格情報の公開を各階層内のロールに最低限必要な権限に縮小する必要もあります。

高い特権を持つアカウントにセキュリティ レベルの低いリソースへのアクセス権を付与しないようにするには、ログオン制限を適用する必要があります。 たとえば、

- ドメイン管理者 (階層 0) は、エンタープライズ サーバー (階層 1 ) と標準ユーザー ワークステーション (階層 2) にログオンできません。
- サーバー管理者 (階層 1) は、標準ユーザー ワークステーション (階層 2) にログオンできません。

>[!NOTE]
> サーバー管理者はドメイン管理者グループに属していてはなりません。 ドメイン コントローラーとエンタープライズ サーバーの両方を管理する責任を負うユーザーは、個別のアカウントが必要です。

ログオン制限は次によって適用することができます。

- 次を含むグループ ポリシー ログオン権限の制限:  
    - ネットワークからのこのコンピュータへのアクセスを拒否する  
    - バッチ ジョブとしてのログオンを拒否する  
    - サービスとしてのログオンを拒否する  
    - ローカルでのログオンを拒否する  
    - リモート デスクトップ設定によるログオンを拒否する  
- Windows Server 2012 以降を使用している場合は、認証ポリシーとサイロ
- アカウントが専用管理者フォレスト内にある場合、認証の選択

次の記事「[Planning a bastion environment (要塞環境の計画)](planning-bastion-environment.md)」では、Microsoft Identity Manager に専用管理フォレストを追加し、管理者アカウントを確立する方法について説明します。

