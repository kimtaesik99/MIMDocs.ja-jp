---
title: "証明書要求の生成オプションを取得する | Microsoft Docs"
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
ms.assetid: 36bd1fc9-3443-4028-90e7-a24fef0ec0ae
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 5822da1b9cf8558becccff815fd0208923fcaaa0
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="get-certificate-request-generation-options"></a>証明書要求の生成オプションを取得する

クライアント側の証明書要求の生成のためのパラメーターを取得します。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
##<a name="request"></a>要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{requestid}/certificaterequestgenerationoptions

###<a name="url-parameters"></a>URL パラメーター
パラメーター | 説明
--------|--------------
requestid| 必須。 証明書の要求生成パラメーターを取得するための MIM CM 要求の GUID 識別子。

###<a name="request-headers"></a>要求ヘッダー
一般的な要求ヘッダーについては、「*CM REST API サービスの詳細*」の「[HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#http-request-and-response-headers)」をご覧ください。
###<a name="request-body"></a>要求本文
なし。


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

**注意**: これらのプロパティの詳細については、[Windows.Security.Cryptography.Certificates.CertificateRequestProperties クラス](https://msdn.microsoft.com/library/windows/apps/br212079.aspx)に関する説明でご覧いただけますが、このクラスと CertificateRequestGenerationOptions オブジェクトは、一対一で対応していないことに注意してください。

##<a name="example"></a>例

###<a name="request"></a>要求
```
GET /certificatemanagement/api/v1.0/requests/a9b4b42c-cc50-4c9b-89d1-bbc0bcd5a099/certificaterequestgenerationoptions HTTP/1.1

```
###<a name="response"></a>[応答]
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
