---
タイトル: サンプル登録チュートリアル
ms.custom: 
  - MIM
ms.prod: identity manager 2015
ms.reviewer: na
ms.suite: na
ms.technology: 
  - セキュリティ
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: 92b97803-9475-4b90-9a6c-430f107a167d
---
# サンプル登録チュートリアル
このトピックでは、仮想スマートカードのセルフサービス登録を実行するための手順を説明します。 自動承認の要求とユーザー設定の PIN を示します。
1.  クライアントは、ユーザーを認証した後、認証済みユーザーが登録できるプロファイル テンプレートの一覧を要求します。

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates. 
    ```
    <br/>
2.  クライアントは、結果の一覧をユーザーに対して表示します。 ユーザーは、名前が “Virtual Smartcard VPN” で UUID が *97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1*の vSC (仮想スマートカード) プロファイル テンプレートを選択します。

3.  クライアントは、前の手順で返された UUID を使用して、選択されたプロファイル テンプレートの登録ポリシーを取得します。

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policy/workflow/enroll
    ```
 <br/>   
4.  クライアントは、返されたポリシーの “DataCollection” フィールドを調べて、“Request Reason” という名前のデータ コレクション項目が 1 つあることを認識します。 また、“collectComments” フラグが *false*に設定されていることも認識し、ユーザーには何も入力を要求しません。

5.  ユーザーが証明書を要求する理由を入力した後、クライアントは要求を作成します。 

    ```
    POST /CertificateManagement/api/v1.0/requests 
    
    {
        "datacollection":"[{“Request Reason”:”Need VPN Certs”}]",
        "type":"Enroll",
        "profiletemplateuuid":"97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1",
        "comment":""
    }
    ```
<br/>
6.  サーバーが正常に要求を作成し、クライアントに要求の URI を返します: /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5。

7.  クライアントは、返された URI を呼び出すことによって、要求オブジェクトを取得します。

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5
    ```
<br/> 
8.  クライアントは、要求の “state” プロパティが “approved” に設定されていることを確認します。 要求の実行を開始できます。

9.  クライアントは、"newsmartcarduuid" パラメーターの内容を分析することで、要求に既に関連付けられているスマートカードがあるかどうかを調べます。 

10. 空の GUID のみが含まれているので、クライアントは、MIM CM によってまだ使用されていない既存のカードを使用するか、仮想スマートカード用にプロファイル テンプレートが構成されている場合はスマートカードを作成します。 

11. 登録可能なプロファイル テンプレートの最初のクエリによって後者であることがクライアントに示されているので (手順 1)、クライアントは仮想スマートカード デバイスを作成する必要があります。 

12. クライアントは、プロファイル テンプレートからスマートカード ポリシーを取得します (手順 3 で選択したテンプレートの UUID を使用します)。 

    ```
    GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/configuration/smartcards 
    ```
13. スマートカード ポリシーの "DefaultAdminKeyHex" プロパティには、カードの既定の管理キーが含まれます。 スマートカードを作成するとき、クライアントはスマートカードの初期管理キーをこのキーに設定する必要があります。  

14. スマートカード デバイスを作成した後、クライアントはそれを要求に割り当てる必要があります。

    ```
    POST /CertificateManagement/api/v1.0/smartcards 
    
    {
        "requestid":" C6BAD97C-F97F-4920-8947-BE980C98C6B5",
        "cardid":"23CADD5F-020D-4C3B-A5CA-307B7A06F9C9",
        "atr":"3b8d0180fba000000397425446590301c8"
    }
    ```
<br/>
15. サーバーは、新しく作成されたスマートカード オブジェクトへの URI でクライアントに応答します: api/v1.0/smartcards/D700D97C-F91F-4930-8947-BE980C98A644。

16. クライアントは、スマートカード オブジェクトを取得します。

    ```
    GET /CertificateManagement/api/v1.0/smartcards/ D700D97C-F91F-4930-8947-BE980C98A644
    ```
<br/>
17. 手順 12 で取得したスマートカード ポリシーの "diversifyadminkey" フラグの値をチェックすることにより、クライアントは管理キーを展開する必要があることを認識します。

18. クライアントは、提案された管理キーを取得します。

    ``` 
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/diversifiedkey?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9 
    ```
<br/>
19. クライアントは、管理キーを設定するには、管理者としてカードに認証する必要があります。 これを行うには、クライアントは、スマートカードからの認証チャレンジを要求し、サーバーに送信します。

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&atr=AFDEE6&diversified=false
    ```
<br/>
20. サーバーは HTTP 応答の本文でチャレンジ応答を送信し、クライアントはそれを使用して管理キーを展開します。

21. クライアントは、ユーザーはスマートカード ポリシーの "UserPinOption" フィールドを調べることによって目的の PIN を指定する必要があることを知ります。 

22. ユーザーがダイアログに PIN を入力した後、クライアントは、手順 19 で示したチャレンジ応答認証を実行しますが、展開フラグを "true" に設定する必要があります。

    ```
    GET /CertificateManagement/api/v1.0/ requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/smartcards/D700D97C-F91F-4930-8947-BE980C98A644/authenticationresponse?cardid=23CADD5F-020D-4C3B-A5CA-307B7A06F9C9&challenge=CFAA62118BBD25&atr=AFDEE6&diversified=true 
    ```
<br/>
23. クライアントは、サーバーから受け取った応答を使用して目的のユーザー PIN を設定します。

24. クライアントは、証明書の要求を生成する準備ができます。 クライアントは、プロファイル テンプレート証明書生成パラメーターをクエリし、キー/要求の生成方法を決定します。 

    ```
    GET /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificaterequestgenerationoptions.
    ```
<br/>
25. サーバーは、キーのエクスポート可能性、証明書のフレンドリ名、ハッシュ アルゴリズム、キー アルゴリズム、キー サイズなどの詳細を含む単一の JSON でシリアル化された CertificateRequestGenerationOptions オブジェクトで応答します。クライアントはこれらのパラメーターを使用して、証明書の要求を生成します。

26. 単一の証明書要求がサーバーに送信されます。 クライアントは、証明書テンプレートで CA への証明書のアーカイブが指定されている場合 (つまり、CA がキーペアを生成してそれをクライアントに送信します)、PFX BLOB の暗号化解除に使用する PFX パスワードを追加指定できます。 また、クライアントはコメントを追加することもできます。

    ```
    POST /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5/certificatedata
    
    {
       "pfxpassword":"",
       "certificaterequests":[
           "MIIDZDC…”
        ]
    }   
    ```
<br/>
27. 数秒後、サーバーは JSON でシリアル化された 1 つの Microsoft.Clm.Shared.Certificate オブジェクトで応答します。 "IsPkcs7" フラグをチェックして、クライアントはこの応答が PFX BLOB ではないことを知ります。 クライアントは、"pkcs7" 文字列を base64 でデコードして BLOB を抽出し、それをインストールします。 

28. クライアントは、要求を完了としてマークします。

    ```
    PUT /CertificateManagement/api/v1.0/requests/C6BAD97C-F97F-4920-8947-BE980C98C6B5 
    
    {
        "status":"Completed"
    }
    ```


<!--HONumber=Mar16_HO1-->


