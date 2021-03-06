---
title: Microsoft Identity Manager 同期サービスのインストール | Microsoft Docs
description: 同期サービスをインストールおよび構成して、MIM 2016 コンポーネントを使用開始します。
keywords: ''
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 05/01/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 2585e9c5-ce34-46c7-bdcf-8c08773901dc
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d2f8c000205aacafaeb4e159ef692e9666b4b965
ms.sourcegitcommit: a98a4c1aee12016d480c400f4ff2c6aadb6518ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/03/2018
---
# <a name="install-mim-2016-mim-synchronization-service"></a>MIM 2016 のインストール: MIM 同期サービス

>[!div class="step-by-step"]
[« Exchange Server](prepare-server-exchange.md)
[MIM サービスおよびポータル »](install-mim-service-portal.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 次に例を示します。
> - ドメイン コントローラー名 - **corpdc**
> - ドメイン名 - **contoso**
> - MIM サービス サーバー名 - **corpservice**
> - MIM 同期サーバー名 - **corpsync**
> - SQL Server 名 - **corpsql**
> - パスワード - **Pass@word1**

Microsoft Identity Manager 2016 をインストールするには、最初にインストール パッケージをセットアップします。

1. ID 管理同期サーバー **corpsync** に使用しているサーバーに *contoso\miminstall* としてサインインします。

2. MIM インストール パッケージを展開するか、MIM イメージ DVD をマウントします。

## <a name="install-mim-2016-sp1-synchronization-service"></a>MIM 2016 SP1 同期サービスのインストール

1. 展開した MIM インストール フォルダー内で、 **同期サービス** のフォルダーに移動します。

2. **MIM 同期サービス インストーラー**を実行します。 インストーラーのガイドラインに従って、インストールを完了します。

3. ウェルカム画面で、 **[次へ]** をクリックします。

    ![MIM インストーラー ウィザードの [ようこそ] の画像](media/install-mim-sync/MIM_Install1.png)

4. ライセンス条項を読み、同意する場合は **[次へ]** をクリックします。

5. **[カスタム セットアップ]** 画面で **[次へ]** をクリックします。

    ![カスタム セットアップの画像](media/install-mim-sync/MIM_Install2.png)

6.  同期サービス データベースの構成画面で、次のように選択します。

    1.  SQL Server の場所: **corpsql.contoso.com** と呼ばれる**リモート マシン**。

    2.  SQL Server のインスタンス: **既定のインスタンス**

    ![データベース接続の画像](media/install-mim-sync/MIM_Install3.png)

7.  前に作成したアカウントに従って同期サービス アカウントを構成します。

    1.  サービス アカウント: *MIMSync*

    2.  パスワード: *Pass@word1*

    3.  サービス アカウント ドメインまたはローカル コンピューターの名前: *contoso*

    ![サービス アカウントの画像](media/install-mim-sync/MIM_Install4.png)

8.  MIM 同期サービスに関連するセキュリティ グループを次のとおりに指定します。

    1. 管理者 = *contoso\MIMSyncAdmins*

    2. 演算子 = *contoso\MIMSyncOperators*

    3. 参加者 = *contoso \mimsyncjoiners*

    4. コネクタ参照 = *contoso \mimsyncbrowse*

    5. WMI パスワード管理 = *contoso \mimsyncpasswordreset*

    ![セキュリティ グループの画像](media/install-mim-sync/MIM_Install5.png)

9. [セキュリティ設定] 画面で、**[受信 RPC 通信用のファイアウォール ルールを有効にする]** をオンにし、**[次へ]** をクリックします。

10. **[インストール]** をクリックして MIM 同期サービスのインストールを開始します。

    1. MIM 同期サービス アカウントに関する警告が表示される場合があります。 **[OK]** をクリックします。

    2. MIM 同期サービスがインストールされます。

    3. 暗号化キーのバックアップ作成に関する注意が表示されます。**[OK]** をクリックし、暗号化キーのバックアップを格納するフォルダーを選択します。

        ![MIM 同期バックアップの暗号化キーの通知の画像](media/MIM-Install7.png)

    4. インストーラーによるインストールが正常に終了したら、 **[完了]** をクリックします。

    5. サインアウトしてサインインし、グループ メンバーシップの変更を有効にする必要があります。 **[はい]** をクリックして、サインアウトします。

>[!div class="step-by-step"]  
[« Exchange Server](prepare-server-exchange.md)
[MIM サービスおよびポータル »](install-mim-service-portal.md)
