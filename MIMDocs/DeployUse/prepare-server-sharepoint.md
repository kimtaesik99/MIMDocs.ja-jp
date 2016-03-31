---
title: Id 管理サーバーと #58; を準備します。SharePoint |Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: kgremban
---
# Id 管理サーバーの準備をしています: SharePoint

>[! div クラスを「ステップバイ ステップ」=]
[前へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-sql2014.html)
**Id 管理サーバーを準備する: SQL Server 2014**

> [!NOTE]
> 次に、すべての例で **mimservername** 、ドメイン コント ローラーの名前を表す **contoso** ドメイン名を表すと **Pass@word1** 例パスワードを表します。


## インストール **SharePoint Foundation 2013 with SP1**

> [!NOTE]
> 前提条件となるものをインストーラーがダウンロードできるように、インターネット接続が必要です。

インストールの最後にサーバーが再起動します。

1.  ドメイン管理者として **PowerShell** を起動します。

    -   SharePoint が展開されたディレクトリに移動します。

    -   次のコマンドを入力します。

        ```
        .\prerequisiteinstaller.exe
        ```

2.   **SharePoint** の必須コンポーネントがインストールされた後、次のコマンドを入力して **SharePoint Foundation 2013 with SP1** をインストールします。

    ```
    .\setup.exe
    ```

3.  完全なサーバーの種類を選択します。

4.  インストールが完了したら、ウィザードを実行します。

## SharePoint 構成ウィザードを実行します。

インライン展開の手順に従って、 **SharePoint 製品構成ウィザード** MIM を使用する SharePoint を構成します。

1.  **[サーバー ファームへの接続]** タブで、「新しいサーバー ファームの作成」に移動します。

2. 構成データベースのデータベース サーバーとしてこのサーバーを指定し、 *contoso \sharepoint* sharepoint で使用するデータベース アクセス アカウントとして。

3. ファーム セキュリティ パスフレーズとしてパスワードを指定します (このラボ環境で後で使用)。

4. 構成ウィザードで 10 個中 10 番目の構成タスク (最後のタスク) の完了後に、[完了] をクリックすると Web ブラウザーが開きます。

5. Internet Explorer のポップアップ画面で、ユーザーとして認証 *contoso \administrator* (または同等のドメイン管理者アカウント) に進みます。

6. ウィザード (Web アプリ) を起動して、SharePoint ファームを構成します。

7. 管理アカウントの既存を使用するオプションを選択 (*contoso \sharepoint*)、をクリックして **次**します。

8.  **[サイト コレクションを作成しています]** ウィンドウで、 **[スキップ]**をクリックします。  次に、 **[完了]**をクリックします。

## MIM ポータルをホストする SharePoint を準備します。

1. 作成、 **SharePoint Foundation 2013 Web アプリケーション**します。

    > [!NOTE]
    > 初めは、SSL は構成されていません。 このポータルへのアクセスを有効にする前に、SSL またはそれと同等のものを構成します。

    1.   **SharePoint 2013 管理シェル** を起動し、次の PowerShell スクリプトを実行します。

        ```
        $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
        New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool"
        -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
        ```

        2. Windows クラシック認証方法が使用されることを警告するメッセージが表示されます。また、最後のコマンドが返されるまで数分かかる場合があります。  完了すると、新しいポータルの URL を示す出力が返されます。   **SharePoint 2013 管理シェル** ウィンドウは、後続のタスクで必要となるので、開いたままとします。

2. 作成、 **SharePoint サイト コレクション** その web アプリケーションに関連付けられています。

    1. SharePoint 2013 管理シェルを起動し、次の PowerShell スクリプトを実行します。

        ```
        $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
        $w = Get-SPWebApplication http://corpidm.contoso.local:82
        New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
        -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
        $s = SpSite($w.Url)
        $s.AllowSelfServiceUpgrade = $false
        $s.CompatibilityLevel
        ```

        2.  *CompatibilityLevel* 変数の結果が「14」であることを確認します。  ([SharePoint Foundation 2013 上に FIM 2010 R2 をインストールする「](http://technet.microsoft.com/library/jj863242.aspx) の詳細). 結果が「15」の場合は、2010 エクスペリエンス バージョンに対応するサイト コレクションが作成されていないので、サイト コレクションを削除して作成し直します。

3. 無効にする **SharePoint サーバー側 Viewstate** でコマンドを SharePoint タスク「正常性解析ジョブ (時間単位、Microsoft SharePoint Foundation Timer すべてのサーバー)"次の PowerShell を実行して、 **SharePoint 2013 管理シェル**:

    ```
    $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
    $contentService.ViewStateOnServer = $false;
    $contentService.Update();
    Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
    ```

4. Id 管理サーバー上で新しい web ブラウザーのタブを開き、http://localhost:82 に移動/およびログイン *contoso \administrator*します。   *MIM ポータル* という名前の空の SharePoint サイトが表示されます。

    ![Http://localhost:82 に MIM ポータルのイメージ/](media/MIM-DeploySP1.png)

5. URL をコピーし、Internet Explorer で開く **インターネット オプション**, に変更、 **セキュリティ] タブ**, [ **ローカル イントラネット**, 、] をクリック **サイト**します。

    ![[インターネット オプションのイメージ](media/MIM-DeploySP2.png)

6.  **ローカル イントラネット** ウィンドウの **詳細** でコピーした URL を貼り付けると、 **この web サイトをゾーンに追加** テキスト ボックスです。 クリックして **追加** ウィンドウを閉じます。

7. 開いている、 **管理ツール** プログラムに移動し、 **サービス**, SharePoint 管理サービスを検索して、まだ実行されていない場合は開始します。

>[! div クラスを「ステップバイ ステップ」=]  
[次へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-exchange.html)
**Id 管理サーバーの準備をしています: Exchange (省略可能)**


<!--HONumber=Mar16_HO3-->


