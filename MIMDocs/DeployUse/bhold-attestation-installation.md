---
title: "BHOLD 構成証明のインストール | Microsoft Docs"
description: "BHOLD 構成証明モジュールでは、レビュー担当者を指定し、レビューを実行できます"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 93d0b9a17d82911b71b1b220465b6d637687444b
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-attestation-installation"></a>BHOLD 構成証明のインストール

BHOLD 構成証明モジュールでは、レビュー担当者を指定し、ユーザーとアプリケーションごとのアクセス許可およびアカウントの間の関係を定期的にレビューできます。

## <a name="bhold-attestation-installation-requirements"></a>BHOLD 構成証明のインストール要件

BHOLD 構成証明モジュールをインストールする前に、BHOLD 構成証明モジュールをインストールするサーバーに、BHOLD コア モジュールをインストールする必要があります。 BHOLD コア モジュールのインストールの詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。 BHOLD 構成証明モジュールの連絡先は、電子メール メッセージをユーザーに送信するため、環境に Microsoft Exchange Server などの簡易メール転送プロトコル (SMTP) 電子メール サーバーが必要です。

>[!IMPORTANT]
BHOLD Reporting および BHOLD 構成証明の両方をインストールする場合は、BHOLD 構成証明をインストールする前に、BHOLD Reporting をインストールする必要があります。

## <a name="before-you-begin"></a>始める前に

BHOLD 構成証明モジュールのインストールを開始する前に、BHOLD 構成証明セットアップ ウィザードでのインストールに必要な情報を準備する必要があります。 次のワークシートを使用すると、こうした情報を必要に応じて指定できるように、記録しておくことができます。

| **項目**                                    | **説明**                                                                                                                                                                                                           | **値**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ドメイン/マシンでセキュリティ プロバイダーを使用する** | オンにすると、Active Directory Domain Services セキュリティによって BHOLD コアへのアクセスが制御されます。                                                                                                                | チェック ボックスをオンにします。 **重要:** このチェック ボックスをオフにすると、インストールは失敗します。                                                                                                                                                                                                                   |
| **ドメイン**                                  | BHOLD コアをインストールするときに作成したサービス アカウントを含むドメインを指定します。 詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。 | ドメイン名は、ウィザードによって自動的に入力されます。 この名前を変更するのは、適切ではない場合のみにします。 **重要:** 完全修飾ドメイン名 (FQDN) ではなく、NetBIOS 名 (短い名前) を使用してドメイン名を指定します。 たとえば、ドメインの FQDN が fabrikam.com の場合、ドメイン名は FABRIKAM として指定します。 |
| **ユーザー**                                    | BHOLD コア サービスのユーザー アカウントのログオン名を指定します。                                                                                                                                                          | ここにユーザー アカウント名を入力します。                                                                                                                                                                                                                                                                                    |
| **パスワード**                                | サービスのユーザー アカウントのパスワードを指定します。                                                                                                                                                                       | ここにパスワードを入力します。**重要:** このパスワードは、セキュリティで保護された非表示の場所に保管してください。                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>BHOLD 構成証明のインストール

BHOLD 構成証明モジュールをインストールするには、Domain Admins グループのメンバーとしてログオンし、次のファイルをダウンロードして、BHOLD 構成証明モジュールをインストールするサーバーで管理者として実行します。

- BholdAttestation*\<Version\>*\_Release.msi

*\<Version\>* は、インストールする BHOLD 構成証明リリースのバージョン番号に置き換えてください。

管理者としてプログラム ファイルを実行するには、そのファイルを右クリックし、**[管理者として実行]** をクリックします。

## <a name="next-steps"></a>次のステップ

- [BHOLD インストール ガイド](bhold-installation-guide.md)
- [BHOLD 開発者用リファレンス](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD のバージョン履歴](../reference/version-bhold-history.md)