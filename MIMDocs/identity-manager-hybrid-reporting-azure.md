---
title: "ハイブリッド レポートとは | Microsoft Docs"
description: "Azure Active Directory のハイブリッド監査アクティビティ レポートでは、クラウドの監査済みイベントとオンプレミスの監査済みイベントの両方を表示することができます。"
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
ms.openlocfilehash: ffe372c273aae55278f9b18b45b65425734aa6f7
ms.sourcegitcommit: e52bab207117390997c6fa8450de24335b502673
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/20/2017
---
# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory---public-previewrefresh"></a>Azure Active Directory でのハイブリッド ID 管理監査レポート - パブリック プレビュー (更新)
Azure Active Directory (AD) の監査アクティビティ レポートでは、オンプレミスまたはクラウドのいずれかで起きた ID 管理アクティビティを監視する、単一のレポートを作成することができます。 この機能を使用することで、すべての ID および アクセス データを 1 か所で管理して、時間と全体的なコストを削減できます。

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Azure Active Directory ハイブリッド レポートとは
IT 担当者は、ハイブリッド監査レポートを使用することで一般的な ID 管理レポートの課題に対処できるようになります。

1. **さまざまなシステムでの ID 管理アクティビティの収集:**  ハイブリッド レポートでは、Azure AD および Identity Manager での ID 管理アクティビティが示されます。

2. **レポート データのエクスポートとカスタム レポートの作成:**  Azure ポータルでレポートを確認するだけでなく、データをエクスポートして独自のカスタム ビューを作成することもできます。

3. **レポート システムのインフラストラクチャ コストの削減:**  クラウドでハイブリッド レポートを行うことにより、オンプレミスのレポート データウェアハウス インフラストラクチャが不要になる場合があります。

## <a name="how-does-it-work"></a>具体的な作用を教えてください。

オンプレミス データを収集するには、まず、レポート エージェントを Identity Manager 2016 サーバーにインストールします。 レポート エージェントは Microsoft ダウンロード ページの[こちら](https://www.microsoft.com/download/details.aspx?id=55112)からダウンロードされます。

ハイブリッド レポートの処理は以下の手順で行われます。
1. レポート エージェントがインストールされると、Identity Manager のアクティビティ データが Windows イベント ログに送信されます。
2. レポート エージェントは、Windows イベント ログでデルタ イベントを 10 分ごとに、またはサービスの再起動時に処理し、Azure Portal にアップロードします。
3. Azure Portal は、受信したデータを 1 時間以内に処理します。
4. アクティビティ データは、Azure で 1か月間保持されます。
5. Azure Portal では、監査レポート データが取得され、監査結果として [Azure Audit Reporing] ブレード内に表示されます。

## <a name="next-steps"></a>次の手順
- 詳細については「[Identity Manager ハイブリッド レポートの操作](working-with-identity-manager-hybrid-reporting.md)」を参照してください。
- 詳細については「[Azure Active Directory ポータルの監査アクティビティ レポート](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)」を参照してください。
- 詳細については「[リテンション期間ポリシーのレポート](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)」を参照してください。
- 詳細については「[Microsoft Azure ログの統合 (SIEM)](https://docs.microsoft.com/azure/security/security-azure-log-integration-overview)」を参照してください。
- 詳細については「[Azure Active Directory reporting API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started)」を参照してください。
