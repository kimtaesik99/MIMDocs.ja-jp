---
title: "Cancel、Abandon、または Complete a Request | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: reference
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: e29e0068-7602-455e-8a3a-690da9ca8eb5
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 4f81c9d86006c477993b2ac8cc2e845de2c1d2f3
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="cancel-abandon-or-complete-a-request"></a>Cancel、Abandon、または Complete a Request
MIM CM 要求に完了、取り消し、または中止とマークします。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
PUT     |/CertificateManagement/api/v1.0/requests/{id}

###<a name="url-parameters"></a>URL パラメーター
プロパティ| 説明
---------|--------
id| 必須。 完了する要求の GUID。


###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
要求本文には、次のプロパティが含まれています。

プロパティ | 説明
---------|-----------
状態 | 要求に設定する状態。 設定可能な値は、次のとおりです。Completed、Canceled、または Abandoned。


##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
200     | OK
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」を参照してください。
###<a name="response-body"></a>応答本文
成功すると、次のような完了とマークされた MIM CM 要求を示すプロパティの [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx) オブジェクトが返されます。

名前 | 説明
-----|------------
備考 | MIM CM 要求に関連付けられているコメント。
Completed | MIM CM 要求が完了した時刻。
DataCollection | MIM CM 要求に関連付けられているデータ収集項目。
DataCollectionフラグ | MIM CM 要求のデータ収集のオプション。
フラグ | MIM CM 要求に関連付けられているオプション。
IsDataCollectionComplete | MIM CM 要求のデータ収集が完了したかどうかを示すブール値。
IsEnrollmentAgent | MIM CM 要求を実行するために登録エージェントが必要かどうかを示すブール値。
IsSmartcard | MIM CM 要求がスマート カード要求かソフトウェア プロファイル要求かを示すブール値。
NewProfileUuid | MIM CM 要求によって生成される新しいソフトウェア プロファイルの ID。
NewSmartcardUuid | MIM CM 要求によって割り当てられる新しいスマート カードの ID。
OldProfileUuid | MIM CM 要求が作成されたソフトウェア プロファイルの ID。
OldSmartcardUuid | MIM CM 要求が作成されたスマート カードの ID。
OrigHTTP の要求ヘッダーおよび応答ヘッダーatorUserUuid | MIM CM 要求を開始したユーザーの ID
優先順位 | MIM CM 要求の優先度。
ProfileTemplateUuid | MIM CM 要求が作成されたプロファイル テンプレートの ID。
要求Type | MIM CM 要求の種類。
SecurityDescriptor | MIM CM 要求のセキュリティ記述子。
状態 | MIM CM 要求の状態。
Submitted | MIM CM 要求が送信された時刻。
TargetUserUuid | MIM CM 要求の対象ユーザーの ID。
Uuid | MIM CM 要求の識別子。

##<a name="example"></a>例

###<a name="request"></a>要求
```
PUT /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099 HTTP/1.1


{
    “status”:”Completed”
}
```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

{
    "Uuid":"a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099",
    "RequestType":1,
    "Status":8,
    "Flags":2,
    "DataCollection":[

    ],
    "DataCollectionFlags":32,
    "OriginatorUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "TargetUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "Submitted":"2015-07-07T23:36:21.93Z",
    "Completed":"2015-07-07T23:37:37.767Z",
    "NewProfileUuid":"b99ff38c-6653-471f-ae3c-325bb351a6bc",
    "OldProfileUuid":"00000000-0000-0000-0000-000000000000",
    "NewSmartcardUuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "OldSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "Priority":0,
    "Comment":"",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "SecurityDescriptor":"O:S-1-5-21-2127521184-1604012920-1887927527-14134865D:(D;;LC;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5094534)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-5219125)(A;;SWRC;;;S-1-5-21-2127521184-1604012920-1887927527-5094533)(A;;RC;;;WD)(A;;CCDCLCSWSDRC;;;S-1-5-21-2127521184-1604012920-1887927527-14134865)(A;;DCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)(A;;CCDCSW;;;S-1-5-21-2127521184-1604012920-1887927527-14995557)\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000\u0000",
    "IsSmartcard":true,
    "IsEnrollmentAgent":false,
    "IsDataCollectionComplete":false
}
```       
##<a name="see-also"></a>関連項目

- [Microsoft.Clm.Provision.ExecuteOperations.Complete メソッド](https://msdn.microsoft.com/library/microsoft.clm.provision.executeoperations.complete.aspx)
- [Microsoft.Clm.Shared.Requests.Request](https://msdn.microsoft.com/library/microsoft.clm.shared.requests.request.aspx)
