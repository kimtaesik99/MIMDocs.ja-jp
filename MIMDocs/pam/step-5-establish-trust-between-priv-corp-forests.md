---
title: "PAM の展開、手順 5 – フォレスト リンク | Microsoft Docs"
description: "PRIV 内の特権を持つユーザーが CORP 内のリソースに引き続きアクセスできるように、PRIV フォレストと CORP フォレストとの間に信頼関係を確立します。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 16208efe08c5a2c0f63ee121c64c45cad5a73909


---

# <a name="step-5-establish-trust-between-priv-and-corp-forests"></a>手順 5 - PRIV フォレストと CORP フォレスト間に信頼関係を確立する

>[!div class="step-by-step"]
[«手順 4](step-4-install-mim-components-on-pam-server.md)
[手順 6 »](step-6-transition-group-to-pam.md)


Contoso.local などの CORP ドメインごとに、PRIV と CONTOSO のドメイン コントローラーを信頼関係でバインドする必要があります。 これにより、PRIV ドメインのユーザーが CORP ドメインのリソースにアクセスできるようになります。

## <a name="connect-each-domain-controller-to-its-counterpart"></a>各ドメイン コントローラーを対応するドメイン コントローラーに接続する

信頼関係を確立する前に、各ドメイン コントローラーで、相手側のドメイン コントローラーと DNS サーバーの IP アドレスに基づいて、相手側の DNS 名解決を構成する必要があります。

1.  MIM ソフトウェアを備えたドメイン コントローラーまたはサーバーが仮想マシンとして展開されている場合は、これらのコンピューターにドメイン名前付けサービスを提供している他の DNS サーバーがないことを確認します。
    - パブリック ネットワークに接続されているネットワーク インターフェイスなど、複数のネットワーク インターフェイスが仮想マシンにある場合は、一時的にそれらの接続を無効にするか、Windows のネットワーク インターフェイス設定を上書きする必要があります。 DHCP で提供された DNS サーバー アドレスがどの仮想マシンでも使用されていないことを必ず確認してください。

2.  既存の CORP ドメイン コントローラーが名前を PRIV フォレストにルーティングできることを確認します。 CORPDC などの PRIV フォレスト外部の各ドメイン コントローラーで、PowerShell を起動し、次のコマンドを入力します。

    ```
    nslookup -qt=ns priv.contoso.local.
    ```
    出力が、正しい IP アドレスを持つ PRIV ドメインのネームサーバー レコードを示すことを確認します。

3.  ドメイン コントローラーが PRIV ドメインをルーティングできない場合は、**DNS マネージャー** (**[開始]** > **[アプリケーション ツール]** > **[DNS]** にある) を使用して、PRIV ドメインから PRIVDC の IP アドレスへの DNS 名転送を構成します。 ドメイン コントローラーが上位ドメイン (例: contoso.local) である場合は、このドメイン コントローラーとそのドメインのノードを **[CORPDC]** > **[前方参照ゾーン]** > **[contoso.local]** のように展開して、ネーム サーバー (NS) の種類として **priv** というキーが表示されていることを確認します。

    ![priv キーのファイル構造 - スクリーンショット](./media/PAM_GS_DNS_Manager.png)

## <a name="establish-trust-on-pamsrv"></a>PAMSRV で信頼関係を確立する

PAMSRV で、CORPDC などの各ドメインとの一方向の信頼関係を確立し、CORP ドメイン コントローラーが PRIV フォレストを信頼するようにします。

1. PRIV ドメイン管理者 (PRIV\Administrator) として PAMSRV にサインインします。

2.  PowerShell を起動します。

3.  既存の各フォレストに対して、次の PowerShell コマンドを入力します。 CORP ドメイン管理者 (CONTOSO\Administrator) の資格情報を求める画面が表示されたら、資格情報を入力します。

    ```
    $ca = get-credential
    New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
    ```

4.  既存のフォレストの各ドメインに対して、次の PowerShell コマンドを入力します。 CORP ドメイン管理者 (CONTOSO\Administrator) の資格情報を求める画面が表示されたら、資格情報を入力します。

    ```
    $ca = get-credential
    New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
    ```

## <a name="give-forests-read-access-to-active-directory"></a>フォレストに Active Directory への読み取りアクセス権を付与する

既存のフォレストごとに、PRIV 管理者と監視サービスによる AD に対する読み取りアクセス権を有効にします。

1.  既存の CORP フォレスト ドメイン コントローラー (CORPDC) に、そのフォレスト (Contoso\Administrator) のトップレベル ドメインのドメイン管理者としてサインインします。  
2.  **[Active Directory ユーザーとコンピューター]**を起動します。  
3.  ドメイン **contoso.local** を右クリックし、**[制御の委任]** を選択します。  
4.  [選択されたユーザーとグループ] タブで、**[追加]**をクリックします。  
5.  [ユーザー、コンピューター、またはグループの選択] ウィンドウで、**[場所]** をクリックし、場所を *priv.contoso.local* に変更します。  オブジェクト名に「*Domain Admins*」と入力し、**[名前の確認]**をクリックします。 ポップアップが表示されたら、ユーザー名に「*priv\administrator*」と入力し、パスワードを入力します。  
6.  「Domain Admins」の後に「*; MIMMonitor*」を追加します。 **Domain Admins** と **MIMMonitor** の名前に下線が表示されたら、**[OK]** をクリックし、**[次へ]** をクリックします。  
7.  一般的なタスクの一覧で、**[すべてのユーザー情報の読み取り]** を選択し、**[次へ]**、**[完了]** の順にクリックします。  
8.  [Active Directory ユーザーとコンピューター] を閉じます。

9.  PowerShell ウィンドウを開きます。  
10.  `netdom` を使用して、SID 履歴が有効で、SID フィルターが無効であることを確認します。 次のように入力します。  
    ```
    netdom trust contoso.local /quarantine /domain priv.contoso.local
    netdom trust /enablesidhistory:yes /domain priv.contoso.local
    ```
    出力に、"**この信頼に対して SID 履歴を有効にしています** または "**SID 履歴はこの信頼に対して既に有効になっています**" という内容が示されるはずです。

    また、出力には "**SID フィルタはこの信頼に対して有効になっていません**" という内容も示されるはずです。 詳細については、「[SID フィルター検疫を無効にする](http://technet.microsoft.com/library/cc772816.aspx)」をご参照ください。

## <a name="start-the-monitoring-and-component-services"></a>監視サービスとコンポーネント サービスを開始する

1.  PRIV ドメイン管理者 (PRIV\Administrator) として PAMSRV にサインインします。

2.  PowerShell を起動します。

3.  次の PowerShell コマンドを入力します。

    ```
    net start "PAM Component service"
    net start "PAM Monitoring service"
    ```

次の手順で、グループを PAM に移動します。

>[!div class="step-by-step"]
[«手順 4](step-4-install-mim-components-on-pam-server.md)
[手順 6 »](step-6-transition-group-to-pam.md)



<!--HONumber=Nov16_HO2-->


