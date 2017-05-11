---
title: "MIM 2016 を使用した Azure のハイブリッド レポートの操作 | Microsoft Docs"
description: "オンプレミスとクラウド データを Azure のハイブリッド レポートに結合する方法と、これらのレポートを管理および表示する方法について説明します。"
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 04/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 7f16c3a054f0a2c59f118ba33bf64fca10034690
ms.openlocfilehash: df842309034ad68151dd8cc4151507e7ece6626d
ms.contentlocale: ja-jp
ms.lasthandoff: 05/02/2017


---

# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>Identity Manager ハイブリッド レポートの操作 - パブリック プレビュー (更新)

## <a name="available-hybrid-reports"></a>使用可能なハイブリッド レポート
Azure AD で使用可能な最初の 3 つの Microsoft Identity Manager (MIM) レポートは、**パスワード リセット アクティビティ**、**パスワード リセット登録**、および**セルフ サービス グループ アクティビティ**です。

-   パスワード リセット アクティビティは、ユーザーが SSPR を使用してパスワード リセットを実行したとき、各インスタンスを表示します。さらに、認証のためのゲートまたは **メソッド** を提供します。

-   パスワード リセット登録は、SSPR および認証に使用する **メソッド** にユーザーが登録するたびに表示されます (たとえば、携帯電話番号、または質問や回答など)。
    パスワード リセット登録の場合、SMS ゲートと MFA ゲートは差別化されず、両方とも **携帯電話**とみなされます。

-   セルフ サービス グループ アクティビティでは、ユーザーがグループおよびグループ作成に自身を追加または削除するための試行がそれぞれ表示されます。

    ![Azure ハイブリッド レポート - パスワード リセット アクティビティの画像](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> レポートは、現在、1 か月前までさかのぼってデータを表示します。</br>
> 以前のハイブリッド エージェントをアンインストールする必要があります。</br>
> ハイブリッド レポートをアンインストールする場合は、MIMreportingAgent.msi エージェントをアンインストールします。

## <a name="prerequisites"></a>必要条件

1.  Microsoft Identity Manager 2016 RTM または SP1 MIM サービスをインストールします。

2.  Azure AD プレミアム テナントとライセンス付き管理者がディレクトリに割り当てられていることを確認します。

3.  Microsoft Identity Manager サーバーから Azure への送信用インターネット接続があることを確認します。

## <a name="requirements"></a>要件
次の表は、Microsoft Identity Manager ハイブリッド レポートを使用するための要件の一覧です。

| 要件 | 説明 |
| --- | --- |
| Azure AD プレミアム | ハイブリッド レポートは Azure AD Premium の機能であり、Azure AD Premium が必要です。 </br></br>詳細については、[Azure AD Premium の概要](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-get-started-premium)に関するページをご覧ください </br>30 日間の無料評価版を開始するには、[評価版の開始](https://azure.microsoft.com/trial/get-started-active-directory/)に関するページをご覧ください |
| Azure AD のグローバル管理者で評価版を開始する必要がある |既定では、グローバル管理者のみが、エージェントをインストールおよび構成し、評価版の開始、ポータルへのアクセス、Azure 内のすべての操作を実行できます。 </br></br>**重要:** エージェントのインストールに使用するアカウントは、職場または学校のアカウントである必要があります。 Microsoft アカウントは使用できません。 詳細については、「[Azure への組織としてのサインアップ](https://docs.microsoft.com/en-us/azure/active-directory/sign-up-organization)」をご覧ください |
| Microsoft Identity Manager ハイブリッド エージェントが各対象の MIM サービス サーバーにインストールされている | ハイブリッド レポートは、データを受信して、監視機能と分析機能を提供するために、対象のサーバーにエージェントをインストールして構成する必要があります </br>|
| Azure サービス エンドポイントへの送信接続 | エージェントのインストール時および実行時には、エージェントが Azure サービス エンドポイントに接続できる必要があります。 ファイアウォールの使用時に送信接続がブロックされた場合は、次のエンドポイントが許可リストに追加されていることを確認します。 </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - ポート: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|IP アドレスに基づく送信接続 | ファイアウォールのフィルター処理に基づく IP アドレスについては、[Azure の IP 範囲](https://www.microsoft.com/en-us/download/details.aspx?id=41653)に関するページをご覧ください。|
| 送信トラフィックの SSL インスペクションが、フィルター処理されている、または無効になっている | エージェントの登録ステップまたはデータのアップロード操作は、ネットワーク層で送信トラフィックの SSL インスペクションまたは SSL ターミネーションがある場合、失敗する可能性があります。 |
| エージェントを実行しているサーバーのファイアウォール ポート。 |エージェントを Azure サービス エンドポイントと通信させるために、次のファイアウォール ポートを開いておく必要があります。</br></br><li>TCP ポート 443</li><li>TCP ポート 5671</li> |
| IE セキュリティ強化が有効になっている場合、次の Web サイトを許可する |IE セキュリティ強化が有効になっている場合、エージェントをインストールするサーバーで、次の Web サイトを許可する必要があります。</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Azure Active Directory によって信頼される組織のフェデレーション サーバー。 例: https://sts.contoso.com</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>Azure AD への Microsoft Identity Manager レポート エージェントのインストール
レポート エージェントがインストールされた後で、Microsoft Identity Manager アクティビティによってもたらされたデータは MIM から Windows イベント ログにエクスポートされます。 MIM レポート エージェントは、イベントを処理して Azure にアップロードします。 Azure では、必要なレポートに合わせて、イベントの解析、暗号化の解除、およびフィルター処理が行われます。

1.  Microsoft Identity Manager 2016 をインストールします。

2.  次の手順に従って、Microsoft Identity Manager レポート エージェントをダウンロードします。

    1.  Azure AD 管理ポータルにログインし、Active Directory のアイコンをクリックします。

    2.  グローバル管理者となっており、Azure AD プレミアム サブスクリプションが用意されているディレクトリをダブルクリックします。

    3.  **[構成]** をクリックし、レポート エージェントをダウンロードします。

3.  次の手順に従って、Microsoft Identity Manager レポート エージェントをインストールします。

    1.  [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe) を Microsoft Identity Manager サービス サーバーにダウンロードします。
    2.  `MIMHReportingAgentSetup.exe` を実行します。 
    3.  エージェント インストーラーを実行します。

    4.  MIM レポート エージェント サービスが実行されていることを確認します。

    5.  MIM サービスを再起動します。

4.  Microsoft Identity Manager レポートが Azure で動作しているか検証します。

    レポート データを作成するには、Microsoft Identity Manager セルフ サービス パスワード リセット ポータルを使用してユーザーのパスワードをリセットします。 パスワード リセットが正常に完了したことを確認し、Azure AD 管理ポータルにデータが表示されていることを確認します。

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Azure Portal でハイブリッド レポートを表示する

1.  テナント用のグローバル管理者アカウントを使用して [Azure ポータル](https://portal.azure.com/)にログインします。

2.  **Azure Active Directory** アイコンをクリックします。

3.  サブスクリプションで使用可能なディレクトリの一覧からテナント ディレクトリを選択します。

4.  **[監査ログ]** をクリックします。

5.  カテゴリ ドロップダウン メニューで **[MIM サービス]** を必ず選択します。

> [!WARNING]
> Microsoft Identity Manager 監査データが Azure Portal に表示されるまで時間がかかる場合があります。

## <a name="stop-creating-hybrid-reports"></a>ハイブリッド レポートの作成を停止する
Microsoft Identity Manager から Azure Active Directory へのレポート監査データのアップロードを停止する場合は、ハイブリッド レポート エージェントをアンインストールします。 Windows の **[プログラムの追加と削除]** ツールを使用して、Microsoft Identity Manager ハイブリッド レポートをアンインストールします。

## <a name="windows-events-used-for-hybrid-reporting"></a>ハイブリッド レポートに使用される Windows イベント
Microsoft Identity Manager によって生成されたイベントは、Windows イベント ログに記録されます。[アプリケーションとサービス ログ]&gt; **[Identity Manager 要求ログ]** の下にあり、イベント ビューアーで表示できます。 それぞれの MIM 要求は、JSON 構造の Windows イベント ログにイベントとしてエクスポートされます。 これは SIEM にエクスポートすることができます。

|イベントの種類|ID|イベントの詳細|
|--------------|------|-----------------|
|説明|4121|すべての要求データを含む MIM イベント データです。|
|説明|4137|MIM イベント 4121 の拡張イベント。1 つのイベントに対するデータが過剰である場合。 このイベントのヘッダーの形式は次のとおりです。 `"Request: <GUID> , message <xxx> out of <xxx>`|

