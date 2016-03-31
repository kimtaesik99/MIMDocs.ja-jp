---
タイトル: スマート カード提示の暗証番号 (pin) を取得します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: ced93932-9912-4b32-9586-ada69b38a796
---
# スマートカードの PIN 候補を取得する
サーバーで生成されたユーザー PIN を取得します。

**注意**:暗証番号 (PIN) がサーバーで設定されるのは、プロファイル テンプレート ポリシーでそのように指定されている場合だけです。 指定されていない場合は、ユーザーが指定する必要があります。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/smartcards/{id}/serverproposedpHTTP の要求ヘッダーおよび応答ヘッダー

###URL パラメーター
パラメーター | 説明
---------|------------
id | スマートカード識別子 (MIM CM 固有)。 Microsft.Clm.Shared.Smartcard オブジェクトから取得されます。
###クエリ パラメーター
パラメーター | 説明
---------|------------
atr | カードの atr。
cardid | カードの ID。
challenge | スマートカードから発行されたチャレンジを表す base 64 エンコードの文字列。

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
成功すると、サーバーから提案された PIN を表す文字列が返されます。

##例

###要求
```
GET GET /CertificateManagement/api/v1.0/smartcards/C6BAD97C-F97F-4920-8947-BE980C98C6B5/serverproposedpin HTTP/1.1
```
###[応答]
```
HTTP/1.1 200 OK

... body coming soon
```       


<!--HONumber=Mar16_HO1-->


