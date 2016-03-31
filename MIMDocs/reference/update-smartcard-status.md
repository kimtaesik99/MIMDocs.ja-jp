---
タイトル: スマート カードの状態の更新
ms.custom:
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: リファレンス
ms.assetid: 598dace3-c6f2-447a-9301-c0b63ee38276
---
# スマートカードの状態を更新する
スマートカードの状態を更新します。

**注意**:このトピックに示されている URL は、API の展開時に選択したホスト名 (例: `https://api.contoso.com`。
## 要求


方法  |[要求 URL]  
---------|---------
GET     |/CertificateManagement/api/v1.0/requests/{reqid}/smartcards/{scid}

### URL パラメーター
パラメーター | 説明
---------|------------
reqid | 必須。 要求識別子 (MIM CM 固有)。
scid | 必須。 スマートカード識別子 (MIM CM 固有)。 これは、[Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/en-us/library/microsoft.clm.shared.smartcards.smartcard(v=vs.100%29.aspx) オブジェクト内の “uuid” プロパティです。

### 要求ヘッダー
一般的な要求ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
### 要求本文
要求本文には、次のプロパティが含まれています。

プロパティ | 説明
---------|-----------
状態 | 要求に設定する状態。 "Retired" など。


## [応答]
### 応答コード
コード  |説明  
---------|---------
200     | OK
204 | コンテンツはありません
403 | 許可されていません
500 | 内部エラー

### 応答ヘッダー
一般的な応答ヘッダーについては、次を参照してください。 [HTTP 要求および応答ヘッダー](certificate-management-rest-api-service-details.md#HttpHeaders) で *CM REST API サービスの詳細*します。
### 応答本文
成功した場合、JSON シリアル化を返す [Microsoft.Clm.Shared.Smartcards.Smartcard](http://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx) 次のプロパティを持つオブジェクト。

名前 | 説明
-----|-----------
AssignedUserUuid | スマート カードが割り当てられるユーザーの識別子。
Atr | 現在初期化中のカードに対するスマート カード ATR (Answer To Reset) 文字列。
コメント | スマート カードを説明するコメント。
フラグ | スマート カードについて説明するフラグ。
ミドルウェア | スマート カードのミドルウェア。
ParentSmartcardUuid | スマート カードが置き換えた古いスマート カードの識別子。
PermanentSmartcardUuid | スマート カードに関連付けられた永続的なスマート カードの識別子。
PrimarySmartcardUuid | プライマリ スマート カードの識別子。
ProfileTemplateUuid | スマート カードを管理するポリシーと設定を含むプロファイル テンプレートの識別子。
ProfileTemplateVersion | スマート カード プロファイルの作成時点でのプロファイル テンプレートのバージョン。
SerialNumber | スマート カードのシリアル番号。
状態 | スマート カードの状態。
Uuid | スマート カード プロファイルの識別子。

## 例

### 要求
```
PUT /certificatemanagement/api/v1.0/requests/b105403d-d021-41ea-9f11-be3d677d229e/smartcards/17cf063d-e337-4aa9-a822-346554ddd3c9 HTTP/1.1

```
### [応答]
```
HTTP/1.1 200 OK

{
    "Uuid":"17cf063d-e337-4aa9-a822-346554ddd3c9",
    "Status":6,
    "Flags":1,
    "ParentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PrimarySmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "PermanentSmartcardUuid":"00000000-0000-0000-0000-000000000000",
    "AssignedUserUuid":"8f1590dc-d932-4b66-8e68-2e91c5880780",
    "ProfileTemplateUuid":"a039b4d0-5ce8-4eff-8651-181c6576fda3",
    "ProfileTemplateVersion":46,
    "Comment":"",
    "SerialNumber":"{bc88f13f-83ba-4037-8262-46eba1291c6e}",
    "Middleware":"MSBaseCSP",
    "Atr":"3b8d0180fba000000397425446590301c8"
}
```       
## 関連項目

- [Microsoft.Clm.Shared.Smartcards.Smartcard クラス](https://msdn.microsoft.com/library/microsoft.clm.shared.smartcards.smartcard.aspx))


<!--HONumber=Mar16_HO1-->


