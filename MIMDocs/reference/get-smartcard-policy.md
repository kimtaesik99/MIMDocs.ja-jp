---
title: "スマート カード ポリシーの取得 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>スマート カード ポリシーの取得

指定したワークフローのプロファイル テンプレート ポリシーを取得します。 このデータは、要求の作成時に使用されます。 ワークフロー ポリシーには、クライアントが要求を作成するために必要なデータを指定します。 たとえば、さまざまなデータ収集項目、要求コメント、ワンタイム パスワード ポリシーなどのデータです。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1。0/profiletemplates/{id}/policy/workflow/{型}

###<a name="url-parameters"></a>URL パラメーター
パラメーター| 説明
--------|-------------
id| 必須。 ポリシーを抽出するプロファイル テンプレートに対応する GUID。
型| 必須。 要求されているポリシーの種類。 設定可能な値は、次のとおりです。 *Enroll*、 *Duplicate*、 *OfflineUnblock*、 *OnlineUpdate*、 *Renew*、 *Recover*、 *RecoverOnBehalf*、 *Reinstate*、 *Retire*、 *Revoke*、 *TemporaryEnroll*、 *Unblock*。

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
なし

##<a name="response"></a>[応答]
###<a name="response-codes"></a>応答コード
コード  |説明  
---------|---------
200     | OK
403 | 許可されていません
204 | コンテンツはありません
500 | 内部エラー

###<a name="response-headers"></a>応答ヘッダー
一般的な応答ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」を参照してください。
###<a name="response-body"></a>応答本文
成功した場合、[ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) オブジェクトに基づいてポリシー オブジェクトが返されます。 ポリシー オブジェクトには、少なくともは次の表のプロパティが含まれますが、要求されたポリシーに応じて他のプロパティも含まれる場合があります。 たとえば、登録ポリシーの要求では、[EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) オブジェクトが返されます。 詳細については、要求の {type} パラメーターに関連付けられたポリシー オブジェクトに関するドキュメントを参照してください。 さまざまな種類のポリシー オブジェクトのドキュメントは、[Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) ドキュメントで確認できます。

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

##<a name="example"></a>例

###<a name="request-1"></a>要求 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>応答 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>要求 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>応答 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>関連項目

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy クラス](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates 名前空間](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
