---
title: "要求の作成 | Microsoft Docs"
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
ms.assetid: 80fe0656-6fb2-400c-9ef8-5f62b61b2a1b
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: d5af8767583cb6999de399de58b321147846291a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="create-request"></a>要求の作成
MIM CM 要求を作成します。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
POST     |/CertificateManagement/api/v1.0/requests

###<a name="url-parameters"></a>URL パラメーター
なし

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
要求本文には、次のプロパティが含まれています。

プロパティ | 説明
---------|-----------
profiletemplateuuid | 必須。 ユーザーが要求を作成するプロファイル テンプレートの GUID です。
datacollection | 必須。 登録者によって指定されるデータを表す、名前と値のペアのコレクションです。 指定しなければならない必要データのコレクションは、プロファイル テンプレートのワークフロー ポリシーから取得できます。 空のコレクションを指定できます。
target | 任意。 要求を作成する対象であるターゲット ユーザーの GUID です。 指定しない場合、既定値は現在のユーザーとなります。
型 | 必須。 作成される要求の種類です。 使用可能な要求の種類は、次のとおりです。"Enroll"、"Duplicate"、"OfflineUnblock"、"OnlineUpdate"、"Renew"、"Recover"、"RecoverOnBehalf"、"Reinstate"、"Retire"、"Revoke"、"TemporaryCards"、"Unblock"。<br/>**注意**:すべてのプロファイル テンプレートで、すべての種類の要求がサポートされているわけではありません。 たとえば、ソフトウェア プロファイル テンプレートで、ブロック解除操作を指定することはできません。
コメント | 必須。 ユーザーが入力する場合があるコメントです。 ワークフロー ポリシーによって、これが必要であるかどうかが定義されます。 空の文字列を指定することができます。
priority | 任意。 要求の優先度です。 指定しない場合、既定の要求優先度 (プロファイル テンプレート設定で決定される) が使用されます。


##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
201     | 作成日
403 | 許可されていません
500 | 内部エラー

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」を参照してください。
###<a name="response-body"></a>応答本文
成功すると、新しく作成された要求の URI を返します。
##<a name="example"></a>例

###<a name="request-1"></a>要求 1
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{
    "datacollection":"[]",
    "type":"Enroll",
    "profiletemplateuuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "comment":""
}
```
###<a name="response-1"></a>応答 1
```
HTTP/1.1 201 Created

"api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099"
```
###<a name="request-2"></a>要求 2
```
POST /CertificateManagement/api/v1.0/requests HTTP/1.1

{  
    "datacollection":"[]",
    "type":"Unblock",
    "smartcard":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "comment":""
}
```
###<a name="response-2"></a>応答 2
```
HTTP/1.1 201 Created

"api/v1.0/requests/0c96d73f-967b-420e-854a-43ad2a1504bc"
```       

###<a name="request-3"></a>要求 3
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
##<a name="see-also"></a>関連項目

- [Microsoft.Clm.Provision.RequestOperations.InitiateEnroll メソッド](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateenroll.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateOfflineUnblock メソッド](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateofflineunblock.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRecover メソッド](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiaterecover.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateRetire メソッド](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateretire.aspx)
- [Microsoft.Clm.Provision.RequestOperations.InitiateUnblock メソッド](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.provision.requestoperations.initiateunblock.aspx)
