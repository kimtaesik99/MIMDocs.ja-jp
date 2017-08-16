---
title: "PAM セッション情報を取得する | Microsoft Docs"
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
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 150bc7e863fc679e785526935155611fb75cd69b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-session-info"></a>PAM セッション情報を取得する
特権アカウントで、セッションにログインにしているアカウントのユーザー名を取得するために使用されます。

**注意**: このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `http://api.contoso.com`) の相対 URL です。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/api/session/sessionHTTP の要求ヘッダーおよび応答ヘッダーfo

###<a name="query-parameters"></a>クエリ パラメーター
パラメーター | 説明
----------|--------------
v | 任意。 API のバージョン。 指定しない場合は、API の現在 (最新リリース) のバージョンが使用されます。 詳細については、「PAM REST API サービスの詳細」の「[バージョン管理](privileged-access-management-rest-api-service-details.md#versioning)」をご覧ください。

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*PAM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
なし

##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
200 | OK
401 | 未承認
403 | 許可されていません
408 | 要求のタイムアウト   
500 | 内部サーバー エラー
503 | サービス利用不可

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、「*PAM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="response-body"></a>応答本文
成功応答の場合、次のプロパティの PAM セッション オブジェクトが返されます。

プロパティ| 説明
--------|-------------
Username| このセッションにログインしているアカウントのユーザー名。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       
