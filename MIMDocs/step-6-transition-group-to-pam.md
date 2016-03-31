---
タイトル: ステップ 6: Privileged Access Management のグループを移行します。
ms.custom: 
  - Id 管理
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
---
# ステップ 6: Privileged Access Managment へのグループの移行
PRIV フォレストで特権アカウントを作成する場合は、いくつかの新しい PowerShell コマンドレットを使用します。  これらのコマンドレットは、以下の機能を実行します。

-   新しいグループを作成、 *PRIV* では、同じフォレスト *SID* (セキュリティ識別子) でグループとして、 *CORP* フォレストし、MIM サービス内のオブジェクトとしてのデータベース内のグループに対応する、 *PRIV* フォレストです。

-   コマンドレットがユーザーに対応する MIM サービス データベースに 2 つのオブジェクトを作成の各ユーザー アカウント、 *CORP* フォレストと、新しいユーザー アカウント、 *PRIV* フォレストです。

-   作成、 *PAM ロール* MIM サービス データベース内のオブジェクト。

    このプレリリース プレビューでは、コマンドレットを、グループごとに 1 回、およびグループのメンバーごとに 1 回実行する必要があります  (移行コマンドレットの変更、すべてのユーザーまたはグループを変更はないので注意して、 *CORP* フォレスト: 手動で行う、PAM 管理者が、その後です)。

    1.  ログイン *PAMSRV* ドメイン管理者として、サービスが実行されていることを確認します。

        1.  ログインしていることを確認 *PAMSRV* として *priv \administrator*します。

        2.   **[サービス]**を開きます。

        3.  いることを確認、 **Forefront Identity Manager** サービスが実行されています。  サービスが実行されていない場合は、サービスと選択] を右クリックし **開始** サービスを開始します。

    2.  CorpAdmins グループとそのメンバー、Jen をコピーする MIM PAM グループとユーザー管理のコマンドレットを実行 *CONTOSO* に *PRIV* ドメイン。

        1.  PowerShell を起動し、次のコマンドを指定することを実行、 *CORP* ドメイン管理者 (*contoso \administrator*) パスワード場所。

            ```
            Import-Module MIMPAM
            Import-Module ActiveDirectory
            $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
            $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca 
            $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen 
            $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
            Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
            Set-ADUser –identity priv.Jen –Enabled 1 
            Add-ADGroupMember "Protected Users" priv.Jen
            $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
            ```
            リファレンスについては、 **New-pamgroup** コマンドは、次のパラメーターを受け取ります。

            -   NetBIOS 形式の CORP フォレスト ドメイン名

            -   そのドメインからコピーするグループの名前

            -   CORPフォレスト ドメイン コントローラーの NetBIOS 名

            -   CORP フォレストでのドメイン管理ユーザーの資格情報

        次に、現在グループ メンバーであるユーザーを JIT 昇格に移行し、フォレスト間のアクセス権が有効であること、またはユーザーの管理者アカウントであることを確認します。

    3.   *CORPDC*上で、Jen のアカウントがまだ存在している場合は、それを *CONTOSO CorpAdmins* グループから削除します。

        1.  ログイン *CORPDC* として *contoso \administrator*します。

        2.  PowerShell を起動し、次のコマンドを実行して、変更内容を確認します。

            ```
            Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
            ```



<!--HONumber=Mar16_HO1-->


