---
title: "BHOLD Analytics のインストール | Microsoft Docs"
description: "BHOLD Analytics モジュールは、規則ベースのデータ アクセス テストを実施します"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 631e08667e5d1535d8f63cc297aad360080f8b20
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2017
---
# <a name="bhold-analytics-installation"></a>BHOLD Analytics のインストール

BHOLD Analytics モジュールは、規則に基づいてデータ アクセスをテストすることで、組織データへのアクセスを組織が効果的に制御し、内部および外部のアクセス要件に確実に準拠できるようにします。 BHOLD Analytics モジュールによって影響が自動分析され、提案された規則の影響を受けるユーザーの数や、規則に適合するユーザー、規則に違反するユーザーなどの概要が示されます。 また、BHOLD Analytics モジュールでは、規則に準拠するユーザーおよび規則に違反するユーザーの詳細な一覧を表示することもできます。

## <a name="bhold-analytics-installation-requirements"></a>BHOLD Analytics のインストール要件

BHOLD Analytics モジュールをインストールする前に、BHOLD Analytics モジュールをインストールするサーバーに、BHOLD コア モジュールをインストールする必要があります。 BHOLD コア モジュールのインストールの詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。

## <a name="before-you-begin"></a>始める前に

BHOLD Analytics モジュールのインストールを開始する前に、BHOLD Analytics セットアップ ウィザードでのインストールに必要な情報を準備する必要があります。 次のワークシートを使用すると、こうした情報を必要に応じて指定できるように、記録しておくことができます。

| **項目**                                    | **説明**                                                                                                                                                                                                           | **値**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ドメイン/マシンでセキュリティ プロバイダーを使用する** | オンにすると、Active Directory Domain Services セキュリティによって BHOLD コアへのアクセスが制御されます。                                                                                                                | チェック ボックスをオンにします。 **重要:** このチェック ボックスをオフにすると、インストールは失敗します。                                                                                                                                                                                                                   |
| **ドメイン**                                  | BHOLD コアをインストールするときに作成したサービス アカウントを含むドメインを指定します。 詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。 | ドメイン名は、ウィザードによって自動的に入力されます。 この名前を変更するのは、適切ではない場合のみにします。 **重要:** 完全修飾ドメイン名 (FQDN) ではなく、NetBIOS 名 (短い名前) を使用してドメイン名を指定します。 たとえば、ドメインの FQDN が fabrikam.com の場合、ドメイン名は FABRIKAM として指定します。 |
| **ユーザー**                                    | BHOLD コア サービスのユーザー アカウントのログオン名を指定します。                                                                                                                                                          | ここにユーザー アカウント名を入力します。                                                                                                                                                                                                                                                                                    |
| **パスワード**                                | サービスのユーザー アカウントのパスワードを指定します。                                                                                                                                                                       | ここにパスワードを入力します。**重要:** このパスワードは、セキュリティで保護された非表示の場所に保管してください。                                                                                                                                                                                                                  |

## <a name="bhold-analytics-installation"></a>BHOLD Analytics のインストール

BHOLD Analytics モジュールをインストールするには、Domain Admins グループのメンバーとしてログオンし、次のファイルをダウンロードして、BHOLD Analytics モジュールをインストールするサーバーで管理者として実行します。

- BholdAnalytics*\<Version\>*\_Release.msi

*\<Version\>* は、インストールする BHOLD Analytics リリースのバージョン番号に置き換えてください。

管理者としてプログラム ファイルを実行するには、そのファイルを右クリックし、**[管理者として実行]** をクリックします。

# <a name="next-steps"></a>次のステップ

- [BHOLD コアのインストール](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)
- [BHOLD インストール ガイド](bhold-installation-guide.md)
- [BHOLD のバージョン履歴](../reference/version-bhold-history.md)