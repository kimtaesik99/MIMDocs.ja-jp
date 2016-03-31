---
タイトル: PAM 要求を作成します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: fe8b3374-9d32-4cc3-9328-f1eafeadfe8e
---
# PAM 要求を作成する
特権アカウントが PAM ロールに昇格するために使用します。

**注**: このトピックに示すように Url が API の展開時に選択したホスト名はたとえば: `http://api.contoso.com`です。
##要求


方法  |[要求 URL]  
---------|---------
POST     |/api/pamresources/pamrequests

###クエリ パラメーター
パラメーター | 説明
--------|-------------
Justification | 任意。 昇格要求にユーザーが指定した理由。
RoleId| 必須。 昇格する PAM ロールの一意の識別子 (GUID)。
要求edTTL| 必須。 要求の有効期限 (秒)。
要求edTime | 省略可能。 特権を昇格する時間。  
v | 任意。 API のバージョン。 指定しない場合は、API の現在 (最新リリース) のバージョンが使用されます。 詳細については、次を参照してください [PAM REST API サービスの詳細でのバージョン管理。](privileged-access-management-rest-api-service-details.md#Versioning)

**注意**: *Justification*、 *RoleId*、 *RequestedTTL*、および *RequestedTime* パラメーターは、クエリ パラメーターとしてではなく、要求本文のプロパティとして指定できます。  *v* パラメーターは、クエリ パラメーターとしてのみ指定できます。

###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](privileged-access-management-rest-api-service-details.md#HttpHeaders) で *PAM REST API サービスの詳細*します。
###要求本文
任意。 前述のように、 *Justification*、 *RoleId*、 *RequestedTTL*、および *RequestedTime* パラメーターは、URL クエリ文字列で指定するのではなく、要求本文のプロパティとして指定できます。

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
成功応答には、次のプロパティの PAM 要求オブジェクトが含まれます。

プロパティ | 説明
--------|-------------
要求ID | PAM 要求の一意の識別子 (GUID)。
CreatorID | 要求を作成したアカウントの MIM サービスの一意の識別子 (GUID)。
Justification | 昇格の理由。
表示名 | MIM での PAM 要求の表示名。
CreationTime | 要求の作成時間。
Creation方法 | 要求の作成に使用されるメソッド。
ExpirationTime | 要求の有効期限。
RoleID| PAM ロールの一意の識別子 (GUID)。
要求edTTL | 要求の有効期限タイムアウト (秒)。
要求edTime | 昇格の要求時間。
要求edStatus | 要求の状態。 設定可能な値は、次のとおりです。"Active"、"Closed"、"Closing"、"Expired"、"Pending Approval"、"Rejected"。

##例

###要求 1
```
POST /api/pamresources/pamrequests?Justification=Sample+Reason&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=7200&RequestedTime=2015%2F07%2F11+23%3A40 HTTP/1.1
```
###応答 1
```
HTTP/1.1 201 Created

{  
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"c0112f13-b16b-40ad-b547-07f23a7fba52",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":"Sample Reason",
    "CreationTime":"2015-07-11T23:38:09.036164-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"7200",
    "RequestedTime":"2015-07-12T06:40:00Z",
    "RequestStatus":"PendingApproval"
}
```       

###要求 2
```
POST /api/pamresources/pamrequests?Justification=&RoleId=c28eab4a-95cf-4c08-a153-d5e8a9e660cd&RequestedTTL=3600&RequestedTime= HTTP/1.1
```
###応答 2
```
HTTP/1.1 201 Created

{
    "odata.metadata":"http://localhost:8086/api/pamresources/%24metadata#pamrequests/@Element",
    "RequestId":"504f9c49-00db-42bd-a157-ee5664617189",
    "CreatorID":"73257e5e-00b3-4309-a330-f1e607ff113a",
    "Justification":null,
    "CreationTime":"2015-07-11T23:07:30.2200123-07:00",
    "CreationMethod":"PAM Web API",
    "ExpirationTime":"0001-01-01T00:00:00",
    "RoleId":"c28eab4a-95cf-4c08-a153-d5e8a9e660cd",
    "RequestedTTL":"3600",
    "RequestedTime":"2015-07-12T06:07:27.7229894Z",
    "RequestStatus":"PendingApproval"
}
```       


<!--HONumber=Mar16_HO1-->


