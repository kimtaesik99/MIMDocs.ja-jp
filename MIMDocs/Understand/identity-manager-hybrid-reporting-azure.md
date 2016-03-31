---
title: Azure でのレポートの identity Manager ハイブリッド |Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
author: kgremban
---
# Azure での Identity Manager ハイブリッド レポート
Azure サブスクリプションを所有している場合、オンプレミスとクラウドの両方で発生したイベントのレポートを容易に作成できるようになりました。 レポートは、Azure ポータルで表示できます。 さらに、レポートは Azure Active Directory のアクティビティと結合されます。 Identity Manager ハイブリッド レポート機能を使用すると、クラウド アクティビティとオンプレミス アクティビティの両方の ID 管理アクティビティ レポートを、Azure AD 管理ポータルで表示することができます。 このレポート機能により、以下のことを実現できます。

-   エクスペリエンスの統合: クラウドとオンプレミスの両方について、IAM アクティビティ レポートが統合されます。

-   コストの削減: 必要がなくなるため、オンプレミス レポート データ ウェアハウス インフラストラクチャ

-   データのカスタマイズ: レポートのデータがオンプレミス Identity Manager から、または Azure AD から簡単にエクスポートでき、カスタム ビュー レポートを生成するために使用します。

## Azure AD ハイブリッド レポートとは何ですか。
ハイブリッド レポートを使用すると、統一された ID 管理アクティビティ レポートを Azure AD 管理ポータルで表示できます。 これには、アクティビティがどこで実行されたか (Identity Manager または Azure AD) は関係ありません。 たとえば、セルフサービス パスワード リセット (SSPR) に先月登録されたユーザーを確認する場合は、Azure AD 管理ポータルですべての対象者を表示することができます。 このレポートには、アプリケーション アクセス パネル (myapps.microsoft.com) と Identity Manager の両方で SSPR に登録されたユーザーが表示されます。

![Azure でのパスワード リセット アクティビティのイメージ](media/MIM-Hybrid-passwordreset.jpg)

## なぜ、Identity Manager アクティビティ レポートを Azure AD で使用する必要があるですか。
ハイブリッド レポートを使用することで、IT 専門家はいくつかの一般的な ID 管理レポートの課題に対処することができます。

1.  さまざまなシステムで実行された ID 管理アクティビティのレポート:Azure AD および Identity Manager でのアクティビティに基づく ID 管理レポートを Azure AD 管理ポータルで確認できるようになりました。

2.  レポート データのエクスポートと、カスタム レポートの作成:Azure AD でのレポートに加えて、この新しい機能では、Identity Manager アクティビティを反映する Windows イベントも追加しました。 これにより、SIEM システムへの統合、Identity Manger アクティビティの表示、およびカスタム レポートの作成が以前に比べてかなり簡単になりました。

3.  レポート システムのインフラストラクチャ コストの最小化: この新しい機能は数分の作業時間で展開できます。 必要な作業は、Identity Manager サーバーにレポート エージェントをインストールすることだけです。

レポート エージェントは、Azure AD 管理ポータルの [ディレクトリの構成] 画面からダウンロードします。

![MIM reporting エージェント ダウンロード イメージ](media/MIM-Hybrid-downloadReportAgent.jpg)

## 機能
レポート エージェントがインストールされると、Identity Manager のアクティビティ データが Windows イベント ログに送信されます。 レポート エージェントは、イベントを処理し、それらを Azure にアップロードします。 Azure に、アクティビティ データが格納されます (現在、保持される期間は 1 か月)。 レポートの取得時には、必要なレポートに合わせてアクティビティ イベントが解析され、フィルター処理されます。 最後に、Azure 管理ポータルは、レポート　データを取得し、これをアクティビティ レポートとして表示します。

![ハイブリッド レポート ダイアグラム](media/MIM-Hybrid-howitworks.png)


<!--HONumber=Mar16_HO3-->


