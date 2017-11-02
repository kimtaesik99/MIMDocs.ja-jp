---
title: "Microsoft Identity Manager サービスおよびポータルのインストール | Microsoft Docs"
description: "MIM サービスおよび Microsoft Identity Manager 2016 用のポータルをインストールする手順を説明します。"
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldiwn
ms.date: 10/12/2017
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 77ceaf1b2152a6fa6e1047656bedda31ce383871
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2017
---
# <a name="install-mim-2016-mim-service-and-portal"></a>MIM 2016: MIM サービスおよびポータルのインストール

>[!div class="step-by-step"]
[« MIM 同期サービス](install-mim-sync.md)
[データベースを同期する »](install-mim-sync-ad-service.md)

> [!NOTE]
> このチュートリアルでは、"Contoso" という架空の会社の名前と値を使用します。 これらは独自の値に置き換えてください。 たとえば、
> - ドメイン コントローラー名 - **mimservername**
> - ドメイン名 - **contoso**
> - パスワード - **Pass@word1**
> - サービス アカウント名 - **MIMService**

最後の手順で MIM インストール パッケージをセットアップしなかった場合は、Microsoft Identity Manager 2016 コンポーネントをインストールしてから次の手順に進みます。


## <a name="configure-mim-service-and-portal-for-installation"></a>インストールするために MIM サービスおよびポータルを構成する

1. アンパックした **サービスおよびポータル** サブフォルダから **MIM サービスおよびポータルのインストーラー**を実行します。

2. ウェルカム画面で、 **[次へ]**をクリックします。

3. 使用許諾契約書を読み、ライセンス条件に同意する場合は **[次へ]** をクリックします。

4. **[MIM カスタマー エクスペリエンス向上プログラム]** 画面で、**[次へ]** をクリックします。

5. この展開にコンポーネント機能を選択する際に、MIM サービス (MIM レポートは例外) と MIM ポータル機能を含める必要があります。 MIM パスワード リセット ポータルと MIM パスワード変更通知サービスを選択することもできます。

6. **[MIM データベース接続の構成]** ページで、**[新しいデータベースを作成]** を選択します。

    ![MIM データベース接続イメージを構成する](media/MIM-Install10.png)

7. **[メール サーバーの接続の構成]** で、**[メール サーバー]** に Exchange サーバーの名前を入力します。 メール サーバーが構成されていない場合は、メール サーバー名として**localhost**を使用し、一番上の 2 つのチェック ボックスをオフにします。 **[Next]** をクリックします。

    ![メール サーバーの接続の構成の画像](media/MIM-Install11.png)

8. 新しい自己署名証明書を生成するか、適切な証明書を選択するかを指定します。

9. 使用するサービス アカウント名 (例: *MIMService*)、サービス アカウント パスワード (例: *Pass@word1*)、サービス アカウント ドメイン (例: *contoso*)、サービス電子メール アカウント (例: *contoso*) を指定します。

    ![MIMサービス アカウント イメージを構成する](media/MIM-Install12.png)

10. 現在の構成ではサービス アカウントがセキュリティ保護されていないことを警告するメッセージが表示されます。

11. 同期サーバーの場所の既定値をそのまま使用し、MIM 管理エージェント アカウントに *contoso\MIMMA* を指定します。

    ![MIM サービスおよびポータル イメージを構成する](media/MIM-Install13.png)

12. MIM ポータルの MIM サービス サーバー アドレスとして、*CORPIDM* (このコンピューターの名前) を指定します。

13. SharePoint サイト コレクション URL に *http://CorpIDM.contoso.local* を指定します。

14. パスワード登録 URL に *http://CorpIDM.contoso.local:8080* を指定します。

15. パスワード リセット URL に *http://CorpIDM.contoso.local:8088* を指定します。

16. ファイアウォールのポート 5725 および 5726 を開くためのチェック ボックスをオンにし、さらに、すべての認証されたユーザーに MIM ポータルへのアクセス権を付与するためのチェック ボックスを選択します。

## <a name="configure-mim-password-registration-portal"></a>MIM パスワード登録ポータルを構成する

1.  SSPR 登録用のサービス アカウント名を *contoso\MIMSSPR* に、そのパスワードを *Pass@word1* に設定します。

2.  MIM パスワード登録のホスト名として *CORPIDM* を指定し、ポートを **8080** に設定します。 **[ファイアウォールでポートを開く]** オプションを有効にします。

    ![IIS で使用される構成情報を入力する画像](media/MIM-Install14.png)

3.  警告が表示されます。確認して **[次へ]**をクリックします。

4. 次の MIM パスワード登録ポータルの構成画面で、パスワード登録ポータルの [MIM サービス サーバーのアドレス] に *http://CorpIDM.contoso.local* を指定します。

## <a name="configure-mim-password-reset-portal"></a>MIM パスワード リセット ポータルを構成する

1.  SSPR 登録用のサービス アカウント名を *Contoso\MIMSSPRService* に、そのパスワードを *Pass@word1* に設定します。

2.  MIM パスワード リセット ポータルのホスト名として *CORPIDM* を指定し、ポートを **8088** に設定します。 **[ファイアウォールでポートを開く]** オプションを有効にします。

    ![IIS で使用される構成情報を入力する画像](media/MIM-Install15.png)

3.  警告が表示されます。確認して **[次へ]**をクリックします。

4. 次の MIM パスワード登録ポータルの構成画面で、パスワード リセット ポータルの [MIM サービス サーバーのアドレス] に *CorpIDname  http://CorpIDname.domain.local* を指定します。

## <a name="install-mim-service-and-portal"></a>MIM サービスおよびポータルのインストール

インストール前のすべての定義が準備できたら、**[インストール]** をクリックして、選択した**サービスおよびポータル** コンポーネントのインストールを開始します。

インストールが完了したら、MIM ポータルがアクティブであることを確認します。

1. Internet Explorer を起動し、*http://corpidm.contoso.local/identitymanagement* 上の MIM ポータルに接続します。 このページに初めてアクセスする場合、少し時間がかかることがあります。

    - 必要に応じて、Internet Explorer に対して *contoso\Administrator* として認証します。

2. Internet Explorer で、 **[インターネット オプション]**を開き、 **[セキュリティ]** タブに切り替えます。 **[ローカル イントラネット]** ゾーンに該当するサイトがまだ存在しない場合は、そのサイトを追加します。  **[インターネット オプション]** ダイアログを閉じます。

3. MIM でユーザーが独自のエントリを表示できるようにします。

    1.  Internet Explorer を使用し、 **MIM ポータル**で、 **[管理ポリシー規則]**をクリックします。

    2.  管理ポリシー規則の **[ユーザー管理: ユーザーは自分の属性を読み取ることができる]**を検索します。

    3.  この管理ポリシー規則を選択し、**[ポリシーを無効にする]** をオフにします。

    4.  **[OK]** をクリックし、 **[送信]**をクリックします。

4.  ファイアウォールが TCP ポート 5725 と 5726 との受信接続を許可していることを確認します。

    1.  **[管理ツール]** を起動し、**セキュリティが強化された Windows ファイアウォール**を選択します。

    2.  **[受信の規則]**をクリックします。

    3.  次の 2 つの規則が表示されていることを確認します。

        -   Forefront Identity Manager サービス (STS)。

        -   Forefront Identity Manager サービス (Webservice)。

    4.  ウィザードを完了し、 **Windows ファイアウォール** アプリケーションを閉じます。

    5.  **[コントロール パネル] を起動し、[ネットワークとインターネット] をクリックし、[ネットワークの状態とタスクの表示]** をクリックします。

    6.  ドメイン ネットワークとして contoso.local と表示されたアクティブなネットワークがあることを確認します。

    7.  **[コントロール パネル]**を閉じます。

> [!NOTE]
> 省略可能: この時点で MIM アドインおよび拡張機能をインストールできます。

>[!div class="step-by-step"]  
[« MIM 同期サービス](install-mim-sync.md)
[データベースを同期する »](install-mim-sync-ad-service.md)
