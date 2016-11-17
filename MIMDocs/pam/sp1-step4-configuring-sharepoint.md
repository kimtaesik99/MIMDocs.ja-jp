---
title: "手順 4. SharePoint の構成"
description: "スクリプトによって、Privileged Identity Manager で管理する既存の ID または新規の ID を使用して CORP ドメインを準備する"
keywords: 
author: barclayn
ms.author: barclayn
manager: MBaldwin
ms.date: 10/25/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: 
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 365989693f844f117f76ee2b69db85df82f06f35
ms.openlocfilehash: 76696f7dce3d79a845c2a8ba9ae8d284012a0df7


---

# <a name="step-4-configuring-sharepoint"></a>手順 4. SharePoint の構成

>[!div class="step-by-step"]
[«手順 3](sp1-step3-installing-configuring-sql.md)
[手順 5 »](sp1-step5-configuring-pam.md)

SharePoint は、SP1 を適用した SharePoint Foundation 2013 である必要があります。

ドメインに参加しているサーバーの場合、MIMAdmin としてログイン

1. 管理者として PowerShell を実行
2.  .\PAMDeployment.ps1
3.  メニュー オプション 4 を選択 (SharePoint セットアップ)


ワークグループ サーバーの場合

1. 管理者として PowerShell を実行
2.  cd $env:path: SYSTEMDRIVE\PAM
3.  .\PAMDeployment.ps1
4. メニュー オプション 4 を選択 (SharePoint セットアップ)

SharePoint をインストールする過程で、コンピューターは何回か再起動します。 毎回 SharePoint のセットアップを再実行し、MIMAdmin アカウントでログインする必要があります。
SharePoint をインストールするコンピューターのインターネット接続が、前提条件のファイルをダウンロードするのに不十分な場合は、個別にダウンロードして、ローカル フォルダーに配置できます。 **このローカル フォルダーのパスは <PrerequisitesBinaryLocation/> の PAMConfiguration.xml ファイルで更新する必要があります。**  ファイルをダウンロードするリンクについては、補遺 5 を参照してください。
インストール後に、SharePoint 構成 GUI が開いて、SharePoint のインストールを完了する手順を紹介します。 完全なサーバーを選択し、残りの UI に従ってください。 インストールが完了したら、構成ウィザードを実行するよう求められます。 次に指定されている手順を完了します。

1. **[サーバー ファームへの接続]** タブで、**[新しいサーバー ファームの作成]** に移動します。
2. 構成データベース用のデータベース サーバーとして **SQLServer** を指定し、SharePoint で使用するデータベース アクセス アカウントとして **SharePoint ServiceAccount** を指定します。
3. ファーム セキュリティ パスフレーズとしてパスワードを指定します **(このチュートリアルの後半では使用しません)。**
4. SharePoint 構成ウィザードの残りの既定の設定をそのまま使うことにより、シングル サーバー ファームを作成します。

詳細については、「[Step 3 - Prepare a PAM server (手順 3. PAM サーバーの準備)](/microsoft-identity-manager/pam/step-3-prepare-pam-server)」の「**Configure SharePoint (SharePoint の構成)**」セクションを参照してください。完了したら、".\PAMDeployment.ps1" スクリプトを実行し、オプション 4 (SharePoint セットアップ) を選択してこの手順を完了します。

>[!div class="step-by-step"]
[«手順 3](sp1-step3-installing-configuring-sql.md)
[手順 5 »](sp1-step5-configuring-pam.md)



<!--HONumber=Nov16_HO2-->


