---
title: "スマート カードまたはプロファイルの証明書の取得 | Microsoft Docs"
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
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 786562f1a6ffd0dcaefd7b11af8756b08727ecae
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-or-profile-certificates"></a>スマートカードまたはプロファイルの証明書の取得
特定のスマートカードまたはソフトウェアのプロファイルに関連付けられている証明書の一覧を取得します。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/certificates <br/>/CertificateManagement/api/v1.0/smartcards/{id}/certificates

###<a name="url-parameters"></a>URL パラメーター
パラメーター | 説明
---------|------------
id | プロファイルまたはスマートカードの識別子 (GUID)。

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
なし

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
成功すると、JSON でシリアル化された [Microsoft.Clm.Shared.Certificates.X509ClmCertificate](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx) オブジェクトと次のプロパティの一覧が返されます。

名前 | 説明
-----|------------
ArchivedOnCa | 証明機関 (CA) に証明書がアーカイブされているかどうかを示すブール値。
CertificateType | 証明書のタイプ。
IsKeyHistory | 証明書がキー履歴証明書かどうかを示すブール値。
SerialNumber | 証明書のシリアル番号。
TemplateCommonName | 証明書の証明書テンプレートの共通名。
ThumbprHTTP の要求ヘッダーおよび応答ヘッダーt | 証明書の拇印。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```
###<a name="response"></a>[応答]
```
HTTP/1.1 200 OK

[
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01052AFA01313FB77AC00010000B010",
        "Thumbprint":"C52B0C5FB8AAD31A5B239FF2712ED14122D67D30"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B011AB48AE7D664ED5D900010000B011",
        "Thumbprint":"E7C4324896271BE869544FF28AA2B1BF3B3BDFCF"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B01ABCF2D38A0CCEAD8F00010000B01A",
        "Thumbprint":"D9293B5414C644888444541B64631E90F2612425"
    },
    {
        "IsKeyHistory":false,
        "ArchivedOnCa":false,
        "CertificateType":1,
        "TemplateCommonName":"ContosoVirtualSmartCardLimitedRelease-KSP",
        "SerialNumber":"1B0000B1F02865CF2C3A9DD96D00010000B1F0",
        "Thumbprint":"6615DDC8603DBA789D724502627682F37D0FC2D0"
    }
]
```       
##<a name="see-also"></a>関連項目

- [Microsoft.Clm.Provision.FindOperations.FindCertificates メソッド](https://msdn.microsoft.com/library/microsoft.clm.provision.findoperations.findcertificates.aspx)
- [Microsoft.Clm.Shared.Certificates.X509ClmCertificate クラス](https://msdn.microsoft.com/library/microsoft.clm.shared.certificates.x509clmcertificate.aspx)
