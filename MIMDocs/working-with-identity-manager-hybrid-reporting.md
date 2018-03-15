---
title: "Identity Manager 2016 を使用して Azure でハイブリッド レポートを操作する | Microsoft Docs"
description: "オンプレミスとクラウド データを Azure のハイブリッド レポートに結合する方法と、これらのレポートを管理および表示する方法について説明します。"
keywords: 
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 2/20/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: e135cc5066220765d97568b3a1e1b984a876b2a2
ms.sourcegitcommit: b4a39928c5fa1d7718046563c0809bcbf11d833d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2018
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Identity Manager のハイブリッド レポートを使用する

この記事では、オンプレミスとクラウド データを Azure のハイブリッド レポートに結合する方法と、これらのレポートを管理および表示する方法について説明します。

## <a name="available-hybrid-reports"></a>使用可能なハイブリッド レポート
Azure Active Directory (Azure AD) で使用可能な最初の 3 つの Microsoft Identity Manager レポートは次のとおりです。

- **パスワード リセット アクティビティ**: ユーザーがセルフサービスのパスワード リセット (SSPR) を使用してパスワード リセットを実行した場合の各インスタンスを表示します。さらに、認証のためのゲートやメソッドを提供します。

- **パスワード リセット登録**: ユーザーが SSPR に登録するたびに表示されます。さらに、認証のためのメソッドを表示します。 メソッドの例には、携帯電話番号や質問と回答があります。
   > [!NOTE]
   > *パスワード リセット登録*レポートでは、SMS ゲートと MFA ゲートは区別されません。 両方が、携帯電話メソッドと見なされます。

- **セルフ サービス グループ アクティビティ**: 誰かがグループに対して自身を追加または削除しようとするたびに、またはグループを作成しようとするたびにそれぞれの行動が表示されます。

    ![Azure ハイブリッド レポート - パスワード リセット アクティビティの画像](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * レポートには現在、最大 1 か月分のアクティビティのデータが表示されます。
> * 以前のハイブリッド レポート エージェントをアンインストールする必要があります。
> * ハイブリッド レポートをアンインストールする場合は、MIMreportingAgent.msi エージェントをアンインストールします。

## <a name="prerequisites"></a>必要条件

* Identity Manager 2016 SP1 Identity Manager サービス、推奨ビルド [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager)。

* 使用するディレクトリ内にある、ライセンス付与された管理者のいる Azure AD Premium テナント。

* Identity Manager サーバーから Azure への送信用インターネット接続。

## <a name="requirements"></a>要件
Identity Manager ハイブリッド レポートを使用するための要件を次の表に示します。

| 要件 | 説明 |
| --- | --- |
| Azure AD プレミアム | ハイブリッド レポートは Azure AD Premium の機能であり、Azure AD Premium が必要です。 </br>詳細については、[Azure AD Premium の概要](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium)に関するページをご覧ください。 </br>[Azure AD Premium の 30 日間無料試用版](https://azure.microsoft.com/trial/get-started-active-directory/)を入手できます。 |
| Azure AD のグローバル管理者である必要がある |既定では、グローバル管理者のみが、エージェントをインストールおよび構成し、ポータルにアクセスし、Azure 内のすべての操作を実行できます。 </br>**重要**: エージェントのインストールに使用するアカウントは、職場または学校のアカウントである必要があります。 Microsoft アカウントは使用できません。 詳細については、「[Azure への組織としてのサインアップ](https://docs.microsoft.com/azure/active-directory/sign-up-organization)」をご覧ください。 |
| Identity Manager ハイブリッド エージェントが、対象の各 Identity Manager サービス サーバーにインストールされている | ハイブリッド レポートでは、データを受信し、監視機能と分析機能を提供するには、対象のサーバーにエージェントがインストールされ、構成されている必要があります。  </br>|
| Azure サービス エンドポイントへの送信接続 | エージェントのインストール時および実行時には、エージェントが Azure サービス エンドポイントに接続できる必要があります。 ファイアウォールで送信接続がブロックされた場合、次のエンドポイントが許可リストに追加されていることを確認します。<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li></ul> |
|IP アドレスに基づく送信接続 | ファイアウォールでの IP アドレスに基づくフィルター処理については、[Azure の IP 範囲](https://www.microsoft.com/download/details.aspx?id=41653)に関するページをご覧ください。|
| 送信トラフィックの SSL インスペクションがフィルター処理されている、または無効になっている | ネットワーク層で送信トラフィックの SSL インスペクションまたは SSL ターミネーションがある場合、エージェントの登録手順またはデータのアップロード操作が失敗する可能性があります。 |
| エージェントを実行しているサーバーのファイアウォール ポート | Azure サービス エンドポイントと通信するには、エージェントが次のファイアウォール ポートを開いておく必要があります。<ul><li>TCP ポート 443</li><li>TCP ポート 5671</li></ul> |
| Internet Explorer のセキュリティ強化が有効になっている場合は特定の Web サイトを許可する |Internet Explorer のセキュリティ強化が有効になっている場合、エージェントがインストールされたサーバーで次の Web サイトを許可する必要があります。<ul><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Azure Active Directory から信頼されている組織のフェデレーション サーバー (たとえば、 https://sts.contoso.com)。</li></ul> |
</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Azure AD への Identity Manager レポート エージェントのインストール
レポート エージェントがインストールされると、Identity Manager アクティビティのデータが Identity Manager から Windows イベント ログにエクスポートされます。 Identity Manager レポート エージェントがそれらのイベントを処理し、Azure にアップロードします。 Azure では、必要なレポートに合わせて、イベントの解析、暗号化の解除、およびフィルター処理が行われます。

1.  Identity Manager 2016 をインストールします。

2.  Identity Manager レポート エージェントをダウンロードして、以下を行います。

    」を参照します。 Azure AD 管理ポータルにサインインして、**Active Directory** を選択します。

    b. 自分がグローバル管理者の、Azure AD Premium サブスクリプションが配置されているディレクトリをダブルクリックします。

    c. **[構成]** を選択し、レポート エージェントをダウンロードします。

3.  次の手順に従って、レポート エージェントをインストールします。

    」を参照します。  Identity Manager サービス サーバーの [MIMHReportingAgentSetup.exe ファイル](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe)をダウンロードします。

    b.  
          `MIMHReportingAgentSetup.exe` を実行します。 

    c.  エージェント インストーラーを実行します。

    d.  Identity Manager レポート エージェント サービスが実行されていることを確認します。

    e.  Identity Manager サービスを再起動します。

4.  Identity Manager レポート エージェントが Azure で動作していることを確認します。

    レポート データを作成するには、Identity Manager のセルフサービスのパスワード リセット ポータルを使用して、ユーザーのパスワードをリセットします。 パスワード リセットが正常に完了したことを確認し、Azure AD 管理ポータルにデータが表示されていることを確認します。

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Azure Portal でハイブリッド レポートを表示する

1.  テナント用のグローバル管理者アカウントを使用して [Azure Portal](https://portal.azure.com/) にサインインします。

2.  **Azure Active Directory** を選択します。

3.  サブスクリプションで使用可能なディレクトリの一覧から、テナント ディレクトリを選択します。

4.  **[監査ログ]** を選択します。

5.  **[カテゴリ]** ドロップダウン リストで、**[MIM サービス]**が選択されていることを確認します。

> [!IMPORTANT]
> Identity Manager 監査データが Azure Portal に表示されるまで時間がかかる場合があります。

## <a name="stop-creating-hybrid-reports"></a>ハイブリッド レポートの作成を停止する
Identity Manager から Azure AD へのレポート監査データのアップロードを停止する場合は、ハイブリッド レポート エージェントをアンインストールします。 Windows の [プログラムの追加と削除] ツールを使用して、Identity Manager ハイブリッド レポートをアンインストールします。

## <a name="windows-events-used-for-hybrid-reporting"></a>ハイブリッド レポートに使用される Windows イベント
Identity Manager で生成されるイベントは、Windows イベント ログに格納されます。 **イベント ビューアー**でイベントを表示するには、**[アプリケーションとサービス ログ]** > **[Identity Manager Request Log]\(Identity Manager 要求ログ\)** の順に選択します。 それぞれの Identity Manager 要求は、JSON 構造の Windows イベント ログにイベントとしてエクスポートされます。 結果は、セキュリティ情報およびイベント管理 (SIEM) システムにエクスポートできます。

|イベントの種類|ID|イベントの詳細|
|--------------|------|-----------------|
|説明|4121|すべての要求データを含む Identity Manager イベント データです。|
|説明|4137|1 つのイベントに対するデータが過剰である場合の Identity Manager イベント 4121 の拡張イベント。 このイベントのヘッダーの形式は次のとおりです。`"Request: <GUID> , message <xxx> out of <xxx>`|
