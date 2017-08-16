---
title: "プロファイル データを取得する | Microsoft Docs"
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
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 068497509b07b879fcadde6b60a9530d3d8db093
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-profile-data"></a>プロファイル データを取得する
ユーザーのソフトウェアの証明書プロファイルの一覧と、現在のユーザーによって実行可能な操作の一覧を取得します。 指定した操作のいずれかに対して、要求を開始できます。

**注意**:暗証番号 (PIN) がサーバーで設定されるのは、プロファイル テンプレート ポリシーでそのように指定されている場合だけです。 指定されていない場合は、ユーザーが指定する必要があります。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

###<a name="url-parameters"></a>URL パラメーター
パラメーター | 説明
---------|------------
id | 返すプロファイルの識別子 (GUID)。
requestId | プロファイルを返す要求の識別子。

###<a name="query-parameters"></a>クエリ パラメーター
パラメーター | 説明
---------|------------
状態 | 任意。 データを取得するプロファイルの状態を示します。 可能な状態の種類は次のとおりです。Active、Approved、Canceled、Completed、Denied、Executing、Failed、None および Pending。 <br/>状態が指定されていない場合は、状態に関係なく、すべてのプロファイルが返されます。
###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
なし

##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
200 | OK
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」を参照してください。
###<a name="response-body"></a>応答本文
成功すると、JSON でシリアル化された [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx) オブジェクトと次のプロパティの一覧が返されます。

プロパティ | 説明
---------|------------
AssignedUserUuid | プロファイルが割り当てられるユーザーの識別子。
コメント | プロファイルを説明するコメント。
フラグ | プロファイルを説明するフラグ。
ParentProfileUuid | プロファイルが置き換えられた古いプロファイルの識別子。
PrimaryProfileUuid | プライマリ プロファイルの識別子。
ProfileOperations | プロファイルで現在のユーザーが実行できる操作の一覧。
ProfileTemplateUuid | プロファイルを管理するポリシーと設定を含むプロファイル テンプレートの識別子。
ProfileTemplateVersion | プロファイルが作成されたときのプロファイル テンプレートのバージョン。
状態 | プロファイルの状態。
Uuid | プロファイルの識別子。


##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

[
    {
        "Uuid":"c0dd5c7d-ec35-4346-baca-3ad711e9722f",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"1c9e2606-fea2-4048-a6ac-b014e54c22df",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"5ad77b40-aa42-4533-9396-c9c59fd021a8",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"8f31803f-8afc-49bb-911d-402ec264b589",
        "ProfileTemplateVersion":8,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable",
            "recover"
        ]
    },
    {
        "Uuid":"ff342953-c444-4dc7-b144-f5515d6460c6",
        "Status":2,
        "Flags":1,
        "ParentProfileUuid":"00000000-0000-0000-0000-000000000000",
        "PrimaryProfileUuid":"00000000-0000-0000-0000-000000000000",
        "AssignedUserUuid":"0378a33b-8650-4713-a727-efd233903643",
        "ProfileTemplateUuid":"1e3a31fe-699b-4a6b-945c-18b83c985bc1",
        "ProfileTemplateVersion":9,
        "Comment":"",
        "ProfileOperations":[
            "renew",
            "disable"
        ]
    }
]
```       
##<a name="see-also"></a>関連項目

- [Microsoft.Clm.Shared.Profiles.Profile クラス](https://msdn.microsoft.com/library/microsoft.clm.shared.profiles.profile.aspx)
