---
title: 管理者以外のユーザーの証明書マネージャー |Microsoft Identity Manager
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
ms.assetid: bfabc562-a2f0-4cff-ac31-36927f41e102
author: kgremban
---
# 管理者ではないユーザーの証明書マネージャー
ユーザーがコンピューターのローカル管理者ではない場合、既定では、そのコンピューターにスマートカードを登録できません。 この制限を回避するには、次の手順を実行します。

## MIM 2016 証明書マネージャーで管理者以外のスマート カードの更新を有効にします

1.  **appx ファイルを展開する**

    署名証明書を取得します。 手順に従います [内部 PKI を使用してサインオン Windows 8 アプリケーション](http://blogs.technet.com/b/deploymentguys/archive/2013/06/14/signing-windows-8-applications-using-an-internal-pki.aspx)します。 "アプリケーションに署名する" 段階になったら、停止します。 エクスポートされる pfx ファイルに名前を付けます。 .cer ファイルにもエクスポートし、新しい署名証明書の cer ファイルを使用してクライアントにインポートします。

    次を実行して appx ファイルを展開します。

    `makeappx unpack /l /p <app package name>.appx /d ./appx`

    `ren <app package name>.appx <app package name>.appx.old`

    `cd appx`

2.  **構成ファイルを変更する**

     `CustomDataExample.xml custom.data`というファイルの名前を変更します。 CM アプリは、このファイル名を探します。

    custom.data ファイルを編集し、次のように変更します。

    1.  & Lt;NonAdmin & gt;要素値属性の値を"True"に変更します。

    2.  ファイルを保存してエディターを終了します

    3.  AppxSignature.p7x というファイルを削除します

    4.  AppxManifest.xml というファイルを編集します

    5.  & Lt; Identity & gt;要素は、署名証明書のサブジェクトに発行元属性の値を変更例。"CN = ABCD")

        このサブジェクトは、アプリへのサインインに使用している署名証明書のサブジェクトと同じにする必要があります。

    6.  ファイルを保存してエディターを終了します。

3.  **アプリ パッケージ (appx ファイル) を再パックして署名する**

    次を実行して appx ファイルをパックして署名します。

    `cd ..`

    `makeappx pack /l /d .\appx /p <app package name>.appx`

    s`igntool sign /f <path\>mysign.pfx /p <pfx password> /fd "sha256" <app package name>.appx`

4.  プロファイル テンプレートと MIM サーバーを構成する最初の管理キーの追加を複製する

    1.  管理者特権を持つユーザーとして CM ポータルにログインします。

    2.   **[管理]** &gt; **[プロファイル テンプレートの管理]** を選択し、作成したテンプレートのプロファイルの横にあるボックスをオンにして、選択したプロファイル テンプレートの [コピー] をクリックします。

    3.  プロファイル テンプレートの名前を入力し、"nonAdmin" を追加し、 **[OK]**をクリックします。

    4.  プロファイル テンプレートの全般的な設定が表示されたら、一番下までクロールして、 **[スマートカードの構成]**の **[設定の変更]**をクリックします。

    5.   **管理キーの初期値 (16 進)** に既定の管理キーを入力します。"010203040506070801020304050607080102030405060708"

    6.  下にスクロールして、 **[OK]**をクリックします。

5.  **クライアント コンピューターで管理者以外のアカウントを作成する**

    管理者ではないユーザーは、TPM に仮想スマート カードを作成できないので、管理者が代わりに作成する必要があります。

6.  **TpmVscMgr を使用して仮想スマートカードを作成する**

    コンピューターに空の仮想スマートカードを作成するには、(管理者として) 次の手順を実行します。 この操作は、Intune、SCCM、またはグループ ポリシーを使用して実行できます。

    `TpmVscMgr create /name MyVSC /pin default /adminkey default /generate`

7.  **管理者以外のアカウントで CM アプリをインストールする**

8.  **仮想スマートカードの CM アプリと登録を起動する**


<!--HONumber=Mar16_HO3-->


