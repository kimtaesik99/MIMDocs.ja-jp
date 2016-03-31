---
タイトル: 手順 2 - PRIV ドメイン コント ローラーを準備します。
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
作成者: Kgremban
---
# 手順 2 - PRIV ドメイン コントローラーを準備する
## 新しい特権アクセス管理フォレストの作成

この手順では、新しい特権アクセスの管理フォレスト用に新しいドメインを作成します。

### Windows Server 2012 R2 のインストール
ソフトウェアがインストールされた新しい仮想マシンを別、コンピューター"PRIVDC"するための Windows Server 2012 R2 をインストールします。

1. Windows Server の (アップグレードではなく) カスタム インストールを選択して実行します。 インストール時に、 *[Windows Server 2012 R2 Standard (GUI 使用サーバー) x64]*を指定します。 **Data Center または Server Core** は選択 *しないでください*。

2. ライセンス条項を確認して同意します。

3. ディスクは空になるため、 *[カスタム: Windows のみをインストールする]* を選択し、初期化されていないディスク領域を使用します。

4. オペレーティング システムのバージョンをインストールしたら、この新しいコンピューターに新しい管理者としてログインします。 [コントロール パネル] を使用してコンピューター名を「PRIVDC」に設定し、仮想ネットワーク上の静的 IP アドレスを指定し、DNS サーバーを前の手順でインストールしたドメイン コントローラーの DNS サーバーとなるように構成します。 これにはサーバーの再起動が必要になります。

5. サーバーが再起動したら、管理者としてログインします。 [コントロール パネル] を使用して、更新を確認し、必要な更新をインストールするようにコンピューターを構成します。 これにはサーバーの再起動が必要になる場合があります。

### ロールの追加
Active Directory ドメイン サービス (AD DS) と DNS サーバーのロールを追加し、サーバーを新しいフォレストのドメイン コントローラーに昇格します。

**注:** このドキュメントでは、 *priv.contoso.local* という名前を新しいフォレストのドメイン名として使用します。  フォレストの名前は重要ではないため、組織内の既存のフォレスト名の下位である必要はありません。 ただし、新しいフォレストのドメイン名と NetBIOS 名は、組織内の他のドメインと重複せず、一意である必要があります。

1. 管理者としてログインした状態で、PowerShell を起動します。
2. 次のコマンドを入力します。

   `import-module ServerManager`

    `Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools`

   `$ca= get-credential`

   `Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca`

3. ポップアップが表示されたら、CORP フォレスト管理者の資格情報 (たとえば、手順 1 のユーザー名 CONTOSO\\Administrator と、対応するパスワード) を指定します。 これにより、セーフ モードの PowerShell ウィンドウ内に、使用する管理者パスワードの入力が求められます。新しいパスワードを 2 回入力します。 DNS 委任や暗号化の設定に対する警告メッセージが表示されますが、これは正常です。

フォレストの作成が完了すると、サーバーは自動的に再起動します。

### ユーザーおよびサービス アカウントの作成
 **[priv.contoso.local]** ドメインの [ユーザー] コンテナーに、MIM サービスおよびポータルのセットアップ時に必要となるユーザー アカウントとサービス アカウントを作成します。

1. コンピューターの再起動後、PRIVDC にドメイン管理者 (PRIV\\Administrator) としてログオンします。

2. 次のコマンドを入力します。

    `import-module activedirectory`

    `$sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force`

    `New-ADUser –SamAccountName MIMMA –name MIMMA`

    `Set-ADAccountPassword –identity MIMMA –NewPassword $sp`

    `Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor`

    `Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp`

    `Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1`

    `New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent`

    `Set-ADAccountPassword –identity MIMComponent –NewPassword $sp`

   `Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1`

    `New-ADUser –SamAccountName MIMSync –name MIMSync`

    `Set-ADAccountPassword –identity MIMSync –NewPassword $sp`

    `Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName MIMService –name MIMService`

   `Set-ADAccountPassword –identity MIMService –NewPassword $sp`

    `Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName SharePoint –name SharePoint`

   `Set-ADAccountPassword –identity SharePoint –NewPassword $sp`

   `Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName SqlServer –name SqlServer`

   `Set-ADAccountPassword –identity SqlServer –NewPassword $sp`

   `Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1`

   `New-ADUser –SamAccountName BackupAdmin –name BackupAdmin`

   `Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp`

   `Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1`

   `New-ADUser -SamAccountName MIMAdmin -name MIMAdmin`

   `Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp`

   `Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1`

   `Add-ADGroupMember "Domain Admins" SharePoint`

    `Add-ADGroupMember "Domain Admins" MIMService`

### 監査およびログオン権限の構成

1. ドメイン管理者 (PRIV\\Administrator など) としてログオンしていることを確認します。

2.  **[スタート]** 、 **[管理ツール]** 、 **[グループ ポリシーの管理]**の順に移動します。

3.  *[フォレスト: priv.contoso.local]* 、 **[ドメイン]** 、 *[priv.contoso.local]* 、 **[ドメイン コントローラー]** 、 **[既定のドメイン コントローラー ポリシー]**の順に移動します。 警告メッセージが表示されます。

4.  **[既定のドメイン コントローラー ポリシー]** を右クリックし、メニューから **[編集]** を選択します。

5.  **[グループ ポリシー管理エディター]** のコンソール ツリーで、 **[コンピューターの構成]** 、 **[ポリシー]** 、 **[Windows の設定]** 、 **[セキュリティの設定]** 、 **[ローカル ポリシー]** 、 **[監査ポリシー]**の順に移動します。

6.  **[詳細]** ウィンドウで **[アカウント管理の監査]** を右クリックして、メニューから **[プロパティ]** を選択します。  **[これらのポリシーの設定を定義する]**をクリックし、 **[成功]** チェック ボックスと **[失敗]** チェック ボックスをオンにして、 **[適用]** 、 **[OK]**の順にクリックします。

7.  **[詳細]** ウィンドウで **[ディレクトリ サービスのアクセスの監査]** を右クリックして、メニューから **[プロパティ]** を選択します。  **[これらのポリシーの設定を定義する]**をクリックし、 **[成功]** チェック ボックスと **[失敗]** チェック ボックスをオンにして、 **[適用]** 、 **[OK]**の順にクリックします。

8.  **[コンピューターの構成]** 、 **[ポリシー]** 、 **[Windows の設定]** 、 **[セキュリティの設定]** 、 **[アカウント ポリシー]** 、 **[Kerberos ポリシー]**の順に移動します。

9.  **[詳細]** ウィンドウで **[チケットの最長有効期間]** を右クリックして、メニューから **[プロパティ]** を選択します。  **[これらのポリシーの設定を定義する]**をクリックし、時間数を *1*に設定して、 **[適用]** 、 **[OK]**の順にクリックします。 ウィンドウ内の他の設定も変更されることに注意してください。

10.  **[グループ ポリシーの管理]** ウィンドウで、 **[既定のドメイン ポリシー]**を選択して右クリックし、 **[編集]**を選択します。  **[グループ ポリシー管理エディター]** ウィンドウが表示されます。

11.  **[コンピューターの構成]** 、 **[ポリシー]** 、 **[Windows の設定]** 、 **[セキュリティの設定]** 、 **[ローカル ポリシー]** の順に展開し、 **[ユーザー権利の割り当て]**を選択します。

12.  **[詳細]** ウィンドウで、 **[バッチ ジョブとしてログオンを拒否する]**を右クリックして、 **[プロパティ]**を選択します。

13.  **[これらのポリシーの設定を定義する]** チェックボックスをオンにして、 **[ユーザーまたはグループの追加]**をクリックし、 **[ユーザーとグループ名]** フィールドに「 *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* 」と入力し、 **[OK]**をクリックします。

14.  **[OK]** をクリックして **[バッチ ジョブとしてログオンを拒否する]** プロパティ ウィンドウを閉じます。

15.  **[詳細]** ウィンドウで、 **[リモート デスクトップ サービスを使ったログオンを拒否]**を右クリックして、 **[プロパティ]**を選択します。

16.  **[これらのポリシーの設定を定義する]** チェックボックスをオンにして、 **[ユーザーまたはグループの追加]**をクリックし、 **[ユーザーとグループ名]** フィールドに「 *priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent* 」と入力し、 **[OK]**をクリックします。

17.  **[OK]** をクリックして、 **[リモート デスクトップ サービスを使ったログオンを拒否]** プロパティ ウィンドウを閉じます。

18.  **[グループ ポリシー管理エディター]** ウィンドウと **[グループ ポリシー管理]** ウィンドウを閉じます。

19. 管理者として PowerShell ウィンドウを起動し、次のコマンドを入力してグループ ポリシー設定からドメイン コント ローラーを更新します。

    `gpupdate /force /target:computer`

  1 分後に、「コンピューター ポリシーの更新が正常に完了しました。」というメッセージが表示され、更新が完了します。

### SID 履歴の移行に必要なレジストリ設定を構成します。

PowerShell を起動し、次のコマンドを入力して、リモート プロシージャ コール (RPC) のセキュリティ アカウント マネージャー (SAM) データベースへのアクセスを許可するようにソース ドメインを構成します。


```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

### PRIVDC で DNS 名の転送を構成する

PRIVDC で PowerShell を使用して、DNS 名の転送を構成します。 DNS ドメインに *contoso.local* を指定し、CORPDC コンピューターの仮想ネットワーク IP アドレスをマスター サーバーの IP アドレスとして指定します。

1. PowerShell を起動し、次のコマンドを入力すると、IP アドレスが "10.1.1.31" から CORPDC コンピューターの仮想ネットワーク IP アドレスに変わります。

   `Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31`

2. PowerShell を使用して、SPN を追加して、SharePoint、PAM REST API、MIM サービスで使用される Kerberos 認証を有効にします (SharePoint は後述の手順 3 で構成します)。

    `setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint`

    `setspn -S http/pamsrv PRIV\SharePoint`

   `setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService`

   `setspn -S FIMService/pamsrv PRIV\MIMService`

**注:** このガイドでは、MIM 2016 サーバー コンポーネントを 1 つのシステム上にテスト目的でインストールする方法を説明します。  説明など、高可用性のための複数のシステムに MIM 2016 サーバー コンポーネントをインストールする追加の Kerberos 構成が必要 [FIM 2010: Kerberos 認証のセットアップ](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx)します。

### 委任の構成

1.  **[Active Directory ユーザーとコンピューター]**を起動します。

2.  *[priv.contoso.local]* ドメインを右クリックして、 **[制御の委任]**を選択します。

3.  **[選択されたユーザーとグループ]** タブで、 **[追加]**をクリックします。

4.  **[ユーザー、コンピュータ、またはグループの選択]** ウィンドウで、「 *mimcomponent; mimmonitor; mimservice* 」と入力し、 **[名前の確認]**をクリックします。 名前に下線が引かれたら、 **[OK]**、 **[次へ]**の順にクリックします。

5. 共通タスクの一覧で、 **[ユーザー アカウントの作成、削除、および管理]** 、 **[グループのメンバーシップの変更]**の順に選択して、 **[次へ]** 、 **[完了]**の順にクリックします。

6.  *[priv.contoso.local]* ドメインを右クリックして、 **[制御の委任]**を選択します。

7.  **[選択されたユーザーとグループ]** タブで、 **[追加]**をクリックします。

8.  **[ユーザー、コンピューター、またはグループの選択]** ポップアップで、「 *MIMAdmin* 」と入力し、 **[名前の確認]**をクリックします。

9. 名前に下線が引かれたら、 **[OK]**、 **[次へ]**の順にクリックします。

10.  **[カスタム タスク]**を選択して **[このフォルダー]**に適用し、 **[全般]**アクセス許可をオンにします。  

11. アクセス許可の一覧で、次を選択します: **[読み取り]**、 **[書き込み]**、 **[すべての子オブジェクトの作成]**、 **[すべての子オブジェクトの削除]**、 **[すべてのプロパティの読み取り]**、 **[すべてのプロパティの書き込み]**、および **[SID 履歴の移行]**。  **[次へ]** 、 **[完了]**の順にクリックします。

12. もう一度、 *[priv.contoso.local]* ドメインを右クリックして、 **[制御の委任]**を選択します。

13.  **[選択されたユーザーとグループ]** タブで、 **[追加]**をクリックします。

14.  **[ユーザー、コンピューター、またはグループの選択]** ポップアップで、「 *MIMAdmin* 」と入力し、 **[名前の確認]**をクリックします。

15. 名前に下線が引かれたら、 **[OK]**、 **[次へ]**の順にクリックします。

15.  **[カスタム タスク]**を選択して **[このフォルダー]**に適用し、 **[ユーザー オブジェクト]**のみをクリックします。  

16. アクセス許可の一覧で、 **[パスワードの変更]** と **[パスワードのリセット]**を選択します。 その後、 **[次へ]** 、 **[完了]**の順にクリックします。

17.  **[Active Directory ユーザーとコンピューター]**を閉じます。

16. これらの変更が反映されるように、 *PRIVDC* サーバーを再起動します。


<!--HONumber=Mar16_HO1-->


