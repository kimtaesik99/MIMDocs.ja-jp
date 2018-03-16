---
title: "Microsoft Identity Manager Certificate Manager の展開 | Microsoft Docs"
description: "Microsoft Identity Manager 2016 Certificate Manager のインストール"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/19/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 2473ef1c3d6fc5350d60d81bd508296a33343f01
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
# <a name="deploying-microsoft-identity-manager-certificate-manager-2016-mim-cm"></a>Microsoft Identity Manager Certificate Manager 2016 (MIM CM) の展開

Microsoft Identity Manager Certificate Manager 2016 (MIM CM) のインストールには、多くの手順があります。 プロセスを簡略化するため、手順を分割して説明します。 MIM CM の実際のインストールを実行する前に、実行すべき準備作業があります。 準備作業を行わないと、インストールが失敗する可能性があります。 

1. 展開の概要
2. 展開前の手順
3. 他に何があるでしょうか。

次の図は、使用される可能性がある環境の種類の例を示しています。 番号の付いているシステムは、図の下のリストに含まれており、この記事で説明している手順を正常に完了するために必要なシステムです。 最後に、Windows 2016 Datacenter Server が使用されます。

![環境の図](media/mim-cm-deploy/image001.png)

1. CORPDC – ドメイン コントローラー
2. CORPCM – MIM CM サーバー
3. CORPCA – 証明機関
4. CORPCMR – MIM CM Rest API Web – Rest API の CM ポータル – 後で使用
5. CORPSQL1 – SQL 2016 SP1
6. CORPWK1 – Windows 10 ドメイン参加

## <a name="deployment-overview"></a>展開の概要

- ベース オペレーティング システムのインストール
  - ラボは、Windows 2016 Datacenter Server で構成されています。
       >[!NOTE]
MIM 2016 でサポートされるプラットフォームの詳細については、「[MIM 2016 でサポートされるプラットフォーム](/microsoft-identity-manager/microsoft-identity-manager-2016-supported-platforms.md)」というタイトルの記事をご覧ください。
- 展開前の手順
  - [スキーマの拡張](https://msdn.microsoft.com/library/ms676929(v=vs.85).aspx)
  - サービス アカウントの作成
  - [証明書テンプレートの作成](https://technet.microsoft.com/library/cc753370(v=ws.11).aspx)
  - IIS
  - Kerberos の構成
  - データベース関連の手順
    - SQL の構成要件
    - データベースのアクセス許可
- 展開

## <a name="pre-deployment-steps"></a>展開前の手順

MIM CM 構成ウィザードでは、構成を正常に完了するため、進行中に情報の提供が求められます。 
![](media/mim-cm-deploy/image003.png)

### <a name="extending-the-schema"></a>スキーマの拡張

スキーマを拡張するためのプロセスは単純ですが、元に戻せない性質上、注意して対処する必要があります。

>[!NOTE]
この手順では、使用するアカウントにスキーマ管理者権限がある必要があります。

- MIM メディアの場所を参照し、\\Certificate Management\\x64 フォルダーに移動します。

- Schema フォルダーを CORPDC にコピーして、そこに移動します。

    ![](media/mim-cm-deploy/image005.png)

- スクリプト resourceForestModifySchema.vbs 単一フォレスト シナリオを実行します。

- リソース フォレストのシナリオの場合は、次のスクリプトを実行します。
  - DomainA – ユーザーが見つけた場所 (userForestModifySchema.vbs)
  - ResourceForestB – CM のインストール場所 (resourceForestModifySchema.vbs)

>[!NOTE]
スキーマの変更は、一方向の操作で、ロールバックにはフォレストの回復が必要なため、必要なバックアップがあることを確認してください。 この操作を実行することでスキーマに加えられる変更の詳細については、記事「[Forefront Identity Manager 2010 Certificate Management Schema Changes](https://technet.microsoft.com/library/jj159298(v=ws.10).aspx)」 (Forefront Identity Manager 2010 Certificate Management スキーマの変更) で確認してください。

![](media/mim-cm-deploy/image007.png)

スクリプトを実行し、スクリプトが完了すると、成功のメッセージを受信します。

![成功メッセージ](media/mim-cm-deploy/image009.png)

AD 内のスキーマが MIM CM をサポートするために拡張されました。

### <a name="creating-service-accounts-and-groups"></a>サービス アカウントとグループの作成

次の表は、MIM CM で必要なアカウントとアクセス許可をまとめたものです。
MIM CM が次のアカウントを自動的に作成することを許可することも、インストールする前に自分で作成することもできます。 実際のアカウント名は変更できます。 自分でアカウントを作成する場合は、ユーザー アカウントとその機能を簡単に照合できる方法で、ユーザー アカウントを命名することを検討してください。

Users:

![](media/mim-cm-deploy/image010.png)

![](media/mim-cm-deploy/image012.png)

| **ロール**                   | **ユーザー ログオン名** |
|----------------------------|---------------------|
| MIM CM エージェント               | MIMCMAgent          |
| MIM CM キー回復エージェント  | MIMCMKRAgent        |
| MIM CM 認証エージェント | MIMCMAuthAgent      |
| MIM CM CA マネージャー エージェント    | MIMCMManagerAgent   |
| MIM CM Web プール エージェント      | MIMCMWebAgent       |
| MIM CM 登録エージェント    | MIMCMEnrollAgent    |
| MIM CM 更新サービス      | MIMCMService        |
| MIM インストール アカウント        | MIMINSTALL          |
| ヘルプ デスク エージェント            | CMHelpdesk1-2       |
| CM マネージャー                 | CMManager1-2        |
| サブスクライバー ユーザー            | CMUser1-2           |

グループ:

| **ロール**               | **グループ**         |
|------------------------|-------------------|
| CM ヘルプデスク メンバー    | MIMCM-Helpdesk    |
| CM マネージャー メンバー     | MIMCM-Managers    |
| CM サブスクライバー メンバー | MIMCM-Subscribers |

Powershell: エージェント アカウント

```
import-module activedirectory
## Agent accounts used during setup
$cmagents = @{
"MIMCMKRAgent" = "MIM CM Key Recovery Agent"; 
"MIMCMAuthAgent" = "MIM CM Authorization Agent"
"MIMCMManagerAgent" = "MIM CM CA Manager Agent";
"MIMCMWebAgent" = "MIM CM Web Pool Agent";
"MIMCMEnrollAgent" = "MIM CM Enrollment Agent";
"MIMCMService" = "MIM CM Update Service";
"MIMCMAgent" = "MIM CM Agent";
}
##Groups Used for CM Management
$cmgroups = @{
"MIMCM-Managers" = "MIMCM-Managers"
"MIMCM-Helpdesk" = "MIMCM-Helpdesk"
"MIMCM-Subscribers" = "MIMCM-Subscribers" 
}
##Users Used during testlab
$cmusers = @{
"CMManager1" = "CM Manager1"
"CMManager2" = "CM Manager2"
"CMUser1" = "CM User1"
"CMUser2" = "CM User2"
"CMHelpdesk1" = "CM Helpdesk1"
"CMHelpdesk2" = "CM Helpdesk2"
}

## OU Paths
$aoupath = "OU=Service Accounts,DC=contoso,DC=com" ## Location of Agent accounts
$oupath = "OU=CMLAB,DC=contoso,DC=com" ## Location of Users and Groups for CM Lab


#Create Agents – Update UserprincipalName
$cmagents.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -UserPrincipalName ($_.Name + "@contoso.com")  -Path $aoupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}


#Create Users
$cmusers.GetEnumerator() | Foreach-Object { 
New-ADUser -Name $_.Name -Description $_.Value -Path $oupath
$cmpwd = ConvertTo-SecureString "Pass@word1" –asplaintext –force
Set-ADAccountPassword –identity $_.Name –NewPassword $cmpwd
Set-ADUser -Identity $_.Name -Enabled $true
}
```

### <a name="update-corpcm-server-local-policy-for-agent-accounts"></a>エージェント アカウントの **CORPCM** サーバーのローカル ポリシーの更新 

| **ユーザー ログオン名** | **説明とアクセス許可**   |
|------|---------------------|
| MIMCMAgent          | 次のサービスを提供します。 </br>-   CA から暗号化された秘密キーを取得します。 </br>-   FIM CM データベース内のスマート カードの暗証番号 (PIN) 情報を保護します。 </br>-   FIM CM と CA 間の通信を保護します。 </br></br> このユーザー アカウントには、次のアクセスの制御の設定が必要です。</br>-   **ローカル ログオンを許可**のユーザー権限。</br>-   **証明書の発行と管理**のユーザー権限。 </br>-   %WINDIR%\\Temp でのシステム Temp フォルダーに対する読み取りと書き込みのアクセス許可。</br>-   デジタル署名と暗号化証明書が発行され、ユーザー ストアにインストールされます。
|MIMCMKRAgent        | CA からアーカイブされた秘密キーを回復します。 このユーザー アカウントには、次のアクセスの制御の設定が必要です。</br> -   **ローカル ログオンを許可**のユーザー権限。</br>-   ローカルの **Administrators** グループのメンバーシップ。 </br>-   **KeyRecoveryAgent** 証明書テンプレートに対する登録アクセス許可。 </br>-   キー回復エージェント証明書が発行され、ユーザー ストアにインストールされます。 証明書は CA のキー回復エージェントのリストに追加する必要があります。 </br>-   ```%WINDIR%\\Temp.``` でのシステム Temp フォルダーに対する読み取りアクセス許可と書き込みアクセス許可。                                                                                                                     |
| MIMCMAuthAgent      | ユーザー権限とユーザーとグループのアクセス許可を決定します。 このユーザー アカウントには、次のアクセスの制御の設定が必要です。 </br>-   Pre-Windows 2000 Compatible Access ドメイン グループのメンバーシップ。 </br> -   **セキュリティ監査の生成**ユーザー権限の付与。             |
| MIMCMManagerAgent   | CA の管理アクティビティを実行します。 </br> このユーザーには、CA の管理アクセス許可を割り当てる必要があります。        |
| MIMCMWebAgent       | IIS アプリケーション プールの ID を提供します。 FIM CM は、このユーザーの資格情報を使用する Microsoft Win32® アプリケーション プログラミング インターフェイス プロセス内で実行されます。 </br> このユーザー アカウントには、次のアクセスの制御の設定が必要です。</br> -   ローカル **IIS_WPG、windows 2016 = IIS_IUSRS** グループのメンバーシップ。 </br>-   ローカルの **Administrators** グループのメンバーシップ。</br>-   **セキュリティ監査の生成**ユーザー権限の付与。 </br>-   **オペレーティング システムの一部として機能**ユーザー権限の付与。 </br>-   **プロセス レベル トークンの置き換え**ユーザー権限の付与。</br>-   IIS アプリケーション プール **CLMAppPool** の ID として割り当て。 </br>-   **HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\CLM\\v1.0\\Server\\WebUser** レジストリ キーに対する読み取りアクセス許可の付与。 </br>-   このアカウントは委任に対しても信頼されている必要があります。|
| MIMCMEnrollAgent    | ユーザーの代理で登録を実行します。 このユーザー アカウントには、次のアクセスの制御の設定が必要です。</br>-   発行され、ユーザー ストアにインストールされる登録エージェント証明書。</br>-   **ローカル ログオンを許可**のユーザー権限。 </br>-   **登録エージェント**証明書テンプレート (またはカスタム テンプレート (使用している場合)) にたいする登録アクセス許可。                 |

### <a name="creating-certificate-templates-for-mim-cm-service-accounts"></a>MIM CM サービス アカウントの証明書テンプレートの作成

MIM CM によって使用されるサービス アカウントのうち、3 つには証明書が必要です。構成ウィザードでは、これらの証明書を要求するために使用する証明書テンプレートの名前を指定することを求められます。

証明書を必要とするサービス アカウントは次のとおりです。

- MIMCMAgent: このアカウントには、ユーザー証明書が必要です。

- MIMCMEnrollAgent: このアカウントには、登録エージェント証明書が必要です。

- MIMCMKRAgent: このアカウントには、**キー回復エージェント**証明書が必要です。

AD には既にテンプレートが存在しますが、MIM CM で使用する独自のバージョンを作成する必要があります。 これは、元のベースライン テンプレートから変更を加える必要があるためです。

上記の 3 つのアカウントはすべて、組織内で昇格された権限を持つため、慎重に扱う必要があります。

#### <a name="create-the-mim-cm-signing-certificate-template"></a>MIM CM 署名の証明書テンプレートの作成

1. **管理ツール**から、**証明機関**を開きます。
2. **証明機関**コンソールのコンソール ツリーで、**[Contoso-CorpCA]** を展開し、**[証明書テンプレート]** をクリックします。
3. **[証明書テンプレート]**を右クリックし、 **[管理]**をクリックします。
4. **証明書テンプレート コンソール**の **[詳細]** ウィンドウで、**[ユーザー]** を選択して右クリックし、**[テンプレートの複製]** をクリックします。
5. **[テンプレートの複製]** ダイアログ ボックスで、**[Windows Server 2003 Enterprise]** を選択して **[OK]** をクリックします。

![結果として生じる変更を表示する](media/mim-cm-deploy/image014.png)

    >[!NOTE]
    MIM CM does not work with certificates based on version 3 certificate templates. You must create a Windows Server® 2003 Enterprise (version 2)certificate template. See the following link for V3 details https://blogs.msdn.microsoft.com/ms-identity-support/2016/07/14/faq-for-fim-2010-to-support-sha2-kspcng-and-v3-certificate-templates-for-issuing-user-and-agent-certificates-and-mim-2016-upgrade

6. **[新しいテンプレートのプロパティ]** ダイアログ ボックスの **[全般]** タブで、**[テンプレート表示名]** ボックスに「**MIM CM 署名**」を入力します。 **[有効期間]** を **[2 年]** に変更し、**[Active Directory で証明書を発行する]** チェック ボックスをオフにします。

7. **[要求処理]** タブで、**[プライベート キーのエクスポートを許可する]** チェック ボックスがオンになっていることを確認してから、**[暗号化]** タブをクリックします。

8. **[暗号化の選択]** ダイアログ ボックスで、**[Microsoft Enhanced Cryptographic Provider v1.0]** を無効にし、**[Microsoft Enhanced RSA and AES Cryptographic Provider]** を有効にして、**[OK]** をクリックします。

**[サブジェクト名]** タブで、**[サブジェクト名に電子メールの名前を含める]** と **[電子メールの名前]** のチェック ボックスをオフにします。

**[拡張]** タブの **[このテンプレートに含まれる拡張機能]** のリストで、**[アプリケーション ポリシー]** が選択されていることを確認し、**[編集]** をクリックします。

**[アプリケーション ポリシーの拡張機能の編集]** ダイアログ ボックスで、**[暗号化ファイル システム]** と **[セキュリティで保護された電子メール]** の両方のアプリケーション ポリシーを選択します。 **[削除]**をクリックし、 **[OK]**をクリックします。

**[セキュリティ]** タブで、次の手順を実行します。

- **Administrator** を削除します。

- **Domain Admins** を削除します。

- **ドメイン ユーザー** を削除します。

- **読み取り**と**書き込み**のアクセス許可のみを **Enterprise Admins** に割り当てます。

- **MIMCMAgent** を追加します。

- **読み取り**と**登録**のアクセス許可を **MIMCMAgent** に割り当てます。

**[新しいテンプレートのプロパティ**] ダイアログ ボックスで、**[OK]** をクリックします。

**証明書テンプレート コンソール**を開いたままにします。

#### <a name="create-the-mim-cm-enrollment-agent-certificate-template"></a>MIM CM 登録エージェントの証明書テンプレートを作成する

-   **証明書テンプレート コンソール**の **[詳細]** ウィンドウで、**[登録エージェント]** を選択して右クリックし、**[テンプレートの複製]** をクリックします。

**[テンプレートの複製]** ダイアログ ボックスで、**[Windows Server 2003 Enterprise]** を選択して **[OK]** をクリックします。

**[新しいテンプレートのプロパティ]** ダイアログ ボックスの **[全般]** タブで、**[テンプレート表示名]** ボックスに「**MIM CM 登録エージェント**」と入力します。 **[有効期間]** が **[2 年]** になっていることを確認します。

**[要求処理]** タブで、**[秘密キーのエクスポートを許可する]** を有効にして、**[CSP] または [暗号化] タブ**をクリックします。

**[CSP の選択]** ダイアログ ボックスで、**[Microsoft Base Cryptographic Provider v1.0]** を無効にし、**[Microsoft Enhanced Cryptographic Provider v1.0]** を無効にし、**[Microsoft Enhanced RSA and AES Cryptographic Provider]** を有効にして **[OK]** をクリックします。

**[セキュリティ]** タブで、次を実行します。

- **Administrator** を削除します。

- **Domain Admins** を削除します。

- **読み取り**と**書き込み**のアクセス許可のみを **Enterprise Admins** に割り当てます。

- **MIMCMEnrollAgent** を追加します。

- **読み取り**と**登録**のアクセス許可を **MIMCMEnrollAgent** に割り当てます。

**[新しいテンプレートのプロパティ**] ダイアログ ボックスで、**[OK]** をクリックします。

**証明書テンプレート コンソール**を開いたままにします。

#### <a name="create-the-mim-cm-key-recovery-agent-certificate-template"></a>MIM CM キー回復エージェントの証明書テンプレートを作成する

1. **証明書テンプレート コンソール**の **[詳細]** ウィンドウで、**[キー回復エージェント]** を選択して右クリックし、**[テンプレートの複製]** をクリックします。

2. **[テンプレートの複製]** ダイアログ ボックスで、**[Windows Server 2003 Enterprise]** を選択して **[OK]** をクリックします。

3. **[新しいテンプレートのプロパティ]** ダイアログ ボックスの **[全般]** タブで、**[テンプレート表示名]** ボックスに「**MIM CM キー回復エージェント**」と入力します。 **[暗号化]** タブで **[有効期間]** が **[2 年]** になっていることを確認します。

4. **[プロバイダーの選択]** ダイアログ ボックスで、**[Microsoft Enhanced Cryptographic Provider v1.0]** を無効にし、**[Microsoft Enhanced RSA and AES Cryptographic Provider]** を有効にして、**[OK]** をクリックします。

5. **[発行要件]** タブで、**[CA 証明書マネージャーの許可]** が**無効**になっていることを確認します。

6. **[セキュリティ]** タブで、次を実行します。

    - **Administrator** を削除します。

    - **Domain Admins** を削除します。

    - **読み取り**と**書き込み**のアクセス許可のみを **Enterprise Admins** に割り当てます。

    - **MIMCMKRAgent** を追加します。

    - **読み取り**と**登録**のアクセス許可を **KRAgent** に割り当てます。

7. **[新しいテンプレートのプロパティ**] ダイアログ ボックスで、**[OK]** をクリックします。

8. **証明書テンプレート コンソール**を閉じます。

#### <a name="publish-the-required-certificate-templates-at-the-certification-authority"></a>証明機関で必要な証明書テンプレートを発行する

1. **証明機関コンソール**を復元します。

2. **証明機関コンソール**のコンソール ツリーで、**[証明書テンプレート]** を右クリックし、**[新規作成]** をポイントして **[発行する証明書テンプレート]** をクリックします。
3. **[証明書テンプレートの選択]** ダイアログ ボックスで、 **[MIM CM 登録エージェント]**、**[MIM CM キー回復エージェント]**、および **[MIM CM 署名]** を選択します。 **[OK]**をクリックします。
4. コンソール ツリーで、**[証明書テンプレート]** をクリックします。
5. **[詳細]** ウィンドウに 3 つの新しいテンプレートが表示されることを確認してから、**証明機関**を閉じます。
    ![MIM CM 署名](media/mim-cm-deploy/image016.png)
6. 開いているすべてのウィンドウを閉じ、ログオフします。

### <a name="iis-configuration"></a>IIS 構成 

CM の Web サイトをホストするため、IIS をインストールして構成します。

#### <a name="install-and-configure-iis"></a>IIS のインストールと構成

1. **MIMINSTALL** アカウントで **CORLog in にログインします。

>[!IMPORTANT]
MIM インストール アカウントはローカル管理者である必要があります。

2. PowerShell を開き、次のコマンドを実行します。

   - ```Install-WindowsFeature –ConfigurationFilePath```

>[!NOTE]
 IIS 7 では、既定の Web サイトという名前のサイトが既定でインストールされます。 そのサイトが名前変更または削除された場合は、MIM CM をインストールする前に、既定の Web サイトという名前のサイトを使用可能にする必要があります。

#### <a name="configuring-kerberos"></a>Kerberos の構成

MIMCMWebAgent アカウントは、MIM CM ポータルを実行します。 既定では、カーネル モード認証が IIS で使用されます。 Kerberos カーネル モード認証を無効にし、代わりに、MIMCMWebAgent アカウントに SPN を構成します。 一部のコマンドには、Active Directory と CORPCM サーバーでの管理者アクセス許可が必要になります。

![](media/mim-cm-deploy/image020.png)

```
#Kerberos settings
#SPN
SETSPN -S http/cm.contoso.com contoso\MIMCMWebAgent
#Delegation for certificate authority
Get-ADUser CONTOSO\MIMCMWebAgent | Set-ADObject -Add @{"msDS-AllowedToDelegateTo"="rpcss/CORPCA","rpcss/CORPCA.contoso.com"}

```

** **CORPCM** での IIS の更新


![](media/mim-cm-deploy/image022.png)

```
add-pssnapin WebAdministration

Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name enabled -Value $true
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useKernelMode -Value $false
Set-WebConfigurationProperty -Filter System.webServer/security/authentication/WindowsAuthentication -Location 'Default Web Site' -Name useAppPoolCredentials -Value $true

```


>[!NOTE]
“cm.contoso.com” に DNS A レコードを追加し、CORPCM IP をポイントする必要があります。

#### <a name="requiring-ssl-on-the-mim-cm-portal"></a>MIM CM ポータルでの SSL の要求

MIM CM ポータルで SSL を要求することを強くお勧めします。 そうしない場合、ウィザードがそれについて警告します。

1. **cm.contoso.com** の Web 証明書に登録し、既定のサイトに割り当てます。

2. **IIS マネージャー**を開き、**[証明書の管理]** に移動します。

3. [機能ビュー] で [SSL 設定] をダブルクリックします。

4. SSL 設定 ページで、**SSL が必要** を選択します。

5. 操作 ウィンドウで、**適用** をクリックします。

### <a name="database-configuration-corpsql-for-mim-cm"></a>データベース構成 MIM CM の **CORPSQL**

1. CORPSQL01 サーバーに接続していることを確認します。

2. SQL データベース管理者としてログオンしていることを確認します。

3. 次の T-SQL スクリプトを実行して、構成手順に進んだときに、CONTOSO\\MIMINSTALL アカウントにデータベースの作成を許可します。

>[!NOTE]
終了とポリシー モジュールの準備ができたら、SQL に戻る必要があります。

```
create login [CONTOSO\\MIMINSTALL] from windows;
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'dbcreator';
exec sp_addsrvrolemember 'CONTOSO\\MIMINSTALL', 'securityadmin';  
```

![MIM CM 構成ウィザードのエラー メッセージ](media/mim-cm-deploy/image024.png)

## <a name="deployment-of-microsoft-identity-manager-2016-certificate-management"></a>Microsoft Identity Manager 2016 Certificate Manager の展開

1. CORPCM サーバーに接続していて、**MIMINSTALL** アカウントが**ローカル管理者**グループのメンバーであることを確認します。

2. Contoso\\MIMINSTALL としてログオンしていることを確認します。

3. Microsoft Identity Manager SP1 ISO をマウントします。

4. **Certificate Management\\x64** ディレクトリを**開きます**。

5. **[x64]** ウィンドウで、**[セットアップ]** を右クリックして、**[管理者として実行]** をクリックします。

6. Microsoft Identity Manager Certificate Management のセットアップ ウィザードの開始 ページで、**次へ** をクリックします。

7. 使用許諾契約書] ページで、契約書の内容を読み、**[使用許諾契約書の条項に同意する]** チェック ボックスをオンにして [次へ をクリックします。

8. カスタム セットアップ ページで、**MIM CM ポータル** と **MIM CM Update Service コンポーネント** がインストールされるように設定されていることを確認し、**次へ** をクリックします。

9. 仮想 Web フォルダー ページで、仮想フォルダー名が **CertificateManagement であることを確認し、**次へ** をクリックします。

10. Microsoft Identity Manager Certificate Management のインストール ページで、**インストール** をクリックします。

11. **[Microsoft Identity Manager Certificate Management のセットアップ ウィザードの完了]** ページで、**[完了]** をクリックします。

![MIM CM ウィザードの完了](media/mim-cm-deploy/image026.png)

### <a name="configuration-wizard-of-microsoft-identity-manager-2016-certificate-management"></a>Microsoft Identity Manager 2016 Certificate Management の構成ウィザード

CORPCM にログインする前に、構成ウィザードの **Domain Admins、Schema Admins、およびローカル管理者**グループに MIMINSTALL を追加してください。 これは構成が完了した後で削除できます。      
    
![エラー メッセージ](media/mim-cm-deploy/image028.png)

1. **[スタート]** メニューメニューから **[Certificate Management の構成ウィザード]** をクリックします。 **Administrator** として実行します。
2. **[構成ウィザードの開始]** ページで、**[次へ]** をクリックします。
3. **[CA 構成]** ページで、選択した CA が **Contoso-CORPCA-CA** であること、選択したサーバーが **CORPCA.CONTOSO.COM** であることを確認し、**[次へ]** をクリックします。
4. **[Microsoft® SQL Server® データベースの設定]** ページで、**[SQL Server の名前]** ボックスに「**CORPSQL1**」と入力し、**[自分の資格情報を使用してデータベースを作成する]** チェック ボックスをオンにしてから、**[次へ]** をクリックします。
5. **[データベースの設定]** ページで、**FIMCertificateManagement** の既定のデータベース名を受け入れ、**[SQL 統合認証]** が選択されていることを確認し、**[次へ]** をクリックします。

6. **[Active Directory のセットアップ]** ページで、サービス接続ポイントの既定の名前を受け入れ、**[次へ]** をクリックします。

7. **[認証方法]** ページで、**[Windows 統合認証]** が選択されていることを確認し、**[次へ]** をクリックします。

8. **[エージェント - FIM CM]** ページで、**[FIM CM の既定の設定を使用する]** チェック ボックスをオフにしてから、**[カスタム アカウント]** をクリックします。

9. **[エージェント - FIM CM]** マルチタブのダイアログ ボックスの各タブに、次の情報を入力します。
   - ユーザー名: **Update** 
   - パスワード: **Pass\@word1**
   - パスワードの確認入力: **Pass\@word1**
   - 既存のユーザーを使用する: **有効**
>[!NOTE]
これらのアカウントは既に作成しました。 手順 8 のプロシージャを 6 つのエージェント アカウントのすべてのタブに繰り返します。

![MIM CM アカウント](media/mim-cm-deploy/image030.png)

10. すべてのエージェント アカウント情報を入力したら、**[OK]** をクリックします。

11. **[エージェント - MIM CM]** ページで、**[次へ]** をクリックします。

12. **[サーバー証明書のセットアップ]** ページで、次の証明書テンプレートを有効にします。
    - 回復エージェントのキー回復エージェント証明書に使用する証明書テンプレート: **MIMCMKeyRecoveryAgent**
    - FIM CM エージェント証明書に使用する証明書テンプレート: **MIMCMSigning**
    - 登録エージェント証明書に使用する証明書テンプレート: **FIMCMEnrollmentAgent**
13. **[サーバー証明書のセットアップ]** ページで、**[次へ]** をクリックします。
14. **[電子メール サーバーとドキュメント印刷のセットアップ]** ページの **[登録通知の電子メール送信に使用する SMTP サーバー名を指定します]** ボックスで、**[次へ]** をクリックします。
15. **[構成の準備]** ページで **[構成]** をクリックします。
16. **構成ウィザード – Microsoft Forefront Identity Manager 2010 R2**の警告ダイアログ ボックスで、**[OK]** をクリックして、IIS 仮想ディレクトリで SSL が有効になっていないことを確認します。

    ![media/image17.png](media/mim-cm-deploy/image032.png)

    >[!NOTE] 
    構成ウィザードの実行が完了するまで、[完了] ボタンをクリックしてしないでください。 ウィザードのログは、**%programfiles%\\Microsoft Forefront Identity Management\\2010\\Certificate Management\\config.log** にあります。
17. **[Finish]** をクリックします。

![MIM CM ウィザードの完了](media/mim-cm-deploy/image033.png)

18. 開いているすべてのウィンドウを閉じます。

19. お使いのブラウザーでローカル イントラネット ゾーンに https://cm.contoso.com/certificatemanagement を追加します。

20. サーバー CORPCM からサイトにアクセスします。 https://cm.contoso.com/certificatemanagement  

    ![](media/mim-cm-deploy/image035.png)

### <a name="verify-the-cng-key-isolation-service"></a>CNG キー分離サービスの検証

1. **[管理ツール]** から、**[サービス]** を開きます。

2. **[詳細]** ウィンドウで、**[CNG キー分離]** をダブルクリックします。

3. **[全般]** タブで、**[スタートアップの種類]** を **[自動]** に変更します。

4. **[全般]** タブで、サービスを開始します (開始状態になっていない場合)。

5. **[全般]** タブで **[OK]** をクリックします。

### <a name="installing-and-configuring-the-ca-modules-"></a>CA モジュールのインストールと構成

この手順では、証明機関に FIM CM CA モジュールをインストールして構成します。

1. 管理操作のユーザーのアクセス許可のみを検査するように FIM CM を構成します。

2. **C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Certificate Management\\web** ウィンドウで、**web.config** のコピーを作成し、コピーに **web.1.config** と名前を付けます。

3. **[Web]** ウィンドウで **Web.config** を右クリックし、**[開く]** をクリックします。

    >[!Note]
    Web.config ファイルはメモ帳で開かれます。

4. ファイルが開いたら、Ctrl キーを押しながら F キーを押します。

5. **[検索と置換]** ダイアログ ボックスの **[検索]** ボックスに、「**UseUser**」を入力し、**[次を検索]** を 3 回クリックします。

6. **[検索と置換]** ダイアログ ボックスを閉じます。

7. **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser,UseGroups” /\>** の行にカーソルがあるはずです。 この行を **\<add key=”Clm.RequestSecurity.Flags” value=”UseUser” /\>** を読み取るように変更します。

8. すべての変更を保存して、ファイルを閉じます。

9. SQL Server で CA コンピューターのアカウントを作成します。\<スクリプトなし\>

10. **CORPSQL01** サーバーに接続していることを確認します。

11. **データベース管理者**としてログオンしていることを確認します。

12. **[スタート]** メニューから **[SQL Server Management Studio]** を起動します。

13. **[サーバーに接続]** ダイアログ ボックスの **[サーバー名]** ボックスに「**CORPSQL01**」と入力し、 **[接続]** をクリックします。

14. コンソール ツリーで、**[セキュリティ]** を展開して **[ログイン]** をクリックします。

15. **[ログイン]** を右クリックして、**[新しいログイン]** をクリックします。

16. **[全般]** ページの **[ログイン名]** ボックスに、「**contoso\\CORPCA\$**」と入力します。 **[Windows 認証]** を選択します。 既定のデータベースは **FIMCertificateManagement** です。

17. 左側のウィンドウで、**[ユーザー マッピング]** を選択します。 右側のウィンドウで、**FIMCertificateManagement** の横の **[マップ]** 列のチェック ボックスをオンにします。 **データベース ロールのメンバーシップ: FIMCertificateManagement** のリストで、**clmApp** ロールを有効にします。

18. **[OK]**をクリックします。

19. **Microsoft SQL Server Management Studio**を閉じます。

### <a name="install-the-fim-cm-ca-modules-on-the-certification-authority"></a>証明機関で FIM CM CA モジュールをインストールする

1. **CORPCA** サーバーに接続していることを確認します。

2. **[x64]** ウィンドウで、**Setup.exe** を右クリックして、**[管理者として実行]** をクリックします。

3. **[Microsoft Identity Manager Certificate Management のセットアップ ウィザードの開始]** ページで、**[次へ]** をクリックします。

4. **[使用許諾契約書]** ページで、契約書を読みます。 **[使用許諾契約書に同意します]** チェック ボックスをオンにして、**[次へ]** をクリックします。

5. **[カスタム セットアップ]** ページで、**[MIM CM ポータル]** をクリックしてから、**[この機能をインストールしない]** をクリックします。

6. **[カスタム セットアップ]** ページで、**[MIM CM 更新サービス]** をクリックしてから、**[この機能をインストールしない]** をクリックします。

    >[!Note]
    これにより、MIM CM CA ファイルがインストール可能な唯一の機能として残ります。

7. **[カスタム セットアップ]** ページで **[次へ]** をクリックします。

8. **[Microsoft Identity Manager Certificate Management のインストール]** ページで、**[インストール]** をクリックします。

9. **[Microsoft Identity Manager Certificate Management のセットアップ ウィザードの完了]** ページで、**[完了]** をクリックします。

10. 開いているすべてのウィンドウを閉じます。

### <a name="configure-the-mim-cm-exit-module"></a>MIM CM の終了モジュールを構成する

1. **管理ツール**から、**証明機関**を開きます。

2. コンソール ツリーで、**[contoso-CORPCA-CA]** を右クリックし、**[プロパティ]** をクリックします。

3. **[終了モジュール]** タブで **[FIM CM 終了モジュール]** を選択し、**[プロパティ]** をクリックします。

4. **[FIM CM データベースの接続文字列を指定します]** ボックスに、「**Connect Timeout=15;Persist Security Info=True; Integrated Security=sspi;Initial Catalog=FIMCertificateManagement;Data Source=CORPSQL01**」を入力します。 **[接続文字列を暗号化する]** チェック ボックスをオンのままにして、**[OK]** をクリックします。
5. **[Microsoft FIM Certificate Management]** メッセージ ボックスで、**[OK]** をクリックします。

6. **[contoso-CORPCA-CA のプロパティ]** ダイアログ ボックスで **[OK]** をクリックします。

7. **contoso-CA CORPCA**** を右クリックして、**[すべてのタスク]** をポイントし、**[サービスの停止]** をクリックします。 Active Directory Certificate Services が停止するのを待ちます。

8. **contoso-CA CORPCA****を右クリックして、**[すべてのタスク]** をポイントし、**[サービスの開始]** をクリックします。

9. **証明機関**コンソールを最小化します。

10. **[管理ツール]** から、**[イベント ビューアー]** を開きます。

11. コンソール ツリーで、**[アプリケーションとサービス ログ]** を展開し、**[FIM Certificate Management]** をクリックします。

12. イベントのリストで、最新のイベントに、最後に証明書サービスを再起動してから、**警告**または**エラー** イベントが含まれて*いない*ことを確認します。

    >[!NOTE] 
    最後のイベントは、終了モジュールが ```SYSTEM\CurrentControlSet\Services\CertSvc\Configuration\ContosoRootCA\ExitModules\Clm.Exit``` の設定を使用して読み込まれていることを示します。

13. **イベント ビューアー** を最小化します。

### <a name="copy-the-mimcmagent-certificates-thumbprint-to-windows-clipboard"></a>MIMCMAgent 証明書のサムプリントを Windows® クリップボードにコピーします。

1. **証明機関コンソール**を復元します。

2. コンソール ツリーで **[contoso-CA CORPCA]** を展開し、**[発行した証明書]** をクリックします。

3. **[詳細]** ウィンドウで、**[要求者名]** 列に **CONTOSO\\MIMCMAgent** があり、**[証明書テンプレート]** 列に **FIM CM Signing** がある証明書をダブルクリックします。

4. **[詳細]** タブで、**[拇印]** フィールドを選択します。

5. サムプリントを選択し、Ctrl + C キーを押します。

    >[!NOTE]
    サムプリント文字のリストでは、先頭にスペースを**含めないでください**。

6. **[証明書]** ダイアログ ボックスで、**[OK]** をクリックします。

7. **[スタート]** メニューの **[プログラムとファイルの検索]** ボックスに「**メモ帳**」と入力し、Enter キーを押します。

8. **メモ帳**で、**[編集]** メニューから **[貼り付け]** をクリックします。

9. **[編集]** メニューから **[置換]** をクリックします。

10. **[検索する文字列]** ボックスで、空白文字を入力し、**[すべて置換]** をクリックします。

    >[!Note]
    これにより、サムプリントの文字間のスペースがすべて削除されます。
    
11. **[置換]** ダイアログ ボックスで、**[キャンセル]** をクリックします。

12. 変換された *thumbprintstring* を選択し、CTRL + C を押します。

13. 変更を保存せずに**メモ帳**を閉じます。

### <a name="configure-the-fim-cm-policy-module"></a>FIM CM ポリシー モジュールを構成する

1. **証明機関コンソール**を復元します。

2. **[contoso-CORPCA-CA]** を右クリックし、**[プロパティ]** をクリックします。

3.  **[contoso-CORPCA-CA のプロパティ]** ダイアログ ボックスの **[ポリシー モジュール]** タブで、**[プロパティ]** をクリックします。

- **[全般]** タブで、**[FIM CM 以外の要求は既定のポリシー モジュールで処理する]** が選択されていることを確認します。
- **[署名証明書]** タブで、**[追加]** をクリックします。
- 証明書 ダイアログ ボックスで、**16 進でエンコードされた証明書ハッシュを指定してください** ボックスを右クリックして、**貼り付け** をクリックします。
- **[証明書]** ダイアログ ボックスで、**[OK]** をクリックします。
    >[!Note]
    **[OK]** ボタンが有効になっていない場合は、clmAgent 証明書からサムプリントをコピーしたときに、隠し文字がサムプリント文字列に誤って含まれてしまいました。 この演習の**タスク 4: MIMCMAgent 証明書のサムプリントを Windows クリップボードにコピーする**からの手順をすべて繰り返します。

- **[構成プロパティ]** ダイアログ ボックスで、サムプリントが **[有効な署名証明書]** リストに表示されていることを確認し、**[OK]** をクリックします。

- **[FIM Certificate Management]** メッセージ ボックスで、**[OK]** をクリックします。

- **[contoso-CORPCA-CA のプロパティ]** ダイアログ ボックスで **[OK]** をクリックします。

- **contoso-CA CORPCA****を右クリックして、**[すべてのタスク]** をポイントし、**[サービスの停止]** をクリックします。

- Active Directory Certificate Services が停止するのを待ちます。

- **contoso-CA CORPCA**** を右クリックして、**[すべてのタスク]** をポイントし、**[サービスの開始]** をクリックします。

- **証明機関**コンソールを閉じます。

- 開いているすべてのウィンドウを閉じ、ログオフします。

- **展開の最終ステップ**では、Schema Admins および Domain Admins でなくても、CONTOSO\\MIMCM-Managers がテンプレートを展開および作成し、システムを構成できることを確認します。 次のスクリプトは、dsacls を使用して証明書テンプレートにアクセス許可を付与します。 セキュリティを変更する完全なアクセス許可を持つアカウントで実行し、フォレスト内の既存の各証明書テンプレートに読み取り/書き込みアクセス許可を付与します。

- 最初のステップ: **サービス接続ポイントとターゲットのグループのアクセス許可の構成と、プロファイル テンプレートの管理の委任**
  - サービス接続ポイント (SCP) にアクセス許可を構成します。

  - 委任されたプロファイル テンプレートの管理を構成します。

  - サービス接続ポイント (SCP) にアクセス許可を構成します。 **\<スクリプトなし\>**

        -   **CORPDC** 仮想サーバーに接続していることを確認します。

        -   **contoso\\corpadmin** としてログオンします。

        -   **[管理ツール]** から **[Active Directory ユーザーとコンピューター]** を開きます。

        -   **[Active Directory ユーザーとコンピューター]** の **[表示]** メニューで、**[拡張機能]** がオンになっていることを確認します。

        -   コンソール ツリーで **Contoso.com** \| **System** \| **Microsoft** \| **Certificate Lifecycle Manager** を展開し、**[CORPCM]** をクリックします。

        -   **[CORPCM]** を右クリックし、**[プロパティ]** をクリックします。

        -   **[CORPCM のプロパティ]** ダイアログ ボックスの **[セキュリティ]** タブで、対応するアクセス許可を持つ次のグループを追加します。

    | Group          | アクセス許可                                                                                                                                                         |
    |----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | mimcm-Managers | 読み取り </br> FIM CM Audit</br> FIM CM Enrollment Agent</br> FIM CM Request Enroll</br> FIM CM Request Recover</br> FIM CM Request Renew</br> FIM CM Request Revoke </br> FIM CM Request Unblock Smart Card |
    | mimcm-HelpDesk | 読み取り</br> FIM CM Enrollment Agent</br> FIM CM Request Revoke</br> FIM CM Request Unblock Smart Card                                                                                |
- **[CORPDC のプロパティ]** ダイアログ ボックスで **[OK]** をクリックします。

- **[Active Directory ユーザーとコンピューター]** は開いたままにします。

- **子孫ユーザー オブジェクトのアクセス許可を構成します。**
    - **[Active Directory ユーザーとコンピューター]** コンソールにまだいることを確認します。
    - コンソール ツリーで、**[Contoso.com]** を右クリックし、**[プロパティ]** をクリックします。
    - **[セキュリティ]** タブで **[詳細設定]** をクリックします。
    - **[Contoso のセキュリティの詳細設定]** ダイアログ ボックスで、**[追加]** をクリックします。
    - **[ユーザー、コンピューター、サービス アカウント、またはグループの選択]** ダイアログ ボックスの **[選択するオブジェクト名を入力します]** テキスト ボックスに「**mimcm-Managers**」と入力し、**[OK]** をクリックします。
    - **[Contoso のアクセス許可エントリ]** ダイアログ ボックスの **[適用先]** リストで、**[子孫ユーザー オブジェクト]** を選択し、次の **[アクセス許可]** の **[許可]** チェック ボックスをオンにします。
        - **すべてのプロパティの読み取り**
        - **読み取りアクセス許可**
        - **FIM CM Audit**
        - **FIM CM Enrollment Agent**
        - **FIM CM Request Enroll**
        - **FIM CM Request Recover**
        - **FIM CM Request Renew**
        - **FIM CM Request Revoke**
        - **FIM CM Request Unblock Smart Card**
    - **[Contoso のアクセス許可エントリ]** ダイアログ ボックスで、**[OK]** をクリックします。
    - **[Contoso のセキュリティの詳細設定]** ダイアログ ボックスで、**[追加]** をクリックします。
    - **[ユーザー、コンピューター、サービス アカウント、またはグループの選択]** ダイアログ ボックスの **[選択するオブジェクト名を入力します]** テキスト ボックスに「**mimcm-HelpDesk**」と入力し、**[OK]** をクリックします。
    - **[Contoso のアクセス許可エントリ]** ダイアログ ボックスの **[適用先]** リストで、**[子孫ユーザー オブジェクト]** を選択し、次の **[アクセス許可]** の **[許可]** チェック ボックスをオンにします。       - **すべてのプロパティの読み取り** - **読み取りアクセス許可** - **FIM CM Enrollment Agent** - **FIM CM Request Revoke** - **FIM CM Request Unblock Smart Card**
    - **[Contoso のアクセス許可エントリ]** ダイアログ ボックスで、**[OK]** をクリックします。
    **[Contoso のセキュリティの詳細設定]** ダイアログ ボックスで、**[OK]** をクリックします。
    - **[contoso.com のプロパティ]** ダイアログ ボックスで **[OK]** をクリックします。
    - **[Active Directory ユーザーとコンピューター]** は開いたままにします。

    - **子孫ユーザー オブジェクトのアクセス許可を構成します。\<スクリプトなし\>**
        - **[Active Directory ユーザーとコンピューター]** コンソールにまだいることを確認します。
        - コンソール ツリーで、**[Contoso.com]** を右クリックし、**[プロパティ]** をクリックします。
        - **[セキュリティ]** タブで **[詳細設定]** をクリックします。
        - **[Contoso のセキュリティの詳細設定]** ダイアログ ボックスで、**[追加]** をクリックします。
        - **[ユーザー、コンピューター、サービス アカウント、またはグループの選択]** ダイアログ ボックスの **[選択するオブジェクト名を入力します]** テキスト ボックスに「**mimcm-Managers**」と入力し、**[OK]** をクリックします。
        - **[CONTOSO のアクセス許可エントリ]** ダイアログ ボックスの **[適用先]** リストで、**[子孫ユーザー オブジェクト]** を選択し、次の **[アクセス許可]** の **[許可]** チェック ボックスをオンにします。
            - **すべてのプロパティの読み取り**
            - **読み取りアクセス許可**
            - **FIM CM Audit**
            - **FIM CM Enrollment Agent**
            - **FIM CM Request Enroll**
            - **FIM CM Request Recover**
            - **FIM CM Request Renew**
            - **FIM CM Request Revoke**
            - **FIM CM Request Unblock Smart Card**
    - **[CONTOSO のアクセス許可エントリ]** ダイアログ ボックスで、**[OK]** をクリックします。
    - **[CONTOSO のセキュリティの詳細設定]** ダイアログ ボックスで、**[追加]** をクリックします。
    - **[ユーザー、コンピューター、サービス アカウント、またはグループの選択]** ダイアログ ボックスの **[選択するオブジェクト名を入力します]** テキスト ボックスに「**mimcm-HelpDesk**」と入力し、**[OK]** をクリックします。
    - **[CONTOSO のアクセス許可エントリ]** ダイアログ ボックスの **[適用先]** リストで、**[子孫ユーザー オブジェクト]** を選択し、次の **[アクセス許可]** の **[許可]** チェック ボックスをオンにします。       - **すべてのプロパティの読み取り** - **読み取りアクセス許可** - **FIM CM Enrollment Agent** - **FIM CM Request Revoke** - **FIM CM Request Unblock Smart Card**
    - **[contoso のアクセス許可エントリ]** ダイアログ ボックスで、**[OK]** をクリックします。
    - **[Contoso のセキュリティの詳細設定]** ダイアログ ボックスで、**[OK]** をクリックします。
    - **[contoso.com のプロパティ]** ダイアログ ボックスで **[OK]** をクリックします。
    - **[Active Directory ユーザーとコンピューター]** は開いたままにします。
- 2 番目のステップ: **証明書テンプレートの管理アクセス許可を委任する \<スクリプト\>**
    - 証明書テンプレート コンテナーに対するアクセス許可を委任します。
    - OID コンテナーに対するアクセス許可を委任します。
    - 既存の証明書テンプレートに対するアクセス許可を委任します。
- 証明書テンプレート コンテナーに対するアクセス許可を定義する
     1. **Active Directory サイトとサービス** コンソールを復元します。
     2. コンソール ツリーで、**[Services]**、**[Public Key Services]** の順に展開し、**[証明書テンプレート]** をクリックします。
     3. コンソール ツリーで、**[証明書テンプレート]** を右クリックし、**[委任コントロール]** をクリックします。
     4. **オブジェクト制御の委任**ウィザードで、**[次へ]** をクリックします。
     5. **[ユーザーまたはグループ]** ページで、**[追加]** をクリックします。
     6. **[ユーザー、コンピューター、またはグループの選択]** ダイアログ ボックスの **[選択するオブジェクト名を入力してください]** ボックスに「**mimcm-Managers**」と入力し、**[OK]** をクリックします。
     7. **[ユーザーまたはグループ]** ページで、**[次へ]** をクリックします。
     8. **[委任するタスク]** ページで、**[委任するカスタム タスクを作成する]** をクリックし、**[次へ]** をクリックします。
     9.  **[Active Directory オブジェクトの種類]** ページで、**[このフォルダー、このフォルダー内の既存のオブジェクト、およびこのフォルダー内の新しいオブジェクトの作成]** が選択されていることを確認し、**[次へ]** をクリックします。
     10. **[アクセス許可]** ページの **[アクセス許可]** リストで、**[フル コントロール]** チェック ボックスをオンにして、**[次へ]** をクリックします。
     11. オブジェクト制御の委任ウィザードの **[完了]** ページで、**[完了]** をクリックします。

- OID コンテナーに対するアクセス許可を定義する
     1. コンソール ツリーで **[OID]** を右クリックし、**[プロパティ]** をクリックします。
     2. **[OID のプロパティ]** ダイアログ ボックスの **[セキュリティ]** タブで、**[詳細設定]** をクリックします。
     3. **[OID のセキュリティの詳細設定]** ダイアログ ボックスで、**[追加]** をクリックします。
     4. **[ユーザー、コンピューター、サービス アカウント、またはグループの選択]** ダイアログ ボックスの **[選択するオブジェクト名を入力します]** テキスト ボックスに「**mimcm-Managers**」と入力し、**[OK]** をクリックします。
     5. **[OID のアクセス許可エントリ]** ダイアログ ボックスで、**[このオブジェクトとすべての子オブジェクト]** にアクセス許可が適用されていること確認し、**[フル コントロール]** をクリックして、**[OK]** をクリックします。
     6. **[OID のセキュリティの詳細設定]** ダイアログ ボックスで、**[OK]** をクリックします。
     7. **[OID のプロパティ]** ダイアログ ボックスで **[OK]** をクリックします。
     8. **[Active Directory サイトとサービス]** を閉じます。

**スクリプト: OID、プロファイル テンプレート、および証明書テンプレートのコンテナーに対するアクセス許可**

![](media/mim-cm-deploy/image021.png)

```import-module activedirectory $adace = @{ "OID" = "AD:\\CN=OID,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com"; "CT" = "AD:\\CN=Certificate Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com"; "PT" = "AD:\\CN=Profile Templates,CN=Public Key Services,CN=Services,CN=Configuration,DC=contoso,DC=com" } $adace.GetEnumerator() | **Foreach-Object** { $acl = **Get-Acl** *-Path* $_.Value $sid=(**Get-ADGroup** "MIMCM-Managers").SID $p = **New-Object** System.Security.Principal.SecurityIdentifier($sid)
##<a name="httpsmsdnmicrosoftcomen-uslibrarysystemdirectoryservicesactivedirectorysecurityinheritancevvs110aspx"></a>https://msdn.microsoft.com/en-us/library/system.directoryservices.activedirectorysecurityinheritance(v=vs.110).aspx
$ace = **New-Object** System.DirectoryServices.ActiveDirectoryAccessRule ($p,[System.DirectoryServices.ActiveDirectoryRights]"GenericAll",[System.Security.AccessControl.AccessControlType]::Allow, [DirectoryServices.ActiveDirectorySecurityInheritance]::All) $acl.AddAccessRule($ace) **Set-Acl** *-Path* $_.Value *-AclObject* $acl }
```

**Scripts: Delegating permissions on the existing certificate templates.**

![](media/mim-cm-deploy/image039.png)

dsacls "CN=Administrator,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CAExchange,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CEPEncryption,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ClientAuth,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CodeSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CrossCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=CTLSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DirectoryEmailReplication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainController,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=DomainControllerAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFS,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EFSRecovery,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=EnrollmentAgentOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=ExchangeUserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=FIMCMKeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOffline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=IPSecIntermediateOnline,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KerberosAuthentication,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=KeyRecoveryAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Machine,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=MachineEnrollmentAgent,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OCSPResponseSigning,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=OfflineRouter,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=RASAndIASServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardLogon,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SmartCardUser,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=SubCA,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=User,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=UserSignature,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=WebServer,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO

dsacls "CN=Workstation,CN=Certificate Templates,CN=Public Key
Services,CN=Services,CN=Configuration,DC=Contoso,DC=com" /G
Contoso\\MIMCM-Managers:SDDTRCWDWOLCWPRPCCDCWSLO
