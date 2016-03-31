---
タイトル: 手順 4-PAM サーバーとワークステーションに MIM のインストール コンポーネント
ms.custom:
  - Id 管理
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: ef605496-7ed7-40f4-9475-5e4db4857b4f
ロボット: noindex、nofollow
---
# 手順 4 - PAM サーバーとワークステーションに MIM コンポーネントをインストールする

1.   *PAMSRV*, 、としてログイン *priv \administrator* MIM サービスおよびポータルとサンプル ポータル web アプリケーションのインストールを可能にします。 (ドメイン管理者である必要があります。ドメイン管理者として次のコマンドを実行していない場合、次の手順で行う後続の信頼検証チェックは完了しません)。

2.  MIM をダウンロードした場合は、新しいフォルダーに MIM のインストール アーカイブを展開します。

3.  サービスおよびポータルのインストール プログラムを実行します。  インストーラーのガイドラインに従って、インストールを完了します。

    1.  コンポーネントの機能を選択するときに、次を含める必要があります。

        -   MIM サービス (MIM Reporting を含めずに、Privileged Access Management を含めます)

        -   MIM ポータル

            ![](./media/PAM_GS_MIM_2015_Service_Portal.png)

    2.  一般的なサービスと MIM データベース接続を構成する場合は、[新しいデータベースを作成] を指定します。

    3.  メール サーバーの接続を構成するときに、メール サーバーを設定"*corpdc.contoso.local*""を使用して SSL"をオフにし、"メール サーバーが Exchange Server 2007 または Exchange Server 2010] チェック ボックスをオンします。

    4.  新しい自己署名証明書を生成するように指定します。

    5.  としてサービス アカウント名を指定"*MIMService*"、およびサービス アカウントのパスワードに"*Pass@word1*"(上記の手順 2 で指定したパスワード)、"PRIV"とサービスの電子メールのアカウントとしてサービス アカウントのドメイン"*MIMService@priv.contoso.local*"です。

    6.  同期サーバーのホスト名の既定値をそのまま使用し、として MIM 管理エージェント アカウントを指定"*priv \mimma*"です。  MIM 同期サービスが存在しないという警告メッセージが表示されます。  TLG シナリオでは MIM 同期サービスは使用されないので、この警告は問題ありません。

    7.  指定"*pamsrv*"MIM サービス サーバーのアドレスにします。

    8.  指定"*http://pamsrv.priv.contoso.local:82*"として、SharePoint サイト コレクション URL。

    9. 登録ポータル URL を空白のままにします。

    10. ファイアウォールのポート 5725 と 5726 を開くチェックボックスと、すべての認証済みユーザーに MIM ポータル サイトに対するアクセス権を付与するチェックボックスをオンにします。

    11. (以下のスクリーンショットのように) PAM REST API ホスト名を空白のままにして、ポート番号に 8086 を指定します。

        ![](./media/PAM_GS_MIM_2015_Service_Portal_configure_application_pool.png)

    12. SharePoint と同じアカウントを使用するアカウントを MIM PAM REST API を構成する (MIM ポータルが共存これに存在している"*SharePoint*"、およびアプリケーション プール アカウントのパスワードに"*Pass@word1*"(上記の手順 2 で指定したパスワード) とアプリケーション プール アカウントのドメインに"*PRIV*"です。

        ![](./media/PAM_GS_Configure_Component_Service.png)

        現在の構成ではサービス アカウントがセキュリティ保護されていないことを警告するメッセージが表示されます。

    13. MIM PAM コンポーネント サービスを構成します。 としてアカウント名を指定"*mimcomponent*"、およびサービス アカウントのパスワードに"*Pass@word1*"(上記の手順 2 で指定したパスワード)、およびサービス アカウントのドメインに"*PRIV*"です。

        ![](./media/PAM_GS_Configure_MIM_PAM_component_service.png)

    14. PAM 監視サービスを構成します。 としてアカウント名を指定"*mimmonitor*"、およびサービス アカウントのパスワードに"*Pass@word1*"(上記の手順 2 で指定したパスワード)、およびサービス アカウントのドメインに"*PRIV*"です。

        ![](./media/PAM_GS_Configur_PAM_Monitoring_service.png)

    15. [MIM パスワード ポータルの情報の入力] ページで、チェックボックスをオフのままにして続行します。  クリックして **次** インストールを続行します。

4.  インストールが完了して、サーバーは再起動したら、MIM ポータルがアクティブであることと、ユーザーが MIM で自分のオブジェクト リソースを表示できることを確認します。

    -   PAMSRV が再起動したら、PRIV\Administrator としてログオンします。

    -   Internet Explorer を起動し、上の MIM ポータルに接続"*http://pamsrv.priv.contoso.local:82/identitymanagement*"です。

        このページに初めてアクセスするときは、短時間の遅延が発生することがあります。

    -   必要に応じて、Internet Explorer に対して PRIV\Administrator として認証します。

    -   Internet Explorer で開く、 **インターネット オプション**, 、変更、 **セキュリティ] タブ**, 、サイトの追加と、"*ローカル イントラネット*"ゾーンがない場合。  [インターネット オプション] ダイアログを閉じます。

    -   Internet Explorer を使用して MIM ポータルを表示し、[管理ポリシー規則] をクリックします。

    -   管理ポリシー規則のユーザー管理の検索: *ユーザーは、独自の属性を読み取ることができます*します。

    -   この管理ポリシー規則を選択し、ボックスをオフに"**ポリシーが無効になって**"、] をクリックして **OK** ] をクリックし、 **送信**します。

5.  ファイアウォールが TCP ポート 5725、5726、8086、および 8090 との受信接続を許可していることを確認します。

    1.   **セキュリティが強化された Windows ファイアウォール** を起動します (管理ツールにあります)。

    2.   **[受信の規則]**をクリックします。

    3.  2 つの規則を確認"*Forefront Identity Manager サービス (STS)*「と」*Forefront Identity Manager サービス (Webservice)*"一覧表示されます。

    4.  をクリックして **新しいルール**, [ **ポート**, を選択 **TCP**, 、特定のローカル ポート 8086、8090 を入力します。   既定値のままウィザードを次に進め、規則に名前を付けて **[完了]**をクリックします。

    5.  ウィザードが完了したら、Windows ファイアウォール アプリケーションを閉じます。

    6.   **[コントロール パネル]**を起動します。

    7.  をクリックして"**ネットワークの状態とタスクを表示**「, 下に配置されて」**ネットワークとインターネット**"です。

    8.  として一覧にあるアクティブなネットワークがあることを確認"*priv.contoso.local*"として、"*ドメイン ネットワーク*"です。

    9.  **[コントロール パネル]**を閉じます。

6.  サンプル web アプリケーション アーカイブ (からアーカイブをダウンロード [Id 管理サンプル](https://github.com/Azure/identity-management-samples)) identity-management-samples-master\Privileged-Access-Management-Portal\src フォルダーの内容をフォルダー C:\Program files \microsoft Forefront Identity \2010 内に新しいフォルダー Privileged Access Management Portal に展開します。

7.  MIM PAM REST API のサンプル Web アプリケーションをインストールして構成します。

    1.  IIS でのサイト名で新しい web サイトを作成"*MIM Privileged Access Management Example Portal*"、"C:\Program files \microsoft Forefront Identity manager \2010\privileged Access Management Portal"の物理パス、ポートが 8090 です。  この処理は、次の PowerShell コマンドで実行できます。

        ```
        New-WebSite -Name "MIM Privileged Access Management Example Portal" -Port 8090   -PhysicalPath "C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\"
        ```

    2.  サンプル Web アプリケーションがユーザーを MIM PAM REST API にリダイレクトできるようにします。 ファイルを編集してメモ帳などのテキスト エディターを使用して"*C:\Program files \microsoft Forefront Identity manager \2010\privileged Access Management REST api \web.config*"です。  *& Lt;system.webServer & gt;* セクションで、次の行を追加します。

        ```
        <httpProtocol>
                <customHeaders>
        <add name="Access-Control-Allow-Credentials" value="true"  />
        <add name="Access-Control-Allow-Headers" value="content-type" />
        <add name="Access-Control-Allow-Origin" value="http://pamsrv:8090" />  
                </customHeaders>
        </httpProtocol>
        ```

    3.  サンプル Web アプリケーションを構成します。 メモ帳などのテキスト エディターを使用して、"C:\Program Files\Microsoft Forefront Identity Manager\2010\Privileged Access Management Portal\js\utils.js" ファイルを編集します。 このファイルで、pamRespApiUrl の値を "http://pamsrv.priv.contoso.local:8086/api/pamresources/" に設定します。

    4.  変更を反映するために IIS を再起動します。

        ```
        iisreset
        ```

    5.  (省略可能) ユーザーが REST API に対して認証できることを確認します。  *pamsrv*で、管理者として Web ブラウザーを開きます。  Web サイトの URL に移動 *http://pamsrv.priv.contoso.local:8086/api/し/* (必要な場合) を認証、およびダウンロードが実行できるようにします。

8.  ワークステーションに MIM PAM Requestor コマンドレットをインストールします。

    1.  管理者として、CORPWKSTN にログインします。

    2.  ダウンロード、 **アドインおよび拡張機能** CORPWKSTN コンピューターに存在しない場合。

    3.  フォルダーのアドインと拡張機能のアーカイブを新しいフォルダーに展開します。

    4.  インストーラーを実行 **setup.exe**します。

    5.  カスタム設定で、PAM クライアントをインストールし、Outlook 用 MIM アドイン、MIM パスワードおよび認証の拡張機能はインストールしないように指定しています。

    6.  PAM サーバーのアドレスでは、PRIV MIM サーバーのホスト名として指定 *pamsrv.priv.contoso.local*します。

9. インストールが完了したら、CORPWKSTN を再起動して、新しい PowerShell モジュールの登録を完了します。


<!--HONumber=Mar16_HO1-->


