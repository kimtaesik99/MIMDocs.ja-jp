---
タイトル: 保留中の PAM 要求を取得します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 005dc8fd-d73e-4557-b485-5566f16537eb
---
# 保留中の PAM 要求を取得する
特権アカウントを使用して、承認が必要な保留中の要求の一覧が返されます。

**注**: このトピックに示すように Url が API の展開時に選択したホスト名はたとえば: `http://api.contoso.com`です。
##要求

方法  |[要求 URL]  
---------|---------
GET     |/api/pamresources/pamrequeststoapprove

###クエリ パラメーター
パラメーター | 説明
----------|--------------
$filter | 任意。 フィルター式で任意の保留中の PAM 要求のプロパティを指定して、フィルターされた応答のリストを返します。 サポートされている演算子の詳細については、次を参照してください [PAM REST API サービスの詳細でフィルター処理。](privileged-access-management-rest-api-service-details.md#Filtering)
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
正常な応答には、次のプロパティとともに PAM 要求の承認オブジェクトの一覧が含まれています。

プロパティ | 説明
---------|-------------
RoleName | 承認を必要とするロールの表示名。
要求or | 承認の要求元のユーザー名。
Justification | ユーザーによって提供される位置揃え。
要求edTTL | 要求の有効期限 (秒)。
要求edTime | 昇格の要求時間。
CreationTime | 要求の作成時間。
FIM要求ID | 1 つの要素 ("Value") と PAM 要求の一意の識別子 (GUID) を含む。
要求orID | 1 つの要素 ("Value") と PAM 要求を作成した Active Directory アカウントの一意の識別子 (GUID) を含む。
ApprovalObjectID | 1 つの要素 ("Value") と承認オブジェクトの一意の識別子 (GUID) を含む。

##例

###要求
```
GET /api/pamresources/pamrequeststoapprove HTTP/1.1
```
###[応答]
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequeststoapprove",
    "value":[
        {
            "RoleName":"ApprovalRole",
            "Requestor":"PRIV.Jen",
            "Justification":"Justification Reason",
            "RequestedTTL":"3600",
            "RequestedTime":"2015-07-11T22:25:00Z",
            "CreationTime":"2015-07-11T22:24:52.51Z",
            "FIMRequestID":{
                "Value":"9802d7b7-b4e9-4fe4-8f5c-649cda127e49"
            },
            "RequestorID":{
                "Value":"73257e5e-00b3-4309-a330-f1e607ff113a"
            },
            "ApprovalObjectID":{
                "Value":"5dbd9d0c-0a9d-4f75-8cbd-ff6ffdc00143"
            }
        }
    ]
}
```       


<!--HONumber=Mar16_HO1-->


