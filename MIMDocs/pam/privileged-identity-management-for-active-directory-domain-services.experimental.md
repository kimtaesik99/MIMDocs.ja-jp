---
title: "ADDS の PAM の概要 | Microsoft Identity Manager"
description: "Privileged Access Management について説明すると共に、Active Directory 環境を管理および保護する場合に、これがどのように役に立つのかを説明します。"
keywords: 
author: kgremban
manager: femila
ms.date: 07/27/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
experiment_id: kgremban_images
translationtype: Human Translation
ms.sourcegitcommit: e695dd47e4bd31c4004c7d0d9ec76498d52fb56a
ms.openlocfilehash: 82c97351f66558c3270821f786560ef4b3e0c473

---

# Active Directory ドメイン サービスの Privileged Access Management
Privileged Access Management (PAM) は、組織の既存の Active Directory 環境内で特権アクセスを制限するのに役立ちます。

![PAM の手順: 準備、保護、運用、監視 - 図](media/MIM_PIM_SetupProcess.png)

環境の準備、保護、運用、監視というサイクルに焦点を当てて、Privileged Access Management は次の 2 つの目標を達成します。

- 悪意のある攻撃の影響を受けていないと認識されている別の要塞環境を維持することにより、侵害された Active Directory 環境の制御を再び確立します。  
- 特権アカウントの使用を分離して、これらの資格情報が盗まれるリスクを低減します。

> [!NOTE]
> PAM は、Microsoft Identity Manager (MIM) を使用して実装される [Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/) (PIM) のインスタンスです。

## PAM で解決できる問題
現在、企業は Active Directory 環境内のリソースへのアクセスを大変懸念しています。 特に問題とされるのは、脆弱性に関するニュース、承認されていない特権のエスカレーション、および Pass-the-Hash、Pass-the-Ticket、スピア フィッシング、Kerberos 侵害などを含む他の種類の不正なアクセスです。

今日では、攻撃者はいとも簡単に Domain Admins アカウントの資格情報を取得でき、侵害されてから攻撃を検出することは非常に困難です。 PAM の目的は、悪意のあるユーザーがアクセスできる機会を削減しながら、環境の制御と認識を向上させることです。

PAM は攻撃者がネットワークに侵入して特権アカウント アクセスを取得するのを困難にします。 PAM は、ドメインに参加しているコンピューターおよびそのコンピューター上のアプリケーションのアクセスを制御する特権グループに対する保護を強化します。 また、監視、可視性、およびきめ細かい制御を強化することで、特権管理者が誰で何をしているかを組織が認識できるようにします。 PAM により、組織は管理アカウントが環境内でどのように使用されているかを詳細に知ることができます。

## PAM の設定方法
PAM は Just-in-Time 管理の原則に基づいており、[Just Enough Administration (JEA)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362) に関連しています。 JEA は Windows PowerShell のツールキットであり、特権アクティビティを実行するためのコマンドのセットと、管理者がこれらのコマンドを実行するための権限を取得できるエンドポイントが定義されています。 JEA では、管理者は、特定の権限を持つユーザーが特定のタスクを実行できることを判断します。 そのタスクを実行する必要があるたびに、対象となるユーザーはそのアクセス許可を有効にします。 特定の期間の経過後、アクセス許可の有効期限が切れるため、ユーザーがアクセス権を盗むことはできません。

PAM のセットアップと操作は 4 つの手順で行います。


1.  **準備**:既存のフォレスト内で重要な特権を持つグループを識別します。 要塞フォレスト内のメンバーを含めずにこれらのグループを再作成します。

2.  **保護**: ユーザーが Just-in-Time 管理を要求したときのライフサイクル、および多要素認証 (MFA) などの認証保護を設定します。 MFA は、悪意のあるソフトウェアからのプログラムによる攻撃またはそれに続く資格情報の盗難を防ぐのに役立ちます。

3.  **運用**:認証要件が満たされて要求が承認された後、ユーザー アカウントが要塞フォレスト内の特権グループに一時的に追加されます。 事前に設定された期間、管理者はそのグループに割り当てられているすべての特権およびアクセス許可を持ちます。 この期間が経過すると、アカウントはグループから削除されます。

4.  **監視**:PAM は、特権アクセス要求の監査、アラート、レポートを追加します。 特権アクセスの履歴や、アクティビティを実行したユーザーを確認できます。 アクティビティが有効かどうかを判断することができ、元のフォレストの特権グループに直接ユーザーを追加する試みなど、承認されていないアクティビティを簡単に識別できます。 この手順は、悪意のあるソフトウェアを識別するだけでなく、「内部」の攻撃者の追跡のためにも重要です。

## PAM のしくみ
PAM は、AD DS の新機能 (特にドメイン アカウントの認証と承認について)、および Microsoft Identity Manager の新機能に基づいています。 PAM は、既存の Active Directory 環境から特権アカウントを分離します。 特権アカウントを使用する必要があるを場合は、最初に要求して、承認される必要があります。 承認後、特権アカウントには、ユーザーまたはアプリケーションの現在のフォレストではなく、新しい要塞フォレストの外部プリンシパル グループによってアクセス許可が与えられます。 要塞フォレストを使用することで、組織は、ユーザーが特権グループのメンバーになれるときや、ユーザーが認証を必要とする方法など、制御範囲が広がります。

Active Directory、MIM サービス、およびこのソリューションの他の部分は、高可用性構成にも展開できます。

次の例では、さらに詳しく PIM のしくみを説明します。

![PIM のプロセスと参加者 - 図](media/MIM_PIM_howitworks.png)

要塞フォレストは、時間制限のあるグループのメンバーシップを発行し、これによってさらに時間制限付きのチケット保証チケット (TGT) を生成します。 要塞フォレストを信頼するフォレスト内にアプリとサービスが存在する場合、Kerberos ベースのアプリケーションまたはサービスはこれらの TGT を優先して実施できます。

日常的に使用するユーザー アカウントは、新しいフォレストに移動する必要はありません。 コンピューター、アプリケーション、およびそれらのグループにも同じことが当てはまります。 既存フォレストの現在の場所のままです。 このような現在のサイバーセキュリティ問題に関係していて、しかしサーバー インフラストラクチャを Windows Server の次のバージョンにアップグレードするプランを持たない組織の例を検討します。 その組織では、MIM と新しい要塞フォレストを使用してこの結合されたソリューションをまだ利用でき、既存リソースへのアクセスをより細かく制御できます。

PAM には次のような利点があります。

-   **特権の分離とスコープ**: ユーザーは、電子メールの確認またはインターネットの閲覧などの特権のないタスクにも使用されるアカウントに特権を保持しません。 ユーザーは、特権を要求する必要があります。 要求は、PAM 管理者によって定義された MIM ポリシーに基づいて承認または拒否されます。 要求が承認されるまで、特権アクセスは使用できません。

-   **ステップアップおよびプルーフアップ**: これらは、個別の管理アカウントのライフサイクルの管理に役立つ新しい認証および承認のチャレンジです。 ユーザーは管理アカウントの昇格を要求でき、その要求が MIM ワークフローを通過します。

-   **追加のログ記録**: 組み込みの MIM ワークフローと共に、要求、承認された方法、および承認後に発生するイベントを識別する PAM の追加ログがあります。

-   **カスタマイズ可能なワークフロー**:MIM ワークフローはさまざまなシナリオに構成することができ、要求元ユーザーまたは要求された役割のパラメーターに基づいて、複数のワークフローを使用できます。

## ユーザーが特権アクセスを要求する方法
次のようなさまざまな方法で、ユーザーは要求を送信できます。  
- MIM サービスの Web サービス API  
- REST エンドポイント  
- Windows PowerShell (`New-PAMRequest`)

## 使用できるワークフローと監視オプション
たとえば、ユーザーは PIM がセットアップされる前に管理グループのメンバーであったものとします。 PIM セットアップの一部として、ユーザーは管理グループから削除され、MIM にポリシーが作成されます。 ポリシーでは、そのユーザーが管理特権を要求し、MFA によって認証された場合、要求が承認され、ユーザー用の独立したアカウントが要塞フォレストの特権グループに追加されます。

要求が承認されると、アクション ワークフローは要塞フォレストの Active Directory と直接通信し、ユーザーをグループに配置します。 たとえば、Jen が管理者に HR データベースを要求すると、Jen の管理アカウントが数秒以内に要塞フォレストの特権グループに追加されます。 そのグループ内の管理アカウントのメンバーシップは、制限時間を過ぎると期限切れになります。 Windows Server Technical Preview では、そのメンバーシップは Active directory において制限時間に関連付けられます。要塞フォレストの Windows Server 2012 R2 では、その制限時間は MIM によって強制されます。

> [!NOTE]
> 新しいメンバーをグループに追加するときは、要塞フォレスト内の他のドメイン コントローラー (DC) に変更をレプリケートする必要があります。 レプリケーションの潜在期間は、ユーザーがリソースにアクセスする機能に影響します。 レプリケーションの潜在期間の詳細については、「[Active Directory レプリケーション トポロジの機能](https://technet.microsoft.com/library/cc755994.aspx)」を参照してください。
>
> これに対し、有効期限が切れたリンクはセキュリティ アカウント マネージャー (SAM) によってリアルタイムで評価されます。 グループ メンバーの追加はアクセス要求を受信する DC によってレプリケートされる必要がありますが、グループ メンバーの削除はどの DC 上でも即座に評価されます。

このワークフローは、特にこれらの管理アカウントを目的としたものです。 特権グループにアクセスする必要が頻繁ではない管理者 (またはスクリプト) は、そのアクセスを正確に要求できます。 MIM は要求および Active Directory での変更をログに記録し、管理者はそれをイベント ビューアーで確認したり、System Center 2012 - Operations Manager 監査コレクション サービス (ACS) や他のサードパーティ製のツールなどのエンタープライズ監視ソリューションにデータを送信したりできます。



<!--HONumber=Jul16_HO4-->

