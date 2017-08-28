---
title: Microsoft Identity Manager 2016 |Microsoft Docs
description: "MIM には FIM 2010 のアクセス管理機能が付属しており、組織内でのユーザー、資格情報、ポリシー、アクセスを管理するのに役立ちます。"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 21bb12a70850a5f835ca6715d9683558ac6fad1d
ms.sourcegitcommit: f2778c5fa5f0cd04e8a74fc15fa340cd118dded5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/18/2017
---
# <a name="microsoft-identity-manager-2016"></a>Microsoft Identity Manager 2016

Microsoft Identity Manager (MIM) 2016 は、[FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx) の ID およびアクセス管理機能を基盤としています。 FIM 2010 R2 と同様、MIM は、組織内のユーザー、資格情報、ポリシー、およびアクセスの管理に役立ちます。  また MIM 2016 にはハイブリッド対応環境と特権アクセス管理機能が加わっており、新しいプラットフォームもサポートします。

[FIM](https://technet.microsoft.com/library/jj133868) に含まれている既存の ID 管理機能に加えて、 MIM 2016 には以下のような新機能と強化された機能があります。

- Privileged Identity Management
- 証明書管理の新機能
  - [Certificate Management REST API リファレンス](./reference/certificate-management-rest-api-reference.md)
  - マルチフォレストのトポロジのサポート
  - 仮想スマート カード用の Windows アプリ
  - イベントとトラブルシューティング機能の更新 
- セルフ サービスのシナリオには、アカウントのロック解除とパスワード リセットの Azure MFA (多要素認証) ゲートが追加されました。

## <a name="hybrid-experience"></a>ハイブリッド エクスペリエンス

Microsoft Identity Manager 2016 は [Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) と連携して、お使いの環境の完全な制御を実現します。 Azure AD のハイブリッド レポート機能は、クラウドとオンプレミス両方のデータを 1 つにまとめて提示します。 また、[セルフサービスによるパスワードのリセット ポータル](working-with-self-service-password-reset.md)では、Azure Multi-Factor Authentication (MFA) をサポートしています。

## <a name="privileged-identity-management"></a>Privileged Identity Management

Privileged Identity Management は、機密性の高いリソースに対して一時的なタスクベースのアクセス権を付与することで、管理アクセス権を制御および管理します。 つまり、ユーザーには必要な権限のみが与えられるため、攻撃者が完全な管理アクセス権を手に入れるリスクを低減します。 さらに、Privileged Identity Management は既存の Active Directory フォレストから管理アカウントを抽出して分離します。

MIM では、Active Directory を管理するため、オンプレミスの Privileged Identity Management ソリューションをサポートしています。 概要については、「[Privileged Access Management の使用](./pam/privileged-identity-management-for-active-directory-domain-services.md)」をご覧ください。

## <a name="related-topics"></a>関連項目

- Microsoft Identity Manager は、その前身である Forefront Identity Manager に密接に関連しています。 現在も FIM を使用している場合や、追加のドキュメントを参照する必要がある場合は、[FIM 2010 R2 のドキュメントのロードマップ](https://technet.microsoft.com/library/jj133885.aspx)をご覧ください。
- [MIM 展開時のトポロジに関する考慮事項](topology-considerations.md) この記事では、実装を検討できる複数の展開トポロジについて説明します。
- [容量計画ガイド](capacity-planning-guide.md) このガイドとテスト環境を併せて活用し、展開に適した範囲を理解してください。