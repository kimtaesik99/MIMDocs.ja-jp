---
title: "スマート カード認証の応答を取得する | Microsoft Docs"
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
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1c7a6f5e7ebd1e6e4cfc0992607adb91743f343e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-authentication-response"></a>スマートカード認証の応答を取得する
Base CSP 認証チャレンジに対する応答を取得する

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

###<a name="url-parameters"></a>URL パラメーター
パラメーター | 説明
---------|------------
reqid | 必須。 要求識別子 (MIM CM 固有)。
scid | 必須。 スマートカード識別子 (MIM CM 固有)。 [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) オブジェクトから取得されます。
###<a name="query-parameters"></a>クエリ パラメーター
パラメーター | 説明
---------|------------
atr | 任意。 スマート カードの ATR (Answer To Reset) 文字列。
cardid | 必須。 カードの ID。
challenge | 必須。 スマートカードから発行されたチャレンジを表す base 64 エンコードの文字列。
diversified | 必須。 スマートカードの管理キーが分散されていたかどうかを示すブール値。


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
成功すると、チャレンジ応答を示すバイト BLOB が返されます。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##<a name="see-also"></a>関連項目

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse メソッド](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.getbasecspresponse.aspx)
