---
title: "PAM 要求を取得する | Microsoft Docs"
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
ms.assetid: 620eebd6-e4c3-473b-b824-ee6cfe83e509
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 00467b6443d3e6e0511ee1817a945ffe569b99b0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-pam-requests"></a>PAM 要求を取得する
以前にポストされた PAM 要求の履歴を返すために特権アカウントによって使用されます。

**注意**: このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `http://api.contoso.com`) の相対 URL です。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>クエリ パラメーター
パラメーター | 説明
----------|--------------
$filter | 任意。 フィルター式で任意の PAM 要求のプロパティを指定して、フィルターされた応答のリストを返します。 サポートされている演算子の詳細については、「PAM REST API サービスの詳細」の「[フィルター](privileged-access-management-rest-api-service-details.md#filtering)」をご覧ください。
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
正常な応答には、次のプロパティとともに PAM 要求オブジェクトの一覧が含まれています。

プロパティ | 説明
--------|-------------
要求ID | PAM 要求の一意の識別子 (GUID)。
CreatorID | PAM 要求を作成した Active Directory アカウントの一意の識別子 (GUID)。
Justification | 昇格の理由。
表示名 | MIM での PAM 要求の表示名。
CreationTime | 要求の作成時間。
Creation方法 | 要求の作成に使用されるメソッド。
ExpirationTime | 要求の有効期限。
RoleID| PAM ロールの一意の識別子 (GUID)。
要求edTTL | 要求の有効期限タイムアウト (秒)。
RequestedTime | 昇格の要求時間。
要求edStatus | 要求の状態。 設定可能な値は、次のとおりです。"Active"、"Closed"、"Closing"、"Expired"、"Pending Approval"、"Rejected"。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /api/pamresources/pamrequests?$filter=CreationTime%20gt%20datetime'2015-06-12T04:49:32.431Z'%20and%20CreationTime%20lt%20datetime'2015-07-13T04:49:32.432Z' HTTP/1.1
```

###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests",
    "value":[
        {
            "RequestId":"b22e1343-9a2b-4e33-a70a-1bb7b2d405b9",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-06-23T11:34:38.58Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-06-23T12:34:38.847Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-23T11:34:36.417Z",
            "RequestStatus":"Expired"
        },
        {
            "RequestId":"3a98d1c7-524d-4b72-9da7-bd9f907eab55",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Reason for Request",
            "CreationTime":"2015-07-12T04:35:14.433Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T04:43:51.95Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:35:00Z",
            "RequestStatus":"Closed"
        },
        {
            "RequestId":"f5e80be1-e9a3-42c4-81f8-4be5a4a429f4",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":null,
            "CreationTime":"2015-07-12T04:48:17.46Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"2015-07-12T05:48:17.853Z",
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-12T04:48:14.057Z",
            "RequestStatus":"Active"
        },
        {
            "RequestId":"b0f0ddc0-c809-4770-9d39-0d706f97a2de",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"",
            "CreationTime":"2015-06-30T07:01:13.147Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-06-30T07:01:13.119Z",
            "RequestStatus":"Rejected"
        },
        {
            "RequestId":"5ec10e61-cdd1-404e-a18e-740467d87dbf",
            "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
            "Justification":"Example Reason",
            "CreationTime":"2015-07-12T04:49:09.963Z",
            "CreationMethod":"PAM Web API",
            "ExpirationTime":"0001-01-01T00:00:00",
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "RequestedTTL":"12960000",
            "RequestedTime":"2015-07-12T04:50:00Z",
            "RequestStatus":"PendingApproval"
        }
    ]
}
```       
