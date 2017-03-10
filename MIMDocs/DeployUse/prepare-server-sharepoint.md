---
title: "Microsoft Identity Manager 2016 向けの SharePoint の構成 | Microsoft Docs"
description: "SharePoint Foundation をインストールして、MIM ポータル ページをホストできるように構成します。"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c01487f2-3de6-4fc4-8c3a-7d62f7c2496c
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 2af432036033f8914d00228cd3d2d1af84f13054


---

# <a name="set-up-an-identity-management-server-sharepoint"></a>ID 管理サーバー: SharePoint のセットアップ

>[!div class="step-by-step"]
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 たとえば、
> - ドメイン コントローラー名 - **mimservername**
> - ドメイン名 - **contoso**
> - パスワード - **Pass@word1**


## <a name="install-sharepoint-foundation-2013-with-sp1"></a>**SharePoint Foundation 2013 with SP1** をインストールします。

> [!NOTE]
> 前提条件となるものをインストーラーがダウンロードできるように、インターネット接続が必要です。 コンピューターがインターネット接続を提供していない仮想ネットワーク上にある場合は、インターネットへの接続を提供するコンピューターへの追加ネットワーク インターフェイスを追加します。 これはインストール完了後に無効にすることができます。

次の手順に従って、SharePoint Foundation 2013 SP1 をインストールしてください。 インストールが完了すると、サーバーが再起動します。 

1.  ドメイン管理者として **PowerShell** を起動します。

    -   SharePoint が展開されたディレクトリに移動します。

    -   次のコマンドを入力します。

        ```
        .\prerequisiteinstaller.exe
        ```

2.  **SharePoint** の必須コンポーネントがインストールされた後、次のコマンドを入力して **SharePoint Foundation 2013 with SP1** をインストールします。

    ```
    .\setup.exe
    ```

3.  完全なサーバーの種類を選択します。

4.  インストールが完了したら、ウィザードを実行します。

## <a name="run-the-wizard-to-configure-sharepoint"></a>ウィザードを実行して SharePoint を構成する

**SharePoint 製品構成ウィザード**で説明されている手順に従って、MIM と連動するように SharePoint を構成します。

1. **[サーバー ファームへの接続]** タブで、「新しいサーバー ファームの作成」に移動します。

2. 構成データベース用のデータベース サーバーとしてこのサーバーを指定し、SharePoint で使用するデータベース アクセス アカウントとして *Contoso\SharePoint* を指定します。

3. ファーム セキュリティ パスフレーズのパスワードを作成します。

4. 構成ウィザードで 10 個中 10 番目の構成タスク (最後のタスク) の完了後に、[完了] をクリックすると Web ブラウザーが開きます。

5. Internet Explorer のポップアップ画面で、*Contoso\Administrator* (または同等のドメイン管理者アカウント) として認証し、先に進みます。

6. ウィザード (Web アプリ) を起動して、SharePoint ファームを構成します。

7. 既存の管理されたアカウント (*Contoso\SharePoint*) を使用するオプションを選択し、**[次へ]** をクリックします。

8. **[サイト コレクションを作成しています]** ウィンドウで、 **[スキップ]**をクリックします。  次に、 **[完了]**をクリックします。

## <a name="prepare-sharepoint-to-host-the-mim-portal"></a>MIM ポータルをホストするように SharePoint を準備する

> [!NOTE]
> 初めは、SSL は構成されていません。 このポータルへのアクセスを有効にする前に、SSL またはそれと同等のものを構成します。

1. **SharePoint 2013 管理シェル** を起動し、次の PowerShell スクリプトを実行して、**SharePoint Foundation 2013 Web アプリケーション**を作成します。

    ```
    $dbManagedAccount = Get-SPManagedAccount -Identity contoso\SharePoint
    New-SpWebApplication -Name "MIM Portal" -ApplicationPool "MIMAppPool" -ApplicationPoolAccount $dbManagedAccount -AuthenticationMethod "Kerberos" -Port 82 -URL http://corpidm.contoso.local
    ```

    > [!NOTE]
    > Windows クラシック認証方法が使用されることを警告するメッセージが表示されます。また、最後のコマンドが返されるまで数分かかる場合があります。 完了すると、新しいポータルの URL を示す出力が返されます。 後で参照するために**SharePoint 2013 管理シェル** ウィンドウを開いたままにしておきます。

2. SharePoint 2013 管理シェルを起動し、次の PowerShell スクリプトを実行して、Web アプリケーションに関連付られた**SharePoint サイト コレクション**を作成します。

  ```
  $t = Get-SPWebTemplate -compatibilityLevel 14 -Identity "STS#1"
  $w = Get-SPWebApplication http://corpidm.contoso.local:82
  New-SPSite -Url $w.Url -Template $t -OwnerAlias contoso\Administrator
  -CompatibilityLevel 14 -Name "MIM Portal" -SecondaryOwnerAlias contoso\BackupAdmin
  $s = SpSite($w.Url)
  $s.AllowSelfServiceUpgrade = $false
  $s.CompatibilityLevel
  ```

  > [!NOTE]
  > *CompatibilityLevel* 変数の結果が「14」であることを確認します。 結果が「15」の場合は、2010 エクスペリエンス バージョンに対応するサイト コレクションが作成されていないので、サイト コレクションを削除して作成し直します。

3. **SharePoint サーバー側 Viewstate**、および SharePoint タスク「正常性解析ジョブ (時間単位で、Microsoft SharePoint Foundation Timer、すべてのサーバーが対象)」を無効にします。そのためには、**SharePoint 2013 管理シェル**内の次の PowerShell コマンドを実行します。

  ```
  $contentService = [Microsoft.SharePoint.Administration.SPWebService]::ContentService;
  $contentService.ViewStateOnServer = $false;
  $contentService.Update();
  Get-SPTimerJob hourly-all-sptimerservice-health-analysis-job | disable-SPTimerJob
  ```

4. ID 管理サーバーで新しい Web ブラウザー タブを開き、http://localhost:82/ に移動し、*contoso\Administrator* としてログインします。  *MIM ポータル* という名前の空の SharePoint サイトが表示されます。

    ![http://localhost:82/ の MIM ポータルの画像](media/MIM-DeploySP1.png)

5. URL をコピーし、Internet Explorer で、**[インターネット オプション]** を開き、**[セキュリティ]** タブに移動し、**[ローカル イントラネット]** を選択して、**[サイト]** をクリックします。

    ![インターネット オプションの画像](media/MIM-DeploySP2.png)

6. **[ローカル イントラネット]** ウィンドウで **[詳細]** をクリックし、コピーした URL を **[この Web サイトをゾーンに追加する]** テキスト ボックスに貼り付けます。 **[追加]** をクリックしてウィンドウを閉じます。

7. **[管理ツール]** プログラムを開き、**[サービス]** に移動して、SharePoint Administration サービスを見つけて開始します (まだ実行されていない場合)。

>[!div class="step-by-step"]  
[« SQL Server 2014](prepare-server-sql2014.md)
[Exchange Server »](prepare-server-exchange.md)



<!--HONumber=Jan17_HO4-->


