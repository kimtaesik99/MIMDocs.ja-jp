---
title: "PAM の展開、手順 7 – ユーザー アクセス | Microsoft Docs"
description: "最後の手順として、Privileged Access Management の展開が成功したことを確認できるように、特権を持つユーザーに一時的なアクセス権を付与します。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 5325fce2-ae35-45b0-9c1a-ad8b592fcd07
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 89d9b38177b91f64e746fea583684abcecc9d7ff
ms.lasthandoff: 05/02/2017


---

# <a name="step-7--elevate-a-users-access"></a>手順 7 - ユーザーのアクセスを昇格する

>[!div class="step-by-step"]
[« 手順 6](step-6-transition-group-to-pam.md)


この手順では、MIM 経由でロールへのアクセスをユーザーが要求する方法を説明します。

## <a name="verify-that-jen-cannot-access-the-privileged-resource"></a>Jen が特権付きリソースにアクセスできないことを確認する
昇格された特権なしでは、Jen は CORP フォレスト内の特権付きリソースにアクセスできません。

1. CORPWKSTN からサインアウトして、キャッシュされたすべての開いている接続を削除します。
2. CORPWKSTN に *CONTOSO\Jen* でサインインして、**デスクトップ** ビューに切り替えます。
3. DOS コマンド プロンプトを開きます。
4. コマンド「`dir \\corpwkstn\corpfs`」を入力します。 エラー メッセージ "**アクセスが拒否されました**" が表示されます。
5. コマンド プロンプト ウィンドウを開いたままにしておきます。

## <a name="request-privileged-access-from-mim"></a>MIM から特権アクセスを要求します。
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

## <a name="validate-the-elevated-access"></a>昇格されたアクセス権を検証します。
新しく開かれたウィンドウで、次のコマンドを入力します。

```
whoami /groups
dir \\corpwkstn\corpfs
```

dir コマンドが失敗し、"**アクセスが拒否されました**" というエラー メッセージが表示された場合は、信頼関係を再度確認します。

## <a name="activate-the-privileged-role"></a>特権ロールをアクティブ化する
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
> この環境では、PAM REST API を使用するアプリケーションを開発する方法も学習することができます (「[Privileged Access Management REST API リファレンス](/microsoft-identity-manager/reference/privileged-access-management-rest-api-reference)」をご参照ください)。

## <a name="summary"></a>[概要]
このチュートリアルの手順を完了すると、ユーザー特権を限られた時間だけ昇格し、別の特権アカウントで保護されたリソースへのアクセスをユーザーに許可する、Privileged Access Managment のシナリオを実際に行ったことになります。 昇格セッションの有効期限が切れると、その特権アカウントでは保護されたリソースにアクセスできなくなります。 どのセキュリティ グループが特権ロールを持つかについての決定は、PAM 管理者が調整します。 アクセス権が Privileged Access Managment システムに移行されると、元のユーザー アカウントを使用してアクセス可能だったものが、要求によって使用可能になる特別な特権アカウントでサインインしないとアクセスできなくなります。 その結果、高い特権を持つグループのグループ メンバーが、限られた時間だけ有効になります。

>[!div class="step-by-step"]
[« 手順 6](step-6-transition-group-to-pam.md)

