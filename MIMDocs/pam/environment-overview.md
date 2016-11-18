---
title: "PAM 環境の概要 | Microsoft Docs"
description: "Privileged Access Management を正常に展開するために必要な仮想マシンの数と構成を確認する"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 750ba152813b4c03c913453c42e0e503e90b883b


---

# <a name="environment-overview"></a>環境の概要

Privileged Access Management は、共有ネットワーク上で互いに接続された個別のドライブを備えた仮想マシン (VM) と連携します。 これらの仮想マシンは、Windows 8.1、Windows Server 2012 R2、または他のオペレーティング システム プラットフォームでホストできます。

![PAM サーバー: リレーションシップとサポートされているプラットフォーム - 図](media/pam-test-lab-architecture.png)

少なくとも 3 台の仮想マシンが必要になります。  管理する PAM 用の AD ドメインをまだ持っていない場合は、CORP ドメイン コントローラーとして機能する追加の VM が 1 つ必要です。  高可用性のために PRIV ソフトウェアを構成する場合は、2 つの追加の VM も必要です。

仮想マシンのディスク イメージの格納先ドライブには、すべての仮想マシンを保持するために 120 GB 以上の空きディスク領域が必要です。  高可用性を展開する予定がある場合は、ディスク サブシステムが SQL の共有記憶域の要件を満たしていることを確認します。  共有記憶域は、Windows Server フェールオーバー クラスタリングのクラスター ディスク、記憶域ネットワーク (SAN) 上のディスク、または SMB サーバー上のファイル共有の形式にすることができます。 なお、これらは要塞環境専用にしてください。要塞環境の保全性が脅かされる可能性があるため、要塞環境以外の他のワークロードと記憶域を共有することは推奨されません。

> [!NOTE]
> 現在の MIM のカスタマー テクニカル プレビュー (CTP) は、以前の CTP のデータベースまたはディレクトリの内容と互換性がありません。 以前に PAM またはその他のシナリオに対して MIM を評価している場合、そのテストで使用した仮想マシンをバックアップしてアーカイブし、これまでに MIM シナリオに使用していない新しい仮想マシン イメージで展開を開始してください。



<!--HONumber=Nov16_HO2-->


