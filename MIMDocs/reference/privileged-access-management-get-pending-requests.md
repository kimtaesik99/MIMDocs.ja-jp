---
title: "保留中の PAM 要求を取得する | Microsoft Docs"
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
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: aa1e8cd48b1bcca6e856bd553b6b92a062a08ff4
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-pending-pam-requests"></a>保留中の PAM 要求を取得する
特権アカウントを使用して、承認が必要な保留中の要求の一覧が返されます。

**注意**: このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `http://api.contoso.com`) の相対 URL です。
##<a name="request"></a>要求

方法  |[要求 URL]  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###<a name="query-parameters"></a>クエリ パラメーター
パラメーター | 説明
----------|--------------
$filter | 任意。 フィルター式で任意の保留中の PAM 要求のプロパティを指定して、フィルターされた応答のリストを返します。 サポートされている演算子の詳細については、「PAM REST API サービスの詳細」の「[フィルター](privileged-access-management-rest-api-service-details.md#filtering)」をご覧ください。
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
正常な応答には、次のプロパティとともに PAM 要求の承認オブジェクトの一覧が含まれています。

プロパティ | 説明
---------|-------------
RoleName | 承認を必要とするロールの表示名。
要求or | 承認の要求元のユーザー名。
Justification | ユーザーによって提供される位置揃え。
RequestedTTL | 要求の有効期限 (秒)。
要求edTime | 昇格の要求時間。
CreationTime | 要求の作成時間。
FIM要求ID | 1 つの要素 ("Value") と PAM 要求の一意の識別子 (GUID) を含む。
要求orID | 1 つの要素 ("Value") と PAM 要求を作成した Active Directory アカウントの一意の識別子 (GUID) を含む。
ApprovalObjectID | 1 つの要素 ("Value") と承認オブジェクトの一意の識別子 (GUID) を含む。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       
