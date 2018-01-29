---
title: "Azure AD のハイブリッド レポートとは | Microsoft Docs"
description: "Azure Active Directory のハイブリッド監査アクティビティ レポートでは、クラウドとオンプレミス両方の監査済みイベントを表示できます。"
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/28/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: fimguy
ms.suite: ems
ms.openlocfilehash: e2391be3d05f61335c134c104673a31ad7fc3830
ms.sourcegitcommit: 3d8a2493eae1218bfdb75a399ffa4adc8c2a8fdf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/20/2018
---
# <a name="hybrid-identity-management-audit-reporting-in-azure-active-directory-public-preview-refresh"></a>Azure Active Directory でのハイブリッド ID 管理監査レポート - パブリック プレビュー (更新)
Azure Active Directory (Azure AD) の監査アクティビティ レポートでは、オンプレミスかクラウドのいずれかで ID 管理アクティビティを監視できます。 1 つのレポートですべての ID データとアクセス データを管理することで、処理時間を短縮し、全体的なコストを削減できます。

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Azure Active Directory ハイブリッド レポートとは
IT 担当者は、ハイブリッド監査レポートを使用することで、次のような ID 管理レポートの一般的な課題に対処できるようになります。

* **さまざまなシステムでの ID 管理アクティビティの収集**。 ハイブリッド レポートでは、Azure AD および Identity Manager での ID 管理アクティビティが示されます。

* **レポート データのエクスポートとカスタム レポートの作成**。 Azure Portal でレポートを確認するだけでなく、データをエクスポートして独自のカスタム ビューを作成できます。

* **レポート システムのインフラストラクチャ コストの削減**。 クラウドのハイブリッド レポートでは、オンプレミスのデータ ウェアハウス インフラストラクチャに関連するコストを減らすことができます。

## <a name="how-does-it-work"></a>具体的な作用を教えてください。

オンプレミス データを収集するには、まず、レポート エージェントを Identity Manager 2016 サーバーにインストールします。 [Microsoft Identity Manager ハイブリッド レポート エージェントのダウンロード](https://www.microsoft.com/download/details.aspx?id=55112)。

ハイブリッド レポートでは、次の処理が行われます。
1. レポート エージェントがインストールされると、Identity Manager のアクティビティ データが Windows イベント ログに送信されます。
2. レポート エージェントが、デルタ イベントを 10 分ごと、または Windows イベント ログ サービスの再起動時に処理します。 エージェントが、それらのイベントを Azure Portal にアップロードします。
3. Azure Portal が、受信したデータを受信して 1 時間以内に処理します。
4. アクティビティ データは、Azure で 1か月間保持されます。
5. Azure Portal が、監査レポート データを取得し、[Azure Audit Reporting]\(Azure 監査レポート\) ウィンドウに表示します。

## <a name="next-steps"></a>次の手順
詳細については、下記のリンクをクリックしてください。
- [Identity Manager ハイブリッド レポートの操作](working-with-identity-manager-hybrid-reporting.md)
- [Azure Active Directory ポータルの監査アクティビティ レポート](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
- [レポート保有ポリシー](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)
- [Microsoft Azure ログ統合 (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)
- [Azure Active Directory レポート API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)
