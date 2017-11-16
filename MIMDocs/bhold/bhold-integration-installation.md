---
title: "BHOLD FIM/MIM 統合のインストール | Microsoft Docs"
description: "BHOLD 統合モジュールは、セルフ サービスのロール管理を MIM と FIM に追加します"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: ef68de19bd0eabd6d9203469ecc991d496f05846
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-fimmim-integration-installation"></a>BHOLD FIM/MIM 統合のインストール

BHOLD FIM 統合モジュールは、セルフ サービスのロール管理を Microsoft Identity Manager に追加して、ユーザーが追加ロールを要求できるようにして、そのロールを誰が引き受けるかを厳しく制限します。 BHOLD FIM 統合モジュールは、FIM ポータルを拡張して、FIM 全体管理の一環としてユーザーのロールを管理しやすくします。 このトピックでは、BHOLD FIM 統合モジュールをインストールして使用するには、ネットワーク インフラストラクチャをどのように構成する必要があるかを説明します。 また、BHOLD FIM 統合モジュールをインストールする方法と、BHOLD FIM 統合モジュールをインストールした後に必要な構成についても説明します。

## <a name="bhold-fim-integration-installation-requirements"></a>BHOLD FIM 統合のインストール要件

BHOLD FIM 統合モジュールは、FIM ポータルと FIM サービスを拡張して、ユーザーが FIM ポータル内で自身のロールを管理できるようにします。 このため、BHOLD FIM 統合モジュールをインストールする前に、BHOLD コア モジュールと必要な FIM 機能をインストールして構成することが重要です。
BHOLD FIM 統合モジュールをインストールするには、コンピューターに次のソフトウェア コンポーネントが存在している必要があります。

- Microsoft Identity Manager 2016 ポータルとサービス
- Microsoft Silverlight 3 以降
- インターネット インフォメーション サービスと ASP.NET
- Microsoft Silverlight ツール

さらに、BHOLD コアおよび Access Management Connector モジュールを環境にデプロイし、1 つ以上の BHOLD 管理エージェントで FIM を構成する必要があります。 BHOLD コア モジュールのインストールと構成の詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。 Access Management Connector モジュールのインストールと使用の詳細については、「[Access Management Connector Installation](https://technet.microsoft.com/en-us/library/jj874042(v=ws.10).aspx)」 (Access Management Connector のインストール) および「[Test Lab Guide: BHOLD Access Management Connector](https://technet.microsoft.com/en-us/library/jj853085(v=ws.10).aspx)」 (テスト ラボ ガイド: BHOLD Access Management Connector) をご覧ください。

>[!IMPORTANT]
FIM サービス データベースの名前は FIMService をする必要があります。 FIM が既定の FIM サービス データベース名でインストールされていない場合、BHOLD FIM 統合のセットアップは失敗します。

## <a name="before-you-begin"></a>始める前に

BHOLD FIM 統合モジュールのインストールを開始する前に、C: ディスク ドライブのルート ディレクトリに BHOLD ディレクトリを作成する必要があります (C:\BHOLD)。

また、BHOLD FIM 統合セットアップ ウィザードでのインストールに必要な情報を準備する必要があります。 次のワークシートを使用すると、こうした情報を必要に応じて指定できるように、記録しておくことができます。

### <a name="bholdfim-account-settings"></a>BHOLDFim アカウント設定

| **項目**                            | **説明**                                                                                                                                                                                                               | **値**                                                                                                                                                                                                                                                                                                            |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ドメインでセキュリティ プロバイダーを使用する** | オンにすると、Active Directory Domain Services セキュリティによって BHOLD コアへのアクセスが制御されます。                                                                                                                    | チェック ボックスをオンにします。 **重要:** このチェック ボックスをオフにすると、インストールは失敗します。                                                                                                                                                                                                                   |
| **ドメイン**                          | BHOLD コアをインストールするときに作成した**サービス アカウント**を含むドメインを指定します。 詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。 | ドメイン名は、ウィザードによって自動的に入力されます。 この名前を変更するのは、適切ではない場合のみにします。 **重要:** 完全修飾ドメイン名 (FQDN) ではなく、NetBIOS 名 (短い名前) を使用してドメイン名を指定します。 たとえば、ドメインの FQDN が fabrikam.com の場合、ドメイン名は FABRIKAM として指定します。 |
| **ユーザー名**                        | BHOLD コア サービスのユーザー アカウントのログオン名を指定します。                                                                                                                                                              | ここにユーザー アカウント名を入力します。                                                                                                                                                                                                                                                                                    |
| **パスワード**                        | サービスのユーザー アカウントのパスワードを指定します。                                                                                                                                                                           | ここにパスワードを入力します。**重要:** このパスワードは、セキュリティで保護された非表示の場所に保管してください。                                                                                                                                                                                                                  |

### <a name="fim-service-settings"></a>FIM サービスの設定

| **項目**            | **説明**                                                                                                                                                                                                                               | **値**                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **ユーザー**            | FIM の管理者特権でアカウントのログオン名を指定します。 Microsoft では BHOLD コアのルート ユーザーに関連付けられているアカウント (既定では、BHOLD コアのインストールに使用するアカウント) を使用しないことを強くお勧めします。 | ここにユーザー アカウント名を入力します。                                                                   |
| **パスワード**        | FIM 管理者ユーザー アカウントのパスワードを指定します。                                                                                                                                                                                 | ここにパスワードを入力します。**重要:** このパスワードは、セキュリティで保護された非表示の場所に保管してください。 |
| **FIM データベース**    | FIM サービス データベースの名前を指定します。                                                                                                                                                                                               | FIMService                                                                                          |
| **Web サイト IP/ポート** | FIM ポータル サーバーの名前または IP アドレス、および Web サイト ポートを指定します。                                                                                                                                                               | ここにサーバー名またはアドレス、およびポートを入力します。                                                     |

### <a name="bhold-core-connection"></a>BHOLD コア接続

| **項目**               | **説明**                                                                                                                                                                                                                                                                                                                                                                               | **値**                                                                                           |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| **ドメイン**             | 以下の**ユーザー**で指定されたアカウントのドメイン名を指定します。 NetBIOS (短い) 形式でドメインを指定します。                                                                                                                                                                                                                                                                   | ここにユーザー アカウント ドメイン名を入力します。                                                            |
| **ユーザー**               | すべてのユーザーとロールの**スーパーバイザーである BHOLD ユーザー**のアカウントのログオン名を指定します。また、ユーザー ロールをリンクおよびリンク解除するアクセス許可が付与されています。 Microsoft では BHOLD コアのルート ユーザーに関連付けられているアカウント (既定では、BHOLD コアのインストールに使用するアカウント) を使用しないことを強くお勧めします。 このアカウントは、FIM への接続に使用するものと同じアカウントを使用できます | ここにユーザー アカウント名を入力します。                                                                   |
| **パスワード**           | 「**ユーザー**」で指定されたユーザー アカウントのパスワードを指定します。                                                                                                                                                                                                                                                                                                                             | ここにパスワードを入力します。**重要:** このパスワードは、セキュリティで保護された非表示の場所に保管してください。 |
| **IP/マシンのアドレス** | BHOLD コア Web サイト サーバーの IP アドレスを指定します。 サーバー名は使用しないでください。                                                                                                                                                                                                                                                                                                        | ここに IP アドレス プールを入力します。                                                                          |
| **ポート番号**        | BHOLD コア Web サイトのポート番号を指定します。                                                                                                                                                                                                                                                                                                                                          | ここにポート番号を入力します。                                                                         |

## <a name="bhold-fim-integration-setup"></a>BHOLD FIM 統合グループ

BHOLD FIM 統合モジュールをインストールするには、Domain Admins グループのメンバーとしてログオンし、次のファイルをダウンロードして、BHOLD FIM 統合モジュールをインストールするサーバーで管理者として実行します。

- BholdFIMIntegration*\<Version\>*\_Release.msi

*\<Version\>* は、インストールする BHOLD FIM 統合リリースのバージョン番号に置き換えてください。

管理者としてプログラム ファイルを実行するには、そのファイルを右クリックし、**[管理者として実行]** をクリックします。

![実行中の msi](media/bhold-integration-installation/cmd.png)

## <a name="post-installation-tasks"></a>インストール後のタスク

BHOLD FIM 統合をインストールしたら、BHOLD サービス アカウントのサイト所有者アクセス許可を提供するように Microsoft SharePoint を構成する必要があります。 また、FIM ポータルが Secure Sockets Layer (SSL) セキュリティを使用するように構成されている場合は、FIM ポータルに追加された BHOLD ページのアドレスへの参照を含むファイルを変更する必要があります。

### <a name="configuring-sharepoint"></a>SharePoint の構成

BHOLD FIM 統合が正常に機能するには、BHOLD サービス アカウントに Microsoft SharePoint のサイト メンバー アクセス許可が必要です。

### <a name="to-grant-site-member-permissions-to-the-bhold-service-account"></a>BHOLD サービス アカウントにサイト メンバー アクセス許可を付与するには

1.  BHOLD FIM 統合を実行しているサーバーに管理者特権でログオンします。

2.  **[スタート]** をクリックし、**[Internet Explorer]** をクリックします。

3.  SharePoint が SSL セキュリティを使用するように構成されている場合は、アドレス バーに「<https://localhost>」と入力します。それ以外の場合は、「<http://localhost>」と入力します。

4.  **[チーム サイト]** ページの左側で、**[ユーザーとグループ]** をクリックします。

5.  **[グループ]** で **[Team Site Members]\(チーム サイトのメンバー\)** をクリックし、中央ウィンドウのツール バーで、**[新規作成]**、**[ユーザーの追加]** の順にクリックします。

6.  **[ユーザーの追加: チーム サイト]** ページで、**[ユーザー/グループ]** に「BHOLDApplicationGroup」と入力し、**[ユーザー/グループ]** ボックスの [名前の確認] をクリックします。 解決されたグループ名にはドメイン名が含まれる必要があります。

7.  **[Give users permissions directly]\(ユーザーのアクセス許可を直接付与する\)** をクリックし、**[Full Control – Has full control]\(フルコントロール – フル コントロール\)** を選択して、**[OK]** をクリックします。

8.  **[アクセス許可: チーム サイト]** に BHOLDApplicationGroup が表示されていることを確認し、Internet Explorer を閉じます。

![実行中の msi](media/bhold-integration-installation/sharepoint.png)

### <a name="configuring-bhold-to-support-ssl"></a>SSL をサポートするように BHOLD を構成

SSL セキュリティを使用するように FIM ポータルが構成されている場合は、BHOLD ページへのリンクが開くように FIM サーバーのファイルを変更する必要があります。 ファイルは ```C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\12\TEMPLATE\LAYOUTS\BHOLD``` フォルダーにあります。

次の表は、ファイルと、編集前の元の文字列および変更後の文字列を示しています。

| **ファイル**                  | **元の文字列**                                                                                                                   | **変更後の文字列**                                                                                                                                |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
| Analytics.aspx            |   http://<BHOLD_Server>/bhold/Analytics/index_fim.html | https://<BHOLD_Server_FQDN>/bhold/Analytics/index_fim.html       |
| AttestationCampaigns.aspx |    http://<BHOLD_Server>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | https://<BHOLD_Server_FQDN>/bhold/Attestation/Campaigns.aspx?hideMenu=1 | 
| AttestationMain.aspx      |  http://<BHOLD_Server>/bhold/Attestation/Dashboard.aspx?hideMenu=1        | https://<BHOLD_Server_FQDN>/bhold/Attestation/Dashboard.aspx?hideMenu=1 |
| Reporting.aspx            | http://<BHOLD_Server>/bhold/Reporting/index_fim.html |  https://<BHOLD_Server_FQDN>/bhold/Reporting/index_fim.html |
| Selfservice.aspx          | RoleExchangePoint=http://\<*FIM_Server*\>: \<*FIM_Port*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport | RoleExchangePoint=https://\<*FIM_Server_FQDN*\>: \<*FIM_SSL_Port\>*\>/BHOLD/RoleExchangePoint/ BHOLDRoleExchangePoint.svc,TransportMode=Transport |

説明:

-   *\<BHOLD_Server\>* は、ファイルの元のバージョンで見つかった BHOLD サーバーの名前を指定します

-   *\<MIM_Server\>* は、ファイルの元のバージョンで見つかった FIM サーバーの名前を指定します

-   *\<BHOLD_Server_FQDN\>* は、BHOLD サーバーの完全修飾ドメイン名 (FQDN) を指定します

-   *\<MIM_Port\>* は、ファイルの元のバージョンで見つかった FIM サーバーのポート番号を指定します

-   *\<MIM_Server_FQDN\>* は、FIM サーバーの FQDN を指定します

-   *\<MIM_SSL_Port\>* は、FIM サーバーで SSL で使用するための別のポートを指定します

### <a name="enable-approval-workflows-in-bhold-core"></a>BHOLD コアでの承認ワークフローの有効化

FIM と BHOLD がセルフ サービス用に統合されている場合、承認のワークフローは FIM サービスで実行されます。 これは、ユーザーが配布リストの結合要求を送信する場合など、FIM ポータルで発信される要求のワークフロー モデルに似ています。 ただし、BHOLD ロール ワークフローと、FIM サービスでホストされている他のワークフローには重要な違いがあります。 BHOLD の場合、どのユーザーがロール要求を承認する必要があるかを指定するワークフロー パラメーターは BHOLD で発信されます。FIM サービス データベースのワークフロー定義には格納されていません。 こうしたパラメーターは、最初の要求が行われるときに BHOLD によって FIM サービスに提供され、結果はアクション ワークフローによって BHOLD コアに返されます。

BHOLD では、次の 3 つの方法のいずれかで、セルフ サービス要求の承認者が選択されます。

-   **承認者としてのライン マネージャー: 組織単位 (OrgUnit) のロール ベースの選択**: 承認者またはエスカレーターに設定されている roletype という名前の属性がロールにあり、そのロールが、OrgUnit のコンテキストで 1 人以上のユーザーにリンクされている場合、その OrgUnit 内のユーザーからの要求は、承認者またはエスカレーター roletype を持つロールにリンクされているユーザーのいずれかが承認する必要があります。

-   **承認者としてのライン マネージャー: OrgUnit の属性ベースの選択**: 各 OrgUnit が、OrgUnit の他のユーザーのロールの割り当てを承認できるユーザーの別名を指定する、1 つ以上の属性を持つことができます。 こうした属性には、approver1、approver2 などの名前が付けられます。 OrgUnit のユーザーがロールの割り当てを要求すると、BHOLD は、その要求を、OrgUnit 承認者属性によって指定されたユーザーに (FIM 経由で) ルーティングします。 OrgUnit にこうした属性セットがない場合、BHOLD は、親 OrgUnit をルート OrgUnit まで確認します。

-   **承認者としてのロール マネージャー: ロールの属性ベースの選択**: ロールが、そのロールの割り当てを承認できるユーザーの別名を指定する、1 つ以上の属性 (approver1 などの名前) を持つことができます。 こうした承認者属性が設定されているロールの割り当てをユーザーが要求すると、BHOLD は、その要求を、属性によって指定されたユーザーにルーティングします。

上記の方法でセルフサービス ロール要求の承認者が指定されていない場合、既定では、承認なしでロールが自動的に割り当てられます。 このため、BHOLD FIM 統合をインストールしたらすぐに、ルート アカウントなど、承認者の別名でルート OrgUnit を構成する必要があります。 これにより、包括的な承認ポリシーを実装する前に、ユーザーに意図せずロールが付与されることがなくなります。

#### <a name="to-configure-an-approver-for-the-root-orgunit"></a>ルート OrgUnit の承認者を構成するには

1.  管理者として BHOLD コア サーバーにログオンします。

2.  **[スタート]** をクリックし、**[Internet Explorer]** をクリックします。

3.  Internet Explorer のアドレス バーに「<http://localhost:5151/bhold/core>」と入力し、Enter キーを押します。

4.  BHOLD コアのホーム ページで **[Attribute def]\(属性の既定値\)** で **[属性の種類]** をクリックします。

5.  **[属性の種類]** ページで **[追加]** をクリックします。

6.  **[属性の種類の追加] ページ**の **[ID]** に「approver1」と入力し、**[データ型]** の一覧で **[英数字]** をクリックして、**[最大長]** に「255」と入力します。次に、**[値の一覧]** で **[いいえ]** をクリックし、**[英語]** に「承認者 1」と入力して、**[OK]**、**[完了]** の順にクリックします。

7.  左側のウィンドウの **[Attribute def]\(属性の既定値\)** で **[Attribute type sets]\(属性の種類のセット\)** をクリックします。

8.  **[Attribute type sets]\(属性の種類のセット\)** ページで **[追加]** をクリックします。

9.  **[Add attribute type set]\(属性の種類のセットを追加\)** ページの **[説明]** に、「OrgUnit 属性」と入力し、**[OK]** をクリックします。

10. **[OrgUnit 属性]** ページで、**[属性の種類]** を展開し、**[変更]** をクリックします。

11. **[属性の種類]** ボックスの一覧で、**[approver1]**、**[追加]**、**[完了]** の順にクリックします。

12. 左側のウィンドウで、**[オブジェクトの種類]** をクリックします。

13. **[オブジェクトの種類]** ページで **[OrgUnit]** をクリックします。

14. **[OrgUnit 属性/OrgUnit]** ページで、**[Attribute type sets]\(属性の種類のセット\)** を展開し、**[変更]** をクリックします。

15. **[Link attribute type set/OrgUnit]\(属性の種類のセットをリンク/OrgUnit\)** ページの **[順序]** に「10」と入力し、**[Attribute type sets]\(属性の種類のセット\)** で、**[OrgUnit 属性]**、**[追加]**、**[完了]** の順にクリックします。

16. 左側のウィンドウの **[モデル]** で **[組織単位]** をクリックします。

17. **[組織単位]** ページで **[ルート]** をクリックします。

18. **[組織単位/ルート]** ページで **[変更]** をクリックします。

19. **[Modify organizational unit attributes/root]\(組織単位の属性/ルートの変更\)** ページの **[承認者]** に、ロールの割り当て要求を承認するユーザーのドメインとユーザー名を、*\<domain\>*\\*\<user\>* 形式で入力します。ここで、*\<domain\>* は NetBIOS ドメイン名 (短い名前)、*\<user\>* はユーザーのログオン名です。
20. **[OK]**をクリックします。

>[!IMPORTANT]
ドメインとユーザー名は、BHOLD コア データベースのユーザーの既定の別名と一致する必要があります。

組織単位の承認者を指定する代わりに、BHOLD コア データベースの提案されたロールの承認者を指定することもできます。 これを行うには、approver1 属性を作成し、ロール オブジェクトの種類に関連付けられている属性の種類のセットに追加して、承認者を指定するように、提案された各ロールを変更します。

ワークフローのセキュリティを強化するには、OrgUnit とロールに対して次の属性を作成および設定して、承認者のほかに、追加の承認モードとユーザーを指定します。

- escalator*\<n\>*

- owner*\<n\>*

- securityOfficer*\<n\>*

- notification*\<n\>*

ここで *\<n\>* は、同じ種類の属性を複数提供する省略可能な数字のサフィックスです。

### <a name="verify-approval-workflows-configured-in-the-fim-service"></a>FIM サービスで構成されている承認ワークフローの確認

BHOLD FIM 統合インストールでは、FIM サービスに対するセット、ワークフロー定義、および管理ポリシー規則 (MPR) が作成されます。 FIM デプロイをカスタマイズして、管理者セット、または要求を行うことができるユーザー セットを変更した場合は、MPR が適切なユーザー セットを参照するように指定する必要があります。

>[!NOTE]
BHOLD で提供されるセルフ サービス機能を FIM ポータルのユーザーが使用するには、ユーザー アカウントが、FIM 同期サービスから BHOLD データベースに同期されるようにしておきます。 具体的には、セルフ サービス要求を行うことができるユーザー、またはセルフ サービス要求の承認者またはエスカレーターとして指定されているユーザーごとに、BHOLD コア データベースと FIM サービス データベースにユーザー レコードが必要です。

## <a name="next-steps"></a>次のステップ

- FIM ポータルおよび他の FIM 機能のインストールについては、Microsoft Forefront テクニカル ライブラリの「[Planning and Architecture](https://technet.microsoft.com/library/ee808044.aspx)」 (計画とアーキテクチャ) をご覧ください。
- [BHOLD インストール ガイド](bhold-installation-guide.md)
- [BHOLD 開発者用リファレンス](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD のバージョン履歴](../reference/version-bhold-history.md)
