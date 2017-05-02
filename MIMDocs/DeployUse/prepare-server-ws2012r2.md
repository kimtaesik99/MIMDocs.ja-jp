---
title: "MIM 2016 向けの Windows Server 2012 R2 の構成 | Microsoft Docs"
description: "MIM 2016 と連動するように Windows Server 2012 RS を準備するための手順と最小要件を説明します。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/23/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 51507d0a-2aeb-4cfd-a642-7c71e666d6cd
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 1cb0d6cd310372ecaeff47c9cc4461ebc43b3390
ms.lasthandoff: 01/24/2017


---

# <a name="set-up-an-identity-management-server-windows-server-2012-r2"></a>ID 管理サーバー: Windows Server 2012 R2 のセットアップ

>[!div class="step-by-step"]
[« ドメインの準備](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 たとえば、
> - ドメイン コントローラー名 - **mimservername**
> - ドメイン名 - **contoso**
> - パスワード - **Pass@word1**

## <a name="join-windows-server-2012-r2-to-your-domain"></a>Windows Server 2012 R2 をドメインに参加させる

8 GB 以上の RAM を備えた Windows Server 2012 R2 コンピューターで開始します。 インストール時に、"Windows Server 2012 R2 Standard (GUI 搭載サーバー) x64" エディションを指定します。

1. その新しいコンピューターに管理者としてログインします。

2. コントロール パネルを使用して、コンピューターにネットワーク上の静的 IP アドレスを指定します。 前の手順のドメイン コントローラーの IP アドレスに DNS クエリを送信するようにそのネットワーク インターフェイスを構成し、さらにコンピューター名を **CORPIDM** に設定します。  これにはサーバーの再起動が必要になります。

3. コントロール パネルを開き、コンピューターを最後の手順で構成したドメイン *contoso.local* に参加させます。  これには、ドメイン管理者のユーザー名と資格情報 (たとえば、*Contoso\Administrator*) を指定することも含まれます。  ウェルカム メッセージが表示されたら、ダイアログ ボックスを閉じて、このサーバーを再起動します。

4. コンピューター *CorpIDM* に *Contoso\Administrator* などのドメイン管理者としてサインインします。

5. 管理者として PowerShell ウィンドウを起動し、次のコマンドを入力してグループ ポリシー設定でコンピューターを更新します。

    ```
    gpupdate /force /target:computer
    ```

    最大 1 分で、「コンピューター ポリシーの更新が正常に完了しました。」というメッセージが表示され、更新が完了します。

6. **Web サーバー (IIS)** および **アプリケーション サーバー** の役割、 **.NET Framework** 3.5、4.0、4.5 の機能、**Windows PowerShell 用の Active Directory モジュール**を追加します。

    ![PowerShell 機能の画像](media/MIM-DeployWS2.png)

7. PowerShell で次のコマンドを入力します。 **.NET Framework** 3.5 の機能のソース ファイルに対しては、別の場所を指定することが必要になる場合があります。 Windows Server のインストール時に、これらの機能は通常提示されませんが、OS インストール ディスク ソース フォルダー上のサイド バイ サイド (SxS) フォルダー (例: “*d:\Sources\SxS\*”) にあります。

    ```
    import-module ServerManager
    Install-WindowsFeature Web-WebServer, Net-Framework-Features,rsat-ad-powershell,Web-Mgmt-Tools,Application-Server,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer –includeallsubfeature -restart -source d:\sources\SxS
    ```

## <a name="configure-the-server-security-policy"></a>サーバーのセキュリティ ポリシーを構成する

新しく作成したアカウントがサービスとして実行されるように、サーバー セキュリティ ポリシーを設定します。

1. ローカル セキュリティ ポリシー プログラムを起動します。

2. **[ローカル ポリシー]、[ユーザー権利の割り当て]** の順に移動します。

3. 詳細ウィンドウで、 **[サービスとしてログオン]**を右クリックして、 **[プロパティ]**を選択します。

    ![ローカル セキュリティ ポリシーの画像](media/MIM-DeployWS3.png)

4. **[ユーザーまたはグループの追加]**をクリックし、テキスト ボックスに「`contoso\MIMSync; contoso\MIMMA; contoso\MIMService; contoso\SharePoint; contoso\SqlServer; contoso\MIMSSPR`」と入力し、**[名前の確認]** をクリックして **[OK]** をクリックします。

5. **[OK]** をクリックして、**[サービスとしてログオン]** プロパティ ウィンドウを閉じます。

6.  詳細ウィンドウで、**[ネットワークからのこのコンピューターへのアクセスを拒否する]**を右クリックし、**[プロパティ]** を選択します。

7. **[ユーザーまたはグループの追加]** をクリックし、テキスト ボックスに「`contoso\MIMSync; contoso\MIMService`」と入力し、**[OK]** をクリックします。

8. **[OK]** をクリックして、**[ネットワーク経由でコンピューターへアクセスを拒否する]** プロパティ ウィンドウを閉じます。

9. 詳細ウィンドウで、**[ローカルでのログオンを拒否する]**を右クリックして、**[プロパティ]** を選択します。

10. **[ユーザーまたはグループの追加]** をクリックし、テキスト ボックスに「`contoso\MIMSync; contoso\MIMService`」と入力し、**[OK]** をクリックします。

11. **[OK]** をクリックして、**[ローカルでのログオンを拒否する]** プロパティ ウィンドウを閉じます。

12. [ローカル セキュリティ ポリシー] ウィンドウを閉じます。


## <a name="change-the-iis-windows-authentication-mode"></a>IIS Windows 認証モードを変更する

1.  PowerShell ウィンドウを開きます。

2.  *iisreset /STOP* コマンドを使用して IIS を停止します。

    ```
    iisreset /STOP
    C:\Windows\System32\inetsrv\appcmd.exe unlock config /section:windowsAuthentication -commit:apphost
    iisreset /START
    ```

>[!div class="step-by-step"]  
[« ドメインの準備](preparing-domain.md)
[SQL Server 2014 »](prepare-server-sql2014.md)

