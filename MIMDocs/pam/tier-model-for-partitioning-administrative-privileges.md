---
title: "PAM 環境階層モデル | Microsoft Docs"
description: "リスクに対する脆弱性に基づいてシステムを分離する階層モデルについて説明します。"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/30/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b6598857d5704accbee461366838bb8efb9b2fc0
ms.sourcegitcommit: c049dceaf02ab8b6008fe440daae4d07b752ca2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2017
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>管理者特権のパーティション分割の階層モデル

この記事では、危険度の高いゾーンから高い特権のアクティビティを隔離することによって、特権の昇格から保護するためのセキュリティ モデルについて説明します。 このモデルは、ベスト プラクティスとセキュリティの原則に従いながら、優れたユーザー エクスペリエンスを実現します。

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Active Directory フォレストにおける特権の昇格

Windows Server Active Directory (AD) フォレストに対して永続的な管理者特権を与えられているユーザー、サービス、アプリケーションのアカウントにより、組織の任務と業務に多大なリスクが発生します。 これらのアカウントは、侵害に成功した場合、ドメイン内の他のサーバーやアプリケーションに接続する権利を持つことになるため、攻撃者の標的となることがよくあります。

階層モデルは、管理するリソースに基づいて管理者の間に区分を設けます。 ユーザー ワークステーションを制御する管理者は、アプリケーションを制御する管理者やエンタープライズ ID を管理する管理者から分離されています。 このモデルについては、「[Securing privileged access reference material (特権を持つアクセスのセキュリティ強化に関する参考資料)](http://aka.ms/tiermodel)」をご覧ください。

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>ログオン制限による資格情報の公開の制限

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
    - ネットワークからのこのコンピューターへのアクセスを拒否する
    - バッチ ジョブとしてのログオンを拒否する
    - サービスとしてのログオンを拒否する
    - ローカルでのログオンを拒否する
    - リモート デスクトップ設定によるログオンを拒否する  
- Windows Server 2012 以降を使用している場合は、認証ポリシーとサイロ
- アカウントが専用管理者フォレスト内にある場合、認証の選択

## <a name="next-steps"></a>次のステップ

- 次の記事「[Planning a bastion environment (要塞環境の計画)](planning-bastion-environment.md)」では、Microsoft Identity Manager に専用管理フォレストを追加し、管理者アカウントを確立する方法について説明します。
- [特権アクセス ワークステーション](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)では、インターネットからの攻撃や脅威ベクターから保護される機密タスク用の専用のオペレーティング システムを利用できます。