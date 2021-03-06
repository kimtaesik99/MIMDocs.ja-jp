---
title: "BHOLD Reporting のインストール | Microsoft Docs"
description: "BHOLD Reporting モジュールを使用すると、ロールおよび承認ポリシーに関するレポートを生成できます"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-reporting-installation"></a>BHOLD Reporting のインストール

BHOLD Reporting モジュールを使用すると、BHOLD でロールおよび承認ポリシーに関するレポートを生成できます。 こうしたレポートは、多くの場合、監査や、規制の要件への準拠を示すときに役立ちます。 また、このモジュールによって、ユーザー ロールのメンバーシップ分析に必要な情報がユーザーに提供されるため、組織における承認管理機能がさらに拡張されます。 レポートのビューは、ユーザーがレポートを作成するときに自身が許可されている情報のみが表示されるように制限できます。

## <a name="bhold-reporting-installation-requirements"></a>BHOLD Reporting のインストール要件

BHOLD Reporting モジュールをインストールする前に、BHOLD Reporting モジュールをインストールするサーバーに、BHOLD コア モジュールをインストールする必要があります。 BHOLD コア モジュールのインストールの詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。

>[!IMPORTANT]
BHOLD Reporting および BHOLD 構成証明の両方をインストールする場合は、BHOLD 構成証明をインストールする前に、BHOLD Reporting をインストールする必要があります。

## <a name="before-you-begin"></a>始める前に

BHOLD Reporting モジュールのインストールを開始する前に、BHOLD Reporting セットアップ ウィザードでのインストールに必要な情報を準備する必要があります。 次のワークシートを使用すると、こうした情報を必要に応じて指定できるように、記録しておくことができます。

| **項目**                                    | **説明**                                                                                                                                                                                                           | **値**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ドメイン/マシンでセキュリティ プロバイダーを使用する** | オンにすると、Active Directory Domain Services セキュリティによって BHOLD コアへのアクセスが制御されます。                                                                                                                | チェック ボックスをオンにします。 </br>**重要:** このチェック ボックスをオフにすると、インストールは失敗します。                                                                                                                                                                                                                   |
| **ドメイン**                                  | BHOLD コアをインストールするときに作成したサービス アカウントを含むドメインを指定します。 詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。 | ドメイン名は、ウィザードによって自動的に入力されます。 この名前を変更するのは、適切ではない場合のみにします。 **重要:** 完全修飾ドメイン名 (FQDN) ではなく、NetBIOS 名 (短い名前) を使用してドメイン名を指定します。 たとえば、ドメインの FQDN が fabrikam.com の場合、ドメイン名は FABRIKAM として指定します。 |
| **ユーザー**                                    | BHOLD コア サービスのユーザー アカウントのログオン名を指定します。                                                                                                                                                          | ここにユーザー アカウント名を入力します。                                                                                                                                                                                                                                                                                    |
| **パスワード**                                | サービスのユーザー アカウントのパスワードを指定します。                                                                                                                                                                       | ここにパスワードを入力します。 </br>**重要:** このパスワードは、セキュリティで保護された非表示の場所に保管してください。                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>BHOLD Reporting のインストール

BHOLD Reporting モジュールをインストールするには、Domain Admins グループのメンバーとしてログオンし、次のファイルをダウンロードして、BHOLD Reporting モジュールをインストールするサーバーで管理者として実行します。

- BholdReporting*\<Version\>*\_Release.msi

*\<Version\>* は、インストールする BHOLD Reporting リリースのバージョン番号に置き換えてください。

管理者としてプログラム ファイルを実行するには、そのファイルを右クリックし、**[管理者として実行]** をクリックします。

## <a name="next-steps"></a>次のステップ

- [BHOLD インストール ガイド](bhold-installation-guide.md)
- [BHOLD 開発者用リファレンス](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD のバージョン履歴](../reference/version-bhold-history.md)