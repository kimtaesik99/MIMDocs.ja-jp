---
title: MIM パスワード変更通知サービス (PCNS) のドメイン コント ローラーを展開する |Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 97edae12-6f86-4f9f-8620-a95a096e482a
author: kgremban
---
# ドメイン コント ローラーへの MIM パスワード変更通知サービス (PCNS) の展開

## PCNS のインストール
PCNS サービスをドメイン コントローラーにインストールすると、MIM は、別のベンダーのディレクトリ サーバーなどの他のシステムにパスワードを同期できます。 パスワード同期のためには、各ドメイン コントローラー サーバーに PCNS をインストールします。

1.  Active Directory ドメイン サービスの役割を持つ Windows Server 搭載サーバーに、ドメイン管理者としてログインします。

2.  パスワード変更通知サービスのセットアップ フォルダーをコンピューターにコピーします。

3.   *Password Change Notification Service.msi* ファイルを探し、右クリックして、ショートカットを作成します。

4.  ショートカット ファイルを右クリックして、 **[プロパティ]**を表示します。

5.  対象のフィールドで、preamble を追加 *msiexec.exe/i* msi ファイルと指定したサフィックスへのパスの前に *SCHEMAONLY = TRUE* msi パスの後です。 たとえば、セットアップ フォルダーが *C:\PCNS* を実行するコマンドは次のようになります: (すべてで 1 行)。

    ```
    msiexec.exe /i "C:\PCNS\x64\Password Change Notification Service.msi" SCHEMAONLY=TRUE
    ```

6.  変更をショートカット ファイルに保存します。

7.  ショートカット ファイルを実行し、スキーマ拡張モードで PCNS のインストールを開始します。 次の画面が表示されたら、 **[次へ]**をクリックします。

8.  セットアップによってパスワード変更通知サービス用に Active Directory スキーマが更新されることが通知されます。  **[OK]** をクリックしてスキーマの更新を続行します。

9. スキーマ拡張処理が完了し、次の画面が表示されたら、 **[完了]**をクリックします。

10.  *Password Change Notification Service.msi* ファイルを再び、今度は直接実行します (run 文字列は必要ありません)。  次の画面が表示されたら、 **[次へ]**をクリックします。

11. 使用許諾契約書に同意し、 **[次へ]**をクリックします。

12. クリックしてインストールを開始します。

13. インストールの正常終了画面が表示されたら、 **[完了]**をクリックします。

14. コンピューターを再起動し、MIM パスワード変更通知サービスに対する構成の変更を有効にします。 表示されるポップアップ画面で **[はい]** をクリックしても、後で再起動してもかまいません。

## PCNS の構成
ドメイン管理者として DC サーバーに再接続した後に移動 *C:\Program \microsoft Password Change Notification です。* 実行 *pcnscfg.exe*します。


<!--HONumber=Mar16_HO3-->


