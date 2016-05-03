---
# required metadata

title: Identity Manager ハイブリッド レポートの操作 |Microsoft Identity Manager
description: オンプレミスとクラウド データを Azure のハイブリッド レポートに結合する方法と、これらのレポートを管理および表示する方法について説明します。
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Identity Manager ハイブリッド レポートの操作

## 使用可能なハイブリッド レポート
Azure AD で使用可能な最初の 3 つの Microsoft Identity Manager (MIM) レポートは、**パスワード リセット アクティビティ**、**パスワード リセット登録**、および**セルフ サービス グループ アクティビティ**です。

-   パスワード リセット アクティビティは、ユーザーが SSPR を使用してパスワード リセットを実行したとき、各インスタンスを表示します。さらに、認証のためのゲートまたは **メソッド** を提供します。

    ![Azure ハイブリッド レポート - パスワード リセット アクティビティの画像](media/MIM-Hybrid-passwordreset.jpg)

-   パスワード リセット登録は、SSPR および認証に使用する **メソッド** にユーザーが登録するたびに表示されます (たとえば、携帯電話番号、または質問や回答など)。
    パスワード リセット登録の場合、SMS ゲートと MFA ゲートは差別化されず、両方とも **携帯電話**とみなされます。

-   セルフ サービス グループ アクティビティでは、ユーザーがグループおよびグループ作成に自身を追加または削除するための試行がそれぞれ表示されます。

> [!NOTE]
> レポートは、現在、1 か月前までさかのぼってデータを表示します。
>
> ハイブリッド レポートをアンインストールする場合は、MIMreportingAgent.msi エージェントをアンインストールします。

## 前提条件

1.  MIM サービスを含む Microsoft Identity Manager 2016 をインストールします。

2.  Azure AD プレミアム テナントとライセンス付き管理者がディレクトリに割り当てられていることを確認します。

3.  Microsoft Identity Manager サーバーから Azure への送信用インターネット接続があることを確認します。

## Azure AD への Microsoft Identity Manager レポートのインストール
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

## Azure クラシック ポータルでハイブリッド レポートを表示する

1.  テナント用のグローバル管理者アカウントを使用して [Azure クラシック ポータル](https://manage.windowsazure.com/)にログインします。

2.  **Active Directory** アイコンをクリックします。

3.  サブスクリプションで使用可能なディレクトリの一覧からテナント ディレクトリを選択します。

4.  **[レポート]** をクリックし、 **[パスワード リセット アクティビティ]**をクリックします。

5.  ソース ドロップダウン メニューでは、 **[Identity Manager]** を必ず選択します。

> [!WARNING]
> Microsoft Identity Manager データが Azure AD に表示されるまで時間がかかる場合があります。

## ハイブリッド レポートの作成を停止する
Microsoft Identity Manager から Azure Active Directory へのレポート データのアップロードを停止する場合は、ハイブリッド レポート エージェントをアンインストールします。 Windows の **[プログラムの追加と削除]** ツールを使用して、Microsoft Identity Manager ハイブリッド レポートをアンインストールします。

## ハイブリッド レポートに使用される Windows イベント
Microsoft Identity Manager によって生成されたイベントは、Windows イベント ログに記録され、[アプリケーションとサービス ログ]、**[Identity Manager 要求ログ]** の下のイベント ビューアーに表示されます。 それぞれの MIM 要求は、JSON 構造の Windows イベント ログにイベントとしてエクスポートされます。 これは SIEM にエクスポートすることができます。

|イベントの種類|ID|イベントの詳細|
|--------------|------|-----------------|
|説明|4121|すべての要求データを含む MIM イベント データです。|
|説明|4137|MIM イベント 4121 の拡張イベント。1 つのイベントに対するデータが過剰である場合。 このイベントのヘッダーの形式は次のとおりです。 `"Request: <GUID> , message <xxx> out of <xxx>`|


<!--HONumber=Apr16_HO2-->


