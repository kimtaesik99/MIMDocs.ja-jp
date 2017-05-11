---
title: "PAM の展開、手順 1 - CORP ドメイン | Microsoft Docs"
description: "Privileged Identity Manager で管理する既存の ID または新規の ID を使用して CORP ドメインを準備する"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 4b524ae7-6610-40a0-8127-de5a08988a8a
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 1164e7efb70d911497b08248b68f8d929bc6d3fb
ms.contentlocale: ja-jp
ms.lasthandoff: 05/02/2017


---

# <a name="step-1---prepare-the-host-and-the-corp-domain"></a>手順 1 - ホストと CORP ドメインの準備

>[!div class="step-by-step"]
[手順 2 »](step-2-prepare-priv-domain-controller.md)


この手順では、要塞環境をホストする準備をします。 さらに、必要に応じて、要塞環境で管理する ID を使って、ドメイン コントローラーとメンバー ワークステーションを新しいドメインとフォレスト (*CORP* フォレスト) に作成します。 この CORP フォレストは、管理対象のリソースを持つ既存のフォレストをシミュレートします。 このドキュメントには、保護対象となるサンプルのリソース (ファイル共有) が含まれます。

Windows Server 2012 R2 以降を実行するドメイン コントローラーを含む既存の Active Directory (AD) ドメイン (お客様がドメイン管理者) を既にお持ちの場合は、そのドメインを代わりに使用できます。  

## <a name="prepare-the-corp-domain-controller"></a>CORP ドメイン コントローラーを準備する

このセクションでは、CORP ドメインのドメイン コントローラーを設定する方法について説明します。 CORP ドメインでは、管理ユーザーは要塞環境によって管理されています。 この例で使用されている CORP ドメインのドメイン ネーム システム (DNS) 名は *contoso.local* です。

### <a name="install-windows-server"></a>Windows Server のインストール

仮想マシンに Windows Server 2012 R2 か Windows Server 2016 Technical Preview 4 以降をインストールして、*CORPDC* というコンピューターを作成します。

1. **Windows Server 2012 R2 Standard (GUI を搭載したサーバー) x64** か **Windows Server 2016 Technical Preview (デスクトップ エクスペリエンスを搭載したサーバー)** を選択します。

2. ライセンス条項を確認して同意します。

3. ディスクは空になるため、**[カスタム: Windows のみをインストールする]** を選択し、初期化されていないディスク領域を使用します。

4. その新しいコンピューターに管理者としてサインインします。 [コントロール パネル] に移動します。 コンピューター名を *CORPDC* に設定し、仮想ネットワーク上の静的 IP アドレスを割り当てます。 サーバーを再起動します。

5. サーバーが再起動したら、管理者としてサインインします。 [コントロール パネル] に移動します。 更新プログラムを確認し、必要な更新プログラムをインストールするようにコンピューターを構成します。 サーバーを再起動します。

### <a name="add-roles-to-establish-a-domain-controller"></a>ドメイン コントローラーを確立するためにロールを追加する

このセクションでは、Active Directory Domain Services (AD DS)、DNS サーバー、ファイル サーバー (ファイルとストレージ サービス セクションの一部) のロールを追加し、このサーバーを新しいフォレスト contoso.local のドメイン コントローラーに昇格します。

> [!NOTE]  
> CORP ドメインとして使うドメインを既にお持ちであり、そのドメインで Windows Server 2012 R2 以降をドメイン コントローラーとして使っている場合は、「[デモンストレーション用に追加のユーザーとグループを作成する](#create-additional-users-and-groups-for-demonstration-purposes)」に進みます。

1. 管理者としてサインインした状態で、PowerShell を起動します。

2. 次のコマンドを入力します。

  ```
  import-module ServerManager

  Add-WindowsFeature AD-Domain-Services,DNS,FS-FileServer –restart –IncludeAllSubFeature -IncludeManagementTools

  Install-ADDSForest –DomainMode Win2008R2 –ForestMode Win2008R2 –DomainName contoso.local –DomainNetbiosName contoso –Force -NoDnsOnNetwork
  ```

  セーフ モードの管理者パスワードを使用するように求められます。 DNS 委任や暗号化の設定に対する警告メッセージが表示されます。 これは正常です。

3. フォレストの作成が完了したら、サインアウトします。 サーバーは自動的に再起動します。

4. サーバーが再起動したら、ドメインの管理者として CORPDC にサインインします。 一般的に、ユーザーは CONTOSO\\Administrator で、Windows を CORPDC にインストールしたときに作成されたパスワードを使用します。

### <a name="create-a-group"></a>グループの作成

グループが存在しない場合は、Active Directory での監査用にグループを作成します。 グループの名前は、NetBIOS ドメイン名の後に 3 個のドル記号を付けたものにします (例: *CONTOSO$$$*)。

各ドメインについて、ドメイン コントローラーにドメイン管理者としてサインインし、次の手順を実行します。

1. PowerShell を起動します。

2. 次のコマンドを入力し、"CONTOSO" をドメインの NetBIOS 名に置き換えます。

  ```
  import-module activedirectory

  New-ADGroup –name 'CONTOSO$$$' –GroupCategory Security –GroupScope DomainLocal –SamAccountName 'CONTOSO$$$'
  ```

場合によっては、グループが既に存在している可能性があります。ドメインが AD の移行シナリオでも使用されている場合に、これは一般的な現象です。

### <a name="create-additional-users-and-groups-for-demonstration-purposes"></a>デモンストレーション用に追加のユーザーとグループを作成する

新しい CORP ドメインを作成した場合、PAM シナリオのデモンストレーション用に追加のユーザーとグループを作成する必要があります。 デモンストレーション用のユーザーやグループを、ドメイン管理者にすることも、AD の adminSDHolder 設定で制御することもできません。

> [!NOTE]
> CORP ドメインとして使用するドメインを既にお持ちであり、そのドメインにデモンストレーション用に使用できるユーザーやグループが含まれている場合は、「[監査を構成する](#configure-auditing)」のセクションに進みます。

*CorpAdmins* という名前のセキュリティ グループと *Jen* という名前のユーザーを作成します。 別の名前を使うこともできます。

1. PowerShell を起動します。

2. 次のコマンドを入力します。 パスワード「Pass@word1」を別のパスワード文字列に置き換えます。

  ```
  import-module activedirectory

  New-ADGroup –name CorpAdmins –GroupCategory Security –GroupScope Global –SamAccountName CorpAdmins

  New-ADUser –SamAccountName Jen –name Jen

  Add-ADGroupMember –identity CorpAdmins –Members Jen

  $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force

  Set-ADAccountPassword –identity Jen –NewPassword $jp

  Set-ADUser –identity Jen –Enabled 1 -DisplayName "Jen"
  ```

### <a name="configure-auditing"></a>監査を構成する

それらのフォレストで PAM 構成を確立するには、既存のフォレストでの監査を使用可能にする必要があります。  

各ドメインについて、ドメイン コントローラーにドメイン管理者としてサインインし、次の手順を実行します。

1. **[スタート]**  >  **[管理ツール]** (Windows Server 2016 の場合は、**[Windows 管理ツール]**) の順に移動し、**[グループ ポリシーの管理]** を起動します。

2. このドメインのドメイン コントローラー ポリシーに移動します。  contoso.local の新しいドメインを作成した場合は、**[フォレスト: contoso.local]**  >  **[ドメイン]**  >  **[contoso.local]**  >  **[ドメイン コントローラー]**  >  **[既定のドメイン コントローラー ポリシー]** の順に移動します。 情報メッセージが表示されます。

3. **[既定のドメイン コントローラー ポリシー]** を右クリックし、**[編集]** を選びます。 新しいウィンドウが表示されます。

4. [グループ ポリシー管理エディター] ウィンドウの [既定のドメイン コントローラー ポリシー] ツリーで、**[コンピューターの構成]**  >  **[ポリシー]**  >  **[Windows の設定]**  >  **[セキュリティの設定]**  >  **[ローカル ポリシー]**  >  **[監査ポリシー]** の順に移動します。

5. [詳細] ウィンドウで **[アカウント管理の監査]** を右クリックして、**[プロパティ]** を選びます。 **[これらのポリシーの設定を定義する]** をクリックし、**[成功]** と **[失敗]** のチェックボックスをオンにして、**[適用]**、**[OK]** の順にクリックします。

6. [詳細] ウィンドウで **[ディレクトリ サービスのアクセスの監査]** を右クリックして、**[プロパティ]** を選びます。 **[これらのポリシーの設定を定義する]** をクリックし、**[成功]** と **[失敗]** のチェックボックスをオンにして、**[適用]**、**[OK]** の順にクリックします。

7. [グループ ポリシー管理エディター] ウィンドウと [グループ ポリシー管理] ウィンドウを閉じます。

8. 監査の設定を適用するには、[PowerShell] ウィンドウを起動し、次のように入力します。

  ```
  gpupdate /force /target:computer
  ```

数分後に、"**コンピューター ポリシーの更新が正常に完了しました**" というメッセージが表示されます。

### <a name="configure-registry-settings"></a>レジストリ設定を構成する

このセクションでは、Privileged Access Management グループの作成に使う SID 履歴移行に必要なレジストリ設定を構成します。

1. PowerShell を起動します。

2. 次のコマンドを入力して、リモート プロシージャ コール (RPC) のセキュリティ アカウント マネージャー (SAM) データベースへのアクセスを許可するようにソース ドメインを構成します。

  ```
  New-ItemProperty –Path HKLM:SYSTEM\CurrentControlSet\Control\Lsa –Name TcpipClientSupport –PropertyType DWORD –Value 1

  Restart-Computer
  ```

これによって、ドメイン コントローラー CORPDC が再起動します。 このレジストリ設定について詳しくは、「[ADMTv2 を使用したフォレスト間の sIDHistory 移行のトラブルシューティングを行う方法](http://support.microsoft.com/kb/322970)」をご覧ください。

## <a name="prepare-a-corp-workstation-and-resource"></a>CORP ワークステーションとリソースを準備する

ドメインに参加しているワークステーション コンピューターをお持ちでない場合は、次の手順に従って準備します。  

> [!NOTE]
> ドメインに参加しているワークステーションを既にお持ちの場合は、「[デモンストレーション用にリソースを作成する](#create-a-resource-for-demonstration-purposes)」に進みます。

### <a name="install-windows-81-or-windows-10-enterprise-as-a-vm"></a>Windows 8.1 や Windows 10 Enterprise を VM インストールする

ソフトウェアがインストールされていない別の新しい仮想マシンに、Windows 8.1 Enterprise か Windows 10 Enterprise をインストールして、コンピューターを *CORPWKSTN* にします。

1. インストールの間は Express 設定を使用します。

2. インストールがインターネットに接続できない場合があることに注意してください。 **[ローカル アカウントの作成]** を選びます。 別のユーザー名を指定します。"Administrator" または "Jen" は使用しないでください。

3. コントロール パネルを使用して、このコンピューターに仮想ネットワーク上の静的 IP アドレスを指定し、インターフェイスの優先 DNS サーバーを CORPDC サーバーの DNS サーバーに設定します。

4. コントロール パネルを使用して、CORPWKSTN コンピューターを contoso.local ドメインに参加させます。 Contoso ドメイン管理者の資格情報を提供する必要があります。 これが完了したら、CORPWKSTN コンピューターを再起動します。

### <a name="create-a-resource-for-demonstration-purposes"></a>デモンストレーション用にリソースを作成する

PAM を使ったセキュリティ グループ ベースのアクセス制御のデモンストレーションにはリソースが必要です。  リソースをお持ちでない場合は、デモンストレーション用のファイル フォルダーを使用できます。  これにより、contoso.local ドメインで作成した "Jen" と "CorpAdmins" AD オブジェクトを利用できるようになります。

1. ワークステーション CORPWKSTN に接続します。 **[ユーザーの切り替え]** アイコンをクリックし、**[他のユーザー]** をクリックします。 ユーザー CONTOSO\\Jen で CORPWKSTN にログインできることを確認します。

2. *CorpFS* という新しいフォルダーを作成し、*CorpAdmins* グループと共有します。

3. 管理者として PowerShell を開きます。

4. 次のコマンドを入力します。

  ```
  mkdir c:\corpfs

  New-SMBShare –Name corpfs –Path c:\corpfs –ChangeAccess CorpAdmins

  $acl = Get-Acl c:\corpfs

  $car = New-Object System.Security.AccessControl.FileSystemAccessRule( "CONTOSO\CorpAdmins", "FullControl", "Allow")

  $acl.SetAccessRule($car)

  Set-Acl c:\corpfs $acl
  ```

次の手順では、PRIV ドメイン コントローラーを準備します。

>[!div class="step-by-step"]
[手順 2 »](step-2-prepare-priv-domain-controller.md)

