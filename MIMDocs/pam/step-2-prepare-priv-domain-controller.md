---
title: "PAM の展開、手順 2 - PRIV DC | Microsoft Docs"
description: "Privileged Access Management が分離される要塞環境が提供されるように、PRIV ドメイン コント ローラーを準備します。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 0e9993a0-b8ae-40e2-8228-040256adb7e2
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: edc15b41d4248887f4a93217f68d8125f6500585
ms.contentlocale: ja-jp
ms.lasthandoff: 07/10/2017


---

<a id="step-2---prepare-the-first-priv-domain-controller" class="xliff"></a>
# 手順 2 - PRIV ドメイン コントローラーの準備

>[!div class="step-by-step"]
[<< 手順 1](step-1-prepare-corp-domain.md)
[手順 3 >>](step-3-prepare-pam-server.md)

この手順では、管理者の認証のために要塞環境を提供する新しいドメインを作成します。  このフォレストには、少なくとも 1 つのドメイン コントローラーと、少なくとも 1 つのメンバー サーバーが必要です。 メンバー サーバーは、後ほど、次の手順で構成します。

<a id="create-a-new-privileged-access-management-domain-controller" class="xliff"></a>
## 新しい Privileged Access Management ドメイン コントローラーの作成

このセクションでは、新しいフォレストのドメイン コントローラーとして機能する仮想マシンをセットアップします

<a id="install-windows-server-2012-r2" class="xliff"></a>
### Windows Server 2012 R2 のインストール
ソフトウェアがインストールされていない別の新しい仮想マシンに、Windows Server 2012 R2 をインストールして、コンピューター "PRIVDC" にします。

1. Windows Server の (アップグレードではなく) カスタム インストールを選択して実行します。 インストール時に、**[Windows Server 2012 R2 Standard (GUI 使用サーバー) x64]** を指定します。**Data Center または Server Core** は選択_しないでください_。

2. ライセンス条項を確認して同意します。

3. ディスクは空になるため、**[カスタム: Windows のみをインストールする]** を選択し、初期化されていないディスク領域を使用します。

4. オペレーティング システムのバージョンをインストールしたら、この新しいコンピューターに新しい管理者としてサインインします。 [コントロール パネル] を使用してコンピューター名を *PRIVDC* に設定し、仮想ネットワーク上の静的 IP アドレスを指定し、DNS サーバーを前の手順でインストールしたドメイン コントローラーの DNS サーバーとなるように構成します。 これにはサーバーの再起動が必要になります。

5. サーバーが再起動したら、管理者としてサインインします。 [コントロール パネル] を使用して、更新を確認し、必要な更新をインストールするようにコンピューターを構成します。 これにはサーバーの再起動が必要になる場合があります。

<a id="add-roles" class="xliff"></a>
### ロールの追加
Active Directory ドメイン サービス (AD DS) と DNS サーバーのロールを追加します。

1. 管理者として PowerShell を起動します。

2. Windows Server Active Directory インストールの準備として、次のコマンドを入力します。

  ```
  import-module ServerManager

  Install-WindowsFeature AD-Domain-Services,DNS –restart –IncludeAllSubFeature -IncludeManagementTools
  ```

<a id="configure-registry-settings-for-sid-history-migration" class="xliff"></a>
### SID 履歴の移行のためにレジストリ設定を構成する

PowerShell を起動し、次のコマンドを入力して、セキュリティ アカウント マネージャー (SAM) データベースに対するリモート プロシージャ コール (RPC) アクセスを許可するようにソース ドメインを構成します。

```
New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1
```

<a id="create-a-new-privileged-access-management-forest" class="xliff"></a>
## 新しい Privileged Access Management フォレストの作成

次に、サーバーを新しいフォレストのドメイン コントローラーに昇格します。

このドキュメントでは、priv.contoso.local という名前を新しいフォレストのドメイン名として使います。  フォレストの名前は重要ではないため、組織内の既存のフォレスト名の下位である必要はありません。 ただし、新しいフォレストのドメイン名と NetBIOS 名は、組織内の他のドメインと重複せず、一意である必要があります。  

<a id="create-a-domain-and-forest" class="xliff"></a>
### ドメインとフォレストの作成

1. PowerShell ウィンドウで次のコマンドを入力して、新しいドメインを作成します。  また、このコマンドでは、前の手順で作成した上位ドメイン (contoso.local) に DNS 委任も作成します。  DNS の構成を後で行う場合は、`CreateDNSDelegation -DNSDelegationCredential $ca` パラメーターを省略してください。

  ```
  $ca= get-credential

  Install-ADDSForest –DomainMode 6 –ForestMode 6 –DomainName priv.contoso.local –DomainNetbiosName priv –Force –CreateDNSDelegation –DNSDelegationCredential $ca
  ```

2. ポップアップが表示されたら、CORP フォレスト管理者の資格情報 (たとえば、手順 1 で使ったユーザー名 CONTOSO\\Administrator と、それに対応するパスワード) を指定します。

3. PowerShell ウィンドウで、セーフ モードの管理者パスワードを入力するように求められます。 新しいパスワードを 2 回入力します。 DNS 委任と暗号化の設定に対する警告メッセージが表示されますが、これは正常です。

フォレストの作成が完了すると、サーバーは自動的に再起動します。

<a id="create-user-and-service-accounts" class="xliff"></a>
### ユーザーおよびサービス アカウントの作成
MIM サービスおよびポータル セットアップのユーザー アカウントとサービス アカウントを作成します。 これらのアカウントは priv.contoso.local ドメインのユーザー コンテナーに入れられます。

1. サーバーの再起動後、PRIVDC にドメイン管理者 (PRIV\\Administrator) としてサインインします。

2. PowerShell を起動し、次のコマンドを入力します。 パスワード 'Pass@word1' は一例ですので、アカウントでは別のパスワードを使用してください。

  ```
  import-module activedirectory

  $sp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  New-ADUser –SamAccountName MIMMA –name MIMMA

  Set-ADAccountPassword –identity MIMMA –NewPassword $sp

  Set-ADUser –identity MIMMA –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMMonitor –name MIMMonitor -DisplayName MIMMonitor

  Set-ADAccountPassword –identity MIMMonitor –NewPassword $sp

  Set-ADUser –identity MIMMonitor –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMComponent –name MIMComponent -DisplayName MIMComponent

  Set-ADAccountPassword –identity MIMComponent –NewPassword $sp

  Set-ADUser –identity MIMComponent –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMSync –name MIMSync

  Set-ADAccountPassword –identity MIMSync –NewPassword $sp

  Set-ADUser –identity MIMSync –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName MIMService –name MIMService

  Set-ADAccountPassword –identity MIMService –NewPassword $sp

  Set-ADUser –identity MIMService –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SharePoint –name SharePoint

  Set-ADAccountPassword –identity SharePoint –NewPassword $sp

  Set-ADUser –identity SharePoint –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName SqlServer –name SqlServer

  Set-ADAccountPassword –identity SqlServer –NewPassword $sp

  Set-ADUser –identity SqlServer –Enabled 1 –PasswordNeverExpires 1

  New-ADUser –SamAccountName BackupAdmin –name BackupAdmin

  Set-ADAccountPassword –identity BackupAdmin –NewPassword $sp

  Set-ADUser –identity BackupAdmin –Enabled 1 -PasswordNeverExpires 1

  New-ADUser -SamAccountName MIMAdmin -name MIMAdmin

  Set-ADAccountPassword –identity MIMAdmin  -NewPassword $sp

  Set-ADUser -identity MIMAdmin -Enabled 1 -PasswordNeverExpires 1

  Add-ADGroupMember "Domain Admins" SharePoint

  Add-ADGroupMember "Domain Admins" MIMService
  ```

<a id="configure-auditing-and-logon-rights" class="xliff"></a>
### 監査およびログオン権限の構成

PAM 構成がフォレストをまたいで確立されるように監査を設定する必要があります。  

1. ドメイン管理者 (PRIV\\Administrator) としてサインインしていることを確認します。

2. **[スタート]**  >  **[管理ツール]**  >  **[グループ ポリシーの管理]** の順に移動します。

3. **[フォレスト: priv.contoso.local]**  >  **[ドメイン]**  >  **[priv.contoso.local]**  >  **[ドメイン コントローラー]**  >  **[既定のドメイン コントローラー ポリシー]** の順に移動します。 警告メッセージが表示されます。

4. **[既定のドメイン コントローラー ポリシー]** を右クリックし、**[編集]** を選びます。

5. グループ ポリシー管理エディターのコンソール ツリーで、**[コンピューターの構成]**  >  **[ポリシー]**  >  **[Windows の設定]**  >  **[セキュリティの設定]**  >  **[ローカル ポリシー]**  >  **[監査ポリシー]** の順に移動します。

6. [詳細] ウィンドウで **[アカウント管理の監査]** を右クリックして、**[プロパティ]** を選びます。 **[これらのポリシーの設定を定義する]**をクリックし、 **[成功]** チェック ボックスと **[失敗]** チェック ボックスをオンにして、 **[適用]** 、 **[OK]**の順にクリックします。

7. [詳細] ウィンドウで **[ディレクトリ サービスのアクセスの監査]** を右クリックして、**[プロパティ]** を選びます。 **[これらのポリシーの設定を定義する]**をクリックし、 **[成功]** チェック ボックスと **[失敗]** チェック ボックスをオンにして、 **[適用]** 、 **[OK]**の順にクリックします。

8. **[コンピューターの構成]**  >  **[ポリシー]**  >  **[Windows の設定]**  >  **[セキュリティの設定]**  >  **[アカウント ポリシー]**  >  **[Kerberos ポリシー]** の順に移動します。

9. [詳細] ウィンドウで **[チケットの最長有効期間]** を右クリックして、**[プロパティ]** を選びます。 **[これらのポリシーの設定を定義する]**をクリックし、時間数を *1*に設定して、 **[適用]** 、 **[OK]**の順にクリックします。 ウィンドウ内の他の設定も変更されることに注意してください。

10. [グループ ポリシーの管理] ウィンドウで、**[既定のドメイン ポリシー]** を選択して右クリックし、**[編集]** を選びます。

11. **[コンピューターの構成]**  >  **[ポリシー]**  >  **[Windows の設定]**  >  **[セキュリティの設定]**  >  **[ローカル ポリシー]** の順に展開し、**[ユーザー権利の割り当て]** を選びます。

12. [詳細] ウィンドウで、**[バッチ ジョブとしてログオンを拒否する]** を右クリックして、**[プロパティ]** を選びます。

13. **[これらのポリシーの設定を定義する]** チェック ボックスをオンにして、**[ユーザーまたはグループの追加]** をクリックし、[ユーザーとグループ名] フィールドに「*priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent*」と入力し、**[OK]** をクリックします。

14. **[OK]** をクリックして、ウィンドウを閉じます。

15. [詳細] ウィンドウで、**[リモート デスクトップ サービスを使ったログオンを拒否]** を右クリックして、**[プロパティ]** を選びます。

16. **[これらのポリシーの設定を定義する]** チェック ボックスをクリックして、**[ユーザーまたはグループの追加]** をクリックし、[ユーザーとグループ名] フィールドに「*priv\\mimmonitor; priv\\MIMService; priv\\mimcomponent*」と入力し、**[OK]** をクリックします。

17. **[OK]** をクリックして、ウィンドウを閉じます。

18. [グループ ポリシー管理エディター] ウィンドウと [グループ ポリシー管理] ウィンドウを閉じます。

19. 管理者として PowerShell ウィンドウを起動し、次のコマンドを入力してグループ ポリシー設定からドメイン コント ローラーを更新します。

  ```
  gpupdate /force /target:computer
  ```

  1 分後に、「コンピューター ポリシーの更新が正常に完了しました。」というメッセージが表示され、更新が完了します。


<a id="configure-dns-name-forwarding-on-privdc" class="xliff"></a>
### PRIVDC で DNS 名の転送を構成する

PRIVDC で PowerShell を使って、PRIV ドメインが他の既存のフォレストを認識するように DNS 名の転送を構成します。

1. PowerShell を起動します。

2. 既存の各フォレストの上位にある各ドメインに対して、既存の DNS ドメイン (例: contoso.local) とそのドメインのマスター サーバーの IP アドレスを指定する次のコマンドを入力します。  

  前の手順で 1 つのドメイン contoso.local を作成した場合は、CORPDC コンピューターの仮想ネットワークの IP アドレスに *10.1.1.31* を指定します。

  ```
  Add-DnsServerConditionalForwarderZone –name "contoso.local" –masterservers 10.1.1.31
  ```

> [!NOTE]
> 他のフォレストは、PRIV フォレストに対する DNS クエリをこのドメイン コントローラーにルーティングできる必要もあります。  複数の既存の Active Directory フォレストがある場合は、それらの各フォレストに DNS 条件付きフォワーダーを追加する必要もあります。

<a id="configure-kerberos" class="xliff"></a>
### Kerberos の構成

1. PowerShell を使用して SPN を追加することにより、SharePoint、PAM REST API、MIM サービスが Kerberos 認証を使えるようにします。

  ```
  setspn -S http/pamsrv.priv.contoso.local PRIV\SharePoint
  setspn -S http/pamsrv PRIV\SharePoint
  setspn -S FIMService/pamsrv.priv.contoso.local PRIV\MIMService
  setspn -S FIMService/pamsrv PRIV\MIMService
  ```

> [!NOTE]
> このドキュメントの次のステップでは、1 台のコンピューターに MIM 2016 サーバー コンポーネントをインストールする方法について説明します。 高可用性のためにもう 1 台のサーバーを追加する場合は、「[FIM 2010: Kerberos Authentication Setup (Kerberos 認証のセットアップ)](http://social.technet.microsoft.com/wiki/contents/articles/3385.fim-2010-kerberos-authentication-setup.aspx)」の説明に従って、追加の Kerberos 構成を用意する必要があります。

<a id="configure-delegation-to-give-mim-service-accounts-access" class="xliff"></a>
### MIM サービス アカウントにアクセス許可を与える委任を構成する

ドメイン管理者として PRIVDC で次の手順を実行します。

1. **[Active Directory ユーザーとコンピューター]**を起動します。  
2. **[priv.contoso.local]** ドメインを右クリックし、**[制御の委任]** を選びます。  
3. [選択されたユーザーとグループ] タブで、**[追加]** をクリックします。  
4. [ユーザー、コンピューター、またはグループの選択] ウィンドウで、「*mimcomponent; mimmonitor; mimservice*」と入力し、**[名前の確認]** をクリックします。 名前に下線が引かれたら、**[OK]**、**[次へ]** の順にクリックします。  
5. 共通タスクの一覧で、**[ユーザー アカウントの作成、削除、および管理]**、**[グループのメンバーシップの変更]** の順に選択し、**[次へ]**、**[完了]** の順にクリックします。

6. もう一度、**[priv.contoso.local]** ドメインを右クリックして、**[制御の委任]** を選択します。  
7. [選択されたユーザーとグループ] タブで、**[追加]** をクリックします。  
8. [ユーザー、コンピューター、またはグループの選択] ウィンドウで、「*MIMAdmin*」と入力し、**[名前の確認]** をクリックします。 名前に下線が引かれたら、**[OK]**、**[次へ]** の順にクリックします。  
9. **[カスタム タスク]**を選択して **[このフォルダー]**に適用し、 **[全般]**アクセス許可をオンにします。    
10. アクセス許可の一覧で、次の項目を選択します。  
  - **読み取り**  
  - **書き込み**  
  - **すべての子オブジェクトの作成**  
  - **すべての子オブジェクトの削除**  
  - **すべてのプロパティの読み取り**  
  - **すべてのプロパティの書き込み**  
  - **SID 履歴の移行**  
  **[次へ]** 、 **[完了]**の順にクリックします。

11. さらにもう一度、**[priv.contoso.local]** ドメインを右クリックして、**[制御の委任]** を選択します。  
12. [選択されたユーザーとグループ] タブで、**[追加]** をクリックします。  
13. [ユーザー、コンピューター、またはグループの選択] ウィンドウで、「*MIMAdmin*」と入力し、**[名前の確認]** をクリックします。 名前に下線が引かれたら、 **[OK]**、 **[次へ]**の順にクリックします。  
14. **[カスタム タスク]**を選択して **[このフォルダー]**に適用し、 **[ユーザー オブジェクト]**のみをクリックします。    
15. アクセス許可の一覧で、 **[パスワードの変更]** と **[パスワードのリセット]**を選択します。 その後、 **[次へ]** 、 **[完了]**の順にクリックします。  
16. [Active Directory ユーザーとコンピューター] を閉じます。

17. コマンド プロンプトを開きます。  
18. PRIV ドメインで管理者 SD 所有者オブジェクトのアクセス制御リストを確認します。 たとえば、自分のドメインが "priv.contoso.local" の場合は、次のコマンドを入力します  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local"
  ```
19. 必要に応じてアクセス制御リストを更新し、この ACL によって保護されているグループのメンバーシップを MIM サービスと MIM コンポーネント サービスが更新できるようにします。  次のコマンドを入力します。  
  ```
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimservice:WP;"member"  
  dsacls "cn=adminsdholder,cn=system,dc=priv,dc=contoso,dc=local" /G priv\mimcomponent:WP;"member"
  ```
20. これらの変更が反映されるように、PRIVDC サーバーを再起動します。

<a id="prepare-a-priv-workstation" class="xliff"></a>
## PRIV ワークステーションの準備

PRIV リソースのメンテナンス (MIM など) を実行するために PRIV ドメインに参加できるワークステーション コンピューターがまだない場合は、次の手順に従ってワークステーションを準備します。  

<a id="install-windows-81-or-windows-10-enterprise" class="xliff"></a>
### Windows 8.1 または Windows 10 Enterprise をインストールする

ソフトウェアがインストールされていない別の新しい仮想マシンに、Windows 8.1 Enterprise または Windows 10 Enterprise をインストールして、コンピューターを *"PRIVWKSTN"* にします。

1. インストールの間は Express 設定を使用します。

2. インストールがインターネットに接続できない場合があることに注意してください。 **[ローカル アカウントの作成]** をクリックします。 別のユーザー名を指定します。"Administrator" または "Jen" は使用しないでください。

3. コントロール パネルを使用して、このコンピューターに仮想ネットワーク上の静的 IP アドレスを指定し、インターフェイスの優先 DNS サーバーを PRIVDC サーバーの DNS サーバーに設定します。

4. コントロール パネルを使用して、PRIVWKSTN コンピューターを priv.contoso.local domain ドメインに参加させます。 このとき、PRIV ドメイン管理者の資格情報を提供する必要があります。 これが完了したら、PRIVWKSTN コンピューターを再起動します。

詳細については、[特権アクセス ワークステーションのセキュリティ保護](https://technet.microsoft.com/en-us/library/mt634654.aspx)に関する記事をご覧ください。

次の手順では、PAM サーバーを準備します。

>[!div class="step-by-step"]
[<< 手順 1](step-1-prepare-corp-domain.md)
[手順 3 >>](step-3-prepare-pam-server.md)

