---
# required metadata

title: MIM 2016 のインストール&#58; Active Directory と MIM サービスを同期する | Microsoft Identity Manager
description: 管理エージェントと MIM 同期サービスを使用して、Active Directory と MIM データベースを同期します。
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# MIM 2016 のインストール: Active Directory と MIM サービスを同期する

>[!div class="step-by-step"]
[« MIM サービスおよびポータル](install-mim-service-portal.md)

> [!NOTE]
> 以下の例ではすべて、**mimservername** はドメイン コントローラー名、**contoso** はドメイン名、**Pass@word1** は例で使用するパスワードをそれぞれ表しています。

既定では、MIM 同期サービス (Sync) にはコネクタが何も構成されていません。  一般的な最初の手順では、MIM 同期を使用して、MIM サービス データベースに、既存の Active Directory アカウントを設定します。 このためには、MIM 同期サービス アプリケーションを使用します。

## MIM 管理エージェントを作成する
MIM 管理エージェント (MA) は、MIM 同期が MIM サービスに接続するためのコネクタです。 このコネクタを作成するには、[管理エージェントを作成する] ウィザードを使用します。

MIM 管理エージェントを構成するときには、ユーザー アカウントを指定する必要があります。 このドキュメントでは、このアカウントの名前として **MIMMA** を使用します。

> [!CAUTION]
> MIM 管理エージェントで使用するアカウントは、MIM サービスのインストール時に指定したのと同じアカウントである必要があります。

###MIM MA を作成するには

1.  Synchronization Service Manager を開きます。

2.  [管理エージェントを作成する] ウィザードを開くには、**[操作]** メニューで、**[作成]** をクリックします。

3.  **[管理エージェントを作成する]** ページで、次の設定を行い、**[次へ]** をクリックします。

    -   管理エージェント: MIM サービス管理エージェント

    -   名前:MIMMA

4.  **[データベースに接続]** ページで、次の設定を行い、**[次へ]** をクリックします。

    -   サーバー: localhost

    -   データベース: MIMService

    -   MIM サービス ベース アドレス: http://localhost:5725

    -   認証モード:Windows 統合認証

    -   ユーザー名: mimma

    -   ［パスワード］:Pass@word

    -   ドメイン: contoso

5.  **[選択したオブジェクトの種類]** ページで、以下に一覧したオブジェクトの種類が選択されていることを確認し、**[次へ]** をクリックします。

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   グループ

6.  **[属性の選択]** ページで、一覧したすべての属性が選択されていることを確認し、**[次へ]** をクリックを確認します。

7.  **[コネクタ フィルターを構成する]** ページで、**[次へ]** をクリックします。

8.  **[オブジェクトの種類のマッピングを構成する]** ページで、次のマッピングを追加し、**[次へ]** をクリックします。

    -   **[データ ソース オブジェクトの種類]** リストで、**[個人]** を選択します。

    -   マッピングのダイアログ ボックスを開くには、**[マッピングの追加]** をクリックします。

    -   **[メタバース オブジェクトの種類]** リストで、**[個人]** を選択します。

    -   [マッピング] ダイアログ ボックスを閉じるには、**[OK]** をクリックします。

9.  **[属性フローを構成する]** ページで、次の属性フロー マッピングを適用し、**[次へ]** をクリックします。

    ||||
    |-|-|-|
    | **フローの方向** | **データ ソース属性** | **メタバース属性** |
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

10.  データ ソース オブジェクトの種類として **[個人]** を選択します。

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

11.  **[プロビジョニング解除を構成する]** ページで、**[次へ]** をクリックします

12.  管理エージェントを作成するには、**[拡張機能を構成する]** ページで **[完了]** をクリックします。

## AD 管理エージェントを作成する
Active Directory 管理エージェントは、AD ドメイン サービス用のコネクタです。 このコネクタを作成するには、[管理エージェントを作成する] ウィザードを使用します。

1. [管理エージェントを作成する] ウィザードを開くには、**[操作]** メニューで、**[作成]** をクリックします。

2. **[管理エージェントを作成する]** ページで、次の設定を行い、**[次へ]** をクリックします。

    - 管理エージェント:Active Directory ドメイン サービス
    - 名前:ADMA

3. **[Active Directory フォレストに接続する]** ページで、次の設定を行い、**[次へ]** をクリックします。

    - フォレスト名: contoso.local
    - ユーザー名: administrator
    - パスワード: & lt;アカウントのパスワード& gt;
    - ドメイン: contoso

4. **[ディレクトリ パーティションを構成する]** ページで、次の設定を行い、**[次へ]** をクリックします。

    - **[ディレクトリ パーティションの選択]** 一覧で、**[DC = CONTOSO, DC = local]** を選択します。

    - [コンテナーの選択] ダイアログ ボックスを開くには、**コンテナー**をクリックします。

    - MIM に特定のコンテナー内のオブジェクトの管理だけをさせるようにコンテナーを変更する場合、**[DC = CONTOSO, DC = local]** ノードをクリックしてから、目的のコンテナーのノードをクリックします。

    - [コンテナーの選択] ダイアログ ボックスを閉じるには、**[OK]** をクリックします。

5. **[プロビジョニング階層を構成する]** ページで、**[次へ]** をクリックします。

6. **[オブジェクトの種類を選択する]** ページで、次の設定を行い、**[次へ]** をクリックします。

    - **[オブジェクトの種類]** リストで、**ユーザー**および**グループ**を選択します。

7. **[属性を選択する]** ページで、次の設定を行い、**[次へ]** をクリックします。

    - **[すべて表示]** を選択します。

8. **[属性]** リストで、次の属性を選択します。

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

9. **[コネクタ フィルターを構成する]** ページで、**[次へ]** をクリックします。

10. **[参加および保護ルールを構成する]** ページで、**[次へ]** をクリックします。

11. **[属性フローを構成する]** ページで、**[次へ]** をクリックします。

12. **[プロビジョニング解除を構成する]** ページで、**[次へ]** をクリックします

13. **[拡張機能を構成する]** ページで、**[完了]** をクリックします。


## 実行プロファイルの作成

ADMA および MIMMA コネクタの実行プロファイルを作成します。

### ADMA コネクタの実行プロファイルの作成

この表では、ADMA コネクタ用に作成する 5 つの実行プロファイルを示しています。

| 名前 | 型 |
| ---- | ---- |
| Profile1 | フル インポート (ステージのみ) |
| Profile2 | 完全な同期 |
| Profile3 | 差分インポート (ステージのみ) |
| Profile4 | 差分同期 |
| Profile5 | エクスポート |

ADMA コネクタの実行プロファイルを作成するには

1. Synchronization Service Manager を開き、**[ツール]** メニューの **[管理エージェント]** をクリックします。

2. **管理エージェント**の一覧で、**ADMA** を選択します。

3. [ダイアログ ボックスの実行プロファイルを構成する] を開くには、**[操作]** メニューの **[実行プロファイルを構成する]** をクリックします。

4. テーブル内の各実行プロファイルに対し、以下の手順を実行します。

    - [実行プロファイルを構成する] ウィザードを開くには、**[新しいプロファイル]** をクリックします。

    - テーブルのプロファイル名を **[名前]** ボックスに入力し、**[次へ]** をクリックします。

    - **[種類]** ボックスでは、テーブルのステップの種類を選択し、**[次へ]** をクリックします。

    - **[完了]** をクリックして、実行プロファイルを作成します。

5. [実行プロファイルを構成する] ダイアログ ボックスを閉じるには、**[OK]** をクリックします。

### MIMMA コネクタの実行プロファイルの作成

この表では、MIMMA コネクタの対応する 5 つの実行プロファイルを示します。

| 名前 | 型 |
| -------- | -------- |
| Profile1 | フル インポート (ステージのみ) |
| Profile2 | 完全な同期 |
| Profile3 | 差分インポート (ステージのみ) |
| Profile4 | 差分同期 |
| Profile5 | エクスポート |

MIMMA コネクタ用のプロファイルを作成するには

1. Synchronization Service Manager を開き、**[ツール]** メニューの **[管理エージェント]** をクリックします。

2. **管理エージェント**の一覧で、**MIMMA** を選択します。

3. [ダイアログ ボックスの実行プロファイルを構成する] を開くには、**[操作]** メニューの **[実行プロファイルを構成する]** をクリックします。

4. テーブル内の各実行プロファイルに対し、以下の手順を実行します。

    - [実行プロファイルを構成する] ウィザードを開くには、**[新しいプロファイル]** をクリックします。

    - テーブルのプロファイル名を **[名前]** ボックスに入力し、**[次へ]** をクリックします。

    - **[種類]** ボックスでは、テーブルのステップの種類を選択し、**[次へ]** をクリックします。

    - **[完了]** をクリックして、実行プロファイルを作成します。

5. [実行プロファイルを構成する] ダイアログ ボックスを閉じるには、**[OK]** をクリックします。

## MIM サービスの構成

MIM ポータルを使用して、MIM サービスの AD ユーザー受信同期規則を作成します。

AD ユーザー受信同期規則を作成するには:

1. MIM ポータル ホーム ページのナビゲーション バーで、**[管理]** をクリックします。

2. [同期規則] ページを開くには、**[同期規則]** をクリックします。

3. [同期規則の作成] ウィザードを開くには、ツールバーで **[新規]** をクリックします。

4. **[全般]** タブで、次の情報を指定し、**[次へ]** をクリックします。

    -   表示名:AD ユーザー受信同期規則
    -   データ フローの方向受信

5. **[スコープ]** タブで、次の情報を指定し、**[次へ]** をクリックします。

    -   メタバース リソースの種類: 個人
    -   外部システム:ADMA
    -   外部システム リソースの種類: 個人

6. **[関係]** タブで、次の情報を指定し、**[次へ]** をクリックします。

    -   リレーションシップ条件を構成するには、MetaverseObject:person(属性) リストおよび ConnectedSystemObject:person(属性) リストから **ObjectSID** を選択します。

    -   **[FIM 内にリソースを作成]** を選択します。

7. **[受信属性フロー]** ページで、次の情報を指定し、**[次へ]** をクリックします。

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

    -   [フロー定義] ダイアログ ボックスを開くには、**[新しい属性フロー]** をクリックします。

    -   **[ソース]** タブ上で、テーブル内の該当する行に対して表示される属性を選択します。

    -   **[送信先]** タブ上で、テーブル内の該当する行に対して表示される属性を選択します。

    -   属性フローの構成を適用するには、**[OK]** をクリックします。

8. **[概要]** タブで、**[送信]** をクリックします。

## テスト環境の初期化
AD データで構成をテストするには、構成を初期化する必要があります。 このプロセスには 4 つのステップがあります。

### プロビジョニングを有効にする

1. Synchronization Service Manager を開きます。

2. [オプション] ダイアログ ボックスを開くには、**[ツール]** メニューの **[オプション]** をクリックします

3. **[同期規則のプロビジョニングを有効にする]** を選択します。

4. [オプション] ダイアログ ボックスを閉じるには、**[OK]** をクリックします。

### MIMMA を初期化する

このコネクタで、完全な同期サイクルを実行します。 サイクル全体は、次の実行プロファイルで構成されます。

    -   Full Import

    -   Full Synchronization

    -   Export

    -   Delta Import

1. Synchronization Service Manager を開き、**[ツール]** メニューの **[管理エージェント]** をクリックします。

2. **管理エージェント**の一覧で、**MIMMA** を選択します。

3. [管理エージェントを実行する] ダイアログ ボックスを開くには、**[操作]** メニューで、**[実行]** をクリックします。

4. 上に示した各実行プロファイルに対し、以下の手順を実行します。

    - [管理エージェントを実行する] ダイアログ ボックスを開くには、**[操作]** メニューで、**[実行]** をクリックします。

    - **[実行プロファイル]** の一覧で、実行する実行プロファイルを選択します

    - 実行プロファイルを開始するには、**[OK]** をクリックします。

#### 属性フローの優先順位の構成

MIM コネクタの初期化中に、構成された同期規則はメタバースに取り込まれました。

このコネクタから提供される属性の属性フロー優先順位を調整し、AD 内に既に存在する属性が確実に、メタバースに配置され、その後 MIM サービス データベースにも配置されるようにします。

### ADMA の初期化

Active Directory コネクタを初期化するには、それに対してフル インポートおよび完全な同期を実行する必要があります。 既存のオブジェクトを AD からコネクタ領域に取り込むには、フル インポートが必要です。 MIM コネクタ領域からメタバースに新しい同期規則を適用することで同期規則が変更されているので、完全な同期が必要です。 学習します。

    -   Full Import

    -   Full Synchronization

1. Synchronization Service Manager を開き、**[ツール]** メニューの **[管理エージェント]** をクリックします。

2. **管理エージェント**の一覧で、**ADMA** を選択します。

3. [管理エージェントを実行する] ダイアログ ボックスを開くには、**[操作]** メニューで、**[実行]** をクリックします。

4. 上に示した各実行プロファイルに対し、以下の手順を実行します。

    - [管理エージェントを実行する] ダイアログ ボックスを開くには、**[操作]** メニューで、**[実行]** をクリックします。

    - **[実行プロファイル]** の一覧で、実行する実行プロファイルを選択します

    - 実行プロファイルを開始するには、**[OK]** をクリックします。

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

>[!div class="step-by-step"]
[« MIM サービスおよびポータル](install-mim-service-portal.md)


<!--HONumber=Apr16_HO2-->


