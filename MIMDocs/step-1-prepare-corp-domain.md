---
タイトル: 手順 1 - CORP ドメインを準備します。
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
作成者: Kgremban
---
# ステップ 1 - CORP ドメインを準備する
このステップでは、新しいフォレストの新しいドメインにドメイン コントローラーとメンバー ワークステーションを作成します。 このフォレストは、管理対象のリソースを持つ既存のフォレストをシミュレートします。 このシナリオで保護されるリソースは、ファイル共有です。

## 4 つの仮想マシンを準備する

異なるドライブに 4 つの仮想マシンを準備し、共有仮想ネットワークで相互に接続します。 これらの仮想マシンは、Windows 8.1、Windows Server 2012 R2、または他のオペレーティング システム プラットフォームでホストできます。 仮想マシンのディスク イメージの格納先ドライブには、すべての仮想マシンを保持するために 100 GB 以上の空きディスク領域が必要です。

### Windows Server 2012 R2 または Windows Server 2016 をインストールする

1 つの仮想マシンに Windows Server 2012 R2 または Windows Server 2016 Technical Preview 3 をインストールしてコンピューター “CORPDC” を作成します。

1. Windows Server 2012 R2 Standard (GUI を搭載したサーバー) x64 または Windows Server 2016 Technical Preview 3 (デスクトップ エクスペリエンスを搭載したサーバー) を指定します。

2. ライセンス条項を確認して同意します。

3. ディスクは空になるため、[カスタム: Windows のみをインストールする] を選択し、初期化されていないディスク領域を使用します。

4. その新しいコンピューターに管理者としてログインします。  **[コントロール パネル]**を使用して、その名前を CORPDC に設定し、仮想ネットワーク上の静的 IP アドレスを割り当てます。 サーバーを再起動します。

5. サーバーが再起動したら、管理者としてログインします。 [コントロール パネル] を使用して、更新を確認し、必要な更新をインストールするようにコンピューターを構成します。 サーバーを再起動します。

### 役割の追加

サーバーが再起動したら、管理者としてログインします。 以下で説明するように、PowerShell を使用して、Active Directory ドメイン サービス (AD DS)、DNS サーバー、ファイル サーバー (ファイルおよびストレージ サービス セクションの一部) の役割を追加し、このサーバーを新しいフォレスト “contoso.local” のドメイン コントローラーに昇格させます。

1. 管理者としてログインした状態で、PowerShell を起動します。

2. 次のコマンドを入力します。

   `import-module ServerManager`

    `Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools`

    `Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork`

  セーフ モードの管理者パスワードを使用するように求められます。 DNS 委任や暗号化の設定に対する警告メッセージが表示されます。 これは正常です。

3. フォレストの作成が完了すると、サインアウトしてサーバーは自動的に再起動します。

4. サーバーが再起動した後、ドメインの管理者として CORPDC にログインします。通常はユーザー *CONTOSO\\Administrator*であり、Windows を *CORPDC*にインストールしたときに指定したものと同じパスワードを使用します。

### 新しいユーザーとグループを作成する

新しいユーザーとグループを作成します。“CorpAdmins” という名前のグループ、“Jen” という名前のユーザー、AD 自体による監査目的に必要なグループを含みます。 グループの名前は、NetBIOS ドメイン名の後に 3 個のドル記号を付けた *“CONTOSO$$$”*です。 グループの範囲は「 *ドメイン ローカル*」、グループの種類は「 *セキュリティ*」にする必要があります。 これは、後のステップで PRIV フォレストにグループを作成できるようにするために必要です。

1. PowerShell を起動します。

2. 次のコマンドを入力します。

   `import-module activedirectory`

    `New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins`

    `New-ADUser –SamAccountName Jen –name Jen`

    `Add-ADGroupMember –identity CorpAdmins –Members Jen`

    `$jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force`

    `Set-ADAccountPassword –identity Jen –NewPassword $jp`

    `Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"`

    `New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'`

### 監査を構成する

1. [スタート]、[管理ツール] (Windows Server 2016 の場合は、[スタート]、[Windows 管理ツール]) の順に選択し、[グループ ポリシーの管理] を起動します。

2. [フォレスト: contoso.local]、[ドメイン]、[contoso.local]、[ドメイン コントローラー]、[既定のドメイン コントローラー ポリシー] の順に移動します。 情報メッセージが表示されます。

3. [既定のドメイン コントローラー ポリシー] を右クリックし、右クリック メニューで [編集...] を選択します。 新しいウィンドウが表示されます。

4. [グループ ポリシー管理エディター] ウィンドウの [既定のドメイン コントローラー ポリシー] ツリーで、[コンピューターの構成]、[ポリシー]、[Windows の設定]、[セキュリティの設定]、[ローカル ポリシー]、[監査ポリシー] の順に展開します。

5. 詳細ウィンドウで [アカウント管理の監査] を右クリックして、右クリック メニューで [プロパティ] を選択します。 [これらのポリシーの設定を定義する] をクリックし、[成功] と [失敗] のチェックボックスをオンにして、[適用]、[OK] の順にクリックします。

6. 詳細ウィンドウで [ディレクトリ サービスのアクセスの監査] を右クリックして、右クリック メニューで [プロパティ] を選択します。 [これらのポリシーの設定を定義する] をクリックし、[成功] と [失敗] のチェックボックスをオンにして、[適用]、[OK] の順にクリックします。

7. [グループ ポリシー管理エディター] ウィンドウと [グループ ポリシー管理] ウィンドウを閉じます。 PowerShell ウィンドウを起動し、次のように入力して、監査の設定を適用します。

    `gpupdate /force /target:computer`

  「コンピューター ポリシーの更新が正常に完了しました。」というメッセージが 数分後に表示されます。

### レジストリ設定を構成する
特権付きアクセス管理グループの作成に使用する SID 履歴移行に必要なレジストリ設定を構成し、ドメイン コントローラーを再起動します。

1. PowerShell を起動します。

2. 次のコマンドを入力して、リモート プロシージャ コール (RPC) のセキュリティ アカウント マネージャー (SAM) データベースへのアクセスを許可するようにソース ドメインを構成します。

   `New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1`

   `Restart-Computer`

これにより CORPDC が再起動します。 次のレジストリ設定については、情報を参照して [サポート技術情報記事](http://support.microsoft.com/kb/322970)します。

### Windows 8.1 または Windows 10 をインストールする

ソフトウェアがインストールされていない別の新しい仮想マシンに、Windows 8.1 Enterprise または Windows 10 Enterprise をインストールして、コンピューターを *"CORPWKSTN"*にします。

1. インストールの間は Express 設定を使用します。

2. インストールがインターネットに接続できない場合があることに注意してください。 [ローカル アカウントの作成] をクリックします。 別のユーザー名を指定します。"Administrator" または "Jen" は使用しないでください。

3. コントロール パネルを使用して、このコンピューターに仮想ネットワーク上の静的 IP アドレスを指定し、インターフェイスの優先 DNS サーバーを CORPDC サーバーの DNS サーバーに設定します。

4. コントロール パネルを使用して、CORPWKSTN コンピューターを contoso.local ドメインに参加させます。 それには、Contoso ドメイン管理者の資格情報を提供する必要があります。 これが完了したら、CORPWKSTN コンピューターを再起動します。

5. コンピューターを再起動した後、**[ユーザーの切り替え]**アイコンをクリックし、**[他のユーザー]**をクリックします。 ユーザー CONTOSO\\Jen が CORPWKSTN にログインできることを確認します。

6. CORPWKSTN コンピューターで、“CorpAdmins” グループに “CorpFS” という名前の新しいフォルダーを作成して共有します。

7.  **[スタート] メニュー**で、「PowerShell」と入力し、[管理者として実行] を選択します。

8. ウィンドウが開いたら、次のコマンドを入力します。

   `mkdir c:\corpfs`

   `New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins`

   `$acl = Get-Acl c:\corpfs`

   `$car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")`

   `$acl.SetAccessRule($car)`

   `Set-Acl c:\corpfs $acl`


<!--HONumber=Mar16_HO1-->


