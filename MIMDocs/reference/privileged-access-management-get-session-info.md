---
タイトル: PAM セッション情報の取得
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: bc30e455-9a9c-413a-b8ca-9669286299cf
---
# PAM セッション情報を取得する
特権アカウントで、セッションにログインにしているアカウントのユーザー名を取得するために使用されます。

**注**: このトピックに示すように Url が API の展開時に選択したホスト名はたとえば: `http://api.contoso.com`です。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/api/session/sessionHTTP の要求ヘッダーおよび応答ヘッダーfo

###クエリ パラメーター
パラメーター | 説明
----------|--------------
v | 任意。 API のバージョン。 指定しない場合は、API の現在 (最新リリース) のバージョンが使用されます。 詳細については、次を参照してください [PAM REST API サービスの詳細でのバージョン管理。](privileged-access-management-rest-api-service-details.md#Versioning)

###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](privileged-access-management-rest-api-service-details.md#HttpHeaders) で *PAM REST API サービスの詳細*します。
###要求本文
なし

##[応答]
###応答コード
コード  |説明  
---------|---------
200 | OK
401 | 未承認
403 | 許可されていません
408 | 要求のタイムアウト   
500 | 内部サーバー エラー
503 | サービス利用不可

###応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](privileged-access-management-rest-api-service-details.md#HttpHeaders) で *PAM REST API サービスの詳細*します。
###応答本文
成功応答の場合、次のプロパティの PAM セッション オブジェクトが返されます。

プロパティ| 説明
--------|-------------
Username| このセッションにログインしているアカウントのユーザー名。

##例

###要求
```
GET /api/session/sessioninfo/ HTTP/1.1
```
###[応答]
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#sessioninfo",
    "value":[
        {
            "Username":"FIMCITONEBOXAD\\Jen"
        }
    ]
}
```       


<!--HONumber=Mar16_HO1-->


