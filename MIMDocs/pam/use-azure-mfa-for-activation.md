---
title: "Azure MFA を使用した PAM のアクティブ化 | Microsoft Docs"
description: "ユーザーが Privileged Access Management でロールをアクティブ化すると、Azure MFA が第 2 のセキュリティ層として設定されます。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5134a112-f73f-41d0-a5a5-a89f285e1f73
ms.reviewer: mwahl
ms.suite: ems
ms.translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: b937b30da2dff9bbfeabf7dceb43fcaca99a1b63
ms.contentlocale: ja-jp
ms.lasthandoff: 07/10/2017


---

<a id="using-azure-mfa-for-activation" class="xliff"></a>
# Azure MFA を使用したアクティブ化
PAM ロールを構成するときは、ロールのアクティブ化を要求するユーザーを承認する方法を選択できます。 PAM の認証アクティビティの実装には、以下の選択肢があります。

- ロールの所有者の承認
- Azure 多要素認証 (MFA)

どちらのチェックも有効ではない場合、候補ユーザーは各自のロールに対して自動的にアクティブになります。

Microsoft Azure Multi-Factor Authentication (MFA) は、ユーザーがモバイル アプリ、電話、またはテキスト メッセージを使用してサインイン試行を確認する必要がある認証サービスです。 Microsoft Azure Active Directory での利用が可能で、クラウドとオンプレミスのエンタープライズ アプリケーション用のサービスとして使用できます。 PAM のシナリオでは、Azure MFA には、候補ユーザーが以前 Windows PRIV ドメインに対して使用していた認証方法に関係なく、認証で使用できる追加の認証メカニズムがあります。

<a id="prerequisites" class="xliff"></a>
## 必要条件

MIM で Azure MFA を使用するには、次の項目が必要です。

- Azure MFA サービスと通信するための、PAM を提供している各 MIM サービスからのインターネット アクセス
- Azure サブスクリプション
- 候補ユーザー用の Azure Active Directory Premium ライセンス、またはその他 Azure MFA のライセンス許諾を受ける方法
- 全候補ユーザーの電話番号

<a id="creating-an-azure-mfa-provider" class="xliff"></a>
## Azure MFA プロバイダーの作成

ここでは、Microsoft Azure Active Directory に Azure MFA プロバイダーを設定します。  すでに Azure MFA をスタンドアロンで使用している場合、または Azure Active Directory Premium を使用して構成している場合は、次のセクションに進んでください。

1.  Web ブラウザーを開いて、Azure サブスクリプション管理者として [Azure クラシック ポータル](https://manage.windowsazure.com)に接続します。

2.  左下隅の **[新規]**をクリックします。

3.  **[App Services] > [Active Directory] > [多要素認証プロバイダー] > [簡易作成]** をクリックします。

4.  **[名前]** フィールドに「 **PAM**」と入力し、[使用モデル] フィールドで [有効化されたユーザーごと] を選択します。 Azure AD ディレクトリが既にある場合は、そのディレクトリを選択します。 最後に **[作成]**をクリックします。

<a id="downloading-the-azure-mfa-service-credentials" class="xliff"></a>
## Azure MFA サービス資格情報のダウンロード

次に、Azure MFA に接続するために PAM の認証情報を含むファイルを生成します。

1. Web ブラウザーを開いて、Azure サブスクリプション管理者として [Azure クラシック ポータル](https://manage.windowsazure.com)に接続します。

2.  Azure ポータル メニューの **[Active Directory]** をクリックして、**[多要素認証プロバイダー]** タブをクリックします。

3.  PAM で使用する Azure MFA プロバイダーをクリックして、**[管理]**をクリックします。

4.  新しいウィンドウの左側のパネルで **[構成]**をクリックし、**[設定]**をクリックします。

5.  **[Azure Multi-factor Authentication]** ウィンドウで、**[ダウンロード]** の下の **[SDK]**をクリックします。

6.  言語 **ASP.net 2.0 C に対応する SDK\#** ファイルの [ZIP] 列の **[ダウンロード]** リンクをクリックします。

![Multi-Factor Authentication SDK のダウンロード - スクリーンショット](media/PAM-Azure-MFA-Activation-Image-1.png)

7.  MIM サービスがインストールされている各システムに、ダウンロードした ZIP ファイルをコピーします。 

>[!NOTE]
> ZIP ファイルには Azure MFA サービスへの認証に使用されるキー生成情報が含まれています。

<a id="configuring-the-mim-service-for-azure-mfa" class="xliff"></a>
## Azure MFA の MIM サービスの構成

1.  MIM サービスがインストールされているコンピューターで、管理者または MIM をインストールしたユーザーとしてサインインします。

2.  `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service\\MfaCerts` などの MIM サービスがインストールされたディレクトリの下に、新しいディレクトリ フォルダーを作成します。

3.  Windows エクスプローラーを使用して、前のセクションでダウンロードした ZIP ファイルの **pf\\certs** フォルダーに移動し、**cert\_key.p12** ファイルを新しいディレクトリにコピーします。

4.  Windows エクスプローラーを使用して、ZIP の **pf** フォルダーに移動し、ファイル **pf\_auth.cs** を Wordpad などのテキスト エディターで開きます。

5.  3 つのパラメーター **LICENSE\_KEY**、**GROUP\_KEY**、**CERT\_PASSWORD** を検索します。

![pf\_auth.cs ファイルの値のコピー - スクリーン ショット](media/PAM-Azure-MFA-Activation-Image-2.png)

6.  メモ帳を使用して、`C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service` にある **MfaSettings.xml** を開きます。

7.  pf\_auth.cs ファイルの LICENSE\_KEY、GROUP\_KEY、および CERT\_PASSWORD の各パラメーターの値を、MfaSettings.xml ファイル内で対応するそれぞれの xml 要素にコピーします。

8.  **<CertFilePath>** XML 要素で、先ほど抽出した cert\_key.p12 ファイルの完全パス名を指定します。

9.  **<username>** 要素にユーザー名を入力します。

10.  **<DefaultCountryCode>** 要素にユーザーがダイヤルする国コード (たとえば米国およびカナダの場合は 1) を入力します。 この値は、ユーザーが登録している電話番号に国コードがない場合に使用されます。 ユーザーの電話番号に、組織で構成された国コードとは異なるコードがある場合は、その国コードを登録する電話番号に含める必要があります。

11.  MIM サービス フォルダー `C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Service` に **MfaSettings.xml** を保存して上書きします。 

> [!NOTE]
> プロセスの最後に、ファイル **MfaSettings.xml** またはそのコピー、あるいは ZIP ファイルが読み取り可能ではないことを確認します。

<a id="configure-pam-users-for-azure-mfa" class="xliff"></a>
## Azure MFA 用に PAM ユーザーを構成する

Azure MFA を必要とするロールをアクティブ化するユーザーについては、そのユーザーの電話番号を MIM に格納する必要があります。 この属性は 2 通りの方法で設定します。

1 つ目は、`New-PAMUser` コマンドにより、電話番号属性を CORP ドメインのユーザーのディレクトリ エントリから MIM サービス データベースにコピーする方法です。 これは、1 回限りの操作であることに注意してください。

2 つ目は、`Set-PAMUser` コマンドにより、MIM サービス データベース内の電話番号属性を更新する方法です。 たとえば、次の内容は、MIM サービス内の既存の PAM ユーザーの電話番号を置換します。 そのディレクトリ エントリは変更されません。

```
Set-PAMUser (Get-PAMUser -SourceDisplayName Jen) -SourcePhoneNumber 12135551212
```


<a id="configure-pam-roles-for-azure-mfa" class="xliff"></a>
## Azure MFA 用に PAM ロールを構成する

PAM ロールの候補ユーザーすべての電話番号を MIM サービス データベースに格納すると、Azure MFA を必要とするようにロールを構成できます。 これは、`New-PAMRole` コマンドまたは `Set-PAMRole` コマンドを使用して行います。 例:

```
Set-PAMRole (Get-PAMRole -DisplayName "R") -MFAEnabled 1
```

`Set-PAMRole` コマンドにパラメーター "-MFAEnabled 0" を指定すると、ロールに対して Azure MFA を無効にすることができます。

<a id="troubleshooting" class="xliff"></a>
## トラブルシューティング

Privileged Access Management のイベント ログには、次のイベントが記録されます。

| ID  | 重大度 | 生成元 | 説明 |
|-----|----------|--------------|-------------|
| 101 | エラー       | MIM サービス            | Azure MFA を完了しなかったユーザー (たとえば、電話に応答しなかった) |
| 103 | 説明 | MIM サービス            | アクティブ化の際に Azure MFA を完了したユーザー                       |
| 825 | 警告     | PAM 監視サービス | 電話番号が変更された                                |

電話に失敗した (イベント 101) 理由について確認するには、Azure MFA からレポートを表示またはダウンロードできます。

1.  Web ブラウザーを開いて、Azure AD 全体管理者として [Azure クラシック ポータル](https://manage.windowsazure.com)に接続します。

2.  Azure ポータル メニューの **[Active Directory]** を選択して、**[多要素認証プロバイダー]** タブを選択します。

3.  PAM で使用する Azure MFA プロバイダーを選択して、**[管理]**をクリックします。

4.  新しいウィンドウで、**[使用状況]**、**[ユーザーの詳細]**の順にクリックします。

5.  時間の範囲を選択して、追加のレポート列の **[名前]** の隣のチェックボックスをオンにします。 **[CSV にエクスポート]**をクリックします。

6.  レポートが生成されたら、それをポータルで表示したり、MFA レポートが大きい場合は CSV ファイルにダウンロードしたりすることができます。 **AUTH TYPE** 列の **SDK** の値は、PAM のアクティブ化要求と関連する行を示します。これは MIM またはその他のオンプレミス ソフトウェアから送信されたイベントです。 **USERNAME** フィールドは、MIM サービス データベース内のユーザー オブジェクトの GUID です。 呼び出しが失敗すると、**AUTHD** 列の値は **No** となり、**CALL RESULT** 列には失敗理由の詳細が格納されます。

