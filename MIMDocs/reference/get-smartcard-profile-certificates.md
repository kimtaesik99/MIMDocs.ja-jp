---
タイトル: スマート カードまたはプロファイルの証明書の取得
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 206bde70-4af8-46fa-9c0b-f574745b0977
---
# スマートカードまたはプロファイルの証明書の取得
特定のスマートカードまたはソフトウェアのプロファイルに関連付けられている証明書の一覧を取得します。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiles/{id}/certificates <br/>/CertificateManagement/api/v1.0/smartcards/{id}/certificates

###URL パラメーター
パラメーター | 説明
---------|------------
id | プロファイルまたはスマートカードの識別子 (GUID)。

###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###要求本文
なし

##[応答]
###応答コード
コード  |説明  
---------|---------
200     | OK
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

###応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###応答本文
成功すると、JSON でシリアル化された [Microsoft.Clm.Shared.Certificates.X509ClmCertificate] (https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.certificates.x509clmcertificate (v=vs.100%29.aspx) オブジェクトの一覧と次のプロパティが返されます。

名前 | 説明
-----|------------
ArchivedOnCa | 証明機関 (CA) に証明書がアーカイブされているかどうかを示すブール値。
CertificateType | 証明書のタイプ。
IsKeyHistory | 証明書がキー履歴証明書かどうかを示すブール値。
SerialNumber | 証明書のシリアル番号。
TemplateCommon名前 | 証明書の証明書テンプレートの共通名。
ThumbprHTTP の要求ヘッダーおよび応答ヘッダーt | 証明書の拇印。

##例

###要求
```
GET /certificatemanagement/api/v1.0/smartcards/5badfea3-de31-4837-99f9-8249515a5473/certificates HTTP/1.1
```
###[応答]
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
##参照

- [Microsoft.Clm.Provision.FHTTP の要求ヘッダーおよび応答ヘッダーdOperations.FHTTP の要求ヘッダーおよび応答ヘッダーdCertificates 方法](https://msdn.microsoft.com/en-us/library/microsoft.clm.provision.fHTTP の要求ヘッダーおよび応答ヘッダーdoperations.fHTTP の要求ヘッダーおよび応答ヘッダーdcertificates(v=vs.100%29.aspx)
- [Microsoft.Clm.Shared.Certificates.X509ClmCertificate Class](https://msdn.microsoft.com/en-us/library/microsoft.clm.shared.certificates.x509clmcertificate(v=vs.100%29.aspx)


<!--HONumber=Mar16_HO1-->


