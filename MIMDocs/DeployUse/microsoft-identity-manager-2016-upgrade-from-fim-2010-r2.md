---
title: Microsoft Identity Manager 2016 への Forefront Identity Manager 2010 R2 のアップグレード |Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9471ccc1-bafe-46ee-b169-1464262380e1
author: kgremban
---
# Microsoft Identity Manager 2016 への Forefront Identity Manager 2010 R2 のアップグレード
このセクションでは、既存のテスト FIM 2010 R2 システムの MIM 2016 へのアップグレードについて説明します。 アップグレードのためのインストーラーは、新しい展開に使用するものと同じです。

このセクションでは、既存の FIM 2010 R2 ソリューションがテスト環境に展開されているものとします。 Windows Server 2012、Windows Server 2012 R2 または FIM 2010 R2 のサーバーでは、一般的なオペレーティング システムを Windows Server 2008 R2 および FIM 2010 r2 の場合、ローカルおよび環境の前提条件 (SQL Server、Exchange Server、SharePoint Services など) が構成済みのすべてでは、サーバーが実行されています。

1.  MIM 同期サービス (Sync) は最初に、AD ドメインに参加しているサーバーにインストールされて実行し、Sync の FIM 2010 R2 インスタンスを置き換えます。

2.  その後、MIM サービスとポータルがインストールされます。必要に応じて、SSPR 登録ポータルと SSPR サービス ポータルを含めることができ、Privileged Access Management 機能セットを除外できます。

3.  SSPR Windows ログイン統合クライアントを含む MIM アドインおよび拡張機能を別のクライアント コンピューターに展開できます。

## 準備

1.  FIM サービス データベース、FIM Sync データベース、FIM Sync、サービスの構成とソフトウェアをバックアップします。

2.  FIM 2010 R2 のコンポーネントがインストールされている-例: 各サーバーで *CORPIDM* – contoso \administrator としてログインします。 この展開の例では、FIM 2010 R2 を **MIM**にアップグレードするには管理者権限が必要です。

3.  MIM ソフトウェアをダウンロードまたは展開します。

## MIM 同期サービスのアップグレード

1.  FIM 2010 R2 同期サービス (“Sync”) が展開されているサーバーに管理者としてログインします。

2.  この手順を開始する前に、データベースをバックアップします。

3.   **[サービス]** コンソールを開き、 **Forefront Identity Manager Synchronization Service**を見つけて停止します。

    ![サービス コンソールの画像](media/MIM-UpgFIM1.PNG)

4.   **MIM 同期サービス インストーラー**を実行します。 インストーラーは既存の Sync バージョンを検出し、アップグレードを提案します。  **[更新]** ボタンをクリックして続行します。

5.  ライセンス条項に同意する場合は、 **[次へ]** をクリックして続行します。

6.  Sync が使用するサービス アカウントのパスワードを入力し、 **[次へ]**をクリックします。

    ![MIM 同期サービス アカウントのイメージを構成します。](media/MIM-UpgFIM3.png)

7.  セキュリティ グループ名が正しいことを確認し、 **[次へ]**をクリックします。

    ![MIM 同期のセキュリティ グループのイメージを構成します。](media/MIM-UpgFIM4.png)

8.  受信 RPC 通信のファイアウォール ルールのチェック ボックスはそのままにします。

9. インストーラーは Sync を FIM 2010 R2 から MIM にアップグレードする準備ができます。  **[アップグレード]** をクリックして、アップグレード プロセスを開始します。

10. アップグレードが進行しています。 アップグレードの進行中は、インストーラーを終了したり、コンピューターを再起動したりしないでください。

    ![MIM 同期のインストール ステータスの画像](media/MIM-UpgFIM7.png)

11. アップグレード中には、Sync データベースのアップグレードに関する警告が表示されます。 これが開始する前に DB をバックアップすることをお勧めします。

12. アップグレードが正常に完了したら、 **[完了]**をクリックします。

    ![MIM 同期のインストール成功イメージ](media/MIM-UpgSP1.png)

13.  **同期サービス** が再起動します。

## サービスおよびポータルのアップグレード

1.  FIM 2010 R2 のサービスおよびポータルが展開されているサーバーに管理者としてログインします。

2.   **[サービス]** コンソールを開き、 **Forefront Identity Manager Service**を見つけて停止します。

    ![サービス コンソールの画像](media/MIM-UpgFIM9.PNG)

3.  MIM サービスおよびポータルのインストーラーを実行します。 をクリックして、 **インストール** を続行する] ボタンをクリックします。

    ![MIM サービスおよびポータルのイメージをインストールします。](media/MIM-UpgSP2.png)

4.  ライセンス条項に同意する場合は、 **[次へ]** をクリックして続行します。

5.  [MIM カスタマー エクスペリエンス向上プログラム] 画面で、 **[次へ]** をクリックして続行します。

6.  MIM 機能およびコンポーネントをインストールし、をクリックするか選択 **次** 終了するとします。

    ![カスタム セットアップ イメージ](media/MIM-UpgSP4.png)

    1.  **MIM サービス:** この機能が、少なくとも 1 つのサーバーで必須であり、配置するか、SQL Server データベース サーバーが必要ですか、別のサーバーにします。

    2.  **MIM ポータル:** この機能は、少なくとも 1 つのサーバーで必須であり、SharePoint 2013 Foundation が必要です。

    3.  **MIM パスワード登録ポータル:** この機能は、セルフ サービス パスワード リセットに必要です。

    4.  **MIM パスワード リセット ポータル:** この機能は、パスワード リセットのために必要です。

7.  FIM サービス データベースに使用されている SQL Server の詳細を提供します。 既存のデータベースを再利用して、データを保持するオプションを選択します。 [次へ] をクリックして続行します。 **** 

8. 既存データベース再利用オプションを選択した場合は、データベースのバックアップを求めるメッセージが表示されます。

9. メール サーバーの詳細を入力します。 メール サーバーは、現在のサーバーに存在する場合は、メール サーバーの場所として"localhost"を入力します。 [次へ] をクリックして続行します。 **** 

    ![メール サーバーの接続のイメージを構成します。](media/MIM-UpgSP6.png)

10. クライアントの検証に使用するサービスの証明書を選択します。 以前 FIM サービスによって使用されていたローカル証明書ストアから既存の証明書を使用する必要があります。

    ![サービス証明書のイメージを構成します。](media/MIM-UpgSP7.png)

    1.  ローカルの証明書ストアのオプションを選択する場合はクリックして、 **証明書の選択** ボタンをクリックし、ポップアップ ウィンドウの一覧から証明書を選択します。 をクリックして **OK** し **次**します。

        ![証明書のポップアップ ウィンドウ イメージを選択します。](media/MIM-UpgSP8.PNG)

11. MIM サービスのサービス アカウント資格情報を構成します。 このサービス アカウントには、同期サービスと同じサービス アカウントを使用できないことに注意してください。 FIM サービスで使用されていたものと同じアカウントにする必要があります。

    ![MIM サービス アカウントの画像を構成します。](media/MIM-UpgSP9.png)

12. 前の手順で構成した MIM サービスの展開に応じて、MIM 同期サーバーの詳細を構成します。

    ![MIM サービスおよびポータルの同期のイメージを構成します。](media/MIM-UpgSP10.png)

13. MIM ポータルをインストールする場合は、MIM サービス サーバーのアドレスを提供します。  **[次へ]**をクリックします。

14. MIM ポータルをインストールする場合は、現在 FIM ポータルがホストされている SharePoint サイト コレクションの URL を提供します。  **[次へ]**をクリックします。

## MIM パスワード登録ポータルをインストールします。

1. MIM パスワード登録ポータルをインストールする場合は、パスワード登録ポータルの要求された URL を提供します。  **[次へ]**をクリックします。

2. クライアントとエンドユーザーがサービスおよびポータルを使用する機能を構成します。

    1.   **ファイアウォールでポート 5725 および 5726 を開く**かどうかを指定します。

    2.  かどうかを確認して **Grant MIM ポータル サイトへのユーザー アクセスを認証する**です。

    3.   **[次へ]**をクリックします。

3. MIM パスワード登録のアクセスの詳細と資格情報を提供します。

    ![MIM パスワード登録ポータルのイメージを構成します。](media/MIM-UpgSP15.png)

    1.  MIM パスワード登録のサービス アカウント名 (ドメインを含む) とパスワードを提供します。

    2.  パスワード登録ポータルのホスト (名前および 8080 などのポート) の詳細を提供します。

    3.   **[ファイアウォールでポートを開く]** オプションをオンにします。

    4.   **[次へ]**をクリックします。

4. 次の MIM パスワード登録構成画面で以下のようにします。

    1.  MIM パスワード登録に MIM サービスをインストールする場所を指示します。通常は同じシステムです。

    2.  このポータルにエクストラネットおよびイントラネットのユーザーがアクセスできるようにするか、または前に FIM パスワード リセットに構成したようにイントラネット ユーザーのみに許可するかを決定します。

## MIM パスワード リセット ポータルをインストールします。

1. MIM パスワード リセット ポータルをインストールする場合は、MIM パスワード リセットのアクセスの詳細と資格情報を提供します。

    ![MIM パスワード リセット ポータルのイメージを構成します。](media/MIM-UpgSP17.png)

    1.  MIM パスワード リセットのサービス アカウント名 (ドメインを含む) とパスワードを提供します。

    2.  パスワード リセット ポータルのホスト (名前および 8088 などのポート) の詳細を提供します。

    3.   **[ファイアウォールでポートを開く]** オプションをオンにします。

    4.   **[次へ]**をクリックします。

2. 次の MIM パスワード リセット構成画面で以下のようにします。

    1.  MIM サービスをインストールする場所を MIM パスワード リセットに指示します。

    2.  エクストラネット ユーザーとイントラネット ユーザーがこのポータルにアクセスできるか、またはイントラネット ユーザーのみかを指定します。

## インストールとアップグレードを完了します。

1. すべての構成の定義が正常に完了すると、インストール ページが表示されます。  **[インストール]** をクリックして、MIM サービスおよびポータルのインストールとアップグレードを開始します。

2. MIM サービスおよびポータルのインストールとアップグレードが行われます。 インストール中に、インストーラーを中止したりコンピューターを再起動したりしないでください。

3. MIM サービスおよびポータルのインストール (アップグレード) が正常に完了すると、確認画面が表示されます。  **[完了]** をクリックしてインストールを完了し、インストーラーを終了します。

4.  **Forefront Identity Manager Service** が再開したことを確認します。

注: FIM のアドインおよび拡張機能に展開されているユーザーのコンピューターに sspr、未構成場合 FIM のアドインおよび拡張機能をすべて MIM 2016 にアップグレードした後に再設定するまでのパスワードを新しい MFA 電話ゲートです。  FIM 2010 と FIM 2010 R2 アドインと拡張機能が新しいゲートを認識しないと、エラーが返ってきますユーザーでは、パスワードのリセットを完了できません。


<!--HONumber=Mar16_HO3-->

