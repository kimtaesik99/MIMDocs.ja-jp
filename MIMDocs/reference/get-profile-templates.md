---
title: "プロファイル テンプレートを取得する | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8736b328926445f6434bd037a1ecf2e5de9ee466
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-templates"></a>プロファイル テンプレートを取得する
指定したユーザーの登録に使用できるプロファイル テンプレートの一覧を取得します。 プロファイル テンプレートのビューの一部が返されます。 要求したユーザーは、返されるプロファイル テンプレート データを使用して、登録する必要があるプロファイル テンプレート (ある場合) を決定できます。 ワークフローとアクセス許可が指定されていない場合、ユーザーに表示可能なすべてのプロファイル テンプレートが返されます。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###<a name="url-parameters"></a>URL パラメーター
パラメーター| 説明
--------|-------------
targetuser| 任意。 プロファイル テンプレートを返す対象ユーザーを指定します。 指定されていない場合は、現在のユーザーの ID が使用されます。 <br/>**注意**:現時点では、現在のユーザーのみがサポートされています。

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、サービスのトピックを参照してください。
###<a name="request-body"></a>要求本文
なし

##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
200     | OK
204 | コンテンツはありません
500 | 内部エラー

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、サービスのトピックを参照してください。
###<a name="response-body"></a>応答本文
成功すると、次のプロパティの ProfileTemplateLimitedView オブジェクトの一覧が返されます。

プロパティ| 種類| 説明
--------|-----|--------
名前| string| プロファイル テンプレートの表示名。
説明| string| プロファイル テンプレートの説明。
Uuid| GUID| プロファイル テンプレートの識別子。
IsSmartcardProfileTemplate| ブール| テンプレートがスマート カード プロファイル テンプレートかどうかを示します。
IsVirtualSmartcardProfileTemplate| ブール| プロファイル テンプレートに仮想スマート カードが必要かどうかを示します。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       
