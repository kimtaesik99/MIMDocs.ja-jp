---
title: Microsoft Identity Manager 2016 |Microsoft Docs
description: "MIM には FIM 2010 のアクセス管理機能が付属しており、組織内でのユーザー、資格情報、ポリシー、アクセスを管理するのに役立ちます。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3797f5789bb4e48836eb21776dafd5a2e0e11613
ms.openlocfilehash: 3f5024cf760834ba2181252325a13e40bda520cd
ms.contentlocale: ja-jp
ms.lasthandoff: 07/10/2017


---

<a id="microsoft-identity-manager-2016" class="xliff"></a>
# Microsoft Identity Manager 2016
Microsoft Identity Manager (MIM) 2016 は、[FIM 2010 R2](https://technet.microsoft.com/library/jj133885.aspx) の ID およびアクセス管理機能を基盤としています。 FIM 2010 R2 と同様、MIM は、組織内のユーザー、資格情報、ポリシー、およびアクセスの管理に役立ちます。  また MIM 2016 にはハイブリッド対応環境と特権アクセス管理機能が加わっており、新しいプラットフォームもサポートします。

このバージョンの Microsoft Identity Manager には、Privileged Identity Management、REST API アクセス用 Certificate Management のサポートなどの新しい機能があります。 Certificate Management には、複数フォレスト トポロジのサポート、仮想スマートカード用 Windows ストア アプリ、および証明書のライフサイクル管理が追加され、イベントとトラブルシューティング機能が更新されました。 セルフ サービスのシナリオには、アカウントのロック解除とパスワード リセットの多要素認証ゲートが追加されました。

<a id="hybrid-experience" class="xliff"></a>
## ハイブリッド エクスペリエンス
Microsoft Identity Manager 2016 は Azure と連携して、お使いの環境の完全な制御を実現します。 Azure のハイブリッド レポート機能は、クラウドとオンプレミス両方のデータを 1 つにまとめて提示します。 また、セルフサービスによるパスワードのリセット ポータルでは、Azure Multi-Factor Authentication (MFA) をサポートしています。

<a id="privileged-identity-management" class="xliff"></a>
## Privileged Identity Management
Privileged Identity Management は、機密性の高いリソースに対して一時的なタスクベースのアクセス権を付与することで、管理アクセス権を制御および管理します。 つまり、ユーザーには必要な権限のみが与えられるため、攻撃者が完全な管理アクセス権を手に入れるリスクを低減します。 さらに、Privileged Identity Management は既存の Active Directory フォレストから管理アカウントを抽出して分離します。

MIM では、Active Directory を管理するため、オンプレミスの Privileged Identity Management ソリューションをサポートしています。 概要については、「[Privileged Access Management の使用](./pam/privileged-identity-management-for-active-directory-domain-services.md)」をご覧ください。

<a id="related-topics" class="xliff"></a>
## 関連項目
Microsoft Identity Manager は、その前身である Forefront Identity Manager に密接に関連しています。 現在も FIM を使用している場合や、追加のドキュメントを参照する必要がある場合は、[FIM 2010 R2 のドキュメントのロードマップ](https://technet.microsoft.com/library/jj133885.aspx)をご覧ください。
