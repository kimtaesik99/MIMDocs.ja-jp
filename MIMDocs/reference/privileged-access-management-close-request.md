---
タイトル: PAM 要求を閉じる
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: ca3a1a68-9a2b-47da-bfc1-eaa360cbe609
---
# PAM 要求を閉じる
特権アカウントが、PAM ロールへの昇格を開始した要求を終了するときに使用します。

**注**: このトピックに示すように Url が API の展開時に選択したホスト名はたとえば: `http://api.contoso.com`です。
##要求


方法  |[要求 URL]  
---------|---------
POST     |/api/pamresources/pamrequests({requestId)/Close

###URL パラメーター
パラメーター | 説明
----------|-----------
requestId | 終了する PAM 要求の識別子 (GUID) は、 `guid'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'`

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
なし
##例

###要求
```
POST /api/pamresources/pamrequests(guid'5ec10e61-cdd1-404e-a18e-740467d87dbf')/Close HTTP/1.1


```
###[応答]
```
HTTP/1.1 200 OK

```       


<!--HONumber=Mar16_HO1-->


