---
title: "スマート カード データの取得 | Microsoft Docs"
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
ms.assetid: 81f4b7cd-e4d9-4b11-b125-78cc9f183cf0
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 9a84982971d169b5c9e7bd758cc74d795416975b
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-data"></a>スマートカード データの取得
ユーザーのスマート カード プロファイルの一覧と、現在のユーザーによって実行可能な操作の一覧を取得します。 指定した操作のいずれかに対して、要求を開始できます。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards <br/> /CertificateManagement/api/v1.0/smartcards/{smartcarduuid}


###<a name="url-parameters"></a>URL パラメーター
プロパティ| 説明
---------|--------
smartcarduuid | 任意。 MIM CM により表されるスマート カード UUID です。 これは、[Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) オブジェクトの “uuid” フィールドです。

###<a name="query-parameters"></a>クエリ パラメーター
プロパティ| 説明
---------|--------
cardid | 任意。 MIM CM により表されるスマート カード UUID です。 これは、[Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) オブジェクトの “uuid” フィールドです。


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
成功した場合、JSON でシリアル化された [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) オブジェクトと次のプロパティを返します。

名前 | 説明
-----|-----------
AssignedUserUuid | スマート カードが割り当てられるユーザーの識別子。
Atr | 現在初期化中のカードに対するスマート カード ATR (Answer To Reset) 文字列。
備考 | スマート カードを説明するコメント。
フラグ | スマート カードについて説明するフラグ。
ミドルウェア | スマート カードのミドルウェア。
ParentSmartcardUuid | スマート カードが置き換えた古いスマート カードの識別子。
PermanentSmartcardUuid | スマート カードに関連付けられた永続的なスマート カードの識別子。
PrimarySmartcardUuid | プライマリ スマート カードの識別子。
ProfileTemplateUuid | スマート カードを管理するポリシーと設定を含むプロファイル テンプレートの識別子。
ProfileTemplateVersion | スマート カード プロファイルの作成時点でのプロファイル テンプレートのバージョン。
SerialNumber | スマート カードのシリアル番号。
状態 | スマート カードの状態。
Uuid | スマート カード プロファイルの識別子。

##<a name="example"></a>例

###<a name="request-1"></a>要求 1
```
GET /certificatemanagement/api/v1.0/smartcards?cardid=d1ef6869-5517-42a0-8f05-16ca1a0b834d HTTP/1.1

```
###<a name="response-1"></a>応答 1
```
HTTP/1.1 200 OK

{
    "Uuid":"438d1b30-f3b4-4bed-85fa-285e08605ba7",
    "Status":3,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{d1ef6869-5517-42a0-8f05-16ca1a0b834d}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
###<a name="request-2"></a>要求 2
```
GET /certificatemanagement/api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1
```
###<a name="response-2"></a>応答 2
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":2,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
##<a name="see-also"></a>関連項目

-[Microsoft.Clm.Shared.Smartcards.Smartcard クラス](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx)
