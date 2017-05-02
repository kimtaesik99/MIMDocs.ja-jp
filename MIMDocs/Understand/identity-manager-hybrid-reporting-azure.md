---
title: "ハイブリッド レポートとは | Microsoft Docs"
description: "Azure Active Directory のハイブリッド監査アクティビティ レポートでは、クラウドの監査済みイベントとオンプレミスの監査済みイベントの両方を表示することができます。"
keywords: 
author: kgremban
ms.author: fimguy
manager: femila
ms.date: 04/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3144ee195675df5dc120896cc801a7124ee12214
ms.openlocfilehash: 8ca0af93f2d72ccf2e314b20d13323b631eb02bc
ms.lasthandoff: 04/27/2017


---

# <a name="hybrid-identity-management-audit-reports-in-azure-active-directory"></a>Azure Active Directory でのハイブリッド ID 管理監査レポート
Azure Active Directory (AD) の監査アクティビティ レポートでは、オンプレミスまたはクラウドのいずれかで起きた ID 管理アクティビティを監視する、単一のレポートを作成することができます。 この機能を使用することで、すべての ID および アクセス データを 1 か所で管理して、時間と全体的なコストを削減できます。

## <a name="what-is-azure-active-directory-hybrid-reporting"></a>Azure Active Directory ハイブリッド レポートとは
IT 担当者は、ハイブリッド監査レポートを使用することで一般的な ID 管理レポートの課題に対処できるようになります。

1. **さまざまなシステムでの ID 管理アクティビティの収集:**  ハイブリッド レポートでは、Azure AD および Identity Manager での ID 管理アクティビティが示されます。

2. **レポート データのエクスポートとカスタム レポートの作成:**  Azure ポータルでレポートを確認するだけでなく、データをエクスポートして独自のカスタム ビューを作成することもできます。

3. **レポート システムのインフラストラクチャ コストの削減:**  クラウドでハイブリッド レポートを行うことにより、オンプレミスのレポート データウェアハウス インフラストラクチャが不要になります。

## <a name="how-does-it-work"></a>具体的な作用を教えてください。

オンプレミス データを収集するには、まず、レポート エージェントを Identity Manager 2016 サーバーにインストールします。 レポート エージェントは Microsoft ダウンロード ページの[こちら](https://www.microsoft.com/en-us/download/details.aspx?id=####/)からダウンロードされます。

ハイブリッド レポートの処理は以下の手順で行われます。
1. レポート エージェントがインストールされると、Identity Manager のアクティビティ データが Windows イベント ログに送信されます。
2. レポート エージェントは、Windows イベント ログでイベントを処理し、Azure ポータルにアップロードします。
3. アクティビティ データは、Azure で 1か月間保持されます。
4. レポートを要求すると、必要なレポートに合わせてアクティビティ イベントが解析され、フィルター処理されます。
5. Azure ポータルでは、監査レポート データを取得し、これをアクティビティ レポート内に監査結果として表示します。

## <a name="see-also"></a>関連項目
- 詳細については「[Identity Manager ハイブリッド レポートの操作](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)」を参照してください。
- 詳細については「[Azure Active Directory ポータルの監査アクティビティ レポート](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-activity-audit-logs)」を参照してください。

