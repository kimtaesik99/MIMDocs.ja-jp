---
タイトル: スマート カード認証の応答を取得します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: e05ec898-06cd-4c17-a4f4-8f3545af0f14
---
# スマートカード認証の応答を取得する
Base CSP 認証チャレンジに対する応答を取得する

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/authenticationresponse

###URL パラメーター
パラメーター | 説明
---------|------------
reqid | 必須。 要求識別子 (MIM CM 固有)。
scid | 必須。 スマートカード識別子 (MIM CM 固有)。 [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) オブジェクトから取得。
###クエリ パラメーター
パラメーター | 説明
---------|------------
atr | 任意。 スマート カードの ATR (Answer To Reset) 文字列。
cardid | 必須。 カードの ID。
challenge | 必須。 スマートカードから発行されたチャレンジを表す base 64 エンコードの文字列。
diversified | 必須。 スマートカードの管理キーが分散されていたかどうかを示すブール値。


###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###要求本文
なし

##[応答]
###応答コード
コード  |説明  
---------|---------
200     | OK
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###応答本文
成功すると、チャレンジ応答を示すバイト BLOB が返されます。

##例

###要求
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/authenticationresponse?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e&challenge=1hFD%2Bcz%2F0so%3D&diversified=False HTTP/1.1

```
###[応答]
```
HTTP/1.1 200 OK

"F0Zudm4wPLY="
```       
##参照

- [Microsoft.Clm.Provision.ExecuteOperations.GetBaseCspResponse メソッド](https://msdn.microsoft.com/en-us/library/microsoft.clm.provision.executeoperations.getbasecspresponse(v=vs.100%29.aspx)


<!--HONumber=Mar16_HO1-->


