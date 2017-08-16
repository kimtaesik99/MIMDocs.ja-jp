---
title: "スマート カードの PIN 候補を取得する | Microsoft Docs"
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
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 08d28819402dd56f996d39aa03b8ac829bef0838
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-proposed-pin"></a>スマートカードの PIN 候補を取得する
サーバーで生成されたユーザー PIN を取得します。

**注意**:暗証番号 (PIN) がサーバーで設定されるのは、プロファイル テンプレート ポリシーでそのように指定されている場合だけです。 指定されていない場合は、ユーザーが指定する必要があります。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpHTTP の要求ヘッダーおよび応答ヘッダー

###<a name="url-parameters"></a>URL パラメーター
パラメーター | 説明
---------|------------
id | スマートカード識別子 (MIM CM 固有)。 Microsft.Clm.Shared.Smartcard オブジェクトから取得されます。
###<a name="query-parameters"></a>クエリ パラメーター
パラメーター | 説明
---------|------------
atr | カードの atr。
cardid | カードの ID。
challenge | スマートカードから発行されたチャレンジを表す base 64 エンコードの文字列。

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
なし

##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
200     | OK
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」を参照してください。
###<a name="response-body"></a>応答本文
成功すると、サーバーから提案された PIN を表す文字列が返されます。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

... body coming soon
```       
