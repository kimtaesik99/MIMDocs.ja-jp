---
タイトル: プロファイル状態操作を取得します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: f62c827b-5229-4b13-ad37-4f62ad231d30
---
# プロファイル状態操作を取得する
指定したプロファイルで現在のユーザーが実行できる操作の一覧を取得します。 指定した操作のいずれかに対して、要求を開始できます。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/operations <br/>/CertificateManagement/api/v1.0/smartcards/{id}/operations

###URL パラメーター
パラメーター | 説明
---------|------------
id | プロファイルまたはスマートカードの識別子 (GUID)。

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
成功時には、スマートカードのユーザーが実行できる操作の一覧が返されます。 この一覧には、任意の数の *OnlineUpdate*、 *Renew*、 *Recover*、 *RecoverOnBehalf*、 *Retire*、 *Revoke*、および *Unblock*が含まれます。

##例

###要求
```
GET /certificatemanagement/api/v1.0/smartcards/438d1b30-f3b4-4bed-85fa-285e08605ba7/operations HTTP/1.1
```
###[応答]
```
HTTP/1.1 200 OK

[
    "renew",
    "unblock",
    "retire"
]
```       


<!--HONumber=Mar16_HO1-->


