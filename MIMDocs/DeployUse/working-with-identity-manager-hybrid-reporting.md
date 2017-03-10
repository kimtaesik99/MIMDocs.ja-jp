---
title: "MIM 2016 を使用した Azure のハイブリッド レポートの操作 | Microsoft Docs"
description: "オンプレミスとクラウド データを Azure のハイブリッド レポートに結合する方法と、これらのレポートを管理および表示する方法について説明します。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 9e64f930a8fe8422c7f6c8d98e558961ae8b88f2


---

# <a name="working-with-identity-manager-hybrid-reporting"></a>Identity Manager ハイブリッド レポートの操作

## <a name="available-hybrid-reports"></a>使用可能なハイブリッド レポート
Azure AD で使用可能な最初の&3; つの Microsoft Identity Manager (MIM) レポートは、**パスワード リセット アクティビティ**、**パスワード リセット登録**、および**セルフ サービス グループ アクティビティ**です。

-   パスワード リセット アクティビティは、ユーザーが SSPR を使用してパスワード リセットを実行したとき、各インスタンスを表示します。さらに、認証のためのゲートまたは **メソッド** を提供します。

    ![Azure ハイブリッド レポート - パスワード リセット アクティビティの画像](media/MIM-Hybrid-passwordreset.jpg)

-   パスワード リセット登録は、SSPR および認証に使用する **メソッド** にユーザーが登録するたびに表示されます (たとえば、携帯電話番号、または質問や回答など)。
    パスワード リセット登録の場合、SMS ゲートと MFA ゲートは差別化されず、両方とも **携帯電話**とみなされます。

-   セルフ サービス グループ アクティビティでは、ユーザーがグループおよびグループ作成に自身を追加または削除するための試行がそれぞれ表示されます。

> [!NOTE]
> レポートは、現在、1 か月前までさかのぼってデータを表示します。
>
> ハイブリッド レポートをアンインストールする場合は、MIMreportingAgent.msi エージェントをアンインストールします。

## <a name="prerequisites"></a>必要条件

1.  MIM サービスを含む Microsoft Identity Manager 2016 をインストールします。

2.  Azure AD プレミアム テナントとライセンス付き管理者がディレクトリに割り当てられていることを確認します。

3.  Microsoft Identity Manager サーバーから Azure への送信用インターネット接続があることを確認します。

## <a name="install-microsoft-identity-manager-reporting-in-azure-ad"></a>Azure AD への Microsoft Identity Manager レポートのインストール
レポート エージェントがインストールされた後で、Microsoft Identity Manager アクティビティによってもたらされたデータは MIM から Windows イベント ログにエクスポートされます。 MIM レポート エージェントは、イベントを処理して Azure にアップロードします。 Azure では、必要なレポートに合わせて、イベントの解析、暗号化の解除、およびフィルター処理が行われます。

1.  Microsoft Identity Manager 2016 をインストールします。

2.  次の手順に従って、Microsoft Identity Manager レポート エージェントをダウンロードします。

    1.  Azure AD 管理ポータルにログインし、Active Directory のアイコンをクリックします。

    2.  グローバル管理者となっており、Azure AD プレミアム サブスクリプションが用意されているディレクトリをダブルクリックします。

    3.  **[構成]** をクリックし、レポート エージェントをダウンロードします。

3.  次の手順に従って、Microsoft Identity Manager レポート エージェントをインストールします。

    1.  コンピューター上にディレクトリを作成します。

    2.  `MIMHybridReportingAgent.msi` ファイルと `tenant.cert` ファイルをディレクトリに解凍します。

    3.  エージェント インストーラーを実行します。

    4.  MIM レポート エージェント サービスが実行されていることを確認します。

    5.  MIM サービスを再起動します。

4.  Microsoft Identity Manager レポートが Azure で動作しているか検証します。

    レポート データを作成するには、Microsoft Identity Manager セルフ サービス パスワード リセット ポータルを使用してユーザーのパスワードをリセットします。 パスワード リセットが正常に完了したことを確認し、Azure AD 管理ポータルにデータが表示されていることを確認します。

## <a name="view-hybrid-reports-in-the-azure-classic-portal"></a>Azure クラシック ポータルでハイブリッド レポートを表示する

1.  テナント用のグローバル管理者アカウントを使用して [Azure クラシック ポータル](https://manage.windowsazure.com/)にログインします。

2.  **Active Directory** アイコンをクリックします。

3.  サブスクリプションで使用可能なディレクトリの一覧からテナント ディレクトリを選択します。

4.  **[レポート]** をクリックし、 **[パスワード リセット アクティビティ]**をクリックします。

5.  ソース ドロップダウン メニューでは、 **[Identity Manager]** を必ず選択します。

> [!WARNING]
> Microsoft Identity Manager データが Azure AD に表示されるまで時間がかかる場合があります。

## <a name="stop-creating-hybrid-reports"></a>ハイブリッド レポートの作成を停止する
Microsoft Identity Manager から Azure Active Directory へのレポート データのアップロードを停止する場合は、ハイブリッド レポート エージェントをアンインストールします。 Windows の **[プログラムの追加と削除]** ツールを使用して、Microsoft Identity Manager ハイブリッド レポートをアンインストールします。

## <a name="windows-events-used-for-hybrid-reporting"></a>ハイブリッド レポートに使用される Windows イベント
Microsoft Identity Manager によって生成されたイベントは、Windows イベント ログに記録されます。[アプリケーションとサービス ログ]&gt; **[Identity Manager 要求ログ]** の下にあり、イベント ビューアーで表示できます。 それぞれの MIM 要求は、JSON 構造の Windows イベント ログにイベントとしてエクスポートされます。 これは SIEM にエクスポートすることができます。

|イベントの種類|ID|イベントの詳細|
|--------------|------|-----------------|
|説明|4121|すべての要求データを含む MIM イベント データです。|
|説明|4137|MIM イベント 4121 の拡張イベント。1 つのイベントに対するデータが過剰である場合。 このイベントのヘッダーの形式は次のとおりです。 `"Request: <GUID> , message <xxx> out of <xxx>`|



<!--HONumber=Jan17_HO4-->


