---
title: "PAM の展開、手順 3 – PAM サーバー | Microsoft Docs"
description: "Privileged Access Management の展開に備えて、SQL と SharePoint の両方をホストする PAM サーバーを準備します。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 9a262a256062688542040827653a7df8d82e1044
ms.contentlocale: ja-jp
ms.lasthandoff: 07/10/2017


---

<a id="step-3--prepare-a-pam-server" class="xliff"></a>
# 手順 3: PAM サーバーの準備

>[!div class="step-by-step"]
[«手順 2](step-2-prepare-priv-domain-controller.md)
[手順 4 »](step-4-install-mim-components-on-pam-server.md)

<a id="install-windows-server-2012-r2" class="xliff"></a>
## Windows Server 2012 R2 のインストール
3 番目の仮想マシンに、Windows Server 2012 R2 (具体的には Windows Server 2012 R2 Standard (GUI 搭載サーバー) x64) をインストールし、「*PAMSRV*」とします。 このコンピューターには SQL Server と SharePoint 2013 がインストールされるので、少なくとも 8 GB の RAM が必要です。

1. **Windows Server 2012 R2 Standard (GUI 搭載サーバー) x64** を選択します。

    ![Windows Server Standard (GUI 搭載) を選択する - スクリーンショット](media/PAM_GS_Select_WS2012.png)

2. ライセンス条項を確認して同意します。

3.  ディスクは空になるため、 **[カスタム: Windows のみをインストールする]** を選択し、 **初期化されていないディスク領域**を使用します。

4.  その新しいコンピューターに管理者としてサインインします。  [コントロール パネル] で、仮想ネットワーク上の静的 IP アドレスを提供し、PRIVDC の IP アドレスに DNS クエリを送信するようにそのネットワーク インターフェイスを構成し、コンピューター名を *PAMSRV* に設定します。  これにはサーバーの再起動が必要になります。

5.  仮想ネットワークがインターネット接続を提供していない場合は、インターネットへの接続を確立するコンピューターに、追加のネットワーク インターフェイスを追加します。  この設定は、SharePoint のインストールに必要です。この手順が完了したら、無効にできます。

6.  サーバーが再起動したら、管理者としてサインインします。 [コントロール パネル] を使用して、更新を確認し、必要な更新をインストールするようにコンピューターを構成します。  これにはサーバーの再起動が必要になる場合があります。

7.  サーバーが再起動したら、管理者としてログインし、[コントロール パネル] を開き、PAMSRV を PRIV ドメイン (priv.contoso.local) に参加させます。  この操作を行うには、PRIV ドメイン管理者 (PRIV\Administrator) のユーザー名と資格情報を提供する必要があります。 ウェルカム メッセージが表示されたら、ダイアログ ボックスを閉じて、このサーバーを再起動します。


<a id="add-the-web-server-iis-and-application-server-roles" class="xliff"></a>
### Web サーバー (IIS) とアプリケーション サーバーの役割を追加する
Web サーバー (IIS) およびアプリケーション サーバーの役割、.NET Framework 3.5 の機能、Windows PowerShell 用の Active Directory モジュール、SharePoint に必要なその他の機能を追加します。

1.  PRIV ドメイン管理者 (PRIV\Administrator) としてサインインし、PowerShell を起動します。

2.  次のコマンドを入力します。 .NET Framework 3.5 の機能のソース ファイルに対しては、別の場所を指定することが必要になる場合があります。 Windows Server のインストール時に、これらの機能は通常提示されませんが、OS インストール ディスク ソース フォルダー上のサイド バイ サイド (SxS) フォルダー (例: “d:\Sources\SxS\”) にあります。

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,
    rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,
    Windows-Identity-Foundation,Server-Media-Foundation,
    Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

<a id="configure-the-server-security-policy" class="xliff"></a>
### サーバーのセキュリティ ポリシーを構成する
新しく作成したアカウントがサービスとして実行されるように、サーバー セキュリティ ポリシーを構成します。

1.  **ローカル セキュリティ ポリシー** プログラムを起動します。   
2.  **[ローカル ポリシー]**  >  **[ユーザー権利の割り当て]** の順に移動します。  
3.  詳細ウィンドウで、 **[サービスとしてログオン]**を右クリックして、 **[プロパティ]**を選択します。  
4.  **[ユーザーまたはグループの追加]** をクリックして、[ユーザーとグループ名] に「*priv\mimmonitor; priv\MIMService; priv\SharePoint; priv\mimcomponent; priv\SqlServer*」と入力します。 **[名前の確認]** をクリックし、**[OK]** をクリックします。  

5.  **[OK]** をクリックし、[プロパティ] ウィンドウを閉じます。  
6.  詳細ウィンドウで、**[ネットワークからのこのコンピューターへのアクセスを拒否する]**を右クリックし、**[プロパティ]** を選択します。  
7.  **[ユーザーまたはグループの追加]** をクリックして、[ユーザーとグループ名] に「*priv\mimmonitor; priv\MIMService; priv\mimcomponent*」と入力し、**[OK]** をクリックします。  
8.  **[OK]** をクリックし、[プロパティ] ウィンドウを閉じます。  

9. 詳細ウィンドウで、**[ローカルでのログオンを拒否する]**を右クリックして、**[プロパティ]** を選択します。  
10. **[ユーザーまたはグループの追加]** をクリックして、[ユーザーとグループ名] に「*priv\mimmonitor; priv\MIMService; priv\mimcomponent*」と入力し、**[OK]** をクリックします。  
11. **[OK]** をクリックし、[プロパティ] ウィンドウを閉じます。  
12. [ローカル セキュリティ ポリシー] ウィンドウを閉じます。  

13. [コントロール パネル] を開き、**[ユーザー アカウント]** に切り替えます。  
14. **[他のユーザーにこのコンピューターへのアクセスを許可]**をクリックします。  
15. **[追加]** をクリックし、ドメインの *PRIV* にユーザーとして「*MIMADMIN*」と入力し、ウィザードの次の画面で **[このユーザーを管理者として追加する]** をクリックします。  
16. **[追加]** をクリックし、ドメインの *PRIV* にユーザーとして「*SharePoint*と入力し、ウィザードの次の画面で **[このユーザーを管理者として追加する]** をクリックします。  
17. [コントロール パネル] を閉じます。  

<a id="change-the-iis-configuration" class="xliff"></a>
### IIS の構成を変更する
アプリケーションで Windows 認証モードを使うように IIS 構成を変更する方法は 2 つあります。 MIMAdmin としてサインインしていることを確認し、次のオプションのいずれかに従います。

PowerShell を使う場合:
1.  [PowerShell] を右クリックし、**[管理者として実行]** を選択します。  
2.  IIS を停止し、次のコマンドを使用して、アプリケーションのホスト設定のロックを解除します。  
    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

メモ帳などのテキスト エディターを使う場合:   
1. ファイル **C:\Windows\System32\inetsrv\config\applicationHost.config** を開きます   
2. そのファイルの 82 行目まで下にスクロールします。 **overrideModeDefault** のタグ値は **<section name="windowsAuthentication" overrideModeDefault="Deny" />** にする必要があります  
3. **overrideModeDefault** の値を *[許可]* に変更します  
4. ファイルを保存し、PowerShell コマンド `iisreset /START` で IIS を再起動します。

<a id="install-sql-server" class="xliff"></a>
## SQL Server のインストール
SQL Server がまだ要塞環境に存在しない場合は、SQL Server 2012 (Service Pack 1 以降) か SQL Server 2014 のいずれかをインストールします。 次の手順は、SQL 2014 であることを前提としています。

1. MIMAdmin としてサインインしていることを確認します。
2. [PowerShell] を右クリックし、**[管理者として実行]** を選択します。   
3. SQL Server セットアップ プログラムがあるディレクトリに移動します。  
4. 次のコマンドを入力します。  
    ```
    .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1" /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\MIMAdmin"
    ```

<a id="install-sharepoint-foundation-2013" class="xliff"></a>
## SharePoint Foundation 2013 をインストールします。

SharePoint Foundation 2013 SP1 のインストーラーを使用して、SharePoint のソフトウェア必須コンポーネントを PAMSRV にインストールします。

> [!NOTE]
> 前提条件となるものをインストーラーでダウンロードするためには、インターネット接続が必要です。 インストールが完了すると、サーバーが再起動します。

1. [PowerShell] を右クリックし、**[管理者として実行]** を選択します。  
2. SharePoint が展開されたディレクトリに移動します。  
3. コマンド「`.\prerequisiteinstaller.exe`」を入力します。

SharePoint の必須コンポーネントがインストールされた後に、SharePoint Foundation 2013 SP1 をインストールします。

1.  [PowerShell] を右クリックし、**[管理者として実行]** を選択します。  
2.  SharePoint が展開されたディレクトリに移動します。  
3.  コマンド「`.\setup.exe`」を入力します。  
4.  **完全なサーバー** の種類を選択します。  
5.  インストールが完了したら、ウィザードの実行を選択します。  

<a id="configure-sharepoint" class="xliff"></a>
### SharePoint を構成する
[SharePoint 製品構成ウィザード] を実行して SharePoint を構成します。

1.  [サーバー ファームへの接続] タブで、**[新しいサーバー ファームの作成]** に移動します。  
2.  構成データベース用のデータベース サーバーとして **PAMSRV** を指定し、SharePoint で使用するデータベース アクセス アカウントとして **PRIV\SharePoint** を指定します。  
3.  ファーム セキュリティ パスフレーズとしてパスワードを指定します (このチュートリアルの後半では使用しません)。  
4.  当面は、SharePoint 構成ウィザードの残りの既定設定をそのまま使うことにより、シングルサーバー ファームを作成します。    
5.  構成ウィザードで 10 個中 10 番目の構成タスク (最後のタスク) の完了後に、**[完了]** をクリックすると Web ブラウザーが開きます。  
6.  Internet Explorer のポップアップで、ドメイン管理者 (PRIV\MIMAdmin) として認証して続行します。  
7.  Web アプリ内のウィザードを起動して、SharePoint ファームを構成します。  
8.  既存の管理アカウント (PRIV\SharePoint) を使用し、オプションのサービスをオフにして無効にし、**[次へ]** をクリックします。  
9. [サイト コレクションを作成しています] ウィンドウが表示されたら、**[スキップ]** をクリックし、**[完了]** をクリックします。  

<a id="create-a-sharepoint-foundation-2013-web-application" class="xliff"></a>
## SharePoint Foundation 2013 Web アプリケーションを作成する
ウィザードが完了したら、PowerShell を使用して、MIM ポータルをホストする SharePoint Foundation 2013 Web アプリケーションを作成します。 このチュートリアルはデモンストレーション用であるため、SSL は使用できません。

1.  [SharePoint 2013 管理シェル] を右クリックし、**[管理者として実行]** をクリックして、次の PowerShell スクリプトを実行します。

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
    ```

2. Windows クラシックの認証方法が使用されることを警告するメッセージが表示されます。また、最後のコマンドが返されるまで数分かかる場合があります。  完了すると、新しいポータルの URL を示す出力が返されます。

> [!NOTE]
> 次の手順で使うため、[SharePoint 2013 管理シェル] ウィンドウを開いたままにしておきます。

<a id="create-a-sharepoint-site-collection" class="xliff"></a>
## SharePoint サイト コレクションを作成する
次に、その Web アプリケーションに関連付けられた SharePoint サイト コレクションを作成して、MIM ポータルをホストします。

1.  **[SharePoint 2013 管理シェル]** をまだ起動していない場合は、それを起動し、次の PowerShell スクリプトを実行します

    ```
    $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
    $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
    New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\MIMAdmin                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
    $s = SpSite($w.Url)
    $s.AllowSelfServiceUpgrade = $false
    $s.CompatibilityLevel
    ```

    **CompatibilityLevel** 変数が *14* に設定されていることを確認します。 *15* が返された場合は、2010 エクスペリエンス バージョンに対応するサイト コレクションが作成されていないので、サイト コレクションを削除して作成し直します。

2.  **[SharePoint 2013 管理シェル]** で次の PowerShell コマンドを実行します。 SharePoint サーバー側 Viewstate と SharePoint タスク「**正常性解析ジョブ (時間単位、Microsoft SharePoint Foundation Timer、すべてのサーバー)**」を無効にします。

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

<a id="change-update-settings" class="xliff"></a>
## 更新プログラムの設定を変更する

1. [コントロール パネル] を開き、**[Windows Update]** に移動し、**[設定の変更]** をクリックします。  
2. Microsoft Updates で、Windows Update と他の製品の更新プログラムを受信するように設定を変更します。  
3. 新しい更新プログラムを確認し、保留中の重要な更新プログラムをインストールしてから、次に進みます。

<a id="set-the-website-as-the-local-intranet" class="xliff"></a>
## Web サイトをローカル イントラネットとして設定する

1. Internet Explorer を起動し、新しい Web ブラウザーのタブを開く
2. http://pamsrv.priv.contoso.local:82/ にアクセスし、PRIV\MIMAdmin としてサインインします。  「MIM ポータル」という名前の空の SharePoint サイトが表示されます。  
3. Internet Explorer で **[インターネット オプション]** を開き、**[セキュリティ]** タブに移動し、**[ローカル イントラネット]** を選択して、URL `http://pamsrv.priv.contoso.local:82/` を追加します。

サインインに失敗した場合は、[手順 2](step-2-prepare-priv-domain-controller.md) で前に作成した Kerberos SPN の更新が必要となる可能性があります。

<a id="start-the-sharepoint-administration-service" class="xliff"></a>
## SharePoint 管理サービスを開始する

**SharePoint Administration** サービスがまだ実行されていない場合は、**[サービス]** ([管理ツール] 内にある) を使って起動します。

手順 4 で、PAM サーバーへの MIM コンポーネントのインストールを開始します。

>[!div class="step-by-step"]
[«手順 2](step-2-prepare-priv-domain-controller.md)
[手順 4 »](step-4-install-mim-components-on-pam-server.md)

