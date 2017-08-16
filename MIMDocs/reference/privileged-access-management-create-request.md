---
title: "PAM 要求を作成する | Microsoft Docs"
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
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: de28af5c49eb98c5a1ccfbd8ed9353cf202e9e66
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="create-pam-request"></a>PAM 要求を作成する
特権アカウントが PAM ロールに昇格するために使用します。

**注意**: このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `http://api.contoso.com`) の相対 URL です。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
POST     |/api/pamresources/pamrequests

###<a name="query-parameters"></a>クエリ パラメーター
パラメーター | 説明
--------|-------------
Justification | 任意。 昇格要求にユーザーが指定した理由。
RoleId| 必須。 昇格する PAM ロールの一意の識別子 (GUID)。
RequestedTTL| 必須。 要求の有効期限 (秒)。
要求edTime | 省略可能。 特権を昇格する時間。  
v | 任意。 API のバージョン。 指定しない場合は、API の現在 (最新リリース) のバージョンが使用されます。 詳細については、「PAM REST API サービスの詳細」の「[バージョン管理](privileged-access-management-rest-api-service-details.md#versioning)」をご覧ください。

**注意**: *Justification*、 *RoleId*、 *RequestedTTL*、および *RequestedTime* パラメーターは、クエリ パラメーターとしてではなく、要求本文のプロパティとして指定できます。 *v* パラメーターは、クエリ パラメーターとしてのみ指定できます。

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*PAM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](privileged-access-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
任意。 前述のように、 *Justification*、 *RoleId*、 *RequestedTTL*、および *RequestedTime* パラメーターは、URL クエリ文字列で指定するのではなく、要求本文のプロパティとして指定できます。

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
成功応答には、次のプロパティの PAM 要求オブジェクトが含まれます。

プロパティ | 説明
--------|-------------
要求ID | PAM 要求の一意の識別子 (GUID)。
CreatorID | 要求を作成したアカウントの MIM サービスの一意の識別子 (GUID)。
Justification | 昇格の理由。
CreationTime | 要求の作成時間。
Creation方法 | 要求の作成に使用されるメソッド。
ExpirationTime | 要求の有効期限。
RoleID| PAM ロールの一意の識別子 (GUID)。
要求edTTL | 要求の有効期限タイムアウト (秒)。
RequestedTime | 昇格の要求時間。
RequestStatus | 要求の状態。 設定可能な値は、次のとおりです。"Processing"、"Active"、"Closed"、"Closing"、"Expired"、"PendingApproval"、"PendingMFA"、"Rejected"。

##<a name="example"></a>例

###<a name="request-1"></a>要求 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###<a name="response-1"></a>応答 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###<a name="request-2"></a>要求 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###<a name="response-2"></a>応答 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       
