---
title: "スマート カードを要求に割り当てる | Microsoft Docs"
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
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 2886186ade7058b034565501e200ab3773b0af31
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="assign-smart-card-to-a-request"></a>スマート カードを要求に割り当てる
指定された要求に指定のスマート カードをバインドします。 バインドされると、その要求はこのカードでしか実行できなくなります。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###<a name="url-parameters"></a>URL パラメーター
なし。
###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
要求本文には、次のプロパティが含まれています。

プロパティ | 説明
---------|-----------
requestid | スマート カードをバインドする要求の ID。
cardid | スマート カードの cardid。
atr | スマート カードの ATR (Answer To Reset) 文字列。


##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
201     | 作成日
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」を参照してください。
###<a name="response-body"></a>応答本文
成功すると、新しく作成されたスマート カード オブジェクトに URI が返されます。
##<a name="example"></a>例

###<a name="request"></a>要求
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###<a name="response"></a>[応答]
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##<a name="see-also"></a>関連項目

- [Microsoft.Clm.Provision.RequestOperations.CreateSmartcard メソッド (String, String, Request)](https://msdn.microsoft.com/library/windows/desktop/bb456812.aspx)
