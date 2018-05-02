---
title: Microsoft Graph 用の Microsoft Identity Manager 管理エージェント | Microsoft Docs
author: fimguy
description: Microsoft Graph (プレビュー) は、外部ユーザーの AD アカウントのライフサイクル管理です。 このシナリオでは、組織は Azure AD ディレクトリにゲストを招待し、オンプレミスの Windows 統合認証ベースまたは Kerberos ベースのアプリケーションへのアクセス権を、これらのゲストに付与しようと考えています
keywords: ''
ms.author: davidste
manager: bhu
ms.date: 04/25/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 77d322f447546897ad18f0981e5faad12efafef1
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
<a name="azure-ad-business-to-business-b2b-collaboration-with-microsoft-identity-managermim-2016-sp1-with-azure-application-proxy-public-preview"></a>Microsoft Identity Manager(MIM) 2016 SP1 と Azure アプリケーション プロキシ (パブリック プレビュー) による Azure AD のビジネス間 (B2B) コラボレーション
============================================================================================================================

プレビュー対象の最初のシナリオは、外部ユーザーの AD アカウントのライフサイクル管理です。   このシナリオでは、組織は Azure AD ディレクトリにゲストを招待し、[Azure AD アプリケーション](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) プロキシまたは他のゲートウェイ メカニズムを介して、オンプレミスの Windows 統合認証ベースまたは Kerberos ベースのアプリケーションへのアクセス権を、これらのゲストに付与しようと考えています。 Azure AD アプリケーション プロキシを利用するには、識別と委任のために、各ユーザーが専用の AD DS アカウントを持っている必要があります

## <a name="scenario-specific-supported-guidance"></a>シナリオ固有のサポートされるガイダンス

このシナリオでは、組織は Azure AD ディレクトリにゲストを招待し、オンプレミスの Windows  統合認証ベースまたは Kerberos ベースのアプリケーションへのアクセス権を、[Azure AD アプリケーション](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) プロキシまたは他のゲートウェイ メカニズムを利用してこれらのゲストに付与しようと考えています。 Azure AD アプリケーション プロキシを利用するには、識別と委任のために、各ユーザーが専用の AD DS アカウントを持っている必要があります

MIM と Azure アプリケーション プロキシによる B2B の構成に関するいくつかの前提条件

-   [Graph 管理エージェント](microsoft-identity-manager-2016-connector-graph.md)を既にインストールしてあること。

-   ユーザーとグループを Azure AD に同期するように、オンプレミスの AD と Azure AD Connect を設定してあること。

    -   [Azure AD Connect](http://robsgroupsblog.com/blog/how-to-write-back-an-office-group-in-azure-active-directory-to-a-mail-enabled-security-group-in-an-on-premises-active-directory) を使用してアプリケーション アクセスを制御する Office グループ

-   アプリケーション プロキシ コネクタとコネクタ グループを既に設定してあること。まだ設定していない場合は、[こちら](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-enable#install-and-register-a-connector)にアクセスしてインストールおよび構成できます

-   Windows 統合認証または Azure AD アプリ プロキシを介した個別の AD アカウントを利用するアプリケーションを公開してあること

-   Azure AD で作成されたゲストを招待してあるか、招待すること。<https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-self-service-portal>

-   Microsoft Identity Manager がインストールされ、サービスとポータルおよび Active Directory 管理エージェントの基本的な構成が行われていること。
    <https://docs.microsoft.com/en-us/microsoft-identity-manager/microsoft-identity-manager-deploy>

## <a name="b2b-end-to-end-deployment"></a>B2B のエンド ツー エンドの展開

通信の種類

Contoso Pharmaceuticals は、研究開発部門の一部として Trey Research Inc. に仕事を依頼しています。 Trey Research の従業員は、Contoso Pharmaceuticals によって提供される研究報告アプリケーションにアクセスする必要があります。

-   Contoso Pharmaceuticals は独立したテナント内にあり、カスタム ドメインを構成してあります。

-   Contoso Pharmaceuticals にテナント外部ユーザーを招待しています。
    このユーザーは招待を受け入れており、共有されているリソースにアクセスできます。

-   この例では、MIM プロセス (例のヘルプデスク シナリオ) に参加するためにゲスト ユーザー向けの MIM サービスおよびポータルを使用する例として、アプリ プロキシでアプリケーションを公開しています。

## <a name="create-the-graph-management-agent"></a>Graph 管理エージェントを作成する

注: コネクタを作成する前に、[Graph 管理エージェント](microsoft-identity-manager-2016-connector-graph.md)に関するページを確認してください。

Synchronization Service Manager の UI で、**[コネクタ]**、**[作成]** の順に選びます。
**[Graph (Microsoft)]\(Graph (Microsoft)\)** を選び、わかりやすい名前を付けます

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d95c6b2cc7951b607388cbd25920d7d0.png)

### <a name="connectivity"></a>接続状況

[接続] ページでは、Graph API のバージョンを指定します (運用環境では **[V 1.0]**、運用環境以外では **[ベータ]**)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/6fabfe20af0207f1556f0df18fd16f60.png)

### <a name="capabilities"></a>機能

[Global Parameters]\(グローバル パラメーター\) ページでは、差分変更ログの DN および追加 LDAP 機能を構成します。 LDAP サーバーによって提供される情報が事前に設定されています。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="global-parameters"></a>グローバル パラメーター

[Global Parameters]\(グローバル パラメーター\) ページでは、差分変更ログの DN および追加 LDAP 機能を構成します。 LDAP サーバーによって提供される情報が事前に設定されています。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/84c4dd62f63b82239cd0cf63d14fc671.png)

### <a name="configure-provisioning-hierarchy"></a>Configure Provisioning Hierarchy (プロビジョニング階層の構成)

このページでは、DN コンポーネント (OU など) をプロビジョニングするオブジェクト型 (organizationalUnit など) にマップします。 既定値のままにして [次へ] をクリックします

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80016dc45b50a0b1b08ea51ad8b37977.png)

### <a name="configure-partitions-and-hierarchies"></a>パーティションと階層を構成する

パーティションと階層のページでは、インポートおよびエクスポートする予定のオブジェクトを含むすべての名前空間を選びます。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/72f0adc789ed78c66d066768146fb874.png)

#### <a name="select-object-types"></a>オブジェクトの種類を選択する

パーティションと階層のページでは、インポートおよびエクスポートする予定のオブジェクトを含むすべての名前空間を選びます。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e18921f65a0d0e4acf0775c8a01ac009.png)

#### <a name="select-attributes"></a>属性を選択する

[Select Attributes]\(属性の選択\) 画面では、B2B ユーザーの管理に必要な属性を選びます。 属性 "id" は必須です

-   id

-   displayName

-   メール

-   givenName

-   surname

-   userPrincipalName

-   userType

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/58da80f5475cf01a97a6843dd279385c.png)

#### <a name="configure-anchors"></a>アンカーを構成する

アンカーの構成は必要な手順です。 既定では、マッピングに id 属性を使います。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9377ab7b760221517a431384689c8c76.png)

#### <a name="configure-connector-filter"></a>コネクタ フィルターを構成する

[Configure Connector Filter]\(コネクタ フィルターの構成\) ページでは、属性フィルターに基づいてオブジェクトを除外できます。 この B2B シナリオでは、userType がメンバーではなくゲストであるユーザーだけを取り込みます。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d90691fce652ba41c7a98c9a863ee710.png)

#### <a name="configure-join-and-projection-rules"></a>参加および保護ルールを構成する

参加とプロジェクションのルールの構成は、同期ルールによって処理されます。コネクタ自体では参加とプロジェクションを識別する必要はありません。 既定値をそのまま使用し、[OK] をクリックします。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/34896440ae6ad404e824eb35d8629986.png)

#### <a name="configure-attribute-flow"></a>属性フローを構成する

参加とプロジェクションと同様に、属性フローを定義する必要はありません。後で作成する同期ルールによって処理されます。 既定値をそのまま使用し、[OK] をクリックします。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b7cd0d294d4f361f0551bf2cb774d5f5.png)

#### <a name="configure-deprovision"></a>Configure Deprovision (プロビジョニング解除の構成)

プロビジョニングの解除を構成すると、メタバース オブジェクトが削除された場合に、オブジェクトを削除することができます。 このテストでは、目標は Azure にそれらを残しておくことであり、インポートだけで Azure には何もエクスポートしないので、ディスコネクタにします。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/2394ad4d11546c6a5c69a6dad56fe6ca.png)

#### <a name="configure-extensions"></a>拡張機能を構成する

この管理エージェントでの拡張機能の構成はオプションですが、同期ルールを使うので必要ありません。 前に属性フローで高度なルールを決定した場合は、オプションを定義します。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/74513d95b10f6ce47b7ac75fe7ab9889.png)

## <a name="creating-mim-service-synchronization-rules"></a>MIM サービス同期ルールの作成

次の手順では、B2B ゲスト アカウントと属性フローのマッピングを始めます。 前提条件として、Active Directory 管理エージェントの構成が済み、MIM サービスおよびポータルと対話するように FIM MA が構成されている必要があります。

同期ルールを作成する前に、MV デザイナーを使って、ユーザー オブジェクトに関連付けられた userPrincipalName という名前の属性を作成する必要があります。

同期クライアントで、[Metaverse Designer]\(メタバース デザイナー\) を選びます

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/db3c1d353168a09aaa68678d39ea4f09.png)

オブジェクトの種類として person を選びます

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/b5e3db86398aed558a481dd64be4f5db.png)

次に、[Actions]\(アクション\) で [Add Attribute]\(属性の追加\) をクリックします

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/47d0056eb496edd2e7b5da11a2c04718.png)

最後に、以下の詳細を設定します

Attribute name (属性の名前): **userPrincipalName**

Attribute Type (属性の型): **String (Indexable)** (文字列 (インデックス付け可能))

Indexed (インデックス付け) = **オン**

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9fba1ff9feefb17b82478ac7010edbfa.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e389ee78beac3bf469ddd97bddb5e9d5.png)

次の手順では、FIM サービス管理エージェントと Active Directory Domain Services 管理エージェントの最小構成が必要です。

構成について詳しくは、「How Do I Provision Users to AD DS」(AD DS にユーザーをプロビジョニングする方法) (<https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx>) をご覧ください

### <a name="synchronization-rule-import-guest-user-to-mv-to-synchronization-service-metaverse-from-azure-active-directorybr"></a>同期ルール: Azure Active Directory から Synchronization Service メタバースに MV のゲスト ユーザーをインポートする<br>

MIM サービスとポータルに移動し、同期ルールを選択して [新規] をクリックします。
![](media/microsoft-identity-manager-2016-graph-b2b-scenario/ba39855f54268aa824cd8d484bae83cf.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/de059b93474c39763f0b27874b716e15.png) [FIM 内にリソースを作成] を選びます ![](media/microsoft-identity-manager-2016-graph-b2b-scenario/9bc4a92136be1557d3596fa2eaa63e61.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0ac7f4d0fd55f4bffd9e6508b494aa74.png)

フロー ルール:

| **初期フローのみ** | **存在テストとして使用** | **フロー (FIM 値 ⇒ フロー後の属性)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [displayName⇒displayName](javascript:void(0);)                        |
|                       |                           | [Left(id,20)⇒accountName](javascript:void(0);)                        |
|                       |                           | [id⇒uid](javascript:void(0);)                                         |
|                       |                           | [userType⇒employeeType](javascript:void(0);)                          |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [surname⇒sn](javascript:void(0);)                                     |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
|                       |                           | [id⇒cn](javascript:void(0);)                                          |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [mobilePhone⇒mobilePhone](javascript:void(0);)                        |

### <a name="synchronization-rule-create-guest-user-account-to-active-directory"></a>同期ルール: Active Directory にゲスト ユーザー アカウントを作成する 

この同期ルールは、Active Directory にユーザーを作成します

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/3463e11aeb9fb566685e775d4e1b825c.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e53c7ac0f376382d890e79dad597c284.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/1c4fad7aa68dc9697fda8f811e9ad37b.png)

フロー ルール:

| **初期フローのみ** | **存在テストとして使用** | **フロー (FIM 値 ⇒ フロー後の属性)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [accountName⇒sAMAccountName](javascript:void(0);)                     |
|                       |                           | [givenName⇒givenName](javascript:void(0);)                            |
|                       |                           | [mail⇒mail](javascript:void(0);)                                      |
|                       |                           | [sn⇒sn](javascript:void(0);)                                          |
|                       |                           | [userPrincipalName⇒userPrincipalName](javascript:void(0);)            |
| **Y**                 |                           | ["CN="+uid+",OU=B2BGuest,DC=scontoso,DC=com"⇒dn](javascript:void(0);) |
| **Y**                 |                           | [RandomNum(0,999)+userPrincipalName⇒unicodePwd](javascript:void(0);)  |
| **Y**                 |                           | [262656⇒userAccountControl](javascript:void(0);)                      |

### <a name="synchronization-rule-import-b2b-guest-user-objects-sid-to-allow-for-login-to-mim"></a>同期ルール: MIM にログインできるように B2B ゲスト ユーザー オブジェクト SID をインポートする 

この同期ルールは、Active Directory にユーザーを作成します

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/263df23fd588c4229b958aee240071f3.png)


![](media/microsoft-identity-manager-2016-graph-b2b-scenario/047ebc3084999246bdd44b1f05ca02b3.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/acc0a871c0bf6f45ee928bed5abd9861.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/80fb9d563ec088925477a645f19b0373.png)

最後に、ユーザーを招待し、次の順序で管理を実行します。

-   MIMMA 管理エージェントでの完全なインポートと同期

-   ADMA_SCONTOSO_B2B 管理エージェントでの完全なインポートと同期

-   B2B Graph 管理エージェントでの完全なインポートと同期

-   ADMA_SCONTOSO_B2B 管理エージェントでのエクスポート、差分インポート、同期

-   MIMMA 管理エージェントでのエクスポート、差分インポート、同期

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/506f0a093c8b58cbb62cc4341b251564.png)

## <a name="finally-application-proxy-with-b2b-guest-and-logging-into-mim"></a>最後: B2B ゲストでのアプリケーション プロキシと MIM へのログイン

MIM で同期ルールを作成しました。 アプリケーション プロキシの構成での定義は、クラウド原則を使用してアプリ プロキシで KCD を許可します。
また、次に、管理ユーザーおよびグループに手動でユーザーを追加しました。 MIM で作成が発生して Office グループにゲストを追加するまで、オプションはユーザーに表示されません。プロビジョニングされたら、このドキュメントで扱っていない構成がもう少し必要です。

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/d0f0b253dbbc5edaf22b22f30f94dd3b.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/a284e69bb14ca19217954881ba860cf2.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/0c2361d137f3efcad9139069c0abcb4d.png)

| **初期フローのみ** | **存在テストとして使用** | **フロー (FIM 値 ⇒ フロー後の属性)**                          |
|-----------------------|---------------------------|-----------------------------------------------------------------------|
|                       |                           | [sAMAccountName⇒accountName](javascript:void(0);)                     |
|                       |                           | ["CONTOSO"⇒domain](javascript:void(0);)                            |
|                       |                           | [objectSid⇒objectSid](javascript:void(0);)                                      |

すべての構成後

最後に、B2B ユーザーをログインさせて、アプリケーションを表示させます

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/275fc989d20d2598df55cde4b4524dca.png)

![](media/microsoft-identity-manager-2016-graph-b2b-scenario/e1a9d7b8c87021de4e43a3501826fa81.png)

<a name="next-steps"></a>次の手順
----------

[AD DS にユーザーをプロビジョニングする方法](https://technet.microsoft.com/en-us/library/ff686263(v=ws.10).aspx)

[FIM 2010 の関数リファレンス](https://technet.microsoft.com/en-us/library/ff800820(v=ws.10).aspx)

[オンプレミス アプリケーションへの安全なリモート アクセスを実現する方法](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

[Microsoft Graph 用 Microsoft Identity Manager 管理エージェントのダウンロード (プレビュー)](http://go.microsoft.com/fwlink/?LinkId=717495)
