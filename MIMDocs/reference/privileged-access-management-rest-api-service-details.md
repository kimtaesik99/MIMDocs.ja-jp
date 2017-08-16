---
title: "PAM REST API サービスの詳細 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>PAM REST API サービスの詳細
次のセクションでは、Microsoft Identity Manager (MIM) Privileged Access Management (PAM) REST API の詳細について説明します。

## <a name="http-request-and-response-headers"></a>HTTP 要求および応答ヘッダー

API に送信された HTTP 要求には、次のヘッダーを含める必要があります (このリストは完全ではありません)。

ヘッダー | 説明
-------|------------
承認 | 必須。 コンテンツは認証方法に依存します。認証方法は構成可能で、WIA (Windows 統合認証) または ADFS に基づくことができます。
Content-Type | 要求本文が含まれる場合は必須。 "application/json" である必要があります。
Content-Length | 要求本文が含まれる場合は必須。 
Cookie | セッションの Cookie。 認証方法によって必要な場合があります。
<br/>
HTTP 応答には、次のヘッダーが含まれます (このリストは完全ではありません)。

Header | 説明
-------|------------
Content-Type | この API は、常に "application/json" を返します。
Content-Length | 要求本文がある場合は、その長さ (バイト)。

## <a name="versioning"></a>バージョン管理 
API の現在のバージョンは 1 です。 API バージョンは、次の例のように、要求 URL でクエリ パラメーターを通じて指定できます: `http://localhost:8086/api/pamresources/pamrequests?v=1`。バージョンが要求で指定されていない場合、一番新しくリリースされたバージョンの API に対して要求が実行されます。 

## <a name="security"></a>セキュリティ 
API へのアクセスには、統合 Windows 認証 (IWA) が必要です。 これは Microsoft Identity Manager (MIM) をインストールする前に、IIS で手動で構成する必要があります。

HTTPS (TLS) はサポートされていますが、IIS で手動で構成する必要があります。 詳細については、「**Implement Secure Sockets Layer (SSL) for the FIM Portal (FIM ポータルに Secure Sockets Layer (SSL) を実装する)**」をご覧ください。これは、テスト ラボ ガイドの FIM 2010 R2 のインストールに関する [Step 9: Perform FIM 2010 R2 Post-Installation Tasks (手順 9: FIM 2010 R2 のインストール後のタスクを実行する)] (https://technet.microsoft.com/library/hh322875(v=ws.10%29.aspx) にあります。 

Visual Studio コマンド プロンプトで、次のコマンドを実行して、新しい SSL サーバー証明書を生成できます。
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
このコマンドにより、URL が test.cwap.com の Web サーバーで SSL を使用する Web アプリケーションをテストするのに使用できる自己署名証明書が作成されます。 -eku オプションで定義される OID は、証明書を SSL サーバー証明書として識別します。 証明書は My ストアに格納され、コンピューター レベルで使用できるため、mmc.exe で証明書スナップインからエクスポートすることができます。

## <a name="cross-domain-access-cors"></a>クロス ドメイン アクセス (CORS) 
CORS はサポートされていますが、IIS で手動で構成する必要があります。 展開済みの API web.config ファイルに次の要素を追加して、クロス ドメインの呼び出しができるように API を構成します。 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>エラー処理 
API は、エラー状態を示すために HTTP エラー応答を返します。 エラーは、OData 準拠です。 次の表に、クライアントに返される可能性があるエラー コードを示します。

HTTP 状態コード | 説明
-----------------|------------
401 | 未承認 
403 | 許可されていません 
408 | 要求のタイムアウト   
500 | 内部サーバー エラー 
503 | サービス利用不可 
<br/>

## <a name="filtering"></a>フィルター 
PAM REST API 要求には、応答に含める必要があるプロパティを指定するフィルターを含めることができます。 フィルター構文は、OData の式に基づいています。

フィルターでは、PAM 要求、PAM ロール、 または保留中の PAM 要求の任意のプロパティを指定できます。 たとえば、次のように入力します。 *ExpirationTime*、 *DisplayName*、または PAM 要求、PAM ロール、保留中の要求のいずれかで、他の有効なプロパティを指定します。

API は、フィルター式で次の演算子の使用をサポートしています。 *And*、 *Equal*、 *NotEqual*、 *GreaterThan*、 *LessThan*、 *GreaterThenOrEqueal*、 *LessThanOrEqual*。 

次の要求例には、次のフィルターが含まれています。

- この要求は、指定した期間のすべての PAM 要求を返します: `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- この要求は、表示名が "SQL File Access" の PAM ロールが返されます: `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
