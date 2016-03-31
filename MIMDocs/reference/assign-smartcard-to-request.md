---
タイトル: スマート カードを要求に割り当てる
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 20f0bf6e-9ae0-4d21-8117-ed63e29315e6
---
# スマートカードを要求に割り当てる
指定された要求に指定のスマートカードをバインドします。 バインドされると、その要求はこのカードでしか実行できなくなります。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
POST     |/CertificateManagement/api/v1.0/smartcards

###URL パラメーター
なし。
###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###要求本文
要求本文には、次のプロパティが含まれています。

プロパティ | 説明
---------|-----------
requestid | スマートカードをバインドする要求の ID。
cardid | スマートカードの cardid。
atr | スマート カードの ATR (Answer To Reset) 文字列。


##[応答]
###応答コード
コード  |説明  
---------|---------
201     | 作成日
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###応答本文
成功すると、新しく作成されたスマートカード オブジェクトに URI が返されます。
##例

###要求
```
POST /CertificateManagement/api/v1.0/smartcards HTTP/1.1

{
    "requestid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "cardid":"bc88f13f-83ba-4037-8262-46eba1291c6e",
    "atr":"3b8d0180fba000000397425446590301c8"
}

```
###[応答]
```
HTTP/1.1 201 Created

"api/v1.0/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9"
```       
##参照

- [Microsoft.Clm.Provision.要求Operations.CreateSmartcard 方法 (StrHTTP の要求ヘッダーおよび応答ヘッダーg, StrHTTP の要求ヘッダーおよび応答ヘッダーg, 要求)](https://msdn.microsoft.com/en-us/library/wHTTP の要求ヘッダーおよび応答ヘッダーdows/desktop/bb456812(v=vs.100%29.aspx)


<!--HONumber=Mar16_HO1-->


