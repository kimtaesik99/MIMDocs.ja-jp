---
title: "FIM 2010 R2 から Microsoft Identity Manager 2016 へのアップグレード | Microsoft Docs"
description: "FIM 2010 R2 コンポーネントをアップグレードし、MIM 2016 で新しく導入されたコンポーネントをインストールする方法について説明します。"
keywords: 
author: fimguy
ms.author: billmath
manager: femila
ms.date: 02/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 2d3092d7d41090e4e03b971fb62ca896cc8db282
ms.openlocfilehash: 20e733f17d6ed590844c526888b649eb6bf5f322
ms.lasthandoff: 02/13/2017


---

# <a name="upgrade-from-forefront-identity-manager-2010-r2"></a>Forefront Identity Manager 2010 R2 からのアップグレード

現在 Forefront Identity Manager (FIM) 2010 R2 をご利用で、Microsoft Identity Manager (MIM) 2016 にアップグレードしたい場合は、この記事を参照してください。 アップグレードは次の 3 ステップです:

1.  MIM 2016 同期サービス (Sync) を、Active Directory (AD) ドメインに参加しているサーバーにインストールします。 これにより Sync の FIM 2010 R2 インスタンスが置き換えられます。

2.  MIM サービスとポータルをインストールします。 その際に、セルフサービス パスワード リセット (SSPR) 登録ポータルおよびサービス ポータルのインストールも選択することができます。 Privileged Access Management 機能セットは除外して、後でインストールすることができます。

3.  MIM アドインおよび拡張機能を別のクライアント コンピューターに展開します。 これには SSPR Windows ログイン統合クライアントが含まれます。


この記事では、次の手順を完了済みであることを前提としています:
- FIM 2010 R2 がテスト環境に展開されている
- サーバーが Windows Server 2012、Windows Server 2012 R2 または Windows Server 2008 R2 で実行されている
- ローカルおよび環境に関するすべての前提条件 (SQL Server、Exchange Server、SharePoint Services など) は FIM 2010 R2 用に構成されている


## <a name="preparation"></a>準備

1.  FIM サービス データベース、FIM Sync データベース、FIM Sync、サービスの構成とソフトウェアをバックアップします。

2.  FIM 2010 R2 のコンポーネント (*CORPIDM* など) がインストールされている各サーバーで、Contoso\Administrator としてログインします。 この展開の例では、FIM 2010 R2 を **MIM**にアップグレードするには管理者権限が必要です。

3.  MIM ソフトウェアをダウンロードまたは展開します。

## <a name="upgrade-the-synchronization-service"></a>同期サービスをアップグレードする

1.  FIM 2010 R2 同期サービス (“Sync”) が展開されているサーバーに管理者としてログインします。

2.  この手順を開始する前に、データベースをバックアップします。

3.  **[サービス]** コンソールを開き、 **Forefront Identity Manager Synchronization Service**を見つけて停止します。

    ![サービス コンソールの画像](media/MIM-UpgFIM1.PNG)

4.  **MIM 同期サービス インストーラー**を実行します。 インストーラーは既存の Sync バージョンを検出し、アップグレードを提案します。 **[更新]** ボタンをクリックして続行します。

5.  ライセンス条項に同意する場合は、 **[次へ]** をクリックして続行します。

6.  Sync が使用するサービス アカウントのパスワードを入力し、 **[次へ]**をクリックします。

    ![MIM 同期サービス アカウントの構成の画像](media/MIM-UpgFIM3.png)

7.  セキュリティ グループ名が正しいことを確認し、 **[次へ]**をクリックします。

    ![MIM 同期サービス セキュリティ グループの構成の画像](media/MIM-UpgFIM4.png)

8.  受信 RPC 通信のファイアウォール ルールのチェック ボックスはそのままにします。

9. インストーラーは Sync を FIM 2010 R2 から MIM にアップグレードする準備ができます。 **[アップグレード]** をクリックして、アップグレード プロセスを開始します。

10. アップグレードが進行しています。 アップグレードの進行中は、インストーラーを終了したり、コンピューターを再起動したりしないでください。

    ![MIM 同期のインストール ステータスの画像](media/MIM-UpgFIM7.png)

11. アップグレード中には、Sync データベースのアップグレードに関する警告が表示されます。 これが開始する前に DB をバックアップすることをお勧めします。

12. アップグレードが正常に完了したら、 **[完了]**をクリックします。

    ![MIM 同期のインストール成功の画像](media/MIM-UpgSP1.png)

13. **同期サービス** が再起動します。

## <a name="upgrade-the-service-and-portal"></a>サービスおよびポータルをアップグレードする

1.  FIM 2010 R2 のサービスおよびポータルが展開されているサーバーに管理者としてログインします。

2.  **[サービス]** コンソールを開き、 **Forefront Identity Manager Service**を見つけて停止します。

    ![サービス コンソールの画像](media/MIM-UpgFIM9.PNG)

3.  MIM サービスおよびポータルのインストーラーを実行します。 **[インストール]** ボタンをクリックして続行します。

    ![MIM サービスおよびポータルのインストールの画像](media/MIM-UpgSP2.png)

4.  ライセンス条項に同意する場合は、 **[次へ]** をクリックして続行します。

5.  [MIM カスタマー エクスペリエンス向上プログラム] 画面で、 **[次へ]** をクリックして続行します。

6.  インストールする MIM 機能およびコンポーネントを選択し、終ったら **[次へ]** をクリックします。

    ![カスタム セットアップの画像](media/MIM-UpgSP4.png)

    1.  **MIM サービス:** この機能は、少なくとも 1 つのサーバーで必須であり、同じサーバーまたは別のサーバーに SQL Server データベース サーバーが必要です。

    2.  **MIM ポータル:** この機能は、少なくとも 1 つのサーバーで必須であり、SharePoint 2013 Foundation が必要です。

    3.  **MIM パスワード登録ポータル:** この機能は、セルフサービスのパスワード リセットのために必要です。

    4.  **MIM パスワード リセット ポータル:** この機能は、パスワード リセットのために必要です。

7.  FIM サービス データベースに使用されている SQL Server の詳細を提供します。 既存のデータベースを再利用して、データを保持するオプションを選択します。 **[次へ]** をクリックして続行します。 

8. 既存データベース再利用オプションを選択した場合は、データベースのバックアップを求めるメッセージが表示されます。

9. メール サーバーの詳細を入力します。 現在のサーバーにメール サーバーが存在する場合は、メール サーバーの場所に "localhost" と入力します。 **[次へ]**をクリックして続行します。 

    ![メール サーバーの接続の構成の画像](media/MIM-UpgSP6.png)

10. クライアントの検証に使用するサービスの証明書を選択します。 以前 FIM サービスによって使用されていたローカル証明書ストアから既存の証明書を使用する必要があります。

    ![サービス証明書の構成の画像](media/MIM-UpgSP7.png)

    1.  ローカルの証明書ストアのオプションを選択する場合は、**[証明書の選択]** ボタンをクリックし、ポップアップ ウィンドウの一覧から証明書を選択します。 **[OK]** をクリックし、**[次へ]** をクリックします。

        ![[証明書の選択] ポップアップ ウィンドウの画像](media/MIM-UpgSP8.PNG)

11. MIM サービスのサービス アカウント資格情報を構成します。 このサービス アカウントには、同期サービスと同じサービス アカウントを使用できないことに注意してください。 FIM サービスで使用されていたものと同じアカウントにする必要があります。

    ![MIMサービス アカウント イメージを構成する](media/MIM-UpgSP9.png)

12. 前の手順で構成した MIM サービスの展開に応じて、MIM 同期サーバーの詳細を構成します。

    ![MIM サービスおよびポータルの同期の構成の画像](media/MIM-UpgSP10.png)

13. MIM ポータルをインストールする場合は、MIM サービス サーバーのアドレスを提供します。 **[次へ]**をクリックします。

14. MIM ポータルをインストールする場合は、現在 FIM ポータルがホストされている SharePoint サイト コレクションの URL を提供します。 **[次へ]**をクリックします。

## <a name="install-the-mim-password-registration-portal"></a>MIM パスワード登録ポータルをインストールする

1. MIM パスワード登録ポータルをインストールする場合は、パスワード登録ポータルの要求された URL を提供します。 **[次へ]**をクリックします。

2. クライアントとエンドユーザーがサービスおよびポータルを使用する機能を構成します。

    1.  **ファイアウォールでポート 5725 および 5726 を開く**かどうかを指定します。

    2.  **認証されたユーザーの MIM ポータル サイトへのアクセスを許可する**かどうかを指定します。

    3.  **[次へ]**をクリックします。

3. MIM パスワード登録のアクセスの詳細と資格情報を提供します。

    ![MIM パスワード登録ポータルの構成の画像](media/MIM-UpgSP15.png)

    1.  MIM パスワード登録のサービス アカウント名 (ドメインを含む) とパスワードを提供します。

    2.  パスワード登録ポータルのホスト (名前および 8080 などのポート) の詳細を提供します。

    3.  **[ファイアウォールでポートを開く]** オプションをオンにします。

    4.  **[次へ]**をクリックします。

4. 次の MIM パスワード登録構成画面で以下のようにします。

    1.  MIM パスワード登録に MIM サービスをインストールする場所を指示します。通常は同じシステムです。

    2.  このポータルにエクストラネットおよびイントラネットのユーザーがアクセスできるようにするか、または前に FIM パスワード リセットに構成したようにイントラネット ユーザーのみに許可するかを決定します。

## <a name="install-the-mim-password-reset-portal"></a>MIM パスワード リセット ポータルをインストールする

1. MIM パスワード リセット ポータルをインストールする場合は、MIM パスワード リセットのアクセスの詳細と資格情報を提供します。

    ![MIM パスワード リセット ポータルの構成の画像](media/MIM-UpgSP17.png)

    1.  MIM パスワード リセットのサービス アカウント名 (ドメインを含む) とパスワードを提供します。

    2.  パスワード リセット ポータルのホスト (名前および 8088 などのポート) の詳細を提供します。

    3.  **[ファイアウォールでポートを開く]** オプションをオンにします。

    4.  **[次へ]**をクリックします。

2. 次の MIM パスワード リセット構成画面で以下のようにします。

    1.  MIM サービスをインストールする場所を MIM パスワード リセットに指示します。

    2.  エクストラネット ユーザーとイントラネット ユーザーがこのポータルにアクセスできるか、またはイントラネット ユーザーのみかを指定します。

## <a name="finish-installation-and-upgrade"></a>インストールの完了とアップグレード

1. すべての構成の定義が正常に完了すると、インストール ページが表示されます。 **[インストール]** をクリックして、MIM サービスおよびポータルのインストールとアップグレードを開始します。

2. MIM サービスおよびポータルのインストールとアップグレードが行われます。 インストール中に、インストーラーを中止したりコンピューターを再起動したりしないでください。

3. MIM サービスおよびポータルのインストール (アップグレード) が正常に完了すると、確認画面が表示されます。 **[完了]** をクリックしてインストールを完了し、インストーラーを終了します。

4. **Forefront Identity Manager Service** が再開したことを確認します。

注: FIM のアドインおよび拡張機能がユーザーの SSPR 用のコンピューターに現在展開されている場合、FIM のアドインおよび拡張機能をすべて MIM 2016 にアップグレードするまで、パスワードのリセットに新しい MFA 電話ゲートを構成しないでください。  FIM 2010 と FIM 2010 R2 のアドインと拡張機能は新しいゲートを認識しないため、エラーが返され、ユーザーはパスワードのリセットを完了できません。

Microsoft Identity Manager 2016 SP1 のアップグレード ガイダンスについては、次の [Microsoft Identity Manager 2016 Service Pack 1 アップグレード パッケージ](https://blogs.technet.microsoft.com/iamsupport/2016/11/08/microsoft-identity-manager-2016-service-pack-1-update-package/)に関する記事をご覧ください。
