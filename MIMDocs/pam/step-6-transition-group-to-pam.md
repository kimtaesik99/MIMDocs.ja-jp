---
title: "PAM の展開、手順 6 – グループの移動 | Microsoft Docs"
description: "グループを Privilege Access Management で管理できるように、PRIV フォレストに移動します。"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 7b689eff-3a10-4f51-97b2-cb1b4827b63c
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 550ad1e68ed8464dc7361e7a35ef35ee97753a9a
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/14/2017
---
# <a name="step-6--transition-a-group-to-privileged-access-management"></a>ステップ 6: Privileged Access Managment へのグループの移行

>[!div class="step-by-step"]
[« 手順 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[手順 7 »](step-7-elevate-user-access.md)

PRIV フォレストで特権アカウントを作成する場合は、PowerShell コマンドレットを使用します。 これらのコマンドレットは、以下の機能を実行します。

- CORP フォレスト内のグループと同じセキュリティ識別子 (SID) を持つ PRIV フォレスト内に新しいグループを作成します。  
- PRIV フォレスト内のグループに対応する MIM サービス データベースにオブジェクトを作成します。  
- ユーザー アカウントごとに 2 つのオブジェクトを MIM サービス データベースに作成します。1 つは CORP フォレスト内のユーザーに対応し、もう 1 つは PRIV フォレスト内の新しいユーザー アカウントに対応します。  
- MIM サービス データベース内に PAM ロール オブジェクトを作成します。  

グループごとに 1 回、グループのメンバーごとに 1 回、コマンドレットを実行する必要があります 移行コマンドレットによって CORP フォレスト内のユーザーやグループは変更または修正されません。このため、PAM 管理者が後で手動で行う必要があります。

1. *PRIV\MIMAdmin* として PAMSRV に直接サインインするか、または PRIV ワークステーションからサインインします。

2.  PowerShell を起動し、次のコマンドを入力します。

```PowerShell
   Import-Module MIMPAM
   Import-Module ActiveDirectory
```

3.  デモンストレーションを目的として、既存のフォレスト内のユーザー アカウントに対応するユーザー アカウントを PRIV 内に作成します。

    PowerShell に次のコマンドを入力します。  前の手順で contoso.local に作成したユーザーの名前が *Jen* でない場合は、それに応じてコマンドのパラメーターを変更してください。 パスワード 'Pass@word1' は一例ですので、固有のパスワード値に変更してください。

 ```PowerShell
        $sj = New-PAMUser –SourceDomain CONTOSO.local –SourceAccountName Jen
        $jp = ConvertTo-SecureString "Pass@word1" –asplaintext –force
        Set-ADAccountPassword –identity priv.Jen –NewPassword $jp
        Set-ADUser –identity priv.Jen –Enabled 1
  ```

4. デモンストレーションを目的として、CONTOSO ドメインから PRIV ドメインに、グループとそのメンバー Jen をコピーします。

    以下のコマンドを実行します。入力を求めるメッセージが表示されたら、CORP ドメイン管理者 (CONTOSO\Administrator) のパスワードを指定します。

 ```PowerShell
        $ca = get-credential –UserName CONTOSO\Administrator –Message "CORP forest domain admin credentials"
        $pg = New-PAMGroup –SourceGroupName "CorpAdmins" –SourceDomain CONTOSO.local                 –SourceDC CORPDC.contoso.local –Credentials $ca
        $pr = New-PAMRole –DisplayName "CorpAdmins" –Privileges $pg –Candidates $sj
 ```

    参照用として、**New-PAMGroup** コマンドは以下のパラメーターを取ります。

     -   NetBIOS 形式の CORP フォレスト ドメイン名  
     -   そのドメインからコピーするグループの名前  
     -   CORPフォレスト ドメイン コントローラーの NetBIOS 名  
     -   CORP フォレストでのドメイン管理ユーザーの資格情報  

5.  (オプション) CORPDC 上で、Jen のアカウントがまだ存在している場合は、それを **CONTOSO CorpAdmins** グループから削除します。  これは、デモンストレーションの目的でのみ必要な操作で、PRIV フォレストで作成したアカウントにアクセス許可を関連付ける方法を示しています。

    1.  *CORPDC* に CONTOSO\Administrator としてサインインします。

    2.  PowerShell を起動し、次のコマンドを実行して、変更内容を確認します。

        ```PowerShell
        Remove-ADGroupMember -identity "CorpAdmins" -Members "Jen"
        ```


ユーザーの管理者アカウントでフォレストをまたがるアクセス許可が有効なことを示すためには、次の手順に進みます。

>[!div class="step-by-step"]
[« 手順 5 ](step-5-establish-trust-between-priv-corp-forests.md)
[手順 7 »](step-7-elevate-user-access.md)
