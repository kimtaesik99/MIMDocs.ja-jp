---
タイトル: プロファイル データを取得
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 3eba062b-7adf-4766-9b94-cba1c7be2fd3
---
# プロファイル データを取得する
ユーザーのソフトウェアの証明書プロファイルの一覧と、現在のユーザーによって実行可能な操作の一覧を取得します。 指定した操作のいずれかに対して、要求を開始できます。

**注意**:暗証番号 (PIN) がサーバーで設定されるのは、プロファイル テンプレート ポリシーでそのように指定されている場合だけです。 指定されていない場合は、ユーザーが指定する必要があります。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles<br/>/CertificateManagement/api/v1.0/profiles/{id} <br/>/CertificateManagement/api/v1.0/requests/{requestid}/profiles

###URL パラメーター
パラメーター | 説明
---------|------------
id | 返すプロファイルの識別子 (GUID)。
requestId | プロファイルを返す要求の識別子。

###クエリ パラメーター
パラメーター | 説明
---------|------------
状態 | 任意。 データを取得するプロファイルの状態を示します。 可能な状態の種類は次のとおりです。Active、Approved、Canceled、Completed、Denied、Executing、Failed、None および Pending。 <br/>状態が指定されていない状態に関係なく、すべてのプロファイルが返されます。
###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###要求本文
なし

##[応答]
###応答コード
コード  |説明  
---------|---------
200 | OK
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###応答本文
成功すると、JSON でシリアル化された [Microsoft.Clm.Shared.Profiles.Profile](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.profiles.profile(v=vs.100%29.aspx) オブジェクトの一覧と次のプロパティが返されます。

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


##例

###要求
```
GET /certificatemanagement/api/v1.0/profiles?status=Active HTTP/1.1
```
###[応答]
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
##参照

- [Microsoft.Clm.Shared.Profiles.Profile Class](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.profiles.profile(v=vs.100%29.aspx)


<!--HONumber=Mar16_HO1-->


