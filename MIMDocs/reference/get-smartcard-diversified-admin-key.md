---
タイトル: スマート カードの分散管理キーの取得
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 68beeec1-8350-4e0e-946f-d94606e1e756
---
# スマート カード用の分散された管理キーの取得
指定したスマート カード用の分散された管理キーを取得します。

**注意**:管理キーを分散する必要があるのは、プロファイル テンプレート ポリシーでその旨が指示されている場合に限られます。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}/diversifiedkey

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
成功すると、分散された管理キーを表す BLOB バイトを返します。

##例

###要求
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9/diversifiedkey?cardid=bc88f13f-83ba-4037-8262-46eba1291c6e HTTP/1.1
```
###[応答]
```
HTTP/1.1 200 OK

"mBVA+HopB/gc+6FuKsQqx+OX01hK1WQI"
```       
##関連項目

- [Microsoft.Clm.Provision.要求Operations.CreateSmartcard 方法 (StrHTTP の要求ヘッダーおよび応答ヘッダーg, StrHTTP の要求ヘッダーおよび応答ヘッダーg, 要求) 方法](https://msdn.microsoft.com/en-us/library/wHTTP の要求ヘッダーおよび応答ヘッダーdows/desktop/bb456812(v=vs.100%29.aspx)


<!--HONumber=Mar16_HO1-->


