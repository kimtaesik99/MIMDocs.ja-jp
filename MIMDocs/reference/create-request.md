---
タイトル: 要求の作成
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
---
# 要求の作成
MIM CM 要求を作成します。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###URL パラメーター
なし

###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###要求本文
要求本文には、次のプロパティが含まれています。

プロパティ | 説明
---------|-----------
profiletemplateuuid | 必須。 ユーザーが要求を作成するプロファイル テンプレートの GUID です。
datacollection | 必須。 登録者によって指定されるデータを表す、名前と値のペアのコレクションです。 指定しなければならない必要データのコレクションは、プロファイル テンプレートのワークフロー ポリシーから取得できます。 空のコレクションを指定できます。
target | 任意。 要求を作成する対象であるターゲット ユーザーの GUID です。 指定しない場合、既定値は現在のユーザーとなります。
型 | 必須。 作成される要求の種類です。 使用可能な要求の種類:「登録」、"Duplicate"、"OfflineUnblock"、"OnlineUpdate"、"Renew"、"Recover"、"RecoverOnBehalf"、"Reinstate"、"Retire"、"Revoke"、"TemporaryCards"、"Unblock"。<br/>**注意**:すべてのプロファイル テンプレートで、すべての種類の要求がサポートされているわけではありません。 たとえば、ソフトウェア プロファイル テンプレートで、ブロック解除操作を指定することはできません。
コメント | 必須。 ユーザーが入力する場合があるコメントです。 ワークフロー ポリシーによって、これが必要であるかどうかが定義されます。 空の文字列を指定することができます。
priority | 任意。 要求の優先度です。 指定しない場合、既定の要求優先度 (プロファイル テンプレート設定で決定される) が使用されます。


##[応答]
###応答コード
コード  |説明  
---------|---------
201     | 作成日
403 | 許可されていません
500 | 内部エラー

###応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###応答本文
成功すると、新しく作成された要求の URI を返します。
##例

###要求 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###応答 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###要求 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###応答 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###要求 3
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "profiletemplateuuid" : "97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
    "datacollection":
    [
        {"name" : "pavle"},
        {"city" : "seattle"}
    ],
    "target" : "97CC3493-F556-4C9B-9D8B-982434201527",
    "type" : "Enroll",
    "comment" : "LALALALA",
    "priority" :  "4"
}
```
##参照

- [Microsoft.Clm.Provision.要求Operations.InitiateEnroll 方法](https://msdn.microsoft.com/en-us/library/wHTTP の要求ヘッダーおよび応答ヘッダーdows/desktop/microsoft.clm.provision.requestoperations.HTTP の要求ヘッダーおよび応答ヘッダーitiateenroll(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.要求Operations.InitiateOfflHTTP の要求ヘッダーおよび応答ヘッダーeUnblock 方法](https://msdn.microsoft.com/en-us/library/wHTTP の要求ヘッダーおよび応答ヘッダーdows/desktop/microsoft.clm.provision.requestoperations.HTTP の要求ヘッダーおよび応答ヘッダーitiateofflHTTP の要求ヘッダーおよび応答ヘッダーeunblock(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.要求Operations.InitiateRecover 方法](https://msdn.microsoft.com/en-us/library/wHTTP の要求ヘッダーおよび応答ヘッダーdows/desktop/microsoft.clm.provision.requestoperations.HTTP の要求ヘッダーおよび応答ヘッダーitiaterecover(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.要求Operations.InitiateRetire 方法](https://msdn.microsoft.com/en-us/library/wHTTP の要求ヘッダーおよび応答ヘッダーdows/desktop/microsoft.clm.provision.requestoperations.HTTP の要求ヘッダーおよび応答ヘッダーitiateretire(v=vs.100%29.aspx)
- [Microsoft.Clm.Provision.要求Operations.InitiateUnblock 方法](https://msdn.microsoft.com/en-us/library/wHTTP の要求ヘッダーおよび応答ヘッダーdows/desktop/microsoft.clm.provision.requestoperations.HTTP の要求ヘッダーおよび応答ヘッダーitiateunblock(v=vs.100%29.aspx)


<!--HONumber=Mar16_HO1-->


