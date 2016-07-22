---
title: Microsoft Identity Manager 2016 |Microsoft Identity Manager
description: "クラウドとオンプレミスでより安全でより便利な ID 管理エクスペリエンスを作成する MIM 2016 のしくみを理解します。"
keywords: 
author: kgremban
manager: femila
ms.date: 06/27/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: 5247cce895344ac6148b735fe550eb16c39103c7


---

# Microsoft Identity Manager 2016
Microsoft Identity Manager (MIM) 2016 は、FIM 2010 R2 の ID およびアクセス管理機能 を基盤としています。 FIM 2010 R2 と同様、MIM は、組織内のユーザー、資格情報、ポリシー、およびアクセスの管理に役立ちます。  また MIM 2016 にはハイブリッド対応環境と特権アクセス管理機能が加わっており、新しいプラットフォームもサポートします。

このバージョンの Microsoft Identity Manager には、Privileged Identity Management、REST API アクセス用 Certificate Management のサポートなどの新しい機能があります。 Certificate Management には、複数フォレスト トポロジのサポート、仮想スマートカード用 Windows ストア アプリ、および証明書のライフサイクル管理が追加され、イベントとトラブルシューティング機能が更新されました。 セルフ サービスのシナリオには、アカウントのロック解除とパスワード リセットの多要素認証ゲートが追加されました。

## ハイブリッド エクスペリエンス
Microsoft Identity Manager 2016 は Azure と連携して、お使いの環境の完全な制御を実現します。 Azure のハイブリッド レポート機能は、クラウドとオンプレミス両方のデータを 1 つにまとめて提示します。 また、セルフサービスによるパスワードのリセット ポータルでは、Azure Multi-Factor Authentication (MFA) をサポートしています。

## Privileged Identity Management
Privileged Identity Management は、機密性の高いリソースに対して一時的なタスクベースのアクセス権を付与することで、管理アクセス権を制御および管理します。 つまり、ユーザーには必要な権限のみが与えられるため、攻撃者が完全な管理アクセス権を手に入れるリスクを低減します。 さらに、Privileged Identity Management は既存の Active Directory フォレストから管理アカウントを抽出して分離します。

MIM では、Active Director を管理するため、オンプレミスの Privileged Identity Management ソリューションをサポートしています。 概要については、「[Privileged Access Management の使用](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)」をご覧ください。



<!--HONumber=Jul16_HO3-->


