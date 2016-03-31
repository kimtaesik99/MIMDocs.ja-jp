    ---
タイトル: 証明書要求の生成オプションを取得します。
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
---
# 証明書要求の生成オプションを取得する
クライアント側の証明書要求の生成のためのパラメーターを取得します。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions

###URL パラメーター
パラメーター | 説明
--------|--------------
requestid| 必須。 証明書の要求生成パラメーターを取得するための MIM CM 要求の GUID 識別子。

###要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
###要求本文
なし。


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
成功すると、CertificateRequestGenerationOptions オブジェクトの一覧が返されます。 CertificateRequestGenerationOptions オブジェクトはそれぞれ、単一の証明書の要求に対応しています。この要求は、クライアントが生成する必要があり、次のプロパティがあります。

プロパティ| 説明
--------|-----------
Exportable | 要求用に作成された秘密キーがエクスポート可能かどうかを指定する値。
FriendlyName | 登録済み証明書の表示名。
HashAlgorithmName | 証明書要求の署名を作成する際に使用されるハッシュ アルゴリズム。
KeyAlgorithmName | 公開キーのアルゴリズム。
KeyProtectionLevel | 強力なキーの保護レベル。
KeySize | 生成される秘密キーのサイズ (ビット)。
KeyStorageProviderNames | 秘密キーを生成するために使用可能なキー格納プロバイダー (KSP) の一覧。 最初の KSP を使用して証明書の要求を生成できない場合は、成功するまで指定した任意の KSP を使用することができます。
KeyUsages | この証明書の要求のために作成された秘密キーで実行可能な操作。 既定値は Signing です。
サブジェクト | サブジェクト名。

**注意**:これらのプロパティの詳細については、「 [Windows.Security.Cryptography.Certificates.CertificateRequestProperties クラス](https://msdn.microsoft.com/en-us/library/windows/apps/br212079.aspx) 」の説明を参照できますが、このクラスと CertificateRequestGenerationOptions オブジェクトは、一対一で対応していないことに注意してください。

##例

###要求
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###[応答]
```
HTTP/1.1 200 OK

[
    {
        "Subject":"",
        "KeyAlgorithmName":"RSA",
        "KeySize":2048,
        "FriendlyName":"",
        "HashAlgorithmName":"SHA1",
        "KeyStorageProviderNames":[
            "Contoso Smart Card Key Storage Provider"
        ],
        "Exportable":0,
        "KeyProtectionLevel":0,
        "KeyUsages":3
    }
]
```       


<!--HONumber=Mar16_HO1-->


