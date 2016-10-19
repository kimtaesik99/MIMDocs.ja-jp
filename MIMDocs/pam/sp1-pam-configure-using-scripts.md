---
title: "スクリプトを使用した PAM の構成"
description: "スクリプトによって、Privileged Identity Manager で管理する既存の ID または新規の ID を使用して CORP ドメインを準備する"
keywords: 
author: barclayn
manager: MBaldwin
ms.date: 10/04/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: a50528778379afac14ff9abd70c7b8d56df8f5bf
ms.openlocfilehash: 971bd5f166edbb2c58cbfe7c9f787c76cb8c7f92


---

# スクリプトを使用した PAM の構成

別々のサーバーに SQL および SharePoint をインストールする場合は、次の手順に従って構成する必要があります。 SQL、SharePoint および PAM コンポーネントが同じコンピューターにインストールされている場合、次の手順は、そのコンピューターから実行する必要があります。

次の手順では、PRIV ドメインが既にセットアップされていることを前提としています。PRIV ドメインを構成する手順については、ドキュメントの最後の補遺を確認してください。

手順:

1. すべてのコンピューターで、圧縮ファイル "PAMDeploymentScripts.zip" を %SYSTEMDRIVE%\PAM フォルダーに解凍します。
2. いずれかのマシンで、**PAMDeploymentConfig.xml** ファイルを開き、次の表またはこの XML ファイル自体に記載されたガイダンスを使用して、詳細を更新します。 CORP および PRIV フォレストは既にセットアップされているため、更新する必要があるのは、**DNSName** と **NetbiosName** だけです。
3. Roles セクションで、**サービス アカウント**、**コンピューターの詳細**、および SQL、SharePoint および MIM の役割の**インストール バイナリの場所**を更新します。
    1. MIM バイナリの場所は、"Service and Portal" フォルダーが含まれるディレクトリを指す必要があります。 クライアント バイナリの場所は、"Add-ins and Extensions.msi" フォルダーが含まれるディレクトリを指す必要があります。

4. PRIVOnly 環境の場合は、PRIVOnly タグを True に設定する必要があります。
    1. PRIVOnly 環境では、PRIV ドメインの **DNSName** と **NetbiosName** を更新して、CORP ドメインと一致させます。 既定のテンプレート ファイルは CORP および PRIV 構成を前提としているため、SQL、SharePoint、および MIM がインストールされるコンピューターに対して、コンピューターのサフィックスが正しいことを確認します。
    2. PRIVOnly 環境の詳細についてはここをクリックします。

5. 同じ PAMDeploymentConfig.xml をすべてのコンピューター、CORPDC、PRIVDC、PAM サーバー、SQL Server および SharePoint サーバー上の %SYSTEMDRIVE%\PAM フォルダーにコピーします。


## 展開ワークシート

続行する前に、PAMDeploymentConfig.xml を更新し、更新されたコピーをすべてのコンピューターに配置します。

### Setup

|マシン   | 次のユーザーとして実行   |コマンド   |
|---|---|---|
|  PRIVDC |PRIV ドメイン管理者   | .\PAMDeployment.ps1 メニュー オプション 1 を選択 (PRIV フォレスト構成)   |
|   |   |  上記の手順では、SIDs.txt が生成されます。 このファイルは、次の手順を実行する前に CORPDC の $envDrive:PAM にコピーする必要があります。 |
| CORPDC  |CORP ドメイン管理者   | .\PAMDeployment.ps1 メニュー オプション 2 を選択 (CORP フォレスト構成)   |
| PAMServer (または SQL Server)   |CORP ドメイン管理者   |  .\PAMDeployment.ps1 メニュー オプション 2 を選択 (CORP フォレスト構成)  |
|  PAMServer |  ローカル管理者 (ドメインへの参加後は MIM 管理者) |  .\PAMDeployment.ps1 メニュー オプション 4 を選択 (SharePoint セットアップ)  |
| PAMServer  | ローカル管理者 (ドメインへの参加後は MIM 管理者)  | .\PAMDeployment.ps1 メニュー オプション 5 を選択 (MIM PAM セットアップ)   |
|  PAMServer |MIMAdmin   | .\PAMDeployment.ps1 メニュー オプション 6 を選択 (PAM 信頼セットアップ) .\PAMDeployment.ps1 メニュー オプション 6 を選択 (PAM 信頼セットアップ) |

### Validation

|  マシン | 次のユーザーとして実行   | コマンド   |
|---|---|---|
| CORPClient  | CORP ユーザー (ローカル管理者)  |   .\PAMDeployment.ps1 メニュー オプション 7 を選択 (MIM PAM クライアント セットアップ)  |
| CORPDC  | CORP ドメイン管理者   | Import-module .\PAMValidation.psm1、Create-PAMValidationCORPDCConfig   |
| PAMServer   | MIMAdmin  | Import-module .\PAMValidation.psm1、Move-PAMValidationUsersToPAM  |
| CORPClient  | CORP ユーザー (ローカル管理者)   |   Import-module .\PAMValidation.psm1、Enable-PAMUsersCORPClientRemote |
|  CORPClient | <PRIV>\PRIV.pamRequestor ユーザーおよび PRIVOnly の場合: <CORP>\pamrequestor   | Import-module .\PAMValidation.psm1、Test-PAMValidationScenarioNoApprovalRequest  |


>[!div class="step-by-step"]
[開始 »](sp1-step1-configuring-priv-domain.md)



<!--HONumber=Oct16_HO1-->


