---
タイトル: ワークフロー ポリシーを取得します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: be636205-c1f0-457c-982e-e17478cf0889
---
# ワークフロー ポリシーを取得する
指定したワークフローのプロファイル テンプレート ポリシーを取得します。 このデータは、要求の作成時に使用されます。 ワークフロー ポリシーには、クライアントが要求を作成するために必要なデータを指定します。 たとえば、さまざまなデータ収集項目、要求コメント、ワンタイム パスワード ポリシーなどのデータです。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1。0/profiletemplates/{id}/policy/workflow/{型}

###URL パラメーター
パラメーター| 説明
--------|-------------
id| 必須。 ポリシーを抽出するプロファイル テンプレートに対応する GUID。
型| 必須。 要求されているポリシーの種類。 設定可能な値は、次のとおりです。 *Enroll*、 *Duplicate*、 *OfflineUnblock*、 *OnlineUpdate*、 *Renew*、 *Recover*、 *RecoverOnBehalf*、 *Reinstate*、 *Retire*、 *Revoke*、 *TemporaryEnroll*、 *Unblock*。

###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###要求本文
なし

##[応答]
###応答コード
コード  |説明  
---------|---------
200     | OK
403 | 許可されていません
204 | コンテンツはありません
500 | 内部エラー

###応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###応答本文
成功時には、[ProfileTemplatePolicy](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy(v=vs.100%29.aspx) オブジェクトに基づいてポリシー オブジェクトが返されます。 ポリシー オブジェクトには、少なくともは次の表のプロパティが含まれますが、要求されたポリシーに応じて他のプロパティも含まれる場合があります。 たとえば、登録ポリシーの要求で、[EnrollPolicy](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy(v=vs.100%29.aspx) オブジェクトが返されます。 詳細については、要求の {type} パラメーターに関連付けられたポリシー オブジェクトに関するドキュメントを参照してください。 各種ポリシー オブジェクトのドキュメントについては、[Microsoft.Clm.Shared.ProfileTemplates 名前空間](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates(v=vs.100%29.aspx) のドキュメントを参照してください。

プロパティ | 説明
---------|------------
ApprovalsNeeded | ポリシーの FIM CM 要求に必要な承認数。
AuthorizedApprover | ポリシーの FIM CM 要求を承認する権限を持つユーザーのセキュリティ記述子。
AuthorizedEnrollmentAgent | ポリシーの登録エージェントとして操作できるユーザーのセキュリティ記述子。
AuthorizedInitiator | ポリシーの FIM CM 要求を開始できるユーザーのセキュリティ記述子。
CollectComments | ポリシーの FIM CM 要求のコメント収集が有効かどうかを示すブール値。
Collect要求Priority | ポリシーの FIM CM 要求の優先度収集が有効かどうかを示すブール値。
Default要求Priority | ポリシーの FIM CM 要求の既定の優先度。
ドキュメント | ポリシーに構成されているポリシー ドキュメント。
Enabled | ポリシーが有効かどうかを示すブール値。
EnrollAgentRequired | 登録エージェントがポリシーの FIM CM 要求に必要かどうかを示すブール値。
OneTimePasswordPolicy | ポリシーの FIM CM 要求のワンタイム パスワードを配布する方法を取得します。
個人設定 | ポリシーのスマート カードのパーソナライズ オプション。
PolicyDataCollection | ポリシーに関連付けられているデータ収集項目。
SelfServiceEnabled | ポリシーの FIM CM 要求のセルフサービス開始が有効かどうかを示すブール値。

##例

###要求 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###応答 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###要求 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###応答 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##参照

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy クラス](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates 名前空間](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.clm.shared.profiletemplates(v=vs.100%29.aspx)


<!--HONumber=Mar16_HO1-->


