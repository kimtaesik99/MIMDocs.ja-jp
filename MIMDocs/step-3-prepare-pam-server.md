---
タイトル: ステップ 3: PAM サーバーの準備
ms.custom:
  - Id 管理
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: 68ec2145-6faa-485e-b79f-2b0c4ce9eff7
ロボット: noindex、nofollow
---
# 手順 3: PAM サーバーの準備

1.  3 番目のバーチャル マシンでは、Windows Server 2012 R2、具体的には Windows Server 2012 R2 Standard (GUI 搭載サーバー) x64 では、新しいコンピューターのインストール"*PAMSRV*"です。   このコンピューターには SQL Server と SharePoint 2013 がインストールされるので、少なくとも 8 GB の RAM が必要です。

    1.  Windows Server 2012 R2 Standard (GUI 搭載サーバー) x64 を指定します。

        ![](./media/PAM_GS_Select_WS2012.png)

    2.  ライセンス条項を確認して同意します。

    3.  ため、ディスクは空であるが、選択 **カスタム: Windows のインストールのみ** し、初期化されていないディスク領域を使用します。

2.  その新しいコンピューターに管理者としてログインします。  仮想ネットワーク上の静的 IP アドレスを付与しの IP アドレスを DNS クエリを送信するには、そのネットワーク インターフェイスを構成するコントロール パネルを使用して、 **PRIVDC** にコンピューター名を設定および **PAMSRV**します。  これにはサーバーの再起動が必要になります。

3.  仮想ネットワークがインターネット接続を提供していない場合は、インターネットへの接続を確立するコンピューターに、追加のネットワーク インターフェイスを追加します。  この設定は、SharePoint のインストールで必要となります。この手順が完了したら、無効にすることができます。

4.  サーバーが再起動したら、管理者としてログインします。 [コントロール パネル] を使用して、更新を確認し、必要な更新をインストールするようにコンピューターを構成します。  これにはサーバーの再起動が必要になる場合があります。

5.  サーバー後管理者は、ログインの再起動を開き、コントロール パネルの [結合 **PAMSRV** に、 **PRIV** ドメイン (*priv.contoso.local*)。  ユーザー名と資格情報を提供することが必要な **PRIV** など、ドメイン管理者 *priv \administrator*します。 ウェルカム メッセージが表示されたら、ダイアログ ボックスを閉じて、このサーバーを再起動します。

6.  コンピューターを起動 **PAMSRV** としてログインし、 **PRIV** など、ドメイン管理者 *priv \administrator*します。

7.  Web サーバー (IIS) およびアプリケーション サーバーの役割、.NET Framework 3.5 の機能、Windows PowerShell 用の Active Directory モジュール、SharePoint に必要なその他の機能を追加します。

    1.  管理者としてログインした状態で、PowerShell を起動します。

    2.  次のコマンドを入力します。   .NET Framework 3.5 の機能のソース ファイルに対しては、別の場所を指定することが必要になる場合があります。 これらの機能は通常提示されません Windows Server のインストール、OS のサイド バイ サイド (SxS) フォルダーにインストール ディスク ソース フォルダーなど、ときに"* d:\Sources\SxS\*"です。

        ```
        import-module ServerManager
        Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
        ```

8.  新しく作成したアカウントがサービスとして実行されるように、サーバー セキュリティ ポリシーを構成します。

    1.   **ローカル セキュリティ ポリシー** プログラムを起動します。

    2.  移動 **ローカル ポリシー» ユーザー権利の割り当て**します。

    3.   **詳細ウィンドウ**, を右クリックして **サービスとしてログオン**, を選択して **プロパティ**します。

    4.  をクリックして **[ユーザーまたはグループ**, 、[ユーザーとグループ名では、次のように入力します。 *「priv \mimmonitor; priv \mimservice; priv \sharepoint; priv \mimcomponent」; priv \sqlserver*, 、] をクリック **名前の確認**, 、] をクリック **[ok]**します。

    5.   **[OK]** をクリックして、[サービスとしてのログオン] プロパティ ウィンドウを閉じます。

    6.   **詳細ウィンドウ**, を右クリックして **ネットワークからこのコンピューターへのアクセスを拒否**, を選択して **プロパティ**します。

    7.  をクリックして **[ユーザーまたはグループ**, 、[ユーザーとグループ名では、次のように入力します。 *「priv \mimmonitor; priv \mimservice; priv \mimcomponent」* ] をクリック **OK**します。

    8.  クリックして **OK** をこのコンピューターにネットワークのプロパティ] ウィンドウからアクセスを拒否] を閉じます。

    9.  **詳細ウィンドウ**, を右クリックして **ローカル ログオンを拒否する**, を選択して **プロパティ**します。

    10. をクリックして **[ユーザーまたはグループ**, 、[ユーザーとグループ名では、次のように入力します。 *「priv \mimmonitor; priv \mimservice; priv \mimcomponent」* ] をクリック **OK**します。

    11. クリックして **OK** をログオンを拒否するローカル プロパティ] ウィンドウを閉じます。

    12. **閉じる** ローカル セキュリティ ポリシー] ウィンドウ。

9. アプリケーションで Windows 認証モードを使用できるように、IIS 構成を変更します。

    1.  PowerShell ウィンドウを開く

    2.  IIS を停止し、次のコマンドを使用して、アプリケーションのホスト設定のロックを解除します。

        ```
        iisreset /STOP
        C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
        iisreset /START
        ```

    3.  あるいは、メモ帳などのテキスト エディターを使用して、次のファイルを開きます。
        C:\Windows\System32\inetsrv\config\applicationHost.config
        そのファイルの 82 行目でのタグの値を置換 *overrideModeDefault*:

        & lt; セクション名 ="windowsAuthentication"overrideModeDefault ="Deny"/& gt;

        を

        & lt; セクション名 ="windowsAuthentication"overrideModeDefault ="Allow"/& gt;

        ファイルを保存して、コマンドを使用して IIS を再起動し、 *iisreset/stop を*

10. インストール **SQL Server 2012 Service Pack 1** またはそれ以降、または **SQL Server 2014**します。 次の手順は、SQL 2014 であることを前提としています。

    1.  ドメイン管理者として PowerShell を起動します。

    2.  SQL Server セットアップ プログラムがあるディレクトリに移動します。

    3.  次のコマンドを入力します。

        ```
        .\setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install /FEATURES=SQL,SSMS /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="PRIV\SqlServer" /SQLSVCPASSWORD="Pass@word1"   /AGTSVCSTARTUPTYPE=Automatic /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /SQLSYSADMINACCOUNTS="PRIV\Administrator"
        ```

11. 使用して、 **SharePoint Foundation 2013 SP1 インストーラー付き**, 、SharePoint のソフトウェアの必須コンポーネントをインストール *PAMSRV*します。  これによりサーバーが再起動すること、また、これには、インストーラーが必須コンポーネントをダウンロードするためにこのコンピューターにインターネット接続が必要であることに注意してください。

    1.  ドメイン管理者として PowerShell を起動します。

    2.  SharePoint が展開されたディレクトリに移動します。

    3.  次のコマンドを入力します。

        ```
        .\prerequisiteinstaller.exe
        ```

12. SharePoint の必須コンポーネントがインストールされるとインストール **SharePoint Foundation 2013 with SP1**します。

    1.  ドメイン管理者として PowerShell を起動します。

    2.  SharePoint が展開されたディレクトリに移動します。

    3.  次のコマンドを入力します。

        ```
        .\setup.exe
        ```

    4.  完全なサーバーの種類を選択します。

    5.  インストールが完了したら、ウィザードの実行を選択します。

13.  **[SharePoint 製品構成ウィザード]** を実行して SharePoint を構成します。

    1.  [サーバー ファームへの接続] タブで、「新しいサーバー ファームの作成」に移動します。

    2.  指定 **PAMSRV** 構成データベースのデータベース サーバーとして、 **priv \sharepoint** sharepoint で使用するデータベース アクセス アカウントとして。

    3.  ファーム セキュリティ パスフレーズとしてパスワードを指定します (このラボ環境で後で使用)。

    4.  このテスト ラボ環境では、SharePoint 構成ウィザードの残りの既定設定をそのまま使用します。

    5.  構成ウィザードには、10 の構成タスク 10 が完了すると、クリックして **完了** と web ブラウザーが開きます。

    6.  Internet Explorer のポップアップ画面で、ユーザーとして認証 **priv \administrator** (または同等のドメイン管理者アカウント) に進みます。

    7.  ウィザード (Web アプリ) を起動して、SharePoint ファームを構成します。

    8.  管理アカウントの既存を使用する] を選択 (**priv \sharepoint**)、をクリック **次**します。

    9. [サイト コレクションを作成しています] というウィンドウが表示されたら、 **[スキップ]**をクリックします。  次に、 **[完了]**をクリックします。

14. ウィザードが完了した後は、PowerShell を使用して、作成、 **SharePoint Foundation 2013 Web アプリケーション** MIM ポータルをホストします。  これはテスト ラボ環境であるため、SSL は有効にされません。

    1.  SharePoint 2013 管理シェルを起動し、次の PowerShell スクリプトを実行します。

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity PRIV\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"            -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://PAMSRV.priv.contoso.local
        ```
        Windows クラシック認証方法が使用されることを警告するメッセージが表示されます。また、最後のコマンドが返されるまで数分かかる場合があります。  完了すると、新しいポータルの URL を示す出力が返されます。  SharePoint 2013 管理シェル ウィンドウは、後続のタスクで必要となるので、開いたままとします。

15. 次に、作成、 **SharePoint サイト コレクション** MIM ポータルをホストするには、その web アプリケーションに関連付けられています。

    1.  起動  **SharePoint 2013 管理シェル**, 場合は、開くには、および次の PowerShell スクリプトを実行していません。

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://pamsrv.priv.contoso.local:82
        New-SPSite -Url $w.Url -Template $t -OwnerAlias PRIV\Administrator                -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias PRIV\BackupAdmin
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```
         *CompatibilityLevel* 変数の結果が「14」であることを確認します。  ([SharePoint Foundation 2013 上に FIM 2010 R2 をインストールする「](http://technet.microsoft.com/library/jj863242.aspx) の詳細). 結果が「15」の場合は、2010 エクスペリエンス バージョンに対応するサイト コレクションが作成されていないので、サイト コレクションを削除して作成し直します。

    2.  SharePoint サーバー側 viewstate、および SharePoint のタスクを無効にする"*正常性解析ジョブ (時間単位、Microsoft SharePoint Foundation Timer、すべてのサーバー*"で次の PowerShell コマンドを実行して、 **SharePoint 2013 管理シェル**:。

        ```
        $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
        $contentService.ViewStateOnServer = $false;
        $contentService.Update();
        Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
        ```

16. 新しい web ブラウザーのタブを開きに移動し、 *http://pamsrv.priv.contoso.local:82/* およびログイン *priv \administrator*します。  「MIM ポータル」という名前の空の SharePoint サイトが表示されます。 Internet Explorer で開く **インターネット オプション**, に変更、 **セキュリティ] タブで** 選択 **ローカル イントラネット**, 、し、web サイトを追加します。 (サインインに失敗した場合は、手順 2 で前に作成した Kerberos SPN を更新する必要があります)。

17. 使用して **サービス** ([管理ツール) を起動、 **SharePoint 管理** サービスを既に開始されていない場合。


<!--HONumber=Mar16_HO1-->


