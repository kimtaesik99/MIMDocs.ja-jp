---
タイトル: プロファイル テンプレートを取得します。
ms.custom: 
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: b7d8ed76-168b-4cb8-b87c-cdb0976c179a
---
# プロファイル テンプレートを取得する
指定したユーザーの登録に使用できるプロファイル テンプレートの一覧を取得します。 プロファイル テンプレートのビューの一部が返されます。 要求したユーザーは、返されるプロファイル テンプレート データを使用して、登録する必要があるプロファイル テンプレート (ある場合) を決定できます。 ワークフローとアクセス許可が指定されていない場合、ユーザーに表示可能なすべてのプロファイル テンプレートが返されます。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates?\[targetuser\] 

###URL パラメーター
パラメーター| 説明
--------|-------------
targetuser| 任意。 プロファイル テンプレートを返す対象ユーザーを指定します。 指定されていない場合は、現在のユーザーの ID が使用されます。 <br/>**注意**:現時点では、現在のユーザーのみがサポートされています。

###要求ヘッダー
一般的な要求ヘッダーについては、サービスのトピックを参照してください。
###要求本文
なし

##[応答]
###応答コード
コード  |説明  
---------|---------
200     | OK
204 | コンテンツはありません
500 | 内部エラー

###応答ヘッダー
一般的な応答ヘッダーについては、サービスのトピックを参照してください。
###応答本文
成功すると、次のプロパティの ProfileTemplateLimitedView オブジェクトの一覧が返されます。

プロパティ| 種類| 説明
--------|-----|--------
名前| string| プロファイル テンプレートの表示名。
説明| string| プロファイル テンプレートの説明。
Uuid| GUID| プロファイル テンプレートの識別子。
IsSmartcardProfileTemplate| ブール| テンプレートがスマート カード プロファイル テンプレートかどうかを示します。
IsVirtualSmartcardProfileTemplate| ブール| プロファイル テンプレートに仮想スマート カードが必要かどうかを示します。

##例

###要求
```
GET /certificatemanagement/api/v1.0/profiletemplates HTTP/1.1
```
###[応答]
```
HTTP/1.1 200 OK

[
    {
        "Name":"FIM CM Sample Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"12bd5120-86a2-4ee1-8d05-131066871578",
        "IsSmartcardProfileTemplate":false,
        "IsVirtualSmartcardProfileTemplate":false
    },
    {
        "Name":"FIM CM Sample Smart Card Logon Profile Template",
        "Description":"Description of the template goes here",
        "Uuid":"2b7044cf-aa96-4911-b886-177947e9271b",
        "IsSmartcardProfileTemplate":true,
        "IsVirtualSmartcardProfileTemplate":false
    }
]

```       


<!--HONumber=Mar16_HO1-->


