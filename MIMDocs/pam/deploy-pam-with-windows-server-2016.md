---
title: "Windows Server 2016 を使用した MIM Privileged Access Management の展開 | Microsoft Docs"
description: "Server 2016 を使用した Privileged Access Management の展開の詳細"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 02/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 18accbf24fc7af1a27e2e88059a9a8371dfd2c4d
ms.openlocfilehash: 49be7f3bd364e9202b177ead1fbe2607be91a323


---



# <a name="deploy-mim-pam-with-windows-server-2016"></a>Windows Server 2016 を使用した MIM PAM の展開


このシナリオでは、MIM 2016 SP1 で Windows Server 2016 の機能を “PRIV” フォレストのドメイン コントローラーとして利用できるようにします。  このシナリオを構成すると、ユーザーの Kerberos チケットは、ロールのアクティブ化の残りの時間に期間が限定されます。 

>[!Note]
Windows Server 2016 の Technical Preview 5 よりも前のテクニカル プレビューは、この MIM リリースには使用できません。

## <a name="preparation"></a>準備

ラボ環境には少なくとも&2; つの VM が必要です。

-   Windows Server 2016 を実行している、PRIV ドメイン コントローラーをホストする VM

-   Windows Server 2016 (推奨) または Windows Server 2012 R2 を実行している、MIM サービスをホストする VM

>[!NOTE]
ラボ環境内にまだ "CORP" ドメインがない場合は、そのドメイン用の追加のドメイン コントローラーが必要です。 “CORP” ドメイン コントローラーは、Windows Server 2016 または Windows Server 2012 R2 のいずれかを実行できます。


「[Getting started guide](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services.md)」(ファースト ステップ ガイド) の説明に従ってインストールを実行しますが、**以下の変更点があります**。

-   新しい CORP ドメインを作成する場合、「[Step 1 - Prepare the CORP domain controller](/microsoft-identity-manager/pam/step-1-prepare-corp-domain.md)」(手順 1 - CORP ドメイン コントローラーを準備する) の指示に従う際に、CORP ドメインを Windows Server 2016 の機能レベルになるように構成することもできます。 **このオプションを選択する場合は、以下の変更点に従ってください。**

    -   Windows Server 2016 メディアを使用する場合、このインストール オプションは、Windows Server 2016 (デスクトップ エクスペリエンス搭載サーバー) と呼ばれます。

    -   CORP フォレストおよびドメインに対して Windows Server 2016 の機能レベルを指定するには、次のように、Install-ADDSForest コマンドの引数でドメインおよびフォレストのバージョン番号として 7 を指定します。
     ```
        Install-ADDSForest –DomainMode 7 –ForestMode 7 –DomainName contoso.local –DomainNetbiosName contoso –Force –NoDnsOnNetwork
        ```
    -   "新しいユーザーとグループの作成" の最後のコマンド (New-ADGroup -name 'CONTOSO\$\$\$' …) は、**CORP ドメイン コントローラーと PRIV ドメイン コントローラーの両方が Windows Server 2016 ドメインの機能レベルである場合は必要ありません**。

    -   CORP ドメイン コントローラーと PRIV ドメイン コントローラーの両方が Windows Server 2016 ドメインの機能レベルである場合、"監査の構成" (項目 8) および "レジストリ設定の構成" (項目 10) で説明されている変更は**推奨されますが必須ではありません**。

-   CORPDC のオペレーティング システムとして Windows Server 2012 R2 を使用する場合は、修正プログラム 2919442、2919355、[および更新プログラム 3155495](http://support.microsoft.com/kb/3156418) を CORPDC にインストールする必要があります。

-   「[Step 2 - Prepare PRIV domain controller](/microsoft-identity-manager/pam/step-2-prepare-priv-domain-controller.md)」(手順 2 - PRIV ドメイン コントローラーを準備する) の指示に従いますが、以下の変更点があります。

    -   Windows Server 2016 メディアを使用してインストールします。 インストール オプションは、Windows Server 2016 (デスクトップ エクスペリエンス搭載サーバー) と呼ばれます。

    -   "ロールの追加" の指示 (項目 4) で、後で説明する Windows Server AD 機能を有効にするために、**PowerShell コマンドの 4 行目でドメインおよびフォレストのバージョン番号に 7 を指定する必要があります**。

        ```
        Install-ADDSForest -DomainMode 7 -ForestMode 7 -DomainName priv.contoso.local  -DomainNetbiosName priv -Force -CreateDNSDelegation -DNSDelegationCredential $ca
        ```  

    -   監査およびログオン権限を構成する際には、グループ ポリシーの管理プログラムが Windows 管理ツール フォルダーに配置されることに注意してください。

    -   SID 履歴の移行 (項目 8) に必要なレジストリ設定の構成は、**PRIV ドメインが Windows Server 2016 ドメインの機能レベルである場合には必要ありません**。

    -   委任を構成した後、サーバーを再起動する前に、管理者として PowerShell ウィンドウを起動して次のコマンドを入力することにより、Windows Server 2016 Active Directory で Privileged Access Management の機能を有効にします。

    ```
    $of = get-ADOptionalFeature -filter "name -eq 'privileged access management feature'"
    Enable-ADOptionalFeature \$of -scope ForestOrConfigurationSet -target "priv.contoso.local"
    ```

  -   委任を構成した後、サーバーを再起動する前に、MIM 管理者と MIM サービス アカウントに対してシャドウのプリンシパルの作成および更新を承認します。

     a. PowerShell ウィンドウを起動し、「ADSIEdit」と入力します。

     b. [操作] メニューの [接続先] をクリックします。 接続ポイントの設定で、名前付けコンテキストを [既定の名前付けコンテキスト] から [構成] に変更し、[OK] をクリックします。

     c. 接続した後、ウィンドウの左側で [ADSI エディター] の下にある [Configuration] ノードを展開し、[CN=Configuration,DC=priv,....] を表示します。 [CN=Configuration]、[CN=Services] の順に展開します。

     d. [CN=Shadow Principal Configuration] を右クリックし、[プロパティ] をクリックします。 プロパティのダイアログが表示されたら、[セキュリティ] タブに切り替えます。

     e. [Add] をクリックします。 アカウント “MIMService” と、後で New-PAMGroup を実行して追加の PAM グループを作成する他の MIM 管理者をすべて指定します。 各ユーザーの許可されている権限のリストで [書き込み]、[すべての子オブジェクトの作成]、および [すべての子オブジェクトの削除] を追加します。 アクセス許可を追加します。

     f. [セキュリティの詳細] の設定を変更します。 MIMService サービスへのアクセスを許可する行で、[編集] をクリックします。 [適用先] の設定を、[このオブジェクトとすべての子オブジェクト] に変更します。 このアクセス許可の設定を更新し、[セキュリティ] ダイアログ ボックスを閉じます。

     g. ADSI エディターを閉じます。

 -   委任を構成した後、サーバーを再起動する前に、MIM 管理者に対して認証ポリシーの作成および更新を承認します。

     a.  Powershell ウィンドウを起動して次のコマンドを入力し、4 つの行のそれぞれで “mimadmin” を MIM 管理者アカウントの名前に置き換えます。
    ```
       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicy /i:s

       dsacls "CN=AuthN Policies,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicy

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:RPWPRCWD;;msDS-AuthNPolicySilo /i:s

       dsacls "CN=AuthN Silos,CN=AuthN Policy
       Configuration,CN=Services,CN=configuration,DC=priv,DC=contoso,DC=local" /g
       mimadmin:CCDC;msDS-AuthNPolicySilo
    ```


-   「[Step 3 - Prepare a PAM server](/microsoft-identity-manager/pam/step-3-prepare-pam-server.md)」(手順 3 - PAM サーバーを準備する) の指示に従いますが、以下の変更点があります。

    -   Windows Server 2016 にインストールする場合は、"ApplicationServer" ロールを使用できません。

    -   Windows Server 2016 に MIM をインストールする場合は、**SharePoint 2013 をインストールすることはできません**。

-   「[Step 4 – Install MIM components on PAM server and workstation](/microsoft-identity-manager/pam/step-4-install-mim-components-on-pam-server.md)」(手順 4 – PAM サーバーとワークステーションに MIM コンポーネントをインストールする) の指示に従いますが、以下の変更点があります。

    -   MIM のインストールによって “PAM オブジェクト” という新しい AD OU が作成されるため、MIM サービスと PAM コンポーネントをインストールするユーザーには **AD の PRIV ドメインに対する書き込みアクセス権が必要です**。

    -   SharePoint がインストールされていない場合は、MIM ポータルをインストールしないでください。

-   「[Step 5 - Establish trust](/microsoft-identity-manager/pam/step-5-establish-trust-between-priv-corp-forests.md)」(手順 5 - 信頼関係を確立する) の指示に従いますが、以下の変更点があります。

    -   一方向の信頼関係を確立する際は、最初の&2; つの PowerShell コマンド (get-credential および New-PAMTrust) のみを実行し、**New-PAMDomainConfiguration コマンドは実行しないでください**。

    -   信頼関係を確立した後、PRIV\\Administrator として PRIVDC にログオンし、PowerShell を起動して次のコマンドを入力します。
  ```
    netdom trust contoso.local /domain:priv.contoso.local /enablesidhistory:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1

     netdom trust contoso.local /domain:priv.contoso.local /quarantine:no
     /usero:contoso\\administrator /passwordo:Pass\@word1  

     netdom trust contoso.local /domain:priv.contoso.local /enablepimtrust:yes
     /usero:contoso\\administrator /passwordo:Pass\@word1
  ```

-   項目 5 (信頼関係の確認) は、**CORP ドメインと PRIV ドメインの両方が Windows Server 2016 ドメインの機能レベルである場合は必要ありません**。

## <a name="more-information"></a>詳細情報

- [Active Directory Domain Services の Privileged Access Management](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services.md)
- [Privileged Access Management の MIM 環境の構成](/microsoft-identity-manager/pam/configuring-mim-environment-for-pam.md)
- [スクリプトを使用した PAM の構成](/microsoft-identity-manager/pam/sp1-pam-configure-using-scripts.md)



<!--HONumber=Feb17_HO3-->


