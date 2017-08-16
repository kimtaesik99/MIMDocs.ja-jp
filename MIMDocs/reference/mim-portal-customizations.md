---
title: "Microsoft Identity Manager 2016 ポータルのカスタマイズ | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: bebb299ece077a6ab2e0d75f3b30ad1776678303
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2017
---
# <a name="microsoft-identity-manager-2016-portal-customization"></a>Microsoft Identity Manager 2016 ポータルのカスタマイズ


>[!Warning]
CSS のカスタマイズを行うときは、ブラウザーのキャッシュをクリアしてください。

Microsoft Identity Manager 2016 (MIM) では、バナー ロゴ、文字列リソース、カスケード スタイル シートを含むパスワード ポータルの選択した要素をカスタマイズできます。

これを行うには、カスタマイズのレベルに応じていくつかの項目が必要です。 MIM 2016 パスワード登録およびリセット ポータルのカスタマイズに必要な項目の一覧を次に示します。

-   Customizations フォルダー - 既定値を使用する前に MIM 2016 で確認されるフォルダーです。 カスタマイズする各ポータルには、Customizations フォルダーが必要です。 セットアップでは、アップグレード、変更モードのインストール、または修復モードのインストール時にこのフォルダーを上書きしないため、カスタマイズはこのフォルダーでのみ行う必要があります。

-   Strings.resources - ポータルに表示される文字列の変更を可能にする XML ベースのファイルです。 このファイルは、Customizations フォルダーに存在する必要があります。

-   Style.css - カスタマイズの際にポータルで使用されるカスケード スタイル シートです。 ロゴを変更するには、このスタイル シートを作成して書き換える必要があります。または、独自のカスタマイズで完全に置き換えることもできます。

パスワード登録およびパスワード リセット ポータルをカスタマイズする方法の詳細な手順については、「Test Lab Guide: Demonstrating MIM 2016 Password Registration and Reset Portal Customization (テスト ラボ ガイド: MIM 2016 パスワード登録およびリセット ポータルのカスタマイズのデモンストレーション)」をご覧ください。

>[警告!] カスタマイズした変更を MIM に認識させるには、iisreset を実行して IIS を再起動する必要があります。


## <a name="creating-the-customizations-folder"></a>Customizations フォルダーの作成

MIM はスタートアップ時に、既定値を使用する前に Customizations フォルダーで Strings.resources ファイルを検索します。 カスタマイズするポータル (つまり、パスワード登録ポータルまたはパスワード リセット ポータル) のディレクトリの下に Customizations フォルダーを作成する必要があります。 両方のポータルをカスタマイズする場合は、次のそれぞれで Customizations フォルダーを作成する必要があります。

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal

-   C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Reset Portal

### <a name="to-create-the-customization-folder"></a>Customizations フォルダーを作成するには

1.  C:\\Program Files\\Microsoft Forefront Identity Manager\\2010\\Password Registration Portal フォルダーに移動します。

2.  Customizations という名前のフォルダーを作成します。

3.  1 レベル戻って Password Reset Portal フォルダーに移動し、Customizations という名前のフォルダーを作成します。

## <a name="customizing-strings"></a>文字列のカスタマイズ

ポータル UI 内の文字列の多くは、Strings.resources ファイルを作成して、Customizations フォルダーにこのファイルを追加することでカスタマイズできます。 カスタマイズする各ポータルの Strings.resources ファイルを作成する必要があります。

### <a name="to-customize-strings"></a>文字列をカスタマイズするには

1.  メモ帳を使用して、次のコードをメモ帳にコピーし、Strings.resources として Customizations フォルダーに保存します。

   ```xml
   <?xml version="1.0" encoding="utf-8"?>
   <root>
     <resheader name="resmimetype">
       <value>text/microsoft-resx</value>
     </resheader>
     <resheader name="version">
       <value>2.0</value>
     </resheader>
     <resheader name="reader">
       <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
    </resheader>
     <resheader name="writer">
       <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
     </resheader>

    <!-- Customizations begin here -->
     <data name=" QAGateResetTitle " xml:space="preserve">
       <value>Contoso Question and Answer Reset</value>
     </data>
     <data name="ResetPageTitle" xml:space="preserve">
       <value>Contoso Self-Service Password Reset</value>
     </data>
   </root>

   ```
2.  `<!-- Customizations begin here -->` セクションの下でデータ名を変更し、カスタマイズする文字列に一致させて、<value></value> タグの間にその文字列の値を入力します。 カスタマイズ可能な文字列とその既定値については、下記の一覧をご覧ください。

>[!NOTE]
**Strings.resources** ファイルは言語に依存しません。 言語固有のカスタマイズされた文字列を作成するには、言語パックをインストールしていて、そのファイルを Strings. `<language>-<culture>.resources` 形式で保存する必要があります (例: Strings.en-us.resources)。

カスタマイズ可能なポータルの文字列の一覧を次に示します。

<a name="portal-strings"></a>ポータルの文字列
--------------

| 名前                                                     | 既定値                                                                                                                                                                                                                                                                                                                                         | 備考                                                                                                                                                                                                                                            |
|---------------------------|-------------------|--------------|
| AboutLinkText                                            | の概要         |       |
| ButtonCancel                                             | キャンセル                                                                                 |     |
| ButtonFinish                                             | [完了]    |    |
| ButtonNext                                               | [次へ]    |    |
| ButtonOk                                                 | OK     |   |
| CancelFinishedMessage                                    | セッションがアクティブではなくなりました。 このウィンドウを閉じるか、下のリンクをクリックして再起動してください。         |      |
| CancelFinishedTitle                                      | セッションが終了しました                                                              |      |
| ErrorDescription_3000                                    | エラーが発生しました。 もう一度実行してください。問題が解消されない場合は、ヘルプ デスクまたはシステム管理者にお問い合わせください (エラー 3000)。       |          |
| ErrorDescription_3001                                    | ユーザー名を正しく入力したことを確認してください。それでもパスワードをリセットできない場合は、      |           |
|                                                          | ヘルプデスクにサポートを要請してください (エラー 3001)。   |                   |
| ErrorDescription_3002                                    | セッションが終了しました。 ホーム ページに戻り、もう一度開始してください (エラー 3002)。                                     |                |
| ErrorDescription_3003                                    | 現在のユーザー アカウントは Forefront Identity Manager で認識されていません。ヘルプデスクまたはシステム管理者に連絡してください (エラー 3003)。           |               |
| ErrorDescription_3004                                    | パスワードのリセットを行うための登録が許可されていません。ヘルプデスクまたはシステム管理者に連絡してください (エラー 3004)。     |              |
| ErrorDescription_3005                                    | 入力された回答のいずれかが、パスワードの登録時に指定された回答と一致しません。パスワードをリセットするには、入力された回答が登録時に指定されたものと一致する必要があります。ホーム ページからやり直すか、ヘルプデスクまたはシステム管理者に連絡してください (エラー 3005)。 |           |
| ErrorDescription_3006                                    | 入力されたパスワードが正しくありません。パスワードのリセットのための登録を行うには、正しいパスワードを入力する必要があります (エラー 3006)。            |              |
| ErrorDescription_3007                                    | パスワードのリセットが一時的に禁止されています。後で再度実行するか、ヘルプデスクまたはシステム管理者に連絡してください (エラー 3007)。     |         |
| ErrorDescription_3008                                    | エラーが発生しました。 もう一度実行してください。問題が解消されない場合は、ヘルプ デスクまたはシステム管理者にお問い合わせください (エラー 3008)。        |          |
| ErrorDescription_3009                                    | 入力内容に、入力できない形式のテキストが含まれています。 入力内容を変更してやり直すか、ヘルプ デスクまたはシステム管理者にお問い合わせください (エラー 3009)。     |             |
| ErrorDescription_3010_Registration                       | ブラウザーでスクリプトが有効になっていません。 スクリプトを有効にしてから、パスワードの登録用のホーム ページに戻ってください。または、ヘルプ デスクにサポートを要請してください。      |               |
| ErrorDescription_3010_Reset                              | ブラウザーでスクリプトが有効になっていません。 スクリプトを有効にしてから、パスワードのリセット用のホーム ページに戻ってください。または、ヘルプ デスクにサポートを要請してください。     |            |
| ErrorDescription_3011                                    | このサイトは Cookie を使用しています。 ブラウザーを Cookie を受け入れるように構成してから再度実行するか、ヘルプ デスクにサポートを要請してください。          |                                           |
| ErrorDescription_3012                                    | 入力されたデータが、お送りしたセキュリティ コードと一致しませんでした。 パスワードのリセットを再試行するか、ヘルプ デスクにサポートを要請してください。      |                                                                                                                                                                                                                                                    |
| ErrorDescription_3013                                    | セキュリティ コードを送信できません。 ヘルプ デスクにサポートを要請してください。                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameFormat                         | ユーザー名を正しい形式で入力してください。                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                    |
| ErrorMessageDomainUsernameRequired                       | 続けるには、ユーザー名を入力してください。                                              |                         |
| ErrorMessagePasswordRequired                             | パスワードを入力してください。              |                                                |
| ErrorMessagePasswordsDoNotMatch                          | 両方のパスワードが一致する必要があります。                                |           |
| ErrorPageDefaultHeading                                  | アプリケーション エラー                                            |               |
| ErrorPageServerTime                                      | サーバー時刻: {0:T}                     | {0} は、例外が発生した時刻です。 'T' は、渡された時刻を示し、"長い形式の時刻" として書式設定されます。 これで時間、分、秒 (および場合によっては午前/午後の指定) が表示されます (現在のカルチャによって異なります)。 |
| ErrorPageTitle                                           | Forefront Identity Management - パスワード エラー                     |   |
| ErrorTitle_3000                                          | エラー                                  |  |
| ErrorTitle_3001                                          | アクセス拒否                          |  |
| ErrorTitle_3002                                          | セッションが終了しました                          |  |
| ErrorTitle_3003                                          | 不明なユーザー                      |  |
| ErrorTitle_3004                                          | 承認されていないユーザー                      |  |
| ErrorTitle_3005                                          | 回答が一致しません                    |  |
| ErrorTitle_3006                                          | パスコードが正しくありません                     |  |  
| ErrorTitle_3007                                          | アクセスが一時的に拒否されています              |  |
| ErrorTitle_3008                                          | 通信エラー                    |  |
| ErrorTitle_3009                                          | 入力不可項目                       |  |
| ErrorTitle_3010                                          | ブラウザーの構成エラー            |  |
| ErrorTitle_3011                                          | ブラウザーの構成エラー            |  |
| ErrorTitle_3012                                          | 検証に失敗しました                    |  |
| ErrorTitle_3013                                          | セキュリティ コードを送信できません   |  |
| FinalizeRegistrationHeading1                             | パスワードをリセットする必要がある場合:        |   |
| FinalizeRegistrationSubHeading1                          | パスワード リセット ポータルにアクセスします   |   |
| FinalizeRegistrationSubHeading2                          | ID を検証します   |   |
| FinalizeRegistrationSubHeading3                          | 新しいパスワードの選択    |     |
| FinishingDescription                                     | 新しいパスワードの選択        |    |
| FinishingTitle                                           | パスワードのリセット:      |     |
| GotoPortalPrefix                                         | 移動     |        |
| GotoPortalSuffix                                         | ホーム ページ     |      |
| LabelTroubleshootingLinkText                             | [詳細の表示]           |    |
| LoadingText                                              | 読み込み中...     |  |
| NoScriptTagErrorMessage                                  | ブラウザーでスクリプトが有効になっていません。 スクリプトを有効にしてから、ホーム ページに戻ってください。または、ヘルプ デスクにサポートを要請してください。      |   |
| PasswordResetOperationGeneralErrorMessage                | パスワードのリセット中にエラーが発生しました。       |   |
| PasswordResetOperationPolicyViolationErrorMessage            | パスワードが組織のパスワード ポリシーに従っていません。             |       |
| PasswordResetOperationUserCantChangePasswordErrorMessage | パスワードのリセット中にエラーが発生し、ユーザーはパスワードを変更できません。    |   |
| PrivacyStatement                                         | プライバシーに関する声明                                                      |    |
| RegistrationDescription                                  | セルフサービスによるパスワード登録     |    |
| RegistrationMission                                      | パスワードを忘れた場合は、ヘルプデスクに連絡しなくても自分でパスワードをリセットできます。   |      |
| RegistrationPageTitle                                    | Forefront Identity Management - パスワード登録                                          |   |
| RegistrationSteps                                        | '[次へ]' をクリックして、登録プロセスを開始します。   |      |
| RegistrationSuccessDescription                           | 登録されていません        |   |
| RegistrationSuccessTitle                                 | 完了:   |    |
| RegistrationWelcomeTitle                                 | パスワード登録:    | |
| ResetDescription                                         | セルフサービスのパスワード リセット  |    |
| ResetEnterNamePrompt                                     | 下にユーザー名を入力してください     |     |
| ResetEnterPassword                                       | 新しいパスワードを入力します:                                                  |   |
| ResetExample1                                            | contoso\\mmeyers                                                                            |      |
| ResetExample2                                            | mmeyers\@contoso.com     |      |
| ResetExamples                                            | 例:  |    |
| ResetPageTitle                                           | Forefront Identity Management - パスワードのリセット       |     |
| ResetReenterPassword                                     | パスワードをもう一度入力します:    | |
| ResetSuccessDescription                                  | パスワードがリセットされました    |    |
| ResetSuccessTitle                                        | 成功しました:                                |    |
| ResetUseNewPassword                                      | 以降は、新しいパスワードでログインできます。     |      |
| ResetUsernameTextFormat                                  | ({0} のパスワードをリセットします)       | {0} はユーザーのログオン名です。             |
| ResetWelcomeTitle                                        | パスワードのリセット:     |      |
| TroubleshootingEmailSubject                              | FIM 要求処理エラーの詳細     |       |
| TroubleshootingLabelAttributes                           | 属性:    |    |
| TroubleshootingLabelCloseButton                          | [閉じる]       |    |
| TroubleshootingLabelCopyToClipboard                      | クリップボードにコピー     |     |
| TroubleshootingLabelCorrelationId                        | 相関 ID:      |          |
| TroubleshootingLabelDetails                              | 詳細:                                                             |    |
| TroubleshootingLabelPostCopyClipboardMessage             | 情報がクリップボードにコピーされました。           |    |
| TroubleshootingLabelRequestId                            | 要求 ID:                  |    |
| TroubleshootingLabelSendEmail                            | 情報を電子メールで送信                            |    |
| TroubleshootingLabelSource                               | 理由:                                                                     |    |
| TroubleshootingLabelViewRequestDetails                   | 要求の詳細を表示        |              |                                                    
| TroubleshootingLinkText                                  | トラブルシューティング情報          |                  | |



カスタマイズ可能な認証ゲートの文字列の一覧を次に示します。

<a name="authentication-gate-strings"></a>認証ゲート文字列
---------------------------

| 名前                                          | 既定値                                                                                                                                                                                                                | コメント |
|-----------|--------------|----------|
| OTPEmailRegistraionEmailTextboxLabel          | [電子メール アドレス:]             |          |
| OTPEmailRegistrationEmailRequiredErrorMessage | 電子メール アドレス フィールドは空欄にできません。     |          |
| OTPEmailRegistrationFooterReadOnly            | 電子メール アドレスを更新するには、所属組織で規定されている手続きに従うか、ヘルプ デスクにお問い合わせください。                   |          |
| OTPEmailRegistrationFooterReadWrite           | 電子メール アドレスは組織の Forefront Identity Manager に保存されます。                       |          |
| OTPEmailRegistrationGateTitle                 | 電子メール アドレスの検証   |          |
| OTPEmailRegistrationHeaderReadOnly            | パスワードをリセットする必要が生じた場合は、登録されている電子メール アドレス宛てに、確認用のセキュリティ コードが送信されます。 下に表示されている電子メール アドレスが正しくない場合、セルフサービス パスワード リセット機能を使用するには、アドレス情報を更新いただく必要があります。 |          |
| OTPEmailRegistrationHeaderReadWrite           | 下に電子メール アドレスを入力してください。 パスワードをリセットする必要が生じた場合は、登録されている電子メール アドレス宛てに確認用のコードが送信されます。                 |          |
| OTPEmailResetGateTitle                        | ID の検証: 電子メールの検証         |          |
| OTPEmailResetHeader                           | 下にセキュリティ コードを入力してください。 組織に登録されている電子メール アドレス宛てに、セキュリティ コードを事前にお送りしています。                                                                                                             |          |
| OTPRegularExpressionErrorMessage              | 指定された値は所定の形式に合致していません。                   |          |
| OTPResetOneTimePasswordRequiredErrorMessage   | セキュリティ コード フィールドは空欄にできません。        |          |
| OTPResetVerificationLabel                     | セキュリティ コード:                    |          |
| OTPSmsRegistrationFooterReadOnly                      | 携帯電話番号を更新するには、所属組織で規定されている手続きに従うか、ヘルプ デスクにお問い合わせください。   ||
| OTPSmsRegistrationFooterReadWrite                     | 携帯電話番号は組織の Forefront Identity Manager に保存されます。                                                                                                                                                     |   |
| OTPSmsRegistrationGateTitle                           | 携帯電話の検証                      |   |
| OTPSmsRegistrationHeaderReadOnly                      | パスワードをリセットする必要が生じた場合は、ご利用の携帯電話宛てに、確認用のセキュリティ コードが送信されます。 下に表示されている携帯電話番号が正しくない場合、セルフサービス パスワード リセット機能を使用するには、電話番号を更新いただく必要があります。 |   |
| OTPSmsRegistrationHeaderReadWrite                     | 下に携帯電話番号を入力してください。 パスワードをリセットする必要が生じた場合は、ご利用の携帯電話宛てに確認用のコードが送信されます。                                                                                                     |   |
| OTPSmsRegistrationMobilePhoneRequiredErrorMessage     | 携帯電話番号フィールドは空欄にできません。      |   |
| OTPSmsRegistrationSMSTextBoxLabel                     | 携帯電話:                    |   |
| OTPSmsResetGateTitle                                  | ID の検証: 携帯電話の検証         |   |
| OTPSmsResetHeader                                     | 下にセキュリティ コードを入力してください。 組織に登録されている携帯電話宛てに、セキュリティ コードを事前にお送りしています。                                                                                                                           |   |
| PasswordGateDescriptionText                           | 下に現在のパスワードを入力し、'[次へ]' をクリックします。        |   |
| PasswordGateErrorMessagePasswordRequired              | 現在のパスワードを入力します。                  |   |
| PasswordGateGateTitle                                 | 現在のパスワード               |   |
| PasswordGatePasswordLabelText                         | ［パスワード］:                 |   |
| PasswordGateUsernameTextFormat                        | `<i>`(`<b>{0}</b>)</i>` としてログイン中)                                                          |   |
| QAGateErrorNotEnoughQuestionsAnswered                 | 少なくとも {0} 個の質問に答える必要があります。        |   |
| QAGateIncorrectAnswer                                 | 回答が正しくありません。       |   |
| QAGatePrivacyNotice                                   | 入力した応答は組織の Forefront Identity Manager に保存されます。                                                                                                                                                  |   |
| QAGateRegistrationNumberOfQuestionsExplanation_Format | 登録するには、少なくとも {0} 個の質問に答える必要があります。     |   |
| QAGateRegistrationOneOrMoreAnswersFailedValidation    | 1 つ以上の回答がポリシーに準拠していません。      |   |
| QAGateRegistrationThisAnswerValidationFailed          | この回答はポリシーに準拠していません。      |   |
| QAGateRegistrationTitle                               | 回答の登録         |   |
| QAGateResetNumberOfQuestionsExplanation_Format        | 次の {1} 個の質問のうち、{0} 個に答える必要があります。   |   |
| QAGateResetTitle                                      | ID の検証: 回答の送信                                  |   |



## <a name="customizing-the-logo-banner"></a>ロゴ バナーのカスタマイズ

ポータル ページの既定のバナーを組織用にカスタマイズできます。
ロゴ バナーをカスタマイズするには:

1.  カスタム バナーを作成し、.png ファイルとして保存します。 ファイルは、次の推奨事項を満たす必要があります。

 - サイズ: 490 X 50 ピクセル

 - ビットの深度: 32

2.  カスタマイズする各ポータルの \\Customizations フォルダーにファイルをコピーします。

3.  各フォルダーで Style.css ファイルを作成します。 ファイルが Customizations フォルダーと新しいロゴを指すようにします。 必要に応じて、ロゴの名前を変更できます (つまり /Customizations/contosologo.png)。 このコードは次のようになります。

   **例 1:**

  `.title-block{background:url(../Customizations/fimlogo.png) no-repeat scroll 0 0 transparent;}`

4.  Internet Explorer 6.0 を使用している場合は、代替の非透過的なロゴを指定して、Style.css に次のコードを追加する必要があります。

  `.ie6 .title-block{background-image:url(../Customizations/fimlogo-ie6.png);}`

  **例 2:**

  `.title-block{background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;}`

Internet Explorer 6.0 を使用している場合は、代替の非透過的なロゴを指定して、Style.css に次のコードを追加する必要があります。

  `.ie6 .title-block{background-image:url(../Customizations/contosologo-ie6.png);}`

## <a name="customizing-image-for-smartphones"></a>スマート フォンのイメージのカスタマイズ

スマート フォンのイメージをカスタマイズするには、次の方法を使用します。 スマート フォンのイメージをカスタマイズするには:

1.  イメージを作成し、.png ファイルとして保存します。 ファイルは、次の推奨事項を満たす必要があります。

    - サイズ: 190 X 50 ピクセル
    - ビットの深度: 32

2.  カスタマイズする各ポータルの \\Customizations フォルダーにファイルをコピーします。

2.  各フォルダーで Style.css ファイルを作成します。 ファイルが Customizations フォルダーと新しいロゴを指すようにします。 必要に応じて、ロゴの名前を変更できます (つまり /Customizations/contosologo.png)。 このコードは次のようになります。

  **例 1:**

```css
@media only screen and (max-width: 480px)

   {

    .title-block

     {
       background: url("path_to_image/imagename.png") no-repeat scroll 0 0 transparent;
     }

   }
   ```

## <a name="customizing-style-sheets"></a>スタイル シートのカスタマイズ

カスタマイズしたカスケード スタイル シート (CSS) を使用して、パスワード ポータルのレイアウトとスタイルを変更できます。 カスタマイズした CSS を使用するには:

1.  カスタマイズした CSS ファイルを作成して、Style.css として保存します。

2.  カスタマイズする各ポータルの \\Customizations フォルダーにファイルをコピーします。

Style.css ファイルの基本的な例を次に示します。

```css
body
{
  font: 15px Algerian;
  color: \#303030;
  background: white;
}

.pad
{
  padding: 30px;
  padding-top: 50px;
  background: white;
}

.backgroundWhite
{
  border: \#e9e9e9 2px solid;
} .
title-block
{
background:url(../Customizations/contosologo.png) no-repeat scroll 0 0 transparent;
}
```


>[!IMPORTANT]
カスタマイズした変更を MIM に認識させるには、iisreset を実行して IIS を再起動する必要があります。                                                                                                                                                                                       

Style.css ファイルのより高度な例を次に示します。 このファイルは、スマート フォンや iPad でポータルを表示するための、それらのデバイスに固有の情報を提供します。

```css
/****************
BASE
*****************/

body {
    font-size: 14px; /*Customizeable- Body Font Size */
    background-color: #ced5ec; /*Customizeable- Backgound Color behind the product */
}

body, button, input, select, textarea {
    font-family: Segoe UI, Arial, Verdana, Sans-Serif, Helvetica; /*Customizeable- Body Font Family */
    color: #595959; /*Customizeable- Body Font Color */
}

/****************
LINKS
*****************/

a { color: #396faf; text-decoration: none; } /*Customizeable- Link Color and Underline */
a:visited { color: #396faf; text-decoration: none; } /*Customizeable- After Link is clicked color and underline */
a:hover { color: #6486ae; text-decoration: none; } /*Customizeable- Hover mouse over Link color and underline */
a:focus { outline: thin dotted; } /*Customizeable- Keyboard event to Link and Link is in focus outline*/
a:hover, a:active { outline: 0; } /*Customizeable- Hover and Active Link outline */

/****************
Typography
*****************/

hr { border-top: 1px solid #acd9ec; } /*Customizeable- Horizontal Rule Color Above the Footer */

/****************
Layout
*****************/
#wrapper {
    background: url(../images/bg-top-slice.png) repeat-x 0 0; /*Customizeable-remove this line to remove top gradient */
}

#container {
    background: url(../images/bg-bottom-slice.png) repeat-x 100% 100% transparent;  /*Customizeable-remove this line to remove bottom gradient */
}

.title-block {
    background: url("../images/fimlogo.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 600px or less in width. Logo must be 50px or less in height. */
    border-bottom: 2px solid #acd9ec;/*Customizeable- 2px border color under logo */
}

.ie6 .title-block {
    background-image: url(../images/fimlogo-ie6.png);   /*Customizeable- Can make a non-transparent image for IE6 only */
}

h2 {
    color: #578e4c; /*Customizeable- h2 page header color */
}

h3 {
    color: #999; /*Customizeable- h3 page header color */
}

input[type=text]:focus, input[type=password]:focus {
    border: #82bd3b 2px solid; /*Customizeable- Highlight color around textbox when cursor is inside */
}

.chromeButton, .chromeButton:visited {
    background-color: #333; /*Customizeable- Color of button */
    color: #fff; /*Customizeable- Color of text on the button */
    border: 1px solid #666; /*Customizeable- Border color of button */
}

.chromeButton:hover {
    background-color: #666; /*Customizeable- Hover color of button */
    border: 1px solid #999; /*Customizeable- Hover border color of button */
}

.qcol /*Style from QAgate.css */ {
    color: #7a7a7a; /*Customizeable- Font color of Q&A container */
    background-color:#e6e7e9; /*Customizeable- Background color of Q&A container */
}

/****************
Media Queries
*****************/

/* Smartphones ----------- */
@media only screen and (max-width: 480px) {
    body {
        font-size:12px; /*Customizeable- Body Font Size for devices */
    }

    .title-block {
        background: url("../images/fim-logo-portrait.png") no-repeat scroll 0 0 transparent;  /*Customizeable- Logo must be 190px (landscape) or less in width. Logo must be 50px or less in height. */
    }
    h2, h3 {
        font-size:14px; /*Customizeable- H2 and H3 Heading Size for devices */
    }
}


/* iPads (landscape) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : landscape)
{
}

/* iPads (portrait) ----------- */
@media only screen and (min-device-width : 768px) and
(max-device-width : 1024px) and
(orientation : portrait)
{
}
```


## <a name="common-customization-issues"></a>カスタマイズの一般的な問題

次の表は、FIM サービスおよびポータルのアップグレードで発生する可能性がある、既知の一般的な問題の一覧を示します。 次の表には、それらの問題の解決策も含まれています。

|問題 |解決方法                                                                    |
|------|------------------------------|
| 文字列のカスタマイズを行いましたが、カスタマイズが UI に反映されません。         | Strings.resources での文字列のカスタマイズでは、常に iisreset を必要とします。         |
| Strings.resources の変更を行った後、文字列の変更がまったく表示されなくなりました。     | Strings.resources の形式が正しくない可能性があります。そのため、ポータルで無視されています。 [Windows ログ] – [アプリケーションとサービス ログ] – [Forefront Identity Manager] でイベント ログを確認します。                        |
| 初めて Style.css を追加しましたが、ポータルでスタイルの変更を確認できません。            | Style.css ファイルを初めて導入する場合、iisreset を実行する必要があります。     |
| 新しいスタイルを Style.css で追加/変更しましたが、変更をブラウザーで確認できません。      | ブラウザー キャッシュをクリアして、ページを最新の情報に更新します。 <br/> CSS の構文を確認します。    |
| path_to_sspr_portal\\css\\\*.css の CSS フォルダーの内容または path_to_sspr_portal\\images\\fimlogo.png のバナー ロゴを直接変更しましたが、アップグレード時にこれらの変更が失われました。 | 使用を開始するにあたって、これらのファイルを変更する必要はありません。 Style.css でバナー ロゴおよび CSS スタイルのカスタマイズを指定する手段としては、Customizations フォルダーのみを使用します。 Customizations フォルダーは、メジャー アップグレードによって意図的に上書きされません。 <br/><br/>ILSpy や Reflector などのツールを使用してポータルのアセンブリ内の文字列を変更しないでください。 既定の文字列を上書きするには、Strings.resources を使用してください。 アセンブリはアップグレード時に置換されます。  |
| バナー ロゴがポータルに表示されません。FIM ロゴが引き続き表示されます。     | Style.css でのイメージの名前/パスが無効であるか、ブラウザーのキャッシュがクリアされていません。          |
| バナー ロゴが IE6 で見づらくなります。       | IE6 の場合、非透過的なイメージと、関連する特殊なスタイルを style.css で指定する必要があります。        |
