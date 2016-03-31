---
title: Id 管理サーバーと #58; を準備します。Windows Server 2012 R2 |Microsoft Identity Manager
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
# Id 管理サーバーの準備: Windows Server 2012 R2

>[! div クラスを「ステップバイ ステップ」=]
[前へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/preparing-domain.html)
**ドメインを準備します。**

> [!NOTE]
> 次に、すべての例で **mimservername** 、ドメイン コント ローラーの名前を表す **contoso** ドメイン名を表すと **Pass@word1** 例パスワードを表します。

## Windows Server 2012 R2 をドメインに参加させる

1. 8 GB 以上の RAM を備えた Windows Server 2012 R2 コンピューターを作成します。 インストール時に、"Windows Server 2012 R2 Standard (GUI 搭載サーバー) x64" エディションを指定します。

2. その新しいコンピューターに管理者としてログインします。

3. コントロール パネルを使用して、コンピューターの静的 IP アドレス ネットワーク上の指定、します。 前の手順で、ドメイン コント ローラーの IP アドレスを DNS クエリを送信するには、そのネットワーク インターフェイスを構成し、コンピューター名を設定 **CORPIDM**します。  これにはサーバーの再起動が必要になります。

4. コンピューターがインターネット接続を提供していない仮想ネットワーク上にある場合は、インターネットへの接続を提供するコンピューターへの追加ネットワーク インターフェイスを追加します。  これにより、SharePoint のインストールに必要なされ、その手順が完了したら無効にすることができます。

5. コントロール パネルを開き、コンピューターを最後の手順で構成されているドメインに参加させる *contoso.local*します。  これは、ユーザー名とドメイン管理者の資格情報を提供するように含まれます。 *contoso \administrator*します。  ウェルカム メッセージが表示されたら、ダイアログ ボックスを閉じて、このサーバーを再起動します。

6. コンピューターにサインイン *CorpIDM* など、ドメイン管理者として *contoso \administrator*します。

7. 管理者として PowerShell ウィンドウを起動し、グループ ポリシー設定でコンピューターを更新するには、次のコマンドを入力します。

    ```
    gpupdate /force /target:computer
    ```

    結局、分よりも多くのジョブが完了と、メッセージ「コンピューターのポリシーの更新プログラムが正常に完了します」

8. 追加、 **Web サーバー (IIS)** と **アプリケーション サーバー** の役割、 **.NET Framework** 3.5、4.0、および 4.5 の機能、および **Windows PowerShell 用 Active Directory モジュール**します。

    ![PowerShell の機能の画像](media/MIM-DeployWS2.png)

9. PowerShell では、管理者は、まだとして、次のコマンドを入力します。  **.NET Framework** 3.5 の機能のソース ファイルに対しては、別の場所を指定することが必要になる場合があります。 これらの機能は通常提示されません Windows Server のインストール、OS のサイド バイ サイド (SxS) フォルダーにインストール ディスク ソース フォルダーなど、ときに"* d:\Sources\SxS\*"です。

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## サーバーのセキュリティ ポリシーを構成する

サービスとして実行するために新しく作成されたアカウントを許可するように、サーバーのセキュリティ ポリシーを設定します。

1. ローカル セキュリティ ポリシー プログラムを起動します。

2. 移動 **ローカル ポリシー > ユーザー権利の割り当て**します。

3. 詳細ウィンドウで、 **[サービスとしてログオン]**を右クリックして、 **[プロパティ]**を選択します。

    ![ローカル セキュリティ ポリシーの画像](media/MIM-DeployWS3.png)

4. をクリックして **[ユーザーまたはグループ**, 、および、テキスト ボックスに「 `contoso\mimsync; contoso\mimma; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\mimsspr`, 、] をクリックして **名前の確認**, ] をクリック **[ok]**します。

5. をクリックして **OK** を閉じる、 **サービスのプロパティとしてログオン** ウィンドウです。

6.  詳細ウィンドウで右クリック **ネットワークからこのコンピューターへのアクセスを拒否**, を選択して **プロパティ**します。

7. をクリックして **[ユーザーまたはグループ**, 、および、テキスト ボックスに「 `contoso\MIMSync; contoso\MIMService` ] をクリック **[ok]**します。

8. をクリックして **OK** を閉じる、 **ネットワークのプロパティからこのコンピューターへのアクセスを拒否** ウィンドウです。

9. 詳細ウィンドウで右クリック **ローカル ログオンを拒否する**, を選択して **プロパティ**します。

10. をクリックして **[ユーザーまたはグループ**, 、および、テキスト ボックスに「 `contoso\MIMSync; contoso\MIMService` ] をクリック **[ok]**します。

11. をクリックして **OK** を閉じる、 **プロパティではローカルでログオンを拒否する** ウィンドウです。

12. ローカル セキュリティ ポリシー] ウィンドウを閉じます。


## IIS Windows 認証モードを変更します。

1.  PowerShell ウィンドウを開きます。

2.  コマンドを使用して IIS を停止 *iisreset/STOP*

        ```
        iisreset /STOP
        C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
        iisreset /START
        ```

>[! div クラスを「ステップバイ ステップ」=]  
[次へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-sql2014.html)
**Id 管理サーバーを準備する: SQL Server 2014**


<!--HONumber=Mar16_HO3-->


