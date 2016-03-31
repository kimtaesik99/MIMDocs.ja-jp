---
title: Privileged Access Management のための MIM 環境の構成
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
ms.assetid: c4ca5b58-ad0c-48af-a9eb-b71b22d0c67c
author: Kgremban
---
# Privileged Access Management のための MIM 環境の構成
クロスフォレスト アクセスの環境を設定し、Active Directory と Microsoft Identity Manager をインストールして構成し、ジャスト イン タイムのアクセス要求を実行するには、7 つの手順があります。

1.   *CORPDC* サーバーをドメイン コントローラーとして、 *CORPWKSTN* をメンバー ワークステーションとして準備します。

2.   *PRIVDC* サーバーをドメイン コントローラーとして準備します。

3.   *PRIV* フォレストに *PAMSRV* サーバーを準備します。

4.   *PAMSRV* に MIM コンポーネントを、 *CONTOSO* フォレスト メンバー ワークステーションにコマンドレットをインストールし、 Privileged Access Managment 用に準備します。

5.   *PRIV* フォレストと *CONTOSO* フォレスト間に信頼関係を確立します。

6.  Just-in-time の Privileged Access Management のために、保護されたリソースとメンバー アカウントに対するアクセス権を持つ特権セキュリティ グループを準備します。

7.  保護されたリソースに対する昇格された特権アクセスの要求、受け取り、および利用をデモンストレーションします。

次のセクションでは、これらのタスクを実行する詳細な方法について説明します。


<!--HONumber=Mar16_HO3-->


