---
# required metadata

title: MIM Certificate Manager の操作 |Microsoft Identity Manager
description: Certificate Manager アプリを展開して、ユーザーが独自のアクセス権を管理できるようにする方法について説明します。 
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 66060045-d0be-4874-914b-5926fd924ede

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# MIM Certificate Manager の操作
MIM 2016 と Certificate Manager を起動して実行している場合、MIM Certificate Manager Windows ストア アプリケーションをデプロイして、ユーザーが物理スマート カード、仮想スマート カード、およびソフトウェアの証明書を簡単に管理できるようにすることができます。 MIM CM アプリを展開する手順は次のとおりです。

1.  証明書テンプレートを作成する。

2.  プロファイル テンプレートを作成する。

3.  アプリを準備する。

4.  SCCM または Intune を使用してアプリを展開する。

## 証明書テンプレートを作成する
CM アプリに証明書テンプレートを作成します。通常と同じ方法で作成できますが、証明書テンプレートがバージョン 3 以降であることを確認する必要があります。

1.  AD CS (証明書サーバー) を実行しているサーバーにログインします。

2.  MMC を開きます。

3.  **[ファイル]、[スナップインの追加と削除]** の順にクリックします。

4.  [利用できるスナップイン] の一覧で、**[証明書テンプレート]** をクリックして、**[追加]** をクリックします。

5.  MMC の **[コンソール ルート]** の下に、 **[証明書テンプレート]** が表示されます。 ダブル クリックすると、すべての利用可能な証明書テンプレートが表示されます。

6.  **[スマートカード ログオン]** テンプレートを右クリックして、**[テンプレートの複製]** をクリックします。

7.  [互換性] タブの [証明機関] の下で [Windows Server 2008] を選択し、[証明書の受信者] の下で [Windows 8.1/Windows Server 2012 R2] を選択します。
    これは、バージョン 3 (以降) の証明書テンプレートがあることを確認し、Certificate Manager アプリでバージョン 3 のみを確実に使用するために非常に重要な手順です。 バージョンは証明書テンプレートを初めて作成して保存するときに設定されるため、この方法で証明書テンプレートを作成していない場合は、正しいバージョンに変更する方法はなく、続行する前に、新しいテンプレートを作成する必要があります。

8.  **[全般]** タブの **[表示名]** フィールドで、アプリの UI に表示する名前 ( **仮想スマート カード ログオン**など) を入力します。

9. **[要求処理]** タブで、 **[目的]** を **[署名と暗号化]** に設定し、**[実行する処理…]** の下で **[登録中にユーザーにメッセージを表示する]**を選択します。

10. **[暗号化]** タブの **[プロバイダーのカテゴリ]**の下で、 **[キー記憶域プロバイダーと要求は対象のコンピューターで使用可能な任意のプロバイダーを使用できる]**を選択します。

    > [!NOTE]
    > キー記憶域プロバイダーがオプションとして表示されるのは、テンプレートがバージョン 3 の場合だけです。 表示されない場合は、正しいバージョンで証明書テンプレートが正しく作成されていない可能性があります。 上記の手順 5 を最初から行ってください。

11. **[セキュリティ]** タブで、 **[登録]** アクセス権を付与するセキュリティ グループを追加します。 たとえば、すべてのユーザーにアクセス権を付与する場合は、 **[Authenticated users]** グループを選択し、そのグループに **[登録]** アクセス許可を選択します。

12. **[OK]** をクリックして変更を完了し、新しいテンプレートを作成します。 証明書テンプレートの一覧に、新しいテンプレートが表示されます。

13. **[ファイル]** を選択し、**[スナップインの追加と削除]** をクリックして、証明機関スナップインを MMC コンソールに追加します。 管理するコンピューターを尋ねられたら、 **[ローカル コンピューター]**を選択します。

14. MMC の左側のウィンドウで **[証明機関 (ローカル)]** を展開し、証明機関の一覧内に自身の CA を展開します。

15. **[証明書テンプレート]** を右クリックし、**[新規作成]、[発行する証明書テンプレート]** の順にクリックします。

16. 一覧から作成した新しいテンプレートを選択し、 **[OK]**をクリックします。

## プロファイル テンプレートを作成する
プロファイル テンプレートを作成する際に、必ずテンプレートを vSC の作成/破棄およびデータ コレクションの削除に設定します。 CM アプリは収集したデータを処理できないため、次の手順で無効にすることが重要です。

1.  管理者特権を持つユーザーとして CM ポータルにログインします。

2.  [管理]、[プロファイル テンプレートの管理] の順に選択し、MIM CM サンプル スマート カードのログオン プロファイル テンプレート の横にあるチェック ボックスをオンにして、選択したプロファイル テンプレートの [コピー]をクリックします。

3.  プロファイル テンプレートの名前を入力し、 **[OK]**をクリックします。

4.  次の画面で、 **[新しい証明書テンプレートを追加]** をクリックして、CA 名の横にあるチェック ボックスがオンになっていることを確認します。

5.  プロファイル テンプレート名 **[Logon]** の横にあるチェック ボックスをオンにして、 **[追加]**をクリックします。

6.  SmartCardLogon テンプレートを削除するには、テンプレートの横にあるチェック ボックスをオンにしてから、 **[選択した証明書テンプレートの削除]** 、 **[OK]**の順にクリックします。

7.  一番下までスクロールして、 **[設定の変更]**をクリックします。

8.  **[仮想スマート カードの作成/破棄]** と **[管理者キーの分散]** の横のチェック ボックスをオンにします。

9. **[ユーザー暗証番号ポリシー]** の下で、 **[ユーザー指定]**を選択します。

10. 左側のウィンドウで、 **[書き換えポリシー] &gt; [全般設定の変更]**をクリックします。 **[書き換えたカードの再利用]** を選択し、 **[OK]**をクリックします。

11. それぞれすべてのポリシーに対するデータ収集項目を無効にする必要があります。これを行うには、左側のウィンドウでポリシーをクリックしてから、 **[サンプル データ項目]** の横のチェック ボックスをオンにして、 **[データ収集項目の削除]**をクリックします。 **[OK]**をクリックします。

## CM アプリの展開を準備する

1.  コマンド プロンプトで次のコマンドを実行して、アプリをアンパックして、appx という名前の新しいサブフォルダーにコンテンツを展開し、元のファイルを変更しないように、コピーを作成します。

    ```
    makeappx unpack /l /p <app package name>.appx /d ./appx
    ren <app package name>.appx <app package name>.appx.original
    cd appx
    ```

2.  appx フォルダー内で、CustomDataExample.xml ファイルの名前を Custom.data に変更します。

3.  Custom.data ファイルを開き、必要に応じてパラメーターを変更します。

    |||
    |-|-|
    |MIMCM URL|CM の構成に使用したポータルの FQDN です。 例: https://mimcmServerAddress/certificatemanagement|
    |ADFS URL|AD FS を使用する場合は、自身の AD FS URL を挿入します。 例: https://adfsServerSame/adfs|
    |PrivacyUrl|証明書の登録のために収集されたユーザーの詳細の用途を説明する Web ページへの URL を含めることができます。|
    |SupportMail|サポートの問題のために電子メール アドレスを含めることができます。|
    |LobComplianceEnable|これは true または false に設定できます。 既定では true に設定されています。|
    |MinimumPinLength|既定では 6 に設定されています。|
    |NonAdmin|これは true または false に設定できます。 既定では false に設定されています。 これは、コンピューター上の管理者ではないユーザーが証明書の登録と更新をできるようにする場合にのみ変更します。|

4.  ファイルを保存してエディターを終了します。

5.  パッケージに署名すると、署名ファイルが 1 つ作成されるため、AppxSignature.p7x という名前の元の署名ファイルを削除する必要があります。

6.  AppxManifest.xml ファイルは、署名証明書のサブジェクト名を指定します。 このファイルを開いて編集します。

7.  このセクションを開始する前に、署名証明書を取得する必要があります。 以下の、「MIM 2016 Certificate Manager で管理者以外のスマートカードの更新を有効にする」の手順 1 を参照してください。

8.  &lt;Identity&gt; 要素の Publisher 属性値を、自身の署名証明書に一覧表示されているサブジェクトと同じ (“CN=SUBJECT” など) になるように変更します。

9. ファイルを保存してエディターを終了します。

10. コマンド プロンプトで次のコマンドを実行し、.appx ファイルを再パックして署名します。

    ```
    cd ..
    makeappx pack /l /d .\appx /p <app package name>.appx
    ```
    ここで、アプリのパッケージ名は、コピーを作成したときに使用したのと同じ名前です。

    ```
    signtool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.ap
    px
    ```
    これにより新しい .appx ファイルが提供されます。 pfx ファイルは、署名証明書の場所と、.pfx ファイルのパスワードを提供します。

11. AD FS 認証を使用するには:

    -   仮想スマート カード アプリケーションを開きます。 これにより、次の手順に必要な値が見つけやすくなります。

    -   アプリケーションをクライアントとして AD FS サーバーに追加して、サーバー上で CM を構成するには、AD FS サーバーで Windows PowerShell を開き、コマンド `ConfigureMimCMClientAndRelyingParty.ps1 –redirectUri <redirectUriString> -serverFQDN <MimCmServerFQDN>`を実行します。

        ConfigureMimCMClientAndRelyingParty.ps1 スクリプトを以下に示します。

        ```
        # HELP

        <#
        .SYNOPSIS
                        Configure ADFS for CM client app and server.
        .DESCRIPTION
           What the Script does:
                                        a. Registers the MIM CM client app on the ADFS server.
                                        b. Registers the MIM CM server relying party (Tells the ADFS server that it issues tokens for the CM server).
                        For parameter information, see 'get-help -detailed'
        .PARAMETER redirectUri
                        The redirectUri for CM client app. Will be added to ADFS server.
                        It can be found as follows:
                        1. Open the settings panel. Under settings, there is a "Redirect Uri" text box (an ADFS server address must be configured in order for the text to display).
                        2. Open MIM CM client app. Navigate to 'C:\Users\<your_username>\AppData\Local\Packages\CmModernAppv.<version>\LocalState', and open 'Logs_Virtual Smart Card Certificate Manager_<version>'. Search for "Redirect URI".
        .PARAMETER serverFqdn
                        Your deployed MIM CM server’s FQDN.
        .EXAMPLE
           .\ConfigureMimCMClientAndRelyingParty.ps1 -redirectUri ms-app://s-1-15-2-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789-0123456789/ -serverFqdn WIN-TRUR24L4CFS.corp.cmteam.com
        #>

        # Parameter declaration
        [CmdletBinding()]
        Param(
          [Parameter(Mandatory=$True)]
           [string]$redirectUri,

           [Parameter(Mandatory=$True)]
           [string]$serverFqdn
        )

        Write-Host "Configuring ADFS Objects for OAuth.."

        #Configure SSO to get persistent sign on cookie
        Set-ADFSProperties -SsoLifetime 2880

        #Configure Authentication Policy
        #Intranet to use Kerberos
        #Extranet to use U/P

        #Create Client Objects

        Write-Host "Creating Client Objects..."

        $existingClient = Get-ADFSClient -Name "MIM CM Modern App"

        if ($existingClient -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Modern App, removing"
            Remove-ADFSClient -TargetName "MIM CM Modern App"
            Write-Host "Client object removed"
        }

        Write-Host "Adding Client Object for MIM CM Modern App client"
        Add-ADFSClient -Name "MIM CM Modern App" -ClientId "70A8B8B1-862C-4473-80AB-4E55BAE45B4F" -RedirectUri $redirectUri
        Write-Host "Client Object for MIM CM Modern App client Created"

        #Create Relying Parties
        Write-Host "Creating Relying Party Objects"

        $existingRp = Get-ADFSRelyingPartyTrust -Name "MIM CM Service"
        if ($existingRp -ne $null)
        {
            Write-Host "Found existing instance of the MIM CM Service RP, removing"
            Remove-ADFSRelyingPartyTrust -TargetName "MIM CM Service"
            Write-Host "RP object Removed"
        }

        $authorizationRules =
        "@RuleTemplate = `"AllowAllAuthzRule`"
        => issue(Type = `"http://schemas.microsoft.com/authorization/claims/permit`", Value = `"true`");"

        $issuanceRules =
        "@RuleTemplate = `"LdapClaims`"
        @RuleName = `"Emit UPN and common name`"
        c:[Type == `"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`", Issuer == `"AD AUTHORITY`"]
        => issue(store = `"Active Directory`", types =
        (`"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`",
        `"http://schemas.xmlsoap.org/claims/CommonName`"), query =
        `";userPrincipalName,cn;{0}`", param = c.Value);

        @RuleTemplate = `"PassThroughClaims`"
        @RuleName = `"Pass through Windows Account Name`"
        c:[Type ==`"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`"] => issue(claim = c);"

        Write-Host "Creating RP Trust for MIM CM Service"
        Add-ADFSRelyingPartyTrust -Name "MIM CM Service" -Identifier ("https://"+$serverFqdn+"/certificatemanagement") -IssuanceAuthorizationRules $authorizationRules -IssuanceTransformRules $issuanceRules
        Write-Host "RP Trust for MIM CM Service has been created"
        ```

    -   redirectUri と serverFQDN の値を更新します。

    -   仮想スマート カード アプリケーションで redirectUri を検索するには、アプリケーションの設定パネルを開き、 **[設定]**をクリックすると、リダイレクト URI が、AD FS サーバーのアドレス バーの下に一覧表示されます。 URI は、ADFS サーバーのアドレスが構成されている場合にのみ表示されます。

    -   ServerFQDN は MIMCM サーバーのフル コンピューター名のみです。

    -    **ConfigureMIimCMClientAndRelyingParty.ps1** のスクリプトのヘルプについては、 `get-help  -detailed ConfigureMimCMClientAndRelyingParty.ps1`を実行します。

## アプリを展開する
CM アプリをセットアップする際には、ダウンロード センターでファイル MIMDMModernApp_&lt;version&gt;_AnyCPU_Test.zip をダウンロードして、すべてのコンテンツを展開します。 .appx ファイルはインストーラーです。 Windows ストア アプリを展開する通常の方法で展開できます。[System Center Configuration Manager](https://technet.microsoft.com/library/dn613840.aspx)を使用したり、[Intune](https://technet.microsoft.com/library/dn613839.aspx) を使用してアプリをサイドロードして、ユーザーがポータル サイトを使用してアクセスしなければならないようにしたり、ユーザーが自身のマシンに直接プッシュされるようにすることができます。


<!--HONumber=Apr16_HO2-->


