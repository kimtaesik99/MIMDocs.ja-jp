---
タイトル: 手順 5-PRIV と CORP フォレスト間の信頼関係の構築
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
ms.assetid: eef248c4-b3b6-4b28-9dd0-ae2f0b552425
---
# 手順 5 - PRIV フォレストと CORP フォレスト間に信頼関係を確立する
 *PRIV* と *CONTOSO* これにより、ユーザーの信頼によってドメイン コント ローラーがバインドされている、 *PRIV* CORP ドメインのリソースにアクセスするドメインです。

1.  信頼関係を確立する前に、各ドメイン コントローラーで、相手側のドメイン コントローラーと DNS サーバーの IP アドレスに基づいて、相手側の DNS 名解決を構成する必要があります。

    1.  いずれかのドメインのコンピューターに対してドメイン名サービスを提供する DNS サーバーが他にないことを確認します。  仮想マシンにパブリック ネットワークに接続されているネットワーク インターフェイスがある場合、必要に応じて、Windows ネットワーク インターフェイスの設定を上書きして、DHCP で提供された DNS サーバー アドレスが仮想マシンに使用されないようにします。

    2.  (省略可能) *CORPDC*, 、PowerShell を起動し、次のコマンドを入力します。

        ```
        nslookup -qt=ns priv.contoso.local.
        ```
        出力にこのドメインのネームサーバー レコードが含まれていることを確認します。

    3.  (省略可能)または、使用して **DNS マネージャー** (に配置されている開始、アプリケーションのツール、DNS) の DNS 名の転送を確認する、 *PRIV* ドメイン *PRIVDC* IP アドレスです。  このプログラムを使用してノードを展開して *CORPDC、前方参照ゾーン contoso.local*, 、という名前のキーを確認して *priv* ネーム サーバー (NS) 型として表されます。

        ![](./media/PAM_GS_DNS_Manager.png)

2.   *PAMSRV*, 、一方向の信頼を確立 *CORPDC* CORP ドメイン コント ローラーを信頼するよう、 *PRIV* フォレストです。

    1.  ログインしていることを確認 *PAMSRV* として、 *PRIV* ドメイン管理者 (たとえば *priv \administrator*)。

    2.  PowerShell を起動します。

    3.  次の PowerShell コマンドを入力し、CORP ドメイン管理者の資格情報を入力してください (例: *contoso \administrator*) が表示されたら、必要な場合です。

        ```
        $ca = get-credential
        New-PAMTrust -SourceForest "contoso.local" -Credentials $ca
        New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials $ca
        ```

3.   *CORPDC*, 、PRIV 管理者と監視サービスによって AD への読み取りアクセスを有効にします。

    1.  ログインしていることを確認 *CORPDC* Contoso ドメイン管理者として (たとえば *contoso \administrator*)。

    2.   **[Active Directory ユーザーとコンピューター]**を起動します。

    3.  ドメインを右クリックして **contoso.local** 選択 **制御の委任**します。

    4.  [選択されたユーザーとグループ] タブで、**[追加]** をクリックします。

    5.   **コンピューターは、ユーザーの選択**, 、または **グループ** ポップアップで、をクリックして **場所** する場所を変更し、 *priv.contoso.local*します。  [オブジェクト名を入力 *Domain Admins* ] をクリック **名前の確認します。**ポップアップが表示されたら、ユーザー名の種類の *priv \administrator* とパスワード。

    6.  後 *Domain Admins*, 、型"*です。MIMMonitor*"です。 Domain Admins と MIMMonitor の名前に下線が表示されたら、**[OK]** をクリックし、**[次へ]** をクリックします。

    7.  一般的なタスクの一覧で選択"**すべてのユーザー情報を読み取る**"、順にクリックして **次** ] をクリック **完了**します。

    8.  閉じる **Active Directory ユーザーとコンピューター**秒です。

4.   *PAMSRV*, 、開始、 **監視およびコンポーネントのサービス**します。

    1.  ログインしていることを確認 *PAMSRV* として、 *PRIV* ドメイン管理者 (たとえば *priv \administrator*)。

    2.  PowerShell を起動します。

    3.  次の PowerShell コマンドを入力します。

        ```
        net start "PAM Component service"
        net start "PAM Monitoring service"
        ```

5.  (省略可能)SID の履歴が有効になってからの信頼に SID フィルター処理が無効になっていることを確認、 *CORP* ドメイン、 *PRIV* ドメイン。

    1.  ログインしていることを確認 *CORPDC* ドメイン管理者として (たとえば *contoso \administrator*)。

    2.  PowerShell ウィンドウを開きます。

    3.  使用 **netdom** SID の履歴が有効になっているし、SID のフィルター処理が無効になっていることを確認します。  種類:

        ```
        netdom trust contoso.local /quarantine /enablesidhistory:yes /domain priv.contoso.local
        ```
        出力は、いずれかを示す必要があります"**この信頼に対して有効にする SID 履歴**「または」**この信頼に対して SID の履歴が既に有効になっている**"です。

        出力を示す必要があります"**この信頼に対して SID のフィルター処理が有効になっていない**"です。 参照してください [を無効にする SID フィルター検疫](http://technet.microsoft.com/library/cc772816.aspx)  の詳細。


<!--HONumber=Mar16_HO1-->


