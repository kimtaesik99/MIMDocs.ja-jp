---
title: MIM 2016 & #58; をインストールします。MIM 同期サービス |Microsoft Identity Manager
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
# MIM 同期サービスのインストールを実行する MIM 2016:

>[! div クラスを「ステップバイ ステップ」=]
[前へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/prepare-server-exchange.html)
**Id 管理サーバーの準備をしています: Exchange (省略可能)**

> [!NOTE]
> 次に、すべての例で **mimservername** 、ドメイン コント ローラーの名前を表す **contoso** ドメイン名を表すと **Pass@word1** 例パスワードを表します。

Microsoft Identity Manager 2016 コンポーネントを最初にインストールするには。

1. としてサインイン *contoso \administrator* id 管理を使用している CORPIDM サーバーにします。

2. MIM インストール パッケージを展開するか、MIM イメージ DVD をマウントします。

## MIM 2016 同期 (Sync) サービスをインストールします。

1. 展開した MIM インストール フォルダー内で、 **同期サービス** のフォルダーに移動します。

2.  **MIM 同期サービス インストーラー**を実行します。 インストーラーのガイドラインに従って、インストールを完了します。

3. ウェルカム画面で、 **[次へ]**をクリックします。

    ![MIM インストーラー ウィザードへようこそ] のイメージ](media/MIM-Install1.png)

4. ライセンス条項を読み、同意する場合は、 **[次へ]**をクリックします。

5.  **カスタム セットアップ** 画面をクリックして **次**します。

    ![カスタム セットアップ イメージ](media/MIM-Install2.png)

6.  同期データベースの構成画面では、次のように選択します。

    1.  SQL Server の配置場所: **このコンピューター**。

    2.  SQL Server のインスタンス: **既定のインスタンス**。

    ![データベース接続のイメージ](media/MIM-Install3.png)

7.  前に作成したアカウントに従って同期サービス アカウントを構成します。

    1.  サービス アカウント: *MIMSync*

    2.  ［パスワード］: *Pass@word1*

    3.  サービス アカウント ドメインまたはローカル コンピューターの名前: *contoso*

    ![サービス アカウントの画像](media/MIM-Install4.png)

8.  MIM 同期インストーラーに、関連するセキュリティ グループを指定します。

    1.  管理者 = *contoso \mimsyncadmins*

    2.  演算子 = *contoso \mimsyncoperators*

    3.  参加者 = *contoso \mimsyncjoiners*

    4.  コネクタ参照 = *contoso \mimsyncbrowse*

    5.  WMI パスワード管理 = *contoso \mimsyncpasswordreset*

    ![セキュリティ グループのイメージ](media/MIM-Install5.png)

9. セキュリティ設定] 画面でチェック **受信 RPC 通信用にファイアウォール ルールを有効にする**, 、] をクリック **次**します。

10. クリックして **インストール** MIM 同期のインストールを開始します。

    1.  MIM 同期サービス アカウントに関する警告が表示される場合があります。 **[OK]**をクリックします。

    2.  これで、MIM 同期がインストールされます。

        ![MIM 同期のインストール ステータスの画像](media/MIM-Install6.png)

    3.  暗号化キーが表示されます: のバックアップを作成するのに注意してください] をクリックして **OK**, 、暗号化キーのバックアップを保存するフォルダーを選択します。

        ![MIM 同期バックアップの暗号化キーの通知の画像](media/MIM-Install7.png)

    4.  インストーラーによるインストールが正常に終了したら、 **[完了]**をクリックします。

        ![MIM 同期のインストール成功イメージ](media/MIM-Install8.png)

    5.  サインアウトしてサインインを有効にするグループのメンバーシップの変更を入力するように求められます。 をクリックして **はい** からサインアウトします。

>[! div クラスを「ステップバイ ステップ」=]  
[次へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/mim-install-service-portal.html)
**インストールを実行する MIM 2016: MIM サービスおよびポータル**


<!--HONumber=Mar16_HO3-->


