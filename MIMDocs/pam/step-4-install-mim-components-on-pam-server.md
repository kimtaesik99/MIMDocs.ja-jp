---
title: "PAM の展開、手順 4 – MIM のインストール | Microsoft Docs"
description: "Privileged Access Management サーバーとワークステーションに MIM サービスとポータルをインストールして構成します。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ROBOTS: noindex,nofollow
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 3a1ec9db6da0a77f963dde76a3efe8d92f89078d
ms.lasthandoff: 05/02/2017


---

# <a name="step-4--install-mim-components-on-pam-server-and-workstation"></a>手順 4 - PAM サーバーとワークステーションに MIM コンポーネントをインストールする

>[!div class="step-by-step"]
[«手順 3](step-3-prepare-pam-server.md)
[手順 5 »](step-5-establish-trust-between-priv-corp-forests.md)


PAMSRV で、MIM サービスおよびポータル、サンプル ポータル Web アプリケーションをインストールできるように、PRIV\Administrator としてサインインします。

  > [!NOTE]
  > ドメイン管理者である必要があります。ドメイン管理者として次のコマンドを実行しない場合、次の手順で行う信頼検証チェックは完了しません。

MIM をダウンロードした場合は、新しいフォルダーに MIM のインストール アーカイブを展開します。

##  <a name="run-the-service-and-portal-install-program"></a>サービスおよびポータルのインストール プログラムを実行します。  

インストーラーのガイドラインに従って、インストールを完了します。

1.  コンポーネントの機能を選択する際には、MIM サービス (MIM Reporting 付きではなく、Privileged Access Management 付きのもの) および MIM ポータルを含めるようにします。  

    ![カスタム セットアップ - スクリーンショット](./media/PAM_GS_MIM_2015_Service_Portal.png)

2.  一般的なサービスと MIM データベース接続を構成する場合は、**[新しいデータベースを作成]** を指定します。

    > [!NOTE]
    > 高可用性のために MIM サービスを 2 回以上インストールする場合は、2 回目以降のすべてのインストールで **[既存のデータベースを使用]** を指定します。

3.  メール サーバーの接続を構成するときは、メール サーバーを CORP 環境の Exchange または SMTP サーバーのホスト名 (メール サーバーがない場合は「corpdc.contoso.local」) に設定し、**[SSL を使用する]** と **[メール サーバーは Exchange Server 2007 または Exchange Server 2010]** の各チェックボックスをオフにします。

4.  新しい自己署名証明書を生成することを選びます。

5.  次のアカウントの資格情報を設定します。
    - サービス アカウント名: *MIMService*  
    - サービス アカウント パスワード: *Pass@word1* (または手順 2 で作成したパスワード)  
    - サービス アカウント ドメイン: *PRIV*  
    - サービス メール アカウント: *MIMService@priv.contoso.local*  

6.  同期サーバーのホスト名は既定値をそのまま使い、MIM 管理エージェント アカウントには「*PRIV\MIMMA*」を指定します。 MIM 同期サービスが存在しないという警告メッセージが表示されます。 このシナリオでは MIM 同期サービスを使わないので、この警告は問題ありません。

7.  MIM サービス サーバーのアドレスとして「*PAMSRV*」を設定します。

8.  SharePoint サイト コレクション URL として「*http://pamsrv.priv.contoso.local:82*」を設定します。

9. 登録ポータル URL を空白のままにします。

10. ファイアウォールのポート 5725 と 5726 を開くチェックボックスと、すべての認証済みユーザーに MIM ポータル サイトに対するアクセス権を付与するチェックボックスをオンにします。

11. PAM REST API ホスト名は空白のままにし、ポート番号として *8086* を設定します。

  ![PAM REST API のバインド情報 - スクリーンショット](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

12. SharePoint と同じアカウントを使用するように MIM PAM REST API アカウントを構成します (MIM ポータルはこのサーバーに併置されているため)。
    - アプリケーション プール アカウント名: *SharePoint*  
    - アプリケーション プール アカウント パスワード: *Pass@word1* (または手順 2 で作成したパスワード)  
    - アプリケーション プール アカウント ドメイン: *PRIV*  

    ![アプリケーション プール アカウントの資格情報のスクリーンショット](./media/PAM_GS_Configure_Component_Service.png)

    現在の構成ではサービス アカウントがセキュリティ保護されていないことを警告するメッセージが表示されることがあります。 これは問題ありません。

13. MIM PAM コンポーネント サービスを構成します。
    - サービス アカウント名: *MIMComponent*
    - サービス アカウント パスワード: *Pass@word1* (または手順 2 で作成したパスワード)  
    - サービス アカウント ドメイン: *PRIV*

  ![PAM コンポーネント サービスのアカウント資格情報 - スクリーンショット](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

14. PAM 監視サービスを構成します。
    - サービス アカウント名: *MIMMonitor*  
    - サービス アカウント パスワード: *Pass@word1* (または手順 2 で作成したパスワード)  
    - サービス アカウント ドメイン: *PRIV*  

  ![PAM 監視サービスのアカウント資格情報のスクリーンショット](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

15. [MIM パスワード ポータルの情報の入力] ページで、チェックボックスをオフのままにして続行します。 **[次へ]** をクリックし、インストールを続行します。

インストールが完了して、サーバーは再起動したら、MIM ポータルがアクティブであることと、ユーザーが MIM で自分のオブジェクト リソースを表示できることを確認します。

## <a name="set-up-mim-portal-management-policy-rules"></a>MIM ポータル管理ポリシー規則のセットアップ

1. PAMSRV が再起動したら、PRIV\Administrator としてサインインします。

2. Internet Explorer を起動し、http://pamsrv.priv.contoso.local:82/identitymanagement 上の MIM ポータルに接続します。 このページに初めてアクセスするときは、短時間の遅延が発生することがあります。

3. 必要に応じて、Internet Explorer に対して PRIV\Administrator としてサインインします。

4. Internet Explorer で、**[インターネット オプション]** を開き、**[セキュリティ]** タブに切り替えます。**[ローカル イントラネット ソーン]** に該当するサイトがまだ存在しない場合は、そのサイトを追加します。 [インターネット オプション] ダイアログを閉じます。

5. Internet Explorer を使って MIM ポータルを表示し、**[管理ポリシー規則]** をクリックします。

6. 管理ポリシー規則の **[ユーザー管理: ユーザーは自分の属性を読み取ることができる]**を検索します。

7. この管理ポリシー規則を選択し、**[ポリシーを無効にする]** をオフにして **[OK]** をクリックし、**[送信]** をクリックします。

## <a name="verify-the-firewall-connections"></a>ファイアウォール接続の確認

ファイアウォールが TCP ポート 5725、5726、8086、8090 との受信接続を許可していることを確認します。

1.  **セキュリティが強化された Windows ファイアウォール** を起動します (管理ツールにあります)。  
2.  **[受信の規則]**をクリックします。  
3.  次の 2 つの規則が一覧に表示されていることを確認します。  
    - Forefront Identity Manager サービス (STS)
    - Forefront Identity Manager サービス (Webservice)  
4.  **[新しい規則]** > **[ポート]** > **[TCP]** をクリックし、特定のローカル ポート *8086* および *8090* を入力します。 既定値のままウィザードを次に進め、規則に名前を付けて **[完了]** をクリックします。  
5.  ウィザードが完了したら、Windows ファイアウォール アプリケーションを閉じます。

6.  **[コントロール パネル]**を起動します。  
7.  [ネットワークとインターネット] の **[ネットワークの状態とタスクの表示]** をクリックします。  
8.  "priv.contoso.local" および [ドメイン ネットワーク] として表示されたアクティブなネットワークがあることを確認します。  
9. **[コントロール パネル]**を閉じます。

## <a name="set-up-the-sample-web-application"></a>サンプル Web アプリケーションのセットアップ

このセクションでは、MIM PAM REST API のサンプル Web アプリケーションをインストールして構成します。

1.  サンプル Web アプリケーション アーカイブから [ID 管理のサンプル](https://github.com/Azure/identity-management-samples)を zip ファイルとしてダウンロードします。

2. フォルダー **identity-management-samples-master\Privileged-Access-Management-Portal\src** の内容を新しいフォルダー **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal** に展開します。

3.  "MIM Privileged Access Management Example Portal"というサイト名で、物理パスが "C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Privileged Access Management Portal" で、ポートが 8090 の新しい Web サイトを IIS で作成します。  この処理は、次の PowerShell コマンドで実行できます。

  ```
  New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
  ```

4.  サンプル Web アプリケーションがユーザーを MIM PAM REST API にリダイレクトできるようにセットアップします。 メモ帳などのテキスト エディターを使って、ファイル **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management REST API\web.config** を編集します。 **system.webServer** セクションに、次の行を追加します。

  ```
  <httpProtocol>
    <customHeaders>
      <add name="Access-Control-Allow-Credentials" value="true"  />
      <add name="Access-Control-Allow-Headers" value="content-type" />
      <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
    </customHeaders>
  </httpProtocol>
  ```

5.  サンプル Web アプリケーションを構成します。 メモ帳などのテキスト エディターを使用して、ファイル **C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js** を編集します。 **pamRespApiUrl** の値を *http://pamsrv.priv.contoso.local:8086/api/pamresources/* に設定します。

6.  これらの変更を有効にするために、次のコマンドで IIS を再起動します。

  ```
  iisreset
  ```

7.  (省略可能) ユーザーが REST API に対して認証できることを確認します。 PAMSRV で管理者として Web ブラウザーを開きます。  Web サイト URL http://pamsrv.priv.contoso.local:8086/api/pamresources/pamroles/ にアクセスし、必要に応じて認証し、ダウンロードが実行されることを確認します。

## <a name="install-the-mim-pam-requestor-cmdlets"></a>MIM PAM Requestor コマンドレットをインストールします。

手順 1 で構成したワークステーションに MIM PAM Requestor コマンドレットをインストールします。

1.  CORPWKSTN に管理者としてサインインします。

2.  **アドインと拡張機能**を CORPWKSTN コンピューターにダウンロードします (まだダウンロードしていない場合)。

3.  フォルダーの **アドインと拡張機能** のアーカイブを新しいフォルダーに展開します。

4.  インストーラー **setup.exe** を実行します。

5.  カスタム設定で、**PAM クライアント**をインストールするように指定し、**Outlook 用 MIM アドイン**と、**MIM パスワードおよび認証の拡張機能**はインストールしないように指定します。

6.  PAM サーバーのアドレスには、PRIV MIM サーバーのホスト名として *pamsrv.priv.contoso.local* を指定します。

インストールが完了したら、CORPWKSTN を再起動して、新しい PowerShell モジュールの登録を完了します。

次の手順では、PRIV フォレストと CORP フォレストの間に信頼関係を確立します。

>[!div class="step-by-step"]
[«手順 3](step-3-prepare-pam-server.md)
[手順 5 »](step-5-establish-trust-between-priv-corp-forests.md)

