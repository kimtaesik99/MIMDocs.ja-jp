---
title: "PAM 環境の概要 | Microsoft Docs"
description: "Privileged Access Management を正常に展開するために必要な仮想マシンの数と構成を確認する"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/31/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3be2e19673a863098739e830d9c83ce264abf412
ms.sourcegitcommit: 210195369d2ecd610569d57d0f519d683ea6a13b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/01/2017
---
# <a name="environment-overview"></a>環境の概要

Privileged Access Management は、共有ネットワーク上で互いに接続された個別のドライブを備えた仮想マシン (VM) と連携します。 これらの仮想マシンは、Windows 8.1、Windows Server 2012 R2、または他のオペレーティング システム プラットフォームでホストできます。

![PAM サーバー: リレーションシップとサポートされているプラットフォーム - 図](media/pam-test-lab-architecture.png)

少なくとも 3 台の仮想マシンを必要とします。  管理する PAM 用の AD ドメインをまだ持っていない場合は、CORP ドメイン コントローラーとして機能する追加の VM が 1 つ必要です。  高可用性のために PRIV ソフトウェアを構成する場合は、VM を 2 つ追加する必要があります。

VM のディスク イメージの格納先ドライブには、120 GB 以上の空きディスク領域が必要です。  高可用性を展開する予定がある場合は、ディスク サブシステムが SQL の共有記憶域の要件を満たしていることを確認します。  共有記憶域は、Windows Server フェールオーバー クラスタリングのクラスター ディスク、記憶域ネットワーク (SAN) 上のディスク、または SMB サーバー上のファイル共有の形式にすることができます。

>[!IMPORTANT]
記憶域は、要塞環境専用にする必要があります。 要塞環境の保全性が脅かされる可能性があるため、要塞環境以外の他のワークロードと記憶域を共有することは推奨されません。

## <a name="next-steps"></a>次のステップ

- 「[Active Directory Domain Services の Privileged Access Management](privileged-identity-management-for-active-directory-domain-services.md)」で、PAM の概要としくみをご説明します。
- 「[PAM のコンポーネントについて理解する](principles-of-operation.md)」で、PAM のさまざまなコンポーネントの概要をご説明します。