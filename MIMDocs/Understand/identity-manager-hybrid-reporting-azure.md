---
title: "ハイブリッド レポートとは | Microsoft Docs"
description: "Azure Active Directory のハイブリッド レポートでは、クラウドとオンプレミスの両方のイベントを含むカスタム レポートを作成することができます。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/21/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 7320f014-8b60-4866-92de-cfbd3e6edc48
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: f21c15fdaa5fba9176cfc60a3c49017fa97fa935


---

# <a name="hybrid-identity-management-reports-in-azure"></a>Azure のハイブリッド ID 管理レポート
Azure Active Directory (AD) では、オンプレミスまたはクラウドのいずれかで起きた ID 管理アクティビティを監視する、単一のレポートを作成することができます。 この機能を使用することで、すべての ID および アクセス データを 1 か所で管理して、時間と全体的なコストを削減できます。

## <a name="what-is-azure-ad-hybrid-reporting"></a>Azure AD ハイブリッド レポートについて
IT 担当者は、ハイブリッド レポートを使用することで一般的な ID 管理レポートの課題に対処できるようになります。

1. **さまざまなシステムでの ID 管理アクティビティの収集:**  ハイブリッド レポートでは、Azure AD および Identity Manager での ID 管理アクティビティが示されます。

2. **レポート データのエクスポートとカスタム レポートの作成:**  Azure ポータルでレポートを確認するだけでなく、データをエクスポートして独自のカスタム ビューを作成することもできます。

3. **レポート システムのインフラストラクチャ コストの削減:**  クラウドでハイブリッド レポートを行うことにより、オンプレミスのレポート データウェアハウス インフラストラクチャが不要になります。

## <a name="how-does-it-work"></a>具体的な作用を教えてください。

オンプレミス データを収集するには、まずレポート エージェントを Identity Manager サーバーにインストールします。 レポート エージェントは、[Azure クラシック ポータル](https://manage.windowsazure.com/)内のディレクトリの [構成] ページからダウンロードできます。

ハイブリッド レポートの処理は以下の手順で行われます。
1. レポート エージェントがインストールされると、Identity Manager のアクティビティ データが Windows イベント ログに送信されます。
2. レポート エージェントは、Windows イベント ログでイベントを処理し、Azure にアップロードします。
3. アクティビティ データは、Azure で 1か月間保持されます。
4. レポートを要求すると、必要なレポートに合わせてアクティビティ イベントが解析され、フィルター処理されます。
5. Azure クラシック ポータルが、レポート データを取得し、これをアクティビティ レポートとして表示します。

## <a name="see-also"></a>関連項目
- 詳細については「[Identity Manager ハイブリッド レポートの操作](/microsoft-identity-manager/deploy-use/working-with-identity-manager-hybrid-reporting)」を参照してください。



<!--HONumber=Nov16_HO2-->


