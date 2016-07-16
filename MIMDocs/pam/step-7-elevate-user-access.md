---
title: "手順 7 - ユーザーのアクセスを昇格する |Microsoft Identity Manager"
description: 
keywords: 
author: kgremban
manager: femila
ms.date: 06/16/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 9e5f51d5ca731b3564b8262db0f4cddeb850231a
ms.openlocfilehash: ee47c69788a98075372ca62943e0c4b101c5354f


---

# 手順 7 - ユーザーのアクセスを昇格する

>[!div class="step-by-step"]
[« 手順 6 ](step-6-transition-group-to-pam.md)


この手順では、MIM 経由でロールへのアクセスをユーザーが要求する方法を説明します。

## Jen が特権付きリソースにアクセスできないことを確認する
昇格された特権なしでは、Jen は CORP フォレスト内の特権付きリソースにアクセスできません。

1. CORPWKSTN からサインアウトして、キャッシュされたすべての開いている接続を削除します。
2. CORPWKSTN に *CONTOSO\Jen* でサインインして、**デスクトップ** ビューに切り替えます。
3. DOS コマンド プロンプトを開きます。
4. コマンド「`dir \\corpwkstn\corpfs`」を入力します。 エラー メッセージ "**アクセスが拒否されました**" が表示されます。
5. コマンド プロンプト ウィンドウを開いたままにしておきます。

## MIM から特権アクセスを要求します。
1. CORPWKSTN で CONTOSO\Jen のまま、次のコマンドを入力します。

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

2. PRIV.Jen アカウントのパスワードを求める画面が表示されたら、パスワードを入力します。 新しいコマンド プロンプト ウィンドウが表示されます。
3. PowerShell ウィンドウが表示されたら、次のコマンドを入力します。

    > [!NOTE] 
    > これらのコマンドを実行すると、以降のすべての手順は時間の影響を受けます。

    ```
    Import-module MIMPAM
    $r = Get-PAMRoleForRequest | ? { $_.DisplayName –eq "CorpAdmins" }
    New-PAMRequest –role $r
    klist purge
    ```

4. 完了したら、PowerShell ウィンドウを閉じます。
5. DOS コマンド ウィンドウで、次のコマンドを入力します。

    ```
    runas /user:Priv.Jen@priv.contoso.local powershell
    ```

6. PRIV.Jen アカウントのパスワードを入力します。 新しいコマンド プロンプト ウィンドウが表示されます。

## 昇格されたアクセス権を検証します。
新しく開かれたウィンドウで、次のコマンドを入力します。

```
whoami /groups
dir \\corpwkstn\corpfs
```

dir コマンドが失敗し、"**アクセスが拒否されました**" というエラー メッセージが表示された場合は、信頼関係を再度確認します。

## 特権ロールをアクティブ化する
PAM のサンプル ポータルを使用して特権アクセスを要求することで昇格します。

1. CORPWKSTN で、CORP\Jen としてサインインしていることを確認します。
2. DOS コマンド ウィンドウで、次のコマンドを入力します。

    ```
    runas /user:Priv.Jen@priv.contoso.local "c:\program files\Internet Explorer\iexplore.exe"
    ```

3. PRIV.Jen アカウントのパスワードを求める画面が表示されたら、パスワードを入力します。 新しい Web ブラウザーのウィンドウが表示されます。
4. http://pamsrv.priv.contoso.local:8090 に移動し、サンプル ポータルから Web ページが閲覧できることを確認します。
5. Internet Explorer で、**[ツール]** > **[インターネット オプション]** の順に選択し、**[セキュリティ]** タブをクリックします。
6. **[ローカル イントラネット ゾーン]** > **[サイト]** > **[詳細]** の順にクリックし、Web サイトをゾーンに追加します。
7. **[インターネット オプション]** ダイアログを閉じます。
8. 左側のタブの **[アクティブ化]**をクリックします。 **PAM ロール** を選択し、**[アクティブ化]**をクリックします。

> [!Note] 
> この環境では、PAM REST API を使用してアクティブ化するアプリケーションを開発する方法についても学習できます (「[Privileged Access Management REST API リファレンス](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference.md)」をご参照ください)。

## [概要]
このチュートリアルの手順を完了すると、ユーザー特権を限られた時間だけ昇格し、別の特権アカウントで保護されたリソースへのアクセスをユーザーに許可する、Privileged Access Managment のシナリオを実際に行ったことになります。 昇格セッションの有効期限が切れると、その特権アカウントでは保護されたリソースにアクセスできなくなります。 どのセキュリティ グループが特権ロールを持つかについての決定は、PAM 管理者が調整します。 アクセス権が Privileged Access Managment システムに移行されると、元のユーザー アカウントを使用してアクセス可能だったものが、要求によって使用可能になる特別な特権アカウントでサインインしないとアクセスできなくなります。 その結果、高い特権を持つグループのグループ メンバーが、限られた時間だけ有効になります。

>[!div class="step-by-step"]
[« 手順 6 ](step-6-transition-group-to-pam.md)



<!--HONumber=Jun16_HO5-->


