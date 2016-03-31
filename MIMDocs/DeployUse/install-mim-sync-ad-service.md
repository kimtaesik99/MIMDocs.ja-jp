---
title: MIM 2016 & #58; をインストールします。Active Directory と MIM サービスに同期 |Microsoft Identity Manager
ms.custom:
  - Identity Management
  - MIM
ms.prod: identity-manager-2015
ms.reviewer: na
ms.suite: na
ms.technology:
  - security
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: kgremban
---
# Active Directory と MIM サービスに同期 MIM 2016 をインストールするには。

>[! div クラスを「ステップバイ ステップ」=]
[前へ](https://docsmsftstage.azurewebsites.net/MIM/DeployUse/install-mim-service-portal.html)
**インストールを実行する MIM 2016: MIM サービスおよびポータル**

> [!NOTE]
> 次に、すべての例で **mimservername** 、ドメイン コント ローラーの名前を表す **contoso** ドメイン名を表すと **Pass@word1** 例パスワードを表します。

既定では、MIM 同期サービス (Sync) に構成されているすべてのコネクタはありません。  一般的な最初の手順では、MIM 同期を使用して、MIM サービス データベースは、既存の Active Directory アカウントに入力です。 このためには、MIM 同期サービス アプリケーションを使用します。

## MIM 管理エージェントを作成します。
MIM 管理エージェント (MA) は、MIM サービスに MIM 同期のためのコネクタです。 このコネクタを作成するには、[管理エージェントを作成する] ウィザードを使用します。

MIM 管理エージェントを構成するときには、ユーザー アカウントを指定する必要があります。 このドキュメントでは **MIMMA** としてこのアカウントの名前。

> [!CAUTION]
> MIM 管理エージェントで使用するアカウントは、MIM サービスのインストール時に指定したのと同じアカウントである必要があります。

###MIM MA を作成するには

1.  Synchronization Service Manager を開きます。

2.  管理エージェントの作成ウィザードを開くには、 **アクション** ] メニューのをクリックして **作成**します。

3.   **管理エージェントを作成する** ] ページで、次の設定を提供してクリックして **次**します。

    -   管理エージェント: MIM サービスの管理エージェント

    -   名前:MIMMA

4.   **データベースへの接続** ] ページで、次の設定を提供してクリックして **次へ]**

    -   サーバー: localhost

    -   データベース: MIMService

    -   MIM サービスのベース アドレス: http://localhost:5725

    -   認証モード:Windows 統合認証

    -   ユーザー名: mimma

    -   ［パスワード］:Pass@word

    -   ドメイン: contoso

5.   **オブジェクトの種類を選択した** ] ページで、示されているオブジェクトの種類以下が選択されていることを確認] をクリックし、 **次へ]**

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   グループ

6.   **選択した属性** ] ページで、表示されているすべての属性が選択されていることを確認] をクリックし、 **次**します。

7.   **コネクタ フィルターを構成する** ] ページで [ **次**します。

8.   **オブジェクトの種類のマッピングの構成** ] ページで、次のマッピングを追加してクリックして **次へ]**

    -    **データ ソース オブジェクトの種類** 一覧で、[ **人**します。

    -   マッピング] ダイアログ ボックスを開くにはクリックして **マッピングの追加**します。

    -    **メタバース オブジェクトの種類** 一覧で、[ **人**します。

    -   マッピング] ダイアログ ボックスを閉じるにはクリックして **OK**します。

9.   **属性フローの構成** ] ページで、次の属性フロー マッピングを適用してクリックして **次へ]**

    ||||
    |-|-|-|
    | **縦書き/横書き** | **データ ソース属性** | **メタバース属性** |
    |インポート|インポート|accountName|
    |インポート|インポート|company|
    |インポート|インポート|displayName|
    |インポート|インポート|employeeID|
    |インポート|インポート|employee種類|
    |インポート|インポート|firstName|
    |インポート|インポート|lastName|
    |インポート|インポート|Manager|
    |インポート|インポート|objectSid|
    |エクスポート|エクスポート|accountName|
    |エクスポート|エクスポート|company|
    |エクスポート|エクスポート|displayName|
    |エクスポート|エクスポート|ドメイン|
    |エクスポート|エクスポート|employeeID|
    |エクスポート|エクスポート|employee種類|
    |エクスポート|エクスポート|firstName|
    |エクスポート|エクスポート|lastName|
    |エクスポート|エクスポート|manager|
    |エクスポート|エクスポート|objectSid|

10.  選択 **人** として、データ ソースのオブジェクトの種類。

    -   Select **Person** as the Metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the previous table, complete the following steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    -   Select **Group** as the data source type and as the metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the following table, complete these steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    | Flow Direction | Data Source Attribute | Metaverse Attribute |
    |-|-|-|
    | Export | AccountName | accountName |
    | Export | DisplayName | displayName |
    | Export | Domain | domain |
    | Export | Scope | scope |
    | Export | Type | type |
    | Export | Member | member |
    | Export | MembershipLocked | membershipLocked |
    | Export | MembershipAddWorkflow | membershipAddWorkflow |
    | Export | Manager | manager |

11.   **プロビジョニング解除を構成する** ] ページで [ **次へ]**

12.  管理エージェントを作成する、 **拡張機能の構成** ] ページで [ **完了**します。

## AD 管理エージェントを作成します。
Active Directory 管理エージェントは、AD ドメイン サービス用のコネクタです。 このコネクタを作成するには、[管理エージェントを作成する] ウィザードを使用します。

1. 管理エージェントの作成ウィザードを開くには、 **アクション** ] メニューのをクリックして **作成**します。

2.  **管理エージェントを作成する** ] ページで、次の設定を提供してクリックして **次**:

    - 管理エージェント:Active Directory ドメイン サービス
    - 名前:ADMA

3.  **Active Directory フォレストへの接続** ] ページで、次の設定を提供してクリックして **次**:

    - フォレスト名: contoso.local
    - ユーザー名: administrator
    - パスワード: & lt; アカウントのパスワード & gt;
    - ドメイン: contoso

4.  **ディレクトリ パーティションの構成** ] ページで、次の設定を提供してクリックして **次**:

    -  **ディレクトリ パーティションの選択** 一覧で、選択 **DC = CONTOSO, DC = local**します。

    - コンテナーの選択] ダイアログ ボックスを開くにはクリックして **コンテナー**します。

    - MIM に特定のコンテナー オブジェクトの管理] をクリックしてコンテナーを変更する場合、 **DC = CONTOSO, DC = local** ノードし関心のあるコンテナーのノードをクリックします。

    - コンテナーの選択] ダイアログ ボックスを閉じるにはクリックして **OK**します。

5.  **プロビジョニング階層を構成する** ] ページで [ **次**します。

6.  **[オブジェクトの種類** ] ページで、次の設定を提供してクリックして **次**:

    -  **オブジェクトの種類** 一覧で、[ **ユーザー** と **グループ**します。

7.  **属性の選択** ] ページで、次の設定を提供してクリックして **次**:

    - 選択 **すべてを表示する**です。

8.  **属性** 一覧で、次の属性を選択します。

    -   company
    -   displayName
    -   employeeID
    -   employee種類
    -   givenName
    -   group種類
    -   manager
    -   managedBy
    -   メンバー
    -   objectSid
    -   sAMAccountName
    -   sAMAccount種類
    -   sn
    -   unicodePwd
    -   userAccountControl

9.  **コネクタ フィルターを構成する** ] ページで [ **次**します。

10.  **構成に参加および保護ルール** ] ページで [ **次**します。

11.  **属性フローの構成** ] ページで [ **次**します。

12.  **プロビジョニング解除を構成する** ] ページで [ **次**します。

13.  **拡張機能の構成** ] ページで [ **完了**します。


## 実行プロファイルの作成

ADMA および MIMMA コネクタの実行プロファイルを作成します。

### ADMA コネクタの実行プロファイルの作成

次の表では、上位 5 つの実行は、ADMA コネクタの作成プロファイルを示します。

| 名前 | 型 |
| ---- | ---- |
| Profile1 | フル インポート (ステージのみ) |
| Profile2 | 完全同期 |
| Profile3 | デルタ インポート (ステージのみ) |
| Profile4 | 差分同期 |
| Profile5 | エクスポート |

ADMA コネクタの実行プロファイルを作成するには

1. 同期サービス マネージャーを開くと、 **ツール** ] メニューのをクリックして **管理エージェント**します。

2.  **管理エージェント** 一覧で、[ **ADMA**します。

3. 実行プロファイルの構成] ダイアログ ボックスを開くには、 **アクション** ] メニューのをクリックして **実行プロファイルを構成する**です。

4. テーブル内の実行プロファイルごとには、次の手順を実行します。

    - 実行プロファイルの構成ウィザードを開くにはクリックして **新しいプロファイル**します。

    -  **名前** ボックスで、テーブルからプロファイル名を入力し、をクリックして **次**します。

    -  **型** 一覧をテーブルからステップの種類を選択してクリックして **次**します。

    - をクリックして **完了** 実行プロファイルを作成します。

5. 実行プロファイルの構成] ダイアログ ボックスを閉じるにはクリックして **OK**します。

### MIMMA コネクタの実行プロファイルの作成

次の表は、MIMMA コネクタの対応する 5 つの実行プロファイルを示します。

| 名前 | 型 |
| -------- | -------- |
| Profile1 | フル インポート (ステージのみ) |
| Profile2 | 完全同期 |
| Profile3 | デルタ インポート (ステージのみ) |
| Profile4 | 差分同期 |
| Profile5 | エクスポート |

MIMMA コネクタ用のプロファイルを作成するには

1. 同期サービス マネージャーを開くと、 **ツール** ] メニューのをクリックして **管理エージェント**します。

2.  **管理エージェント** 一覧で、[ **MIMMA**します。

3. 実行プロファイルの構成] ダイアログ ボックスを開くには、 **アクション** ] メニューのをクリックして **実行プロファイルを構成する**です。

4. テーブル内の実行プロファイルごとには、次の手順を実行します。

    - 実行プロファイルの構成ウィザードを開くにはクリックして **新しいプロファイル**します。

    -  **名前** ボックスで、テーブルからプロファイル名を入力し、をクリックして **次**します。

    -  **型** 一覧をテーブルからステップの種類を選択してクリックして **次**します。

    - をクリックして **完了** 実行プロファイルを作成します。

5. 実行プロファイルの構成] ダイアログ ボックスを閉じるにはクリックして **OK**します。

## MIM サービスの構成

MIM ポータルを使用して、MIM サービスの AD ユーザー受信同期規則を作成します。

AD ユーザー受信同期規則を作成します。

1. MIM ポータル ホーム ページのナビゲーション バーでをクリックして **管理**します。

2. 同期規則] ページを開くにはクリックして **同期規則**します。

3. ツールバーで、同期規則の作成ウィザードを開くにはクリックして **新規**します。

4.  **全般** ] タブで、次の情報を提供してクリックして **次**:

    -   表示名:AD ユーザー受信同期規則
    -   データ フローの方向受信

5.  **スコープ** ] タブで、次の情報を提供してクリックして **次**:

    -   メタバース リソースの種類: 個人
    -   外部システム:ADMA
    -   外部システム リソースの種類: 個人

6.  **リレーションシップ** ] タブで、次の情報を提供してクリックして **次**:

    -   リレーションシップ条件を構成するには、選択 **ObjectSID** Metaverseobject:person リストから、ConnectedSystemObject:person (属性) リストからです。

    -   選択 **MIM にリソースを作成**します。

7.  **受信属性フロー** ] ページで、次の情報を提供してクリックして **次**:

    | フロー ルール | ソース | Destination |
    |-|-|-|
    |規則 1|samAccountName|f|
    |規則 2|displayName|displayName|
    |規則 3|Employee種類|Employee種類|
    |規則 4|givenName|givenName|
    |規則 5|sn|lastName|
    |規則 6|Manager|manager|
    |規則 7|objectSID|ObjectSID|
    |規則 8|"Contoso"|ドメイン|

    テーブル内の行ごとに、次の手順を実行します。

    -   [フロー定義] ダイアログ ボックスを開くにはクリックして **新しい属性フロー**します。

    -    **ソース** タブで、テーブル内の行に対して表示される属性を選択します。

    -    **宛先** タブで、テーブル内の行に対して表示される属性を選択します。

    -   属性フローの構成を適用する] をクリックして **OK**します。

8.  **概要** ] タブ、[ **送信**します。

## テスト環境の初期化
AD データで構成をテストするには、構成を初期化する必要があります。 このプロセスに 4 つのステップがあります。

### プロビジョニングの有効化にします。

1. Synchronization Service Manager を開きます。

2. [オプション] ダイアログ ボックスを開くには、 **ツール** ] メニューをクリックして **オプション**

3. 選択 **同期ルールを有効にするプロビジョニング**します。

4. クリックして [オプション] ダイアログ ボックスを閉じます **OK**します。

### MIMMA を初期化します。

このコネクタでは、完全な同期サイクルを実行します。 完全なサイクルは、次の実行プロファイルで構成されます。

    -   Full Import

    -   Full Synchronization

    -   Export

    -   Delta Import

1. Synchronization Service Manager を開くと、 **ツール** ] メニューのをクリックして **管理エージェント**します。

2.  **管理エージェント** 一覧で、[ **MIMMA**します。

3. 管理エージェントの実行] ダイアログ ボックスを開くには、 **アクション** ] メニューのをクリックして **実行**します。

4. 上に示した各実行プロファイルには、次の手順を実行します。

    - 管理エージェントの実行] ダイアログ ボックスを開くには、 **アクション** ] メニューのをクリックして **実行**します。

    -  **実行プロファイル** 一覧で、実行する実行プロファイルを選択します。

    - 実行プロファイルを開始するには、クリックして **OK**します。

#### 属性フローの優先順位の構成

MIM コネクタの初期化中に、構成された同期規則はメタバースに取り込まれました。

このコネクタから提供される属性の属性フロー優先順位を調整し、AD 内に既に存在する属性が確実に、メタバースに配置され、その後 MIM サービス データベースにも配置されるようにします。

### ADMA の初期化

Active Directory コネクタを初期化するには、それに対してフル インポートおよび完全な同期を実行する必要があります。 既存のオブジェクトを AD からコネクタ領域に取り込むには、フル インポートが必要です。 MIM コネクタ領域からメタバースに新しい同期規則を適用することで同期規則が変更されているので、完全な同期が必要です。 学習します。

    -   Full Import

    -   Full Synchronization

1. Synchronization Service Manager を開くと、 **ツール** ] メニューのをクリックして **管理エージェント**します。

2.  **管理エージェント** 一覧で、[ **ADMA**します。

3. 管理エージェントの実行] ダイアログ ボックスを開くには、 **アクション** ] メニューのをクリックして **実行**します。

4. 上に示した各実行プロファイルには、次の手順を実行します。

    - 管理エージェントの実行] ダイアログ ボックスを開くには、 **アクション** ] メニューのをクリックして **実行**します。

    -  **実行プロファイル** 一覧で、実行する実行プロファイルを選択します。

    - 実行プロファイルを開始するには、クリックして **OK**します。

### MIM サービス データベースの設定

MIM サービス データベースにオブジェクトを設定するには、MIMMA コネクタで同期サイクルを実行する必要があります。 サイクル全体は、以下の実行プロファイルの実行で構成されます。

    -   Export

    -   Full Import

    -   Full Sync

    1. Open the Synchronization Service Manager and, on the **Tools** menu, click **Management Agents**.

    2. In the **Management Agents** list, select **MIMMA**.

    3. To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

    4. For each run profile listed above, complete the following steps:

        - To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

        - In the **Run profiles** list, select the run profile you want to run.

        - To start the run profile, click **OK**.


<!--HONumber=Mar16_HO3-->


