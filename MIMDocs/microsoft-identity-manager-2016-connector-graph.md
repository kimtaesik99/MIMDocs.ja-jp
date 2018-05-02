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
ms.openlocfilehash: a0e2e280c3678867efc2ae8afa46c04ed38a1e11
ms.sourcegitcommit: 637988684768c994398b5725eb142e16e4b03bb3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
<a name="the-microsoft-identity-manager-management-agent-for-microsoft-graph-public-preview"></a>Microsoft Graph 用の Microsoft Identity Manager 管理エージェント (パブリック プレビュー)
=======================================================================================

<a name="summary"></a>[概要] 
=======

[Microsoft Graph 用の Microsoft Identity Manager 管理エージェント (プレビュー)](http://go.microsoft.com/fwlink/?LinkId=717495) では、Azure AD Premium のお客様向けの追加統合シナリオが可能になります。

[Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) は、オンプレミスのディレクトリと Azure AD を統合し、オンプレミスのディレクトリから Azure AD にユーザーとグループを同期することにより、Azure AD と統合された AD DS、Office 365、Azure、SaaS アプリケーションの間で、ユーザーが共通の ID と一貫した認証を使用できるようにします。   この管理エージェントは、Azure AD へのユーザーとグループの同期だけに留まらない、ID とアクセスの特別な管理操作のために展開できます。  この管理エージェントは、[Microsoft Graph API](https://developer.microsoft.com/en-us/graph/) の V1 とベータ版から取得される MIM 同期メタバース追加オブジェクトで公開されます。 

<a name="scenarios-covered"></a>対象となるシナリオ
=================

<a name="b2b-account-lifecycle-management"></a>B2B アカウントのライフサイクル管理
--------------------------------

Microsoft Graph 用 Microsoft Identity Manager 管理エージェント (プレビュー) のプレビューでの最初のシナリオは、外部ユーザーの AD アカウントのライフサイクル管理です。 このシナリオでは、組織は Azure AD ディレクトリにゲストを招待し、[Azure AD アプリケーション](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-publish) プロキシまたは他のゲートウェイ メカニズムを介して、オンプレミスの Windows 統合認証ベースまたは Kerberos ベースのアプリケーションへのアクセス権を、これらのゲストに付与しようと考えています。 Azure AD アプリケーション プロキシを利用するには、識別と委任のために、各ユーザーが専用の AD DS アカウントを持っている必要があります

今後さらに新しいシナリオが追加され、[こちらのドキュメント](./microsoft-identity-manager-2016-graph-b2b-scenario.md)に記載されます

<a name="determining-your-deployment-topology"></a>展開トポロジの決定
====================================


<a name="preparing-to-use-the-management-agentma-for-microsoft-graph"></a>Microsoft Graph 用管理エージェント (MA) を使用する準備
=============================================================

<a name="authorizing-the-ma-to-manage-your-azure-ad-directory"></a>Azure AD ディレクトリを管理するための MA の承認
----------------------------------------------------

1.  Graph 管理エージェントを使用するには、Azure AD で Web アプリ/API アプリケーションを作成する必要があります。

![](media/microsoft-identity-manager-2016-ma-graph/724d3fc33b4c405ab7eb9126e7fe831f.png)

図 1. 新しいアプリケーションの登録

2.  作成されたアプリケーションを開き、MA の接続ページでクライアント ID などのアプリケーション ID を使用します。

![](media/microsoft-identity-manager-2016-ma-graph/ecfcb97674790290aa9ca2dcaccdafbc.png)

図 2. アプリケーション ID

2.  [すべての設定] -\> [キー] を開いて、新しいクライアント シークレットを生成します。 キーの説明を設定し、必要な期間を選択します。 変更を保存します。 ページを終了した後は、シークレット値を使用できなくなります。

![](media/microsoft-identity-manager-2016-ma-graph/fdbae443f9e6ccb650a0cb73c9e1a56f.png)

図 3. 新しいクライアント シークレット

3.  [必要なアクセス許可] を開いて、"Microsoft Graph API" をアプリケーションに追加します。

![](media/microsoft-identity-manager-2016-ma-graph/908788fbf8c3c75101f7b663a8d78a4b.png)

図 4. 新しい API の追加

次のアクセス許可を "Microsoft Graph API" に追加する必要があります。

| オブジェクトでの操作 | 必要なアクセス許可                                                                  | アクセス許可の種類 |
|-----------------------|--------------------------------------------------------------------------------------|-----------------|
| グループをインポートする          | Group.Read.All または Group.ReadWrite.All                                                | アプリケーション     |
| ユーザーをインポートする           | User.Read.All、User.ReadWrite.All、Directory.Read.All、または Directory.ReadWrite.All | アプリケーション     |

必要なアクセス許可について詳しくは、[こちら](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference)をご覧ください

1.  アプリケーション ID と生成されたクライアント シークレットを使用して、コネクタを作成します。同じアプリケーションに対してインポートが並列に実行されないよう、各管理エージェントは Azure AD に専用のアプリケーションを持っている必要があります。 Graph コネクタは、次の一覧で示すオブジェクトの種類をサポートします。

-   User

    -   完全/差分インポート

    -   エクスポート (追加、更新、削除)

-   Group

    -   完全/差分インポート

    -   エクスポート (追加、更新、削除)


サポートされている属性の型の一覧:

-   Edm.Boolean

-   Edm.String

-   Edm.DateTimeOffset (コネクタ スペース内の文字列)

-   microsoft.graph.directoryObject (サポートされる任意のオブジェクトへのコネクタ スペース内での参照)

-   microsoft.graph.contact

前の一覧のどの型についても、複数値属性 (コレクション) もされます。

Graph コネクタは、すべてのオブジェクトのアンカーと DN に対して "id" 属性を使用します。

現在、GraphAPI では既存オブジェクトの "id" 属性を変更できないため、名前の変更はサポートされていません。

<a name="access-token-lifetime"></a>アクセス トークンの有効期間
=====================

Graph アプリケーションでは、GraphAPI にアクセスするためにアクセス トークンが必要です。 コネクタは、インポートのイテレーションごとに新しいアクセス トークンを要求します (インポートのイテレーションはページ サイズによって異なります)。 次に例を示します。

-   Azure AD には、10,000 個のオブジェクトが含まれています

-   コネクタで構成されているページ サイズは 5,000 です

この場合、インポートの間には 2 つのイテレーションがあり、各イテレーションで同期のために 5,000 個のオブジェクトが返されます。そのため、新しいアクセス トークンが 2 回要求されます。

エクスポートでは、追加/更新/削除する必要があるオブジェクトごとに新しいアクセス トークンが要求されることに注意してください。

<a name="installing-the-connector"></a>コネクタのインストール
========================

コネクタを使用する前に、同期サーバーに次のものがあることを確認します。Microsoft .NET 4.5.2 Framework 以降。Microsoft Identity Manager 2016 SP1。修正プログラム 4.4.1642.0 ([KB4021562](https://www.microsoft.com/en-us/download/details.aspx?id=55794)) 以降を使用する必要があります。

Microsoft Identity Manager 2016 SP1 用コネクタ。Graph コネクタは [Microsoft ダウンロード センター](https://www.microsoft.com/en-us/download/details.aspx?id=51495)からダウンロードできます。

<a name="connector-configuration"></a>コネクタの構成
=======================

[接続] ページ:

![](media/microsoft-identity-manager-2016-ma-graph/77c2eb73bab8d5187da06293938f5fd9.png)

図 5. [接続] ページ

[接続] ページ (図 1) には、使用されている Graph API のバージョンと、テナントの名前が含まれています。 クライアント ID とクライアント シークレットは、Azure AD で作成する必要のある WebAPI アプリケーションのアプリケーション ID とキーの値を表します。

[Global Parameters]\(グローバル パラメーター\) ページ:

![](media/microsoft-identity-manager-2016-ma-graph/e22d4ee99f2bb825704dd83c1b26dac2.png)

図 6. [Global Parameters]\(グローバル パラメーター\) ページ

[Global Parameters]\(グローバル パラメーター\) ページには、次の設定が含まれます。

[DateTime format]\(日時形式\) – Edm.DateTimeOffset 型の属性に使用される形式です。 すべての日付は、インポートの間にこの形式を使って文字列に変換されます。 設定されている形式は、日付を保存するすべての属性に適用されます。

[HTTP timeout (seconds)]\(HTTP タイムアウト (秒)\) – WebAPI アプリケーションに対する各 HTTP 呼び出しで使われるタイムアウト秒数です。

[Force change password for created user at next sign]\(作成されるユーザーの次回サインイン時にパスワードの変更を強制する\) – このオプションは、エクスポートの間に作成される新しいユーザーに使われます。 オプションをオンにすると [forceChangePasswordNextSignIn](https://developer.microsoft.com/en-us/graph/docs/api-reference/v1.0/resources/passwordprofile) プロパティが true に設定され、オフにすると false に設定されます。

<a name="troubleshooting"></a>トラブルシューティング
===============

**ログを有効にする**

グラフに問題がある場合は、ログを使用して問題の範囲を絞り込むことができます。 Graph コネクタは、すべての汎用コネクタと同じソースを使います。 したがって、トレースを有効にするには、[汎用コネクタと同じ方法 (Wiki を参照)](https://social.technet.microsoft.com/wiki/contents/articles/21086.fim-2010-r2-troubleshooting-how-to-enable-etw-tracing-for-connectors.aspx) を使います。 または、単に以下の行を miiserver.exe.config (system.diagnostics/sources セクション内) に追加します。

\<source name="ConnectorsLog" switchValue="Verbose"\>

\<listeners\>

>   \<add initializeData="ConnectorsLog"   type="System.Diagnostics.EventLogTraceListener, System, Version=4.0.0.0,   Culture=neutral, PublicKeyToken=b77a5c561934e089"   name="ConnectorsLogListener" traceOutputOptions="LogicalOperationStack,   DateTime, Timestamp, Call stack" /\>

\<remove name="Default" /\>

\</listeners\>

\</source\>

注意: [Run this management agent in a separate process]\(この管理エージェントを別のプロセスで実行する\) を有効にした場合は、miiserver.exe.config ではなく dllhost.exe.config を使う必要があります。

**アクセス トークン期限切れエラー**

コネクタから、HTTP エラー 401 Unauthorized とメッセージ "Access token has expired." (アクセス トークンの有効期限が切れています) が返る場合があります。

![](media/microsoft-identity-manager-2016-ma-graph/ce9e23ffe17e3dac79b58bba31cb5a8d.png)

図 7. "Access token has expired." (アクセス トークンの有効期限が切れています) エラー

この問題の原因は、Azure 側でのアクセス トークンの有効期間の構成である可能性があります。 既定では、アクセス トークンは 1 時間後に有効期限が切れます。 有効期限の時間を増やす方法については、[こちらの記事](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)をご覧ください。

[Azure AD PowerShell モジュールのパブリック プレビュー リリース](https://www.powershellgallery.com/packages/AzureADPreview)を使ってこれを行う例

![](media/microsoft-identity-manager-2016-ma-graph/a26ded518f94b9b557064b73615c71f6.png)

New-AzureADPolicy -Definition \@('{"TokenLifetimePolicy":{"Version":1, **"AccessTokenLifetime":"5:00:00"**}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault \$true -Type "TokenLifetimePolicy"

<a name="next-steps"></a>次の手順
----------
- [Graph エクスプローラー (HTTP 呼び出しに関する問題のトラブルシューティングに最適)]( https://developer.microsoft.com/en-us/graph/graph-explorer)
- [Microsoft Graph のバージョン管理、サポートと重大な変更の方針](https://developer.microsoft.com/en-us/graph/docs/concepts/versioning_and_support)
- [Microsoft Graph 用 Microsoft Identity Manager 管理エージェントのダウンロード (プレビュー)](http://go.microsoft.com/fwlink/?LinkId=717495)

<a name="scenario-specific-supported-guides"></a>シナリオ固有のサポートされるガイド
----------------------------------
[MIM B2B のエンド ツー エンドの展開]( ~/microsoft-identity-manager-2016-graph-b2b-scenario.md)
