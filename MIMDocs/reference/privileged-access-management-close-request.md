---
title: "PAM 要求を閉じる | Microsoft Docs"
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
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 181bf1a2953e461925d1b7efc5da4a2fa0c86914
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="close-pam-request"></a>PAM 要求を閉じる
特権アカウントが、PAM ロールへの昇格を開始した要求を終了するときに使用します。

**注意**: このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `http://api.contoso.com`) の相対 URL です。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

###<a name="url-parameters"></a>URL パラメーター
パラメーター | 説明
----------|-----------
requestId | 終了する PAM 要求の識別子 (GUID) は、 `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

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
なし
##<a name="example"></a>例

###<a name="request"></a>要求
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

```       
