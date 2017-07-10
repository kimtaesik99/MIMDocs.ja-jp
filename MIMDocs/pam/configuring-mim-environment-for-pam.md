---
title: "Privileged Access Management の MIM 2016 を構成する | Microsoft Docs"
description: "MIM をインストールし Privileged Access Management 用に構成するためのロードマップ。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 32815c4ddc51fb9c9187c9fc9a1710239faf7935
ms.contentlocale: ja-jp
ms.lasthandoff: 01/24/2017


---

<a id="configure-the-mim-environment-for-privileged-access-management" class="xliff"></a>
# Privileged Access Management の MIM 環境を構成する
クロスフォレスト アクセスの環境を設定し、Active Directory と Microsoft Identity Manager をインストールして構成し、ジャスト イン タイムのアクセス要求を実行するには、7 つの手順があります。

これらの手順は、テスト環境を最初から作成して構築できるようにレイアウトされています。 PAM を既存の環境に適用している場合は、例と同じ新しいドメイン コントローラーやユーザー アカウントを作成するのではなく、独自のものを使うことができます。

1.  *CORPDC* サーバーをドメイン コントローラーとして、 *CORPWKSTN* をメンバー ワークステーションとして準備します。

2.  *PRIVDC* サーバーをドメイン コントローラーとして準備します。

3.  *PRIV* フォレストに *PAMSRV* サーバーを準備します。

4.  *PAMSRV* に MIM コンポーネントを、 *CONTOSO* フォレスト メンバー ワークステーションにコマンドレットをインストールし、 Privileged Access Managment 用に準備します。

5.  *PRIV* フォレストと *CONTOSO* フォレスト間に信頼関係を確立します。

6.  Just-in-time の Privileged Access Management のために、保護されたリソースとメンバー アカウントに対するアクセス権を持つ特権セキュリティ グループを準備します。

7.  保護されたリソースに対する昇格された特権アクセスの要求、受け取り、および利用をデモンストレーションします。

>[!div class="step-by-step"]
[開始 »](step-1-prepare-corp-domain.md)

