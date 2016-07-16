---
title: "Privileged Access Management のための MIM 環境の構成 | Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.sourcegitcommit: 9cf126d898c93faf89d7119136cce4e4963bb63d
ms.openlocfilehash: c9f2cf2ba1f42ea1513ae38d8089839d85ae5553


---

# Privileged Access Management のための MIM 環境の構成
クロスフォレスト アクセスの環境を設定し、Active Directory と Microsoft Identity Manager をインストールして構成し、ジャスト イン タイムのアクセス要求を実行するには、7 つの手順があります。

これらの手順は、テスト環境を最初から作成して構築できるようにレイアウトされています。 PAM を既存の環境に適用している場合は、例と同じ新しいドメイン コントローラーやユーザー アカウントを作成するのではなく、独自のものを使うことができます。

1.   *CORPDC* サーバーをドメイン コントローラーとして、 *CORPWKSTN* をメンバー ワークステーションとして準備します。

2.  *PRIVDC* サーバーをドメイン コントローラーとして準備します。

3.  *PRIV* フォレストに *PAMSRV* サーバーを準備します。

4.  *PAMSRV* に MIM コンポーネントを、 *CONTOSO* フォレスト メンバー ワークステーションにコマンドレットをインストールし、 Privileged Access Managment 用に準備します。

5.  *PRIV* フォレストと *CONTOSO* フォレスト間に信頼関係を確立します。

6.  Just-in-time の Privileged Access Management のために、保護されたリソースとメンバー アカウントに対するアクセス権を持つ特権セキュリティ グループを準備します。

7.  保護されたリソースに対する昇格された特権アクセスの要求、受け取り、および利用をデモンストレーションします。

>[!div class="step-by-step"]
[[!div class="step-by-step"] [開始 »](step-1-prepare-corp-domain.md)](step-1-prepare-corp-domain.md)



<!--HONumber=Jul16_HO2-->


