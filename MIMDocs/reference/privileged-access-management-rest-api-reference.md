---
title: "Privileged Access Management REST API リファレンス | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 541854b7-f285-4e8b-bbaf-3f15da69467f
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 02de09a7de843cb42080daa4ce43a5a0c74f2c18
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="privileged-access-management-rest-api-reference"></a>Privileged Access Management REST API リファレンス
Microsoft Identity Manager (MIM) 2016 では、Privileged Access Management (PAM) と呼ばれる新しいシナリオが追加されています。 PAM を使用すると、組織は、システム管理者やサービス管理者などの高度な特権を持つユーザー アカウントの機密性の高いリソースへのアクセス権を、より制御できるようになります。 PAM は、アクセス権が必要なときに、ジャスト イン タイム (JIT) で期間限定のアクセス権を提供することで、高い権限を持つアカウントのアクセス権を制御します。

ユーザーは、次のいずれかの方法で、MIM サービスに特権アクセス権 (昇格) を求めることができます。

- PAM REST API を使用する
- PAM PowerShell New-PAMRequest コマンドレットを使用する

このガイドでは、PAM REST API について説明します。 PowerShell コマンドレットの使用に関する詳細は、テスト ラボ ガイドの次のトピックを参照してください。Connect サイトで使用可能な Microsoft Identity Manager を使用した Privileged Access Management のデモ

##<a name="pam-rest-api-resources-and-operations"></a>PAM REST API リソースと操作
次のリソースでの PAM REST API の操作
- **PAM ロール**:PAM ロールは、ユーザーのコレクションとアクセス権のコレクションを関連付けます。 アクセス権は、セキュリティ グループへの参照によって定義されます。  すべての PAM ロールには、PAM ロールへの昇格が許可されている、候補と呼ばれるユーザー アカウントの一覧があります。 PAM ロールでは、次の操作を実行できます。

    - [PAM ロールを取得する](privileged-access-management-get-roles.md)

- **PAM 要求**:PAM ロールのアクセス権への昇格を希望するユーザーは、PAM 要求を送信し、昇格要求に対して承認を得る必要があります。 PAM 要求オブジェクトは、MIM サービスでのこの要求のライフサイクルを追跡します。 PAM 要求では、次の操作を実行できます。

    - [PAM 要求を作成する](privileged-access-management-create-request.md)
    - [PAM 要求を取得する](privileged-access-management-get-requests.md)
    - [PAM 要求を閉じる](privileged-access-management-close-request.md)

- **保留中の PAM 要求**:ユーザーによって送信された PAM 要求を承認または拒否するために使用されます。 保留中の PAM 要求では、次の操作を実行できます。

    - [保留中の PAM 要求を取得する](privileged-access-management-get-pending-requests.md)
    - [保留中の PAM 要求を承認または拒否する](privileged-access-management-approve-reject-pending-request.md)

- **PAM セッション**:PAM REST API を使用する場合、クライアント (Web ブラウザーなど) にはPAM REST API エンドポイントとのセッションがあります。 このセッションでは、クライアントは、REST API エンドポイントに対して認証されます。 PAM セッションでは、次の操作を実行できます。

     - [PAM セッション情報を取得する](privileged-access-management-get-session-info.md)

サービスの詳細については、「[PAM REST API サービスの詳細](privileged-access-management-rest-api-service-details.md)」を参照してください。

##<a name="pam-sample-portal-on-github"></a>GitHub の PAM サンプル ポータル
PAM REST APIの使用方法を学習する 1 つの方法として、PAM サンプル ポータル (この API を使用している Web アプリケーションの例) を使用することができます。 [GitHub の PAM サンプル リポジトリ](http://go.microsoft.com/fwlink/?LinkID=618550&clcid=0x409)で PAM サンプル ポータルのコードを検索することができます。 サンプル ポータルの展開方法は、PAM のテスト ラボ ガイドで確認できます。
