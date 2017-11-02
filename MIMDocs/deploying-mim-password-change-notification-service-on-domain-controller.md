---
title: "パスワード変更通知サービスの展開 | Microsoft Docs"
description: "ドメイン コントローラーに MIM パスワード変更通知サービスをインストールして構成する手順を説明します。"
keywords: 
author: billmath
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6ce6f8b78d7ea3518bd5d4beeada51cbc3fdc5a3
ms.sourcegitcommit: f077508b5569e2a96084267879c5b6551e1e0905
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/12/2017
---
# <a name="deploy-the-mim-password-change-notification-service-on-a-domain-controller"></a>ドメイン コントローラーに MIM パスワード変更通知サービスを展開する

## <a name="install-the-password-change-notification-service"></a>パスワード変更通知サービスをインストールする
パスワード変更通知サービス (PCNS) をドメイン コントローラーにインストールすると、MIM は、別のベンダーのディレクトリ サーバーなどの他のシステムにパスワードを同期できます。 パスワード同期のためには、各ドメイン コントローラー サーバーに PCNS をインストールします。

1.  Active Directory ドメイン サービスの役割を持つ Windows Server 搭載サーバーに、ドメイン管理者としてログインします。

2.  パスワード変更通知サービスのセットアップ フォルダーをコンピューターにコピーします。

3.  *Password Change Notification Service.msi* ファイルを探し、右クリックして、ショートカットを作成します。

4.  ショートカット ファイルを右クリックして、 **[プロパティ]**を表示します。

5.  [ターゲット] フィールドで、msi ファイルへのパスの前に *msiexec.exe /i* を追加し、後に *SCHEMAONLY=TRUE* を追加します。 たとえば、セットアップ フォルダーが *C:\PCNS* の場合、実行するコマンドは次のようになります (全体が 1 行です)。

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  変更をショートカット ファイルに保存します。

7.  ショートカット ファイルを実行し、スキーマ拡張モードで PCNS のインストールを開始します。 次の画面が表示されたら、 **[次へ]**をクリックします。

8.  セットアップによってパスワード変更通知サービス用に Active Directory スキーマが更新されることが通知されます。 **[OK]** をクリックしてスキーマの更新を続行します。

9. スキーマ拡張処理が完了し、次の画面が表示されたら、 **[完了]**をクリックします。

10. *Password Change Notification Service.msi* ファイルを再び、今度は直接実行します (run 文字列は必要ありません)。  次の画面が表示されたら、 **[次へ]**をクリックします。

11. 使用許諾契約書に同意し、 **[次へ]**をクリックします。

12. クリックしてインストールを開始します。

13. インストールの正常終了画面が表示されたら、 **[完了]**をクリックします。

14. コンピューターを再起動し、MIM パスワード変更通知サービスに対する構成の変更を有効にします。 表示されるポップアップ ウィンドウで **[はい]** をクリックしても、後で再起動してもかまいません。

## <a name="configuring-the-password-change-notification-service"></a>パスワード変更通知サービスを構成する
ドメイン管理者として DC サーバーに再接続した後、*C:\Program Files\Microsoft Password Change Notification* に移動します。 *pcnscfg.exe* を実行します。
