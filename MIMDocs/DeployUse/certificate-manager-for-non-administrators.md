---
# required metadata

title: 非管理者のスマート カードの登録 |Microsoft Identity Manager
description: 自身のコンピューターへの管理者アクセス権を持たないユーザーに対し、スマート カードを登録して証明書マネージャーを使用できるようにする方法について説明します。
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# 非管理者のスマート カードの登録
ユーザーがコンピューターのローカル管理者ではない場合、既定では、そのコンピューターにスマート カードを登録できません。 この制限を回避するには、次の手順を実行します。

## MIM 2016 証明書マネージャーで管理者以外のスマート カードの更新を有効にします

1.  **appx ファイルを展開する**

    署名証明書を取得します。 「[Sign Windows 8 applications using an internal PKI (内部 PKI を使用して Windows 8 アプリケーションに署名する)](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx)」の手順に従います。 "アプリケーションに署名する" 段階になったら、停止します。 エクスポートされる pfx ファイルに名前を付けます。 .cer ファイルにもエクスポートし、新しい署名証明書の cer ファイルを使用してクライアントにインポートします。

    次を実行して appx ファイルを展開します。

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **構成ファイルを変更する**

    `CustomDataExample.xml custom.data`というファイルの名前を変更します。 CM アプリは、このファイル名を探します。

    custom.data ファイルを編集し、次のように変更します。

    1.  &lt;NonAdmin&gt; 要素の Value 属性値を "True" に変更します

    2.  ファイルを保存してエディターを終了します

    3.  AppxSignature.p7x というファイルを削除します

    4.  AppxManifest.xml というファイルを編集します

    5.  &lt;Identity&gt; 要素の Publisher 属性値を、署名証明書のサブジェクトに変更します (例:"CN = ABCD")

        このサブジェクトは、アプリへのサインインに使用している署名証明書のサブジェクトと同じにする必要があります。

    6.  ファイルを保存してエディターを終了します。

3.  **アプリ パッケージ (appx ファイル) を再パックして署名する**

    次を実行して appx ファイルをパックして署名します。

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    S`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  プロファイル テンプレートと MIM サーバーを構成する最初の管理キーの追加を複製する

    1.  管理者特権を持つユーザーとして CM ポータルにログインします。

    2.  **[管理]** &gt; **[プロファイル テンプレートの管理]** を選択し、作成したテンプレートのプロファイルの横にあるボックスをオンにして、選択したプロファイル テンプレートの [コピー] をクリックします。

    3.  プロファイル テンプレートの名前を入力し、"nonAdmin" を追加し、 **[OK]**をクリックします。

    4.  プロファイル テンプレートの全般的な設定が表示されたら、一番下までクロールして、 **[スマート カードの構成]**の **[設定の変更]**をクリックします。

    5.  **管理キーの初期値 (16 進)** に既定の管理キーを入力します。"010203040506070801020304050607080102030405060708"

    6.  下にスクロールして、 **[OK]**をクリックします。

5.  **クライアント コンピューターで管理者以外のアカウントを作成する**

    管理者ではないユーザーは、TPM に仮想スマート カードを作成できないので、管理者が代わりに作成する必要があります。

6.  **TpmVscMgr を使用して仮想スマート カードを作成する**

    コンピューターに空の仮想スマート カードを作成するには、(管理者として) 次の手順を実行します。 この操作は、Intune、SCCM、またはグループ ポリシーを使用して実行できます。

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **管理者以外のアカウントで CM アプリをインストールする**

8.  **仮想スマート カードの CM アプリと登録を起動する**


<!--HONumber=Apr16_HO3-->


