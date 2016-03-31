---
タイトル: PAM ロールを取得します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: d3c4f528-c3c8-41c1-905e-7eb84f074ce4
---
# PAM ロールを取得する
特権アカウントが、そのアカウントが候補になる PAM ロールの一覧を取得するときに使用します。

**注**: このトピックに示すように Url が API の展開時に選択したホスト名はたとえば: `http://api.contoso.com`です。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/api/pamresources/pamroles

###クエリ パラメーター
パラメーター | 説明
----------|--------------
$filter | 任意。 フィルター式で任意の PAM ロールのプロパティを指定して、フィルターされた応答のリストを返します。 サポートされている演算子の詳細については、次を参照してください [PAM REST API サービスの詳細でフィルター処理。](privileged-access-management-rest-api-service-details.md#Filtering)
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
成功応答には、1 つ以上の PAM ロールの収集が含まれます。各ロールには次のプロパティが含まれます。

プロパティ | 説明
--------|-------------
RoleID | PAM ロールの一意の識別子 (GUID)。
表示名 | MIM サービスの PAM ロールの表示名。
説明 | MIM サービスの PAM ロールの説明。
TTL | ロールのアクセス権の最長有効期限タイムアウト (秒)。
AvailableFrom | 要求がアクティブになる最も早い時刻。
AvailableTo | 要求がアクティブになる最も遅い時刻。
MFAEnabled | このロールのアクティブ化要求に MFA チャレンジが必要かどうか示すブール値。
ApprovalEnabled | このロールのアクティブ化要求にロール所有者の承認が必要かどうか示すブール値。
AvailibitlyWHTTP の要求ヘッダーおよび応答ヘッダーdowEnabled | 指定した期間中にのみロールをアクティブ化できるかどうかを示すブール値。

##例

###要求
```
GET /api/pamresources/pamroles HTTP/1.1
```
###[応答]
```
HTTP/1.1 200 OK

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamroles",
    "value":[
        {
            "RoleId":"8f5cec1a-ecba-42ec-b76d-e6e0e4bf4c62",
            "DisplayName":"Allow AD Access ",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":false,
            "AvailabilityWindowEnabled":false
        },
        {
            "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
            "DisplayName":"ApprovalRole",
            "Description":null,
            "TTL":"3600",
            "AvailableFrom":"0001-01-01T00:00:00",
            "AvailableTo":"0001-01-01T00:00:00",
            "MFAEnabled":false,
            "ApprovalEnabled":true,
            "AvailabilityWindowEnabled":false
        }
    ]
}
```       


<!--HONumber=Mar16_HO1-->


