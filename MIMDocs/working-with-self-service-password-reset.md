---
title: "セルフサービス パスワード リセット ポータルを使用する | Microsoft Docs"
description: "MIM 2016 でのセルフ サービス パスワード リセットの新機能 (多要素認証による SSPR のしくみなど) を参照してください。"
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0463a91275f3e181a66eb460c167bb9a2008f444
ms.sourcegitcommit: 27a23142393bbb0f66a3d533d89a5a8366a29e41
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2017
---
>[!IMPORTANT]
Azure Multi-factor Authentication ソフトウェア開発キットの廃止予定が発表されました。 Azure MFA SDK は、既存のお客様向けに 2018 年 11 月 14 日の提供終了日までサポートされます。 新規および現在のお客様は、それ以降 Azure クラシック ポータルから SDK をダウンロードできなくなります。 ダウンロードするには、Azure カスタマー サポートに連絡して、生成された MFA サービス資格情報のパッケージを受け取る必要があります。 <br> Microsoft 開発チームは、MFA Server SDK との統合により、MFA の変更を計画しています。この変更は、2018 年初頭に予定されている修正プログラムに含まれる予定です。

# <a name="working-with-self-service-password-reset"></a>セルフサービスのパスワード リセットを使用する
Microsoft Identity Manager 2016 では、セルフサービス パスワード リセット機能に追加機能が提供されています。 この機能は、いくつかの重要な機能で強化されています。

-   セルフサービス パスワード リセット ポータルおよび Windows ログイン画面で、ユーザーが、パスワードを変更したりサポート管理者を呼び出さなくても、自分のアカウントのロックを解除できるようになりました。 ユーザーは、古いパスワードを入力した、2 か国語コンピューターで間違った言語設定のキーボードを使用した、または他のユーザーのアカウントで既に開かれている共有ワークステーションにログインしようとしたなど、多くの正当な理由でアカウントがロックされることがあります。

-   新しい認証ゲートとして電話ゲートが追加されました。 これにより、ユーザーは電話を使用して認証できます。

-   Microsoft Azure Multi-Factor Authentication (MFA) サービスのサポートが追加されました。 これは、既存の SMS ワンタイム パスワード ゲートまたは新しい電話ゲートに使用できます。

## <a name="azure-for-multi-factor-authentication"></a>多要素認証用の Azure
Microsoft Azure Multi-Factor Authentication は、ユーザーがモバイル アプリ、電話、またはテキスト メッセージを使用してサインイン試行を確認する必要がある認証サービスです。 Microsoft Azure Active Directory での利用が可能で、クラウドとオンプレミスのエンタープライズ アプリケーション用のサービスとして使用できます。

Azure MFA の追加認証メカニズムは、セルフサービス ログイン アシスタント用に MIM で実行されるものなどの既存の認証プロセスを強化できます。

Azure MFA を使用する場合、ユーザーは、アカウントやリソースへのアクセスを回復しようとして自身の ID を確認するためにシステムで認証します。 認証には、SMS または電話を使用できます。   認証の強度が高いほど、アクセスしようとしているユーザーが実際に ID を所有しているユーザーである信頼度が高くなります。 認証されると、ユーザーは古いパスワードを新しいパスワードに変更できます。

## <a name="prerequisites-to-set-up-self-service-account-unlock-and-password-reset-using-mfa"></a>MFA を使用してセルフサービス アカウント ロック解除およびパスワード リセットを設定する前提条件
このセクションでは、Microsoft Identity Manager 2016 および以下のコンポーネントとサービスをダウンロードして展開してあるものとします。

-   Windows Server 2008 R2 以降を指定されたドメイン (「企業」ドメイン) の AD ドメイン サービスおよびドメイン コント ローラーを含む Active Directory サーバーとして設定してあります。

-   アカウント ロックアウトのグループ ポリシーが定義されています。

-   MIM 2016 同期サービス (Sync) が、AD ドメインに参加しているサーバーにインストールされて実行しています。

-   SSPR 登録ポータルおよび SSPR リセット ポータルを含む MIM 2016 サービスおよびポータルがサーバーにインストールされて実行しています (Sync と共存可能)

-   MIM Sync が次のように AD-MIM ID 同期用に構成されています。

    -   Active Directory 管理エージェント (ADMA) で、AD DS への接続、および Active Directory との間で ID データをインポートおよびエクスポートする機能を構成します。

    -   MIM 管理エージェント (MIM MA) で、FIM サービス DB への接続、および FIM データベースとの間で ID データをインポートおよびエクスポートする機能を構成します。

    -   ユーザー データの同期を許可し、MIM サービスの同期ベースのアクティビティを容易にするように、MIM ポータルの同期ルールを構成します。

-   SSPR Windows ログイン統合クライアントを含む MIM 2016 アドインおよび拡張機能は、サーバーまたは別のクライアント コンピューターに展開されています。

## <a name="prepare-mim-to-work-with-multi-factor-authentication"></a>多要素認証を使用するように MIM を準備する
パスワード リセットおよびアカウント ロック解除機能をサポートするように MIM Sync を構成します。 詳細については、「[FIM のアドインと拡張機能のインストール](https://technet.microsoft.com/library/ff512688%28v=ws.10%29.aspx)」、「[FIM SSPR のインストール](https://technet.microsoft.com/library/hh322891%28v=ws.10%29.aspx)」、「[SSPR 認証ゲート](https://technet.microsoft.com/library/jj134288%28v=ws.10%29.aspx)」、「[SSPR テスト ラボ ガイド](https://technet.microsoft.com/library/hh826057%28v=ws.10%29.aspx)」を参照してください。

次のセクションでは、Microsoft Azure Active Directory に Azure MFA プロバイダーを設定します。 この作業の一環として、Azure MFA に接続できるようになるために MFA で必要となる認証情報を含むファイルが生成されます。  続行するには、Azure サブスクリプションが必要です。

### <a name="register-your-multi-factor-authentication-provider-in-azure"></a>Azure で多要素認証プロバイダーを登録する

1.  [Azure クラシック ポータル](http://manage.windowsazure.com)に移動し、Azure サブスクリプション管理者としてサインインします。

2.  左下隅の **[新規]**をクリックします。

3.  **[App Services] &gt; [Active Directory] &gt; [多要素認証プロバイダー] &gt; [簡易作成]** をクリックします。

![Azure Portal での MFA の簡易作成の画像](media/MIM-SSPR-Azureportal.png)

4.  **[名前]** フィールドに「**SSPRMFA**」と入力し、**[作成]** をクリックします。

![Azure ポータル MFA の画像](media/MIM-SSPR-Azureportal-2.png)

5.  Azure クラシック ポータル メニューの **[Active Directory]** をクリックして、**[多要素認証プロバイダー]** タブをクリックします。

6.  **[SSPRMFA]** をクリックし、画面下部の **[管理]** をクリックします。

    ![Azure ポータルの管理アイコン](media/MIM-SSPR-ManageButton.png)

7.  新しいウィンドウの左側のパネルの **[構成]** の下で、**[設定]** をクリックします。

8.  **[不正アクセスのアラート]**で、[不正アクセスを通報したユーザーをブロックする] をオフにします。 これは、サービス全体のブロックを回避するために行います。

9. **[Azure Multi-factor Authentication]** ウィンドウで、左側のメニューの **[ダウンロード]** の **[SDK]** をクリックします。

10. 言語 **ASP.net 2.0 C# に対応する SDK** ファイルの [ZIP] 列の **[ダウンロード]** リンクをクリックします。

    ![Azure MFA の zip ファイルのダウンロードの画像](media/MIM-SSPR-Azure-MFA.png)

11. MIM サービスがインストールされている各システムに、ダウンロードした ZIP ファイルをコピーします。  ZIP ファイルには Azure MFA サービスへの認証に使用されるキー生成情報が含まれていることに注意してください。

### <a name="update-the-configuration-file"></a>構成ファイルを更新する

1. MIM サービスがインストールされているコンピューターに、MIM をインストールしたユーザーとしてサインインします。

2. MIM サービスをインストールしたディレクトリの下に、新しいディレクトリ フォルダーを作成します (**C:\Program Files\Microsoft Forefront Identity Manager\2010\Service\MfaCerts** など)。

3. Windows エクスプローラーを使用して、前のセクションでダウンロードした ZIP ファイルの **\pf\certs** フォルダーに移動し、**cert_key.p12** ファイルを新しいディレクトリにコピーします。

4.  SDK zip ファイルの **\pf** フォルダーにある **pf_auth.cs** ファイルを開きます。

5.  3 つのパラメーター `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD`を探します。

    ![pf_auth.cs コードの画像](media/MIM-SSPR-pFile.png)

6.  **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**で、ファイル **MfaSettings**.xml を開きます。

7.  pf_aut.cs ファイルの `LICENSE_KEY, GROUP_KEY, CERT_PASSWORD` パラメーターの値を、MfaSettings.xml ファイルの対応する xml 要素にコピーします。

8.  SDK zip ファイルの \pf\certs にある **cert_key.p12** ファイルを抽出し、それへのフル パスを MfaSettings.xml ファイルの `<CertFilePath>` xml 要素に入力します。

9. `<username>` 要素にユーザー名を入力します。

10. `<DefaultCountryCode>` 要素に既定の国コードを入力します。 電話番号が国コードなしでユーザーに登録されている場合は、ユーザーの国コードです。 ユーザーに国際国コードがある場合は、登録電話番号に含める必要があります。

11. MfaSettings.xml ファイルを同じ名前で同じ場所に保存します。

#### <a name="configure-the-phone-gate-or-the-one-time-password-sms-gate"></a>電話ゲートまたはワンタイム パスワード SMS ゲートの構成

1.  Internet Explorer を起動して MIM ポータルに移動し、MIM 管理者として認証を行った後、左側のナビゲーション バーにある  **[ワークフロー]** をクリックします。

    ![MIM ポータル ナビゲーションの画像](media/MIM-SSPR-workflow.jpg)

2.  **[パスワード リセット AuthN ワークフロー]**をオンにします。

    ![MIM ポータル ワークフローの画像](media/MIM-SSPR-PwdResetAuthNworkflow.jpg)

3.  **[アクティビティ]** タブをクリックし、 **[アクティビティの追加]**まで下にスクロールします。

4.  **[電話ゲート]** または **[ワンタイム パスワードの SMS ゲート]** を選択し、**[選択]** をクリックして、**[OK]** をクリックします。

組織のユーザーが、パスワードのリセットに登録できるようになります。  このプロセスの間に、ユーザーは、システムがユーザーに電話する (または SMS メッセージを送信する) 方法がわかるように、会社の電話番号または携帯電話番号を入力します。

#### <a name="register-users-for-password-reset"></a>パスワード リセットにユーザーを登録する

1.  ユーザーは Web ブラウザーを起動して MIM パスワード リセット登録ポータルに移動します。  (通常、このポータルには Windows 認証が構成されています)。  ポータル内で、ユーザーは再びユーザー名とパスワードを入力して身元の確認を行います。

    ユーザーは、パスワード登録ポータルに入り、ユーザー名とパスワードを使用して認証する必要があります。

2.  ユーザーは、 **[電話番号]** または **[携帯電話]**  フィールドに、国コード、スペース、および電話番号を入力し、 **[次へ]**をクリックします。

    ![MIM 電話検証の画像](media/MIM-SSPR-PhoneVerification.JPG)

    ![MIM 携帯電話検証の画像](media/MIM-SSPR-mobilephoneverification.JPG)

## <a name="how-does-it-work-for-your-users"></a>ユーザーに対する動作方法
すべての構成が済んで動作したので、次に、ユーザーがパスワードを忘れたときのリセット方法を説明します。

ユーザーがパスワード リセットおよびアカウント ロック解除機能を使用する方法は、Windows サインイン画面またはセルフサービス ポータルの 2 種類です。

組織のネットワーク経由で MIM サービスに接続されていて、ドメインに参加しているコンピューターに MIM アドインと拡張機能をインストールすることにより、ユーザーはデスクトップ ログイン操作でパスワードを忘れても回復できます。  手順は以下のとおりです。

#### <a name="windows-desktop-login-integrated-password-reset"></a>Windows デスクトップ ログインに統合されたパスワード リセット

1.  ユーザーがサインイン画面で間違ったパスワードを複数回入力した場合、**[ログインできませんか?]** をクリックできます。 。

    ![サインイン画面のイメージ](media/MIM-SSPR-problemsloggingin.JPG)

    このリンクをクリックすると MIM パスワード リセット画面に移動し、そこでパスワードを変更するか、自分のアカウントのロックを解除できます。

    ![MIM パスワード リセットの画像](media/MIM-SSPR-keepcurrentorsetnewpwd.JPG)

2.  ユーザーは認証に送られます。 MFA が構成されている場合、ユーザーは電話で呼び出されます。

3.  バックグラウンドでは、ユーザーがサービスにサインアップするときに示した番号に、Azure MFA が電話をかけます。

4.  ユーザーが電話に出ると、電話の # キーを押すように求められます。 ユーザーは次にポータルの **[次へ]** をクリックします。

    他のゲートも設定されている場合、ユーザーは後続の画面で詳細情報を提供するように求められます。

    > [!NOTE]
    > ユーザーが待ちきれずに # キーを押す前に **[次へ]** をクリックすると、認証は失敗します。

5.  認証が成功した後、ユーザーには 2 つのオプションがあり、アカウントのロックを解除して現在のパスワードのままにするか、新しいパスワードを設定します。

6.  ユーザーは新しいパスワードを 2 回入力する必要があります。2 回入力すると、パスワードがリセットされます。

#### <a name="access-from-the-self-service-portal"></a>セルフサービス ポータルからのアクセス

1.  ユーザーは Web ブラウザーを開いて **パスワード リセット ポータル** に移動し、ユーザー名を入力して、 **[次へ]**をクリックします。

    MFA が構成されている場合、ユーザーは電話で呼び出されます。 バックグラウンドでは、ユーザーがサービスにサインアップするときに示した番号に、Azure MFA が電話をかけます。

    ユーザーが電話に出ると、電話の # キーを押すように求められます。 ユーザーは次にポータルの **[次へ]** をクリックします。

2.  他のゲートも設定されている場合、ユーザーは後続の画面で詳細情報を提供するように求められます。

    > [!NOTE]
    > ユーザーが待ちきれずに # キーを押す前に **[次へ]** をクリックすると、認証は失敗します。

3.  ユーザーは、パスワードのリセットまたはアカウントのロック解除を選択する必要があります。 ユーザーがアカウントのロック解除を選択した場合、アカウントのロックが解除されます。

    ![MIM ログイン アシスタント アカウント ロック解除の画像](media/MIM-SSPR-accountUnlock.JPG)

4.  認証が成功した後、ユーザーには 2 つのオプションがあり、現在のパスワードのままにするか、新しいパスワードを設定します。

5.  ![MIM アカウント ロック解除成功の画像](media/MIM-SSPR-account-unlock.JPG)

6.  ユーザーがパスワードのリセットを選択した場合、新しいパスワードを 2 回入力して **[次へ]** をクリックするとパスワードが変更されます。

    ![MIM ログイン アシスタントのパスワードのリセットの画像](media/MIM-SSPR-PR1.JPG)
