---
title: "MIM 2016 のインストール&#58; MIM 同期サービス |Microsoft Identity Manager"
description: "同期サービスをインストールおよび構成して、MIM 2016 コンポーネントを使用開始します。"
keywords: 
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: c023d147d0fcc1525fefbe866c952e217f7bee6b
ms.openlocfilehash: 8a99b3a291d2b145f453732a72244c43f9c535d6


---

# MIM 2016 のインストール: MIM 同期サービス

>[!div class="step-by-step"]
[«Exchange Server](prepare-server-exchange.md)
[MIM サービスおよびポータル»](install-mim-service-portal.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 たとえば、
> - ドメイン コントローラー名 - **mimservername**
> - ドメイン名 - **contoso**
> - パスワード- **Pass@word1**

Microsoft Identity Manager 2016 をインストールするには、最初にインストール パッケージをセットアップします。

1. *contoso\Administrator* として、ID 管理に使用しているサーバーにサインインします。

2. MIM インストール パッケージを展開するか、MIM イメージ DVD をマウントします。

## MIM 2016 同期サービスのインストール

1. 展開した MIM インストール フォルダー内で、 **同期サービス** のフォルダーに移動します。

2. **MIM 同期サービス インストーラー**を実行します。 インストーラーのガイドラインに従って、インストールを完了します。

3. ウェルカム画面で、 **[次へ]**をクリックします。

    ![MIM インストーラー ウィザードの [ようこそ] の画像](media/MIM-Install1.png)

4. ライセンス条項を読み、同意する場合は**[次へ]**をクリックします。

5. **[カスタム セットアップ]** 画面で **[次へ]** をクリックします。

    ![カスタム セットアップの画像](media/MIM-Install2.png)

6.  同期データベースの構成画面では、次のように選択します。

    1.  SQL Server の配置場所: **このコンピューター**。

    2.  SQL Server のインスタンス: **既定のインスタンス**。

    ![データベース接続の画像](media/MIM-Install3.png)

7.  前に作成したアカウントに従って同期サービス アカウントを構成します。

    1.  サービス アカウント: *MIMSync*

    2.  ［パスワード］: *Pass@word1*

    3.  サービス アカウント ドメインまたはローカル コンピューターの名前: *contoso*

    ![サービス アカウントの画像](media/MIM-Install4.png)

8.  MIM 同期インストーラーに、関連するセキュリティ グループを指定します。

    1. 管理者 = *contoso\MIMSyncAdmins*

    2. 演算子 = *contoso\MIMSyncOperators*

    3. 参加者 = *contoso \mimsyncjoiners*

    4. コネクタ参照 = *contoso \mimsyncbrowse*

    5. WMI パスワード管理 = *contoso \mimsyncpasswordreset*

    ![セキュリティ グループの画像](media/MIM-Install5.png)

9. [セキュリティ設定] 画面で、**[受信 RPC 通信用のファイアウォール ルールを有効にする]** をオンにし、**[次へ]** をクリックします。

10. **[インストール]** をクリックして、MIM 同期のインストールを開始します。

    1. MIM 同期サービス アカウントに関する警告が表示される場合があります。 **[OK]**をクリックします。

    2. MIM 同期がインストールされます。

    3. 暗号化キーのバックアップ作成に関する注意が表示されます。**[OK]** をクリックし、暗号化キーのバックアップを格納するフォルダーを選択します。

        ![MIM 同期バックアップの暗号化キーの通知の画像](media/MIM-Install7.png)

    4. インストーラーによるインストールが正常に終了したら、 **[完了]**をクリックします。

    5. サインアウトしてサインインし、グループ メンバーシップの変更を有効にする必要があります。 **[はい]** をクリックして、サインアウトします。

>[!div class="step-by-step"]  
[«Exchange Server](prepare-server-exchange.md)
[MIM サービスおよびポータル»](install-mim-service-portal.md)



<!--HONumber=Jun16_HO4-->


