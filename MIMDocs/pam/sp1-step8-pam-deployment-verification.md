---
title: "手順 8. PAM 展開の検証"
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
ms.sourcegitcommit: 99b1ff9f622ddd357866b2a3f9f4cc8e0fc88005
ms.openlocfilehash: 9a617d8a5fbe8bcdac40cdf3250e5efedb7a0b84


---

# 手順 8. PAM 展開の検証

展開パッケージには検証スクリプトが付属していて、PAM シナリオを実行して PAM 展開が想定どおりに動作していることを検証できます。
展開の検証を使用するには、<PamValidation/> という PAMDeploymentConfig.xml セクションを変更します。

>[!NOTE]
>検証には、クライアント コンピューターのドメインが、PAM クライアント側コンポーネントがインストールされた CORP ドメインに参加している必要があります。 クライアントをインストールする方法に関するスクリプトの補遺を参照してください。

PAMDeploymentConfig.xml の <PAMValidationClient/> タグのクライアント コンピューター名を更新する必要があります。この検証ではユーザー/グループの作成が試行されるため、<PAMValidation/> ノードの残りのデータは既存のユーザー/グループと競合する場合にのみ編集する必要があります。
検証を実行するには、次の手順に従います。

手順 1.

1. CORP ドメイン管理者として CORPDC にログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. Import-module .\PAMValidation.psm1
5. Create-PAMValidationonCORPDCConfig

これによって、検証に必要なグループおよびユーザーが作成されます。

手順 2: 

1. MIMAdmin として PAM サーバーにログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. move-PAMVAlidationUsersToPAM

この手順では、PAM 環境にユーザーとグループを移行します。

手順 3: 

1. ローカル管理者として CORP クライアントにログイン
2. 管理者として PowerShell を実行
3. cd $env:path: SYSTEMDRIVE\PAM
4. import-module .\PAMValidation.psm1
5. Enable-PAMUsersCORPClientRemote


この手順では、CORPAdmin 資格情報を求められます。 指定すると、必要なユーザーが "Remote Desktop Users" および "Remote Management Users" グループに追加されます。
CORP クライアントで、次のコマンドを使用して、検証する PRIV ユーザーとして PowerShell を開きます。 </br></br>
**Runas/u:<PRIV domain>\PRIV.pamRequestor powershell.exe**  </br></br>
PowerShell ウィンドウから次のように入力します。

1. cd $env:path: SYSTEMDRIVE\PAM
2. import-module .\PAMValidation.psm1
3. test-PAMValidationScenarioNoApprovalRequest


  これにより、要求の状態が表示されます。
  最初、このユーザーにはリソースへのアクセス権がありません。 ユーザーがジャスト イン タイムでロールに追加されると、ユーザーにアクセス権が付与されます。 要求が期限切れになると、再度ユーザーはアクセス権を失います。
  スクリプトでは、既定値 (11 分) を使用して要求を期限切れにします。

>[!div class="step-by-step"]
[« 手順 7](sp1-step7-setup-sidhistory-sidfiltering.md)
[補遺 »](sp1-pam-deployment-addendum.md)



<!--HONumber=Oct16_HO1-->


