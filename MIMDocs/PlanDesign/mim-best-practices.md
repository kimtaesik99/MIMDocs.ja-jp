
---
title: "Microsoft Identity Manager 2016 のベスト プラクティス | Microsoft ドキュメント"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.translationtype: Human Translation
ms.sourcegitcommit: 1ef7b9816d265d17ef68fc54e010e655535dcdc8
ms.openlocfilehash: 8a572f06b220f055efb68c5e1b82e379ad3bec8f
ms.contentlocale: ja-jp
ms.lasthandoff: 05/11/2017


---


# <a name="microsoft-identity-manager-2016-best-practices"></a>Microsoft Identity Manager 2016 のベスト プラクティス

このトピックでは、Microsoft Identity Manager 2016 (MIM) を展開および運用するためのベスト プラクティスを説明します。

## <a name="sql-setup"></a>SQL の設定
>[!NOTE]
SQL を実行するサーバーを設定するための次の推奨事項では、FIMService 専用の SQL インスタンスおよび FIMSynchronizationService データベース専用の SQL インスタンスがあることが前提です。 統合環境で FIMService を実行する場合は、構成に適した調整を行う必要があります。

システムの最適なパフォーマンスには、SQL (構造化照会言語) サーバーの構成が重要です。 大規模な実装での MIM のパフォーマンスの最適化は、SQL を実行するサーバーにベスト プラクティスを適用できるかによります。 詳細については、次の SQL のベスト プラクティスについてのトピックを参照してください。

-   [Storage Top 10 Best Practices (記憶域トップ 10 のベスト プラクティス)](http://go.microsoft.com/fwlink/?LinkID=183663)

-   [tempdb のパフォーマンスの最適化](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [SQL Server Best Practices Article (SQL Server のベスト プラクティスの記事)](http://go.microsoft.com/fwlink/?LinkID=188268)

-   [Reorganizing and Rebuilding Indexes (インデックスの再編成と再構築)](http://go.microsoft.com/fwlink/?LinkID=188269)

### <a name="presize-data-and-log-files"></a>データ ファイルおよびログ ファイルのサイズを事前設定する

自動拡張には依存しないでください。 代わりに、これらのファイルの拡張は手動で行ってください。 自動拡張は安全のためにオンのままにできますが、データ ファイルの増加には、ユーザーが先を見越した管理を行う必要があります。 MIM データベースのサンプル サイズについては、「[FIM Capacity Planning Guide](http://go.microsoft.com/fwlink/?LinkID=185246)」 (FIM 容量計画ガイド) を参照してください。

### <a name="to-presize-sql-data-and-log-files"></a>SQL のデータ ファイルおよびログ ファイルのサイズを事前設定するには

1.  SQL Server Management Studio を起動します。

2.  データベースの FIMService に移動し、[FIMService] を右クリックし、[プロパティ] をクリックします。

3.  [ファイル] ページで必要なサイズにデータベース ファイルを拡張します。

### <a name="isolate-log-from-data-files"></a>データ ファイルからログを分離する

SQL サーバーのベスト プラクティスに従い、データベースのトランザクション ログ ファイルおよびデータ ログ ファイルを別の物理ディスクに分離してください。

tempdb ファイルを追加作成する

最適なパフォーマンスを得るには、tempdb ファイルに CPU コアあたり 1 つのデータ ファイルを作成することをお勧めします。

### <a name="to-create-additional-tempdb-files"></a>tempdb ファイルを追加作成するには

1.  SQL Server Management Studio を起動します。

2.  システム データベースのデータベース [tempdb] に移動し、[tempdb] を右クリックし、[プロパティ] をクリックします。

3.  [ファイル] ページで CPU コアごとにデータ ファイルを 1 つ作成します。 tempdb データとログ ファイルは必ず別のドライブとスピンドルに分けます。

### <a name="ensure-adequate-space-for-log-files"></a>ログ ファイルに十分な容量を確保する

復旧モデルのディスク要件を理解しておくことが重要です。 システムの初期ロード時、ディスク領域の使用を制限するには、単純な復旧モードを使用するのが適切ですが、これでは最後のバックアップ以降に作成されたデータが失われる可能性があります。 完全復旧モードを使用する場合、ディスク容量が大量に使用されてしまうのを防ぐために、トランザクション ログを頻繁にバックアップするなど、バックアップでディスク領域の使用を管理する必要があります。 詳細については、「[Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370)」 (復旧モデルの概要) を参照してください。

### <a name="limit-sql-server-memory"></a>SQL Server のメモリを制限する

SQL サーバー上のメモリ量、および SQL サーバーを他のサービス (つまり、MIM 2016 サービスおよび MIM 2016 同期サービスなど) と共有しているかによって、SQL のメモリ消費を制限したい場合があります。 これは、次の手順で実行できます。

1.  SQL Server Enterprise Manager を起動します。

2.  [新しいクエリ] を選択します。

3.  次のクエリを実行します:

  ```SQL
  USE master

  EXEC sp_configure 'show advanced options', 1

  RECONFIGURE WITH OVERRIDE

  USE master

  EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
  WITH OVERRIDE
  ```

  この例は、SQL サーバーを再構成して、メモリの使用を 12 GB 未満にします。

4.  次のクエリを使用して、設定を確認します。

  ```SQL
  USE master

  EXEC sp_configure 'max server memory (MB)'--- verify the setting

  USE master

  EXEC sp_configure 'show advanced options', 0

  RECONFIGURE WITH OVERRIDE
  ```

### <a name="backup-and-recovery-configuration"></a>バックアップおよび復旧を構成する

一般的には、データベースのバックアップは各組織のバックアップ ポリシーに従って行います。 ログの増分バックアップが計画されていない場合、データベースは単純な復旧モードに設定する必要があります。 バックアップ戦略を実装したり、これらのモデルに必要なディスク領域を実装したりする前に、さまざまな復旧モデルの影響について、正しく理解しておいてください。 完全復旧モデルでは、ディスク領域が大量に使用されないようにログのバックアップを頻繁に実行する必要があります。 詳細については、「[Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370)」 (復旧モデルの概要) と「[FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)」 (FIM 2010 のバックアップおよび復旧ガイド) を参照してください。

## <a name="create-a-backup-administrator-account-for-the-fimservice-after-installation"></a>インストール後に FIMService 用にバックアップ管理者アカウントを作成する


>[!IMPORTANT]
FIMService 管理者セットのメンバーには、FIM の展開を操作するのに不可欠な固有のアクセス許可があります。 管理者セットの 1 人としてログオンできない場合、唯一の解決策は、システムの以前のバックアップにロールバックすることです。 この状況を緩和するには、インストール後の構成で他のユーザーを FIM 管理者セットに追加することをお勧めします。

## <a name="fim-service"></a>FIM サービス


### <a name="configuring-fim-service-service-exchange-mailbox"></a>FIM サービスのサービス Exchange メールボックスを構成する

次に MIM 2016 サービスのサービス アカウント用に Microsoft Exchange Server を構成するベスト プラクティスを示します。

- 社内からのみ電子メール アドレスを受け付けられるように、サービス アカウントを構成します。 具体的には、サービス アカウントのメールボックスは、外部の SMTP サーバーからメールを受信できないようにします。

#### <a name="to-configure-the-service-account"></a>サービス アカウントを構成するには

1.  Exchange 管理コンソールで、**[FIM サービスのサービス アカウント]** を選択します。

2.  [プロパティ] を選択し、[メール フローの設定] を選択して、**[Mail Delivery Restrictions (メッセージの配信制限)]** を選択します。

3.  **[すべての送信者に認証を要求する]** チェックボックスをオンにします。

詳細については、「[Configure Message Delivery Restrictions](http://go.microsoft.com/fwlink/?LinkID=183625)」 (メッセージの配信制限の構成) を参照してください。

-   サイズが 1 MB より大きいメールを拒否するように、サービス アカウントを構成します。 メールボックスまたはメールが有効なパブリック フォルダーに、[メッセージ サイズの制限を構成](http://go.microsoft.com/fwlink/?LinkID=183626)する場合のベスト プラクティスに従います。

-   メールボックスの記憶域のクォータが 5 GB となるようにサービス アカウントを構成します。 最適な結果を得るには、「[Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929)」 (メールボックスの記憶域のクォータの構成) に記載されているベスト プラクティスに従います。

## <a name="mim-portal"></a>MIM ポータル


### <a name="disable-sharepoint-indexing"></a>SharePoint のインデックス作成を無効にする

Microsoft Office SharePoint® のインデックス作成は無効にすることをお勧めします。 インデックスを必要とするドキュメントはなく、インデックスを作成すると、FIM 2010 で多数エラー ログが作成されたり、パフォーマンス上の問題が発生する可能性があります。 SharePoint のインデックス作成を無効にするには

1.  MIM 2016 ポータルをホストするサーバーで、[スタート] をクリックします。

2.  [すべてのプログラム] をクリックします。

3.  [すべてのプログラム] の一覧から [管理ツール] をクリックします。

4.  [管理ツール] の [SharePoint サーバーの全体管理] をクリックします。

5.  [サーバーの全体管理] ウィンドウで [サーバー構成の管理] をクリックします。

6.  [オペレーション] ページの [グローバル構成] で、[タイマ ジョブの定義] をクリックします。

7.  [タイマ ジョブの定義] ページで、[SharePoint Services Search の更新] をクリックします。

8.  [タイマ ジョブの編集] ページで、[無効] をクリックします。

## <a name="mim-2016-initial-data-load"></a>MIM 2016 の初期データをロードする

このセクションでは、外部システムから FIM 2010 へ初期データをロードする際にパフォーマンスを向上させる手順を示します。 次の手順の多くは、システムの初期作成時に一時的にのみ必要であり、完了時にはリセットする必要があることに注意してください。 これは 1 回限りの操作であり、継続的に同期するものではありません。

>[!NOTE]
FIM 2010 と Active Directory ドメイン サービス (AD DS) 間でユーザーを同期する方法の詳細については、FIM のドキュメントの「[How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277)」 (Active Directory から FIM へユーザーを同期する方法) を参照してください。

>[!IMPORTANT]
このガイドの SQL の設定のセクションで説明しているベスト プラクティスに従っていることを確認してください。                                                                                                                                                      |

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>手順 1: データの初期ロード用に SQL サーバーを構成する
最初に大量のデータをロードするとき、一時的にフルテキスト検索をオフにし、MIM 2016 管理エージェント (FIM MA) のエクスポートが完了した後でそれを再度オンにすることで、データベースへの入力時間を短縮できます。

フルテキスト検索を一時的にオフにするには:

1.  SQL Server Management Studio を起動します。

2.  [新しいクエリ] を選択します。

3.  次の SQL ステートメントを実行します。

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING =
MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

SQL サーバーの復旧モデルに必要なディスク要件を理解していることが重要です。 バックアップ スケジュールによっては、システムの初期ロード時に、ディスク領域の使用を制限する、単純な復旧モードを使用することを検討してください。しかし、データ損失の観点からの影響は理解しておく必要があります。
完全復旧モードを使用する場合は、大量のディスク領域が、トランザクション ログの頻繁なバックアップによって占有されてしまうのを防ぐために、バックアップでディスクの使用量を管理する必要があります。

>[!IMPORTANT]
これらの手順を組み込まないと、ディスク領域が大量に使用されてしまい、領域がなくなる場合があります。 このトピックの詳細については、「[Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370)」 (復旧モデルの概要) を参照してください。 「[The FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)」 (FIM のバックアップと復元のガイド) にも追加の情報があります。

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>手順 2: ロード時に MIM を必要最小限に構成する

初期ロード時には、FIM には管理ポリシー規則やセット定義に必要最小限の構成のみを行います。 データのロードが完了したら、展開に必要な追加のセットを作成します。 アクション ワークフローで [ポリシー更新時に実行] 設定を使用し、それらのポリシーをロードされたデータにさかのぼって適用できます。

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>手順 3: 外部 ID データを使用して FIM サービスを構成および入力する


ここでは、「How Do I Synchronize Users from Active Directory 

Domain Services to FIM」 (グループを Active Directory ドメイン サービスから FIM に同期する方法) ガイドで説明する手順に従い、システムに Active Directory のユーザーを構成し、同期する方法を説明します。 グループ情報を同期する必要がある場合のその進め方の手順については、「How Do I Synchronize Groups from Active Directory Domain Services to FIM」 (グループを Active Directory ドメイン サービスから FIM に同期する方法) ガイドを参照してください。

#### <a name="synchronization-and-export-sequences"></a>シーケンスを同期およびエクスポートする

パフォーマンスを最適化するには、同期の実行後にエクスポートを行います。これにより、コネクタ スペースで多数のエクスポート操作が保留になります。

次いで、確認のインポートを、影響を受けるコネクタ スペースに関連付けられている管理エージェントで実行します。 たとえば、データの初期ロードの一環として、同期実行プロファイルをいくつかの管理エージェントで実行する必要がある場合、同期を実行するたびに、エクスポートを行い、差分インポートを実行します。

初期化のサイクルの一部である各ソース管理エージェントに対して、次の手順を実行します。

1.  ソース管理エージェントでフル インポートを実行します。

2.  ソース管理エージェントで完全な同期を実行します。

3.  すべての影響を受けるターゲット管理エージェントで、ステージングされたエクスポート操作を使用しエクスポートします。

4.  すべての影響を受けるターゲット管理エージェントで、ステージングされたエクスポート操作を使用し差分インポートします。

### <a name="step-4-apply-your-full-mim-configuration"></a>手順 4: 完全な MIM 構成を適用する


データの初期ロードが完了したら、展開に MIM の完全な構成を適用する必要があります。

シナリオに応じ、これは、追加のセット、MPR、およびワークフローを作成することを含みます。 システム内の既存のすべてのオブジェクトにさかのぼって適用する必要のあるすべてのポリシーには、アクション ワークフローで [ポリシー更新時に実行] を使用し、ロードされたデータにそれらのポリシーをさかのぼって適用します。

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>手順 5: 以前の設定に SQL を再構成する


SQL は標準の設定に変更する必要があります。 以下は、必要な操作の例です。

-   フルテキスト検索をオンにします

-   組織のポリシーに従って、バックアップ ポリシーを更新します

データの初期ロードが完了したら、フルテキスト検索を再び有効にする必要があります。 次の SQL ステートメントを実行し、再度フルテキスト検索をオンにします。

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

単純な復旧モードに切り替える場合は、組織のバックアップ ポリシーに従って、バックアップのスケジュールを必ず再構成してください。 FIM のバックアップ スケジュールの詳細については、「[FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)」 (FIM バックアップと復元のガイド) を参照してください。

## <a name="configuration-migration"></a>構成を移行する


### <a name="avoid-changing-display-names"></a>表示名は変更しないようにする

MPR など、オブジェクトの種類の多くで、syncproduction.ps1 スクリプトは 2 つのシステム間の唯一のアンカー属性として表示名を使用します。 その結果、既存の MPR の表示名を変更すると、既存の MPR が削除され、新しい MPR が作成されます。 これは、移行手順で、その結合条件が変更された MPR を正しく結合できないために発生します。 この問題を回避するには、カスタム属性をすべての構成オブジェクトの種類とバインドし、その属性を結合条件として使用します。 これにより、移行手順に影響せずに表示名を変更できます。

### <a name="avoid-changing-the-content-of-intermediate-files"></a>中間ファイルの内容を変更しないようにする

下位レベルのオブジェクトのファイル形式とアプリケーション プログラミング インターフェイス (API) は公開されており、開発者による操作もサポートされていますが、移行時は中間形式の内容は変更しないことをお勧めします。 ただし、changes.xml から ImportObjects 全体を削除するか、pilot.xml で検索または置換操作を実行して、実稼働環境の DNS 情報のバージョン番号やパイロットのドメイン ネーム システム (DNS) 情報を置き換える必要がある場合があります。

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>異なるバージョン間で移行を行うときバージョン番号が pilot.xml で正しいことを確認する

異なるバージョン間での移行は非推奨またはサポート対象外ですが、pilot.xml でパイロット バージョンの番号を実稼働バージョンの番号に置き換え、実行することができます。 特に WorkflowDefinition と 

ActivityInformationConfiguration のオブジェクトは、実稼働環境のワークフロー アクティビティを正確に参照するためにバージョン番号を必要とします。 バージョン番号を置き換えないと、Compare-FIMConfig コマンドレットにより WorkflowDefinitions の拡張可能なオブジェクトのマークアップ言語 (XOML) 属性間に違いがあることが識別され、パイロットのバージョン番号が移行されます。 バージョン番号が不正な場合、実稼働環境の FIM サービスがワークフロー アクティビティを開始できない場合があります。

### <a name="avoid-cyclic-references"></a>循環参照を回避する

一般に、循環参照は、MIM 構成では推奨されません。
ただし、セット A がセット B を参照し、セット B もセット A を参照するなど、循環は発生することがあります。循環参照の問題を回避するには、互いを参照しないように、セット A またはセット B の設定の定義を変更する必要があります。 それから移行プロセスを再開します。 循環参照があり、その結果 Compare-FIMConfig コマンドレットでエラーが発生する場合、手動で循環を絶つ必要があります。 Compare-FIMConfig コマンドレットは優先順位順で変更の一覧を出力するので、構成オブジェクトの参照の間で循環が存在していない必要があります。

## <a name="security"></a>セキュリティ

### <a name="mim-ma-account"></a>MIM MA アカウント

MIM MA アカウントは、サービス アカウントとは見なされておらず、通常のユーザー アカウントである必要があります。 このアカウントは、FIM 同期サービスのサービス アカウントが権限を借用できるようにローカルにログオンできる必要があります。

MIM MA アカウントでローカルにログオンできるようにするには

1.  [スタート]、[管理ツール]、[ローカル セキュリティ ポリシー] の順にクリックします。

2.  [ローカル ポリシー] ノードを開き、[ユーザー権利の割り当て] をクリックします。

3.  [ローカル ログオンを許可する] で FIM MA アカウントが明示的に指定されていることを確認するか、既にアクセス許可があるグループの 1 つに追加します。

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>FIM 同期サービスおよび FIM サービス アカウント

安全な方法で MIM サーバー コンポーネントを実行しているサーバーを構成するには、サービス アカウントを制限する必要があります。 MIM MA アカウントを有効にする前の手順を使用して、FIM 同期サービスおよび FIM サービス アカウントに、次の制限を設定します。

-   バッチ ジョブとしてのログオンを拒否する

-   ローカルでのログオンを拒否する

-   ネットワークからのこのコンピューターへのアクセスを拒否する

サービス アカウントは、ローカルの Administrators グループのメンバーではないようにする必要があります。

FIM 同期サービスのサービス アカウントは、FIM 同期サービス (FIMSyncAdmins など FIMSync で始まるグループ) へのアクセスを制御するために使用するセキュリティ グループのメンバーではないようにする必要があります。

>[!IMPORTANT]
 両方のサービス アカウントに同じアカウントを使用するオプションを選択して、FIM サービスと FIM 同期サービスを分離した場合、ネットワーク上の mms 同期サービス サーバーからこのコンピューターへ拒否アクセス権は設定できません。 アクセスが拒否されると、構成を変更してパスワードを管理するために FIM サービスが FIM 同期サービスと通信できなくなります。

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>キオスクのようなコンピューターに展開されるパスワードのリセットにより、仮想メモリのページファイルがクリアされるようにローカルのセキュリティが設定される必要がある

キオスクにするワークステーションに FIM パスワードのリセットを展開した場合、シャットダウン: 仮想メモリのページファイル ローカル セキュリティ ポリシー設定をオンにして、未承認のユーザーがプロセス メモリの機密情報を使用できないようにすることを推奨します。

### <a name="implementing-ssl-for-the-fim-portal"></a>FIM ポータル用に SSL を実装する

クライアントとサーバー間のトラフィックをセキュリティで保護するには、FIM ポータル サーバーで Secure Socket Layer (SSL) を使用することを強くお勧めします。

SSL を実装するには:

1.  MIM ポータル サーバーで IIS マネージャーを開きます。

2.  ローカル コンピューター名をクリックします。

3.  [サーバー証明書] をクリックします。

4.  [証明書の要求の作成] をクリックします。

5.  [共通名] テキスト ボックスに、サーバー名を入力します。

6.  [次へ]、[次へ] を順にクリックします。

7.  ファイルを任意の場所に保存します。 この場所へは、以降の手順でアクセスする必要があります。

8.  Windows Internet Explorer® で https://servername/certsrv を開きます。 サーバー名を、証明書を発行するサーバー名に置き換えます。

9.  [Request a new Certificate (新しい証明書の要求)] をクリックします。

10. [Submit an Advanced Request (事前要求の送信)] をクリックします。

11. Base 64 エンコードを使用して [Submit a Certificate Request (証明書の要求の送信)] をクリックします。

12. 前の手順で保存したファイルの内容を貼り付けます。

13. [証明書のテンプレート] で、[Web サーバー] を選択します。

14. [送信] をクリックします。

15. 証明書をデスクトップに保存します。

16. IIS マネージャーで [Complete Certification Request (証明書の要求の完了)] をクリックします。

17. IIS マネージャーをデスクトップに保存した証明書をポイントします。

18. [フレンドリ名] にはサーバー名を入力します。

19. [サイト] をクリックし [SharePoint – 80] を選択します。

20. [バインド]、[追加] を順にクリックします。

21. [HTTPS] を選択します。

22. 証明書には、サーバーと同じ名前のものを選択します (これはインポートした証明書です)。

23. [OK] をクリックします。

24. HTTP バインディングを削除します。

25. [SSL 設定] をクリックし、[Require SSL (SSL が必要)] をオンにします。

26. 設定を保存します。

27. [スタート]、[管理ツール] の順にクリックし、[SharePoint 3.0 の全体管理] をクリックします。

28. [サーバー構成の管理]、[代替アクセス マッピング] を順にクリックします。

29. [http://servername] をクリックします。

30. [http://servername] を [https://servername] に変更し、[OK] をクリックします。

31. [スタート] ボタン、[ファイル名を指定して実行] を順にクリックし、「iisreset」と入力して [OK] をクリックします。

## <a name="performance"></a>パフォーマンス

パフォーマンスを最適にするには、次の構成を行います。

-   この文書の SQL の設定セクションの説明に従って、SQL の設定のベスト プラクティスを適用します。

-   FIM 2010 R2 のポータル サイトで SharePoint のインデックス作成をオフにします。 詳細については、この文書の「SharePoint のインデックス作成を無効にする」セクションを参照してください。

## <a name="feature-specific-best-practices--i-want-to-remove-this-and-collapse-this-section-and-just-have-the-specific-features-at-header-2-level-versus-3"></a>機能別のベスト プラクティス (これを削除してこのセクションを閉じ、3 に対し、ヘッダー 2 レベルでは具体的な機能のみにしたいです)


### <a name="request-management"></a>要求管理

既定で、MIM 2016 は、関連付けられている承認、承認応答、およびワークフロー インスタンスがある完了した要求を含む、有効期限が切れたシステム オブジェクトを 30 日間間隔で削除します。 組織でより長い要求履歴が必要な場合、MIM から要求をエクスポートし、それを補助データベースに保存し、30 日の期間を超過しても保持します。 30 日間の要求の削除期間は構成可能ですが、この期間を長くするとシステム内の追加のオブジェクトによりパフォーマンスが悪化する場合があります。

### <a name="management-policy-rules"></a>管理ポリシーの規則

#### <a name="use-the-appropriate-mpr-type"></a>適切な MPR の種類を使用する

MIM には、Request と Set Transition の 2 種類の MPR が用意されています。

-   Request MPR (RMPR)

 - リソースに対する作成、読み取り、更新、または削除 (CRUD) 操作のアクセス制御ポリシー (認証、承認、およびアクション) を定義するために使用されます。
 - FIM のターゲット リソースに対して CRUD 操作が発行されたときに適用されます。
   - 範囲は、CRUD が要求した規則に該当する、規則で定義されている一致する基準に限定されています。


-   Set Transition MPR (TMPR)
 - オブジェクトがどのように遷移の設定が表す現在の状態になったかにかかわらず、ポリシーの定義に使用されます。 TMPR は権利ポリシーのモデル化に使用します。
 - リソースが関連セットに入るか出るときに適用されます。
 - そのセットのメンバーに範囲は限定されています。

>[メモ] 詳細については、「[Designing Business Policy Rules](http://go.microsoft.com/fwlink/?LinkID=183691)」 (ビジネス ポリシー規則の設計) を参照してください。

#### <a name="only-enable-mprs-as-necessary"></a>必要に応じてのみ MPR は有効にする

構成を適用する場合、アクセス許可は最小限のみ適用するという原則に従ってください。 MPR は FIM の展開のアクセス ポリシーを制御します。 多くのユーザーが使用する機能のみを有効にします。 たとえば、すべてのユーザーが FIM をグループ管理に使用するとは限らないので、関連付けられているグループ管理の MPR は無効にする必要があります。 既定で FIM は、管理者以外の多くのアクセス許可が無効になって出荷されます。

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>組み込みの MPR は直接変更するのではなく複製する

組み込みの MPR を変更する必要がある場合、必要な構成で新しい MPR を作成し、組み込みの MPR をオフにします。 これにより、アップグレード手順で導入される組み込みの MPR に対する今後の変更は、システム構成に悪い影響を与えないことが保証されます。

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>エンドユーザーのアクセス許可には、ユーザーのビジネス ニーズに合わせた明示的な属性リストを使用する必要がある

明示的な属性のリストを使用すると、属性がオブジェクトに追加されるとき、アクセス許可のないユーザーに偶発的に権限を割り当ててしまうのを防ぎます。
管理者は、アクセスを削除しようとするのではなく、新しい属性に明示的にアクセスを付与する必要があります。

データへのアクセスは、ユーザーのビジネス ニーズの範囲にする必要があります。 たとえば、グループのメンバーには、メンバーであるグループのフィルター属性へのアクセスは必要ありません。 フィルターにより、ユーザーが通常はアクセスできない組織のデータが、意図せず公開されてしまうことがあります。

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>MPR ではシステムの有効なアクセス許可を反映する必要がある

ユーザーが使用できない属性のアクセス許可は付与しないようにします。 たとえば、objectType などのコア リソースの属性へのアクセス許可は付与しないようにする必要があります。 MPR にかかわらず、作成後にリソースの種類を変更することはシステムで拒否されます。

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>MPR で明示的な属性を使用する場合、読み取りのアクセス許可は変更および作成のアクセス権とは別にする必要がある

MPR で明示的に属性を指定する場合、作成および変更に必要な属性は、読み取りに使用できるものとは通常は異なります。 たとえば、作成または変更はシステム属性には指定できないのに対し、読み取りは Creator または objectId などのシステム属性に付与できます。

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>規則で明示的な属性を使用するとき、作成のアクセス許可は変更のアクセス権とは別にする必要がある

Create 操作では、ユーザーは操作の一環で、objectType を選択する必要があります。 これは、作成操作後には変更できないコア システム属性です。

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>同じアクセス要件を持つすべての属性で 1 つの要求 MPR を使用する

変更される可能性のない同じアクセス要件を持つ属性は、効率化のために 1 つの要求 MPR に結合することができます。

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>選択したプリンシパル グループにも無制限のアクセスは与えないようにする

FIM ではアクセス許可は、肯定アサーションとして定義されます。 FIM では拒否のアクセス許可はサポートしていないため、リソースに無制限のアクセスを与えると、アクセス許可の除外の作成が複雑になります。 ベスト プラクティスとしては、必要なアクセス許可のみを提供します。

>[!NOTE]
以下に、権利のセクションが続きます。 レベル 5 のヘッダーを作成しないように、どのように結合したらよいか考えています。
#### <a name="use-tmprs-to-define-custom-entitlements"></a>カスタムの権利の定義に TMPR を使用する

カスタムの権利の定義には RMPR の代わりに Set Transition MPR (TMPR) を使用してください。
TMPR は、権利の割り当てや削除に、定義済みの遷移セット、ロールおよびそれに伴うワークフロー アクティビティで定義したメンバーシップに基づいて、状態ベースのモデルを提供しています。 TMPR は、移行して入ってくるリソースに 1 つ、そして移行して出るリソースに 1 つ、ペアで常に定義する必要があります。 さらに、各遷移 MPR は、アクティビティをプロビジョニングおよびプロビジョニング解除するために、別のワークフローを含む必要があります。

>[!NOTE]
すべてのプロビジョニング解除のワークフローでは、[ポリシー更新時に実行] 属性を True に設定する必要があります。

#### <a name="enable-the-set-transition-in-mpr-last"></a>Set Transition In MPR は最後に有効にする

TMPR のペアを作成する場合、Set Transition In MPR は最後にオンにします。 この順序によって、Out MPR をオンにする前に、In MPR がオンになっているとき、リソースがセットに追加され削除される場合、それが権利に残されることがないことが保証されます。

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>TMPR のワークフローでは、ターゲット リソースの状態を最初に確認する必要がある

プロビジョニングのワークフローでは、ターゲット リソースが権利に従って既にプロビジョニングされているかを最初に確認する必要があります。 そのような場合、何も実行されないはずです。

プロビジョニング解除のワークフローでは、最初に対象のリソースがプロビジョニングされたかどうかを確認する必要があります。 プロビジョニングされている場合、ターゲットのリソースをプロビジョニング解除します。
そうでない場合、何も行いません。

#### <a name="select-run-on-policy-update-for-tmprs"></a>TMPR に [ポリシー更新時に実行] を選択する

これにより、ポリシーの更新が実装され、TMPR に関連付けられているアクション ワークフローで [ポリシー更新時に実行] を使用したとき、正しいプロビジョニングの動作が適用されることを保証します。 これにより、ポリシーの定義の変更によって、遷移セットの新しいメンバーにアクション ワークフローが適用されることが保証されます。

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>2 つの異なる遷移セットに同じ権利を関連付けないようにする

同じ権利に 2 つの異なる遷移セットを関連付けると、リソースが 1 つのセットから別のリソースに移動したとき、権利の不要な失効と再付与が発生します。 ベスト プラクティスとして、1 つのセットに関連する権利を必要とするすべてのリソースが含まれるようにします。 これにより、遷移セットとワークフローを付与する権利との間に 1 対 1 の関係があることが保証されます。

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>システムで権利を削除するとき、正しい順序で行う

システムで権利を削除するときに使用する手順の順序によって、操作の結果が 2 とおりになる場合があります。 希望する影響がどちらの順序であるか理解しておいてください。

システムから権利を削除するには (そしてその権利を現在保持しているすべてのメンバーから取り消すには):

1.  T-In MPR を無効にします。 これにより新たに付与されることを回避できます。

2.  セットが空になるように、T-Set フィルターを削除するか、変更します。 これにより、既存のすべてのメンバーが移行され、権利に関連付けられている構成済みのプロビジョニング解除ワークフローを含め、移行ポリシーが適用されます。

3.  T-Out MPR を無効にします。

権利は削除するが、現在のメンバーをそのままにする場合 (たとえば、権利の管理に FIM を使用するのをやめる):

1.  T-In MPR を無効にします。 これにより新たに付与されることを回避できます。

2.  T-Out MPR を無効にします。

3.  セットが空になるように、T-Set フィルターを削除するか、変更します。 セットと TMPR の関連付けが解除されたので、プロビジョニング解除のワークフローは適用されません。

### <a name="sets"></a>設定

セットのベスト プラクティスを適用する場合、管理容易性と今後の管理のしやすさから最適化が与える影響を考慮する必要があります。
これらの推奨事項を適用する前に、予想される稼働環境の規模で適切なテスティングを行い、パフォーマンスと管理容易性のバランスを判断する必要があります。

>[!NOTE]
以下のすべてのガイドラインは動的なセットと動的なグループに該当します。


#### <a name="minimize-the-use-of-dynamic-nesting"></a>動的なネストの使用を最低限にする

これは、別のセットの ComputedMember 属性を参照するセットのフィルターを意味します。 セットを入れ子にする一般的な理由は、多数のセットにメンバーシップの条件が重複しないようにするためです。 このアプローチを取ることにより、セットの管理が容易になる一方、パフォーマンスとのトレードオフがあります。 セット自体を入れ子にするのではなく、入れ子になったセットのメンバーシップ条件を複製することにより、パフォーマンスは最適にできます。

機能要件を満たすためにセットを入れ子にすることが避けられない場合があります。 セットを入れ子にする主な状況は次のとおりです。 たとえば、フルタイム従業員所有者なしですべてのグループのセットを定義するには、セットの入れ子は `/Group[not(Owner =
/Set[ObjectID = ‘X’]/ComputedMember]` のように使用する必要があります (ここで 'X' はすべて正社員のセットの ObjectID です)。

#### <a name="minimize-the-use-of-negative-conditions"></a>否定条件の使用を最小限に抑える

否定条件とは、`!=`、`not()`、`\<`、`\<=` などの演算子や関数を使用するメンバーシップ条件です。 可能な場合、パフォーマンスを最適化するには、否定条件ではなく、複数の肯定的な条件を使用して希望の条件を表現します。

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>複数の値を持つ参照属性を使用してメンバーシップ条件の使用を最小限にする

複数の値がある参照属性を使用して条件を使用することは最小限にする必要があります。このようなセットが多数ある場合、メンバーシップ条件で使用される属性の操作上のパフォーマンスに影響する可能性があります。

### <a name="password-reset"></a>パスワード リセット

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>パスワードのリセットに使用する、キオスクのようなコンピューターでは、仮想メモリのページファイルがクリアされるようにローカルのセキュリティを設定する

キオスクを想定したワークステーションに FIM 2010 パスワードのリセットを展開するとき、シャットダウン:クリア仮想メモリ ページファイル ローカル セキュリティ ポリシー設定をオンにし、未承認のユーザーがプロセス メモリの機密情報を使用できないようにすることを推奨します。

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>ログオンしているコンピューターでユーザーは常にパスワードのリセットを登録する必要がある

ユーザーが Web ポータルを使用してパスワードのリセットを登録する場合、FIM 2010 では常に、Web サイトにログオンしているユーザーに関係なく、ログオン ユーザーの代わりに登録を開始します。 ユーザーはログオンしているコンピューターで常にパスワードのリセットを登録する必要があります。

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>AvoidPdcOnWan レジストリ キーは True に設定しない

MIM 2016 パスワードのリセットを使用する場合は、AvoidPdcOnWan レジストリ キーを true に設定しないでください。

レジストリ キーを true に設定した場合、ユーザーはパスワード ゲートを経て、プライマリ ドメイン コント ローラー (PDC) でパスワードをリセットして、ログオンしようとすることになります。 このレジストリ キーのため、ローカルのドメイン コントローラーは PDC で二次認証を行わず、ログオン要求を拒否します。 何度も拒否されると、ユーザーはドメインからロックアウトされ、サポートに連絡しなければならなくなります。

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>クリア テキストのパスワードのログ記録を有効にしない

サービス レベルの診断トレースを Windows 

Communication Foundation (WCF) でオンにするとき、クリア テキストのパスワードを記録できます。 このオプションは既定では無効になっており、実稼働環境では有効にすることは推奨されません。 これらのパスワードは、ユーザーがパスワードのリセットを登録するときに、暗号化された Simple Object Access Protocol (SOAP) メッセージで、クリア テキストの要素として表示されます。 詳細については、「[メッセージ ログの構成](http://go.microsoft.com/fwlink/?LinkID=168572)」を参照してください。

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>パスワードのリセット操作に認証ワークフローをマップしない

パスワードのリセット操作には、認証ワークフローはアタッチしないようにしてください。
パスワードのリセットでは、応答は同期される必要がありますが、承認アクティビティなどのアクティビティを含む承認ワークフローでは非同期です。

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>パスワードのリセットに複数のアクション アクティビティをマップしない

パスワードのリセット操作には、1 つ以上のアクション アクティビティを含むワークフローはアタッチしないでください。 このシナリオ例は、2 度目の AD DS パスワード リセット アクティビティをパスワード リセット MPR にアタッチする場合です。 このシナリオはサポートされていません。

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>既存のワークフローでアクティビティの順番を追加、削除または変更する場合には再登録が必要

既存のワークフローで認証アクティビティを追加、削除したり、その順序を変更する場合、常に再登録を要求するオプションを選択します。 ワークフローにアクティビティが追加されたり、そこから削除された後、ユーザーが再登録される前に、パスワードのリセット認証を試行した場合、望ましくない影響がある場合があります。

### <a name="portal-configuration-and-resource-control-display-configuration"></a>ポータルの構成とリソース コントロールの表示構成

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>ユーザーのプロファイル ページにプライバシーに関する免責事項を追加することを検討してください。

MIM では、既定で一部のユーザー プロファイル情報が他のユーザーに表示される場合があります。 管理者は、ユーザーへの礼儀として、ユーザー プロファイルのページに企業のポリシーと一致するカスタムテキストを追加することを検討してください。 MIM のポータル ページにカスタム テキストを追加する方法の詳細については、「[Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848)」 (FIM ポータルの構成およびカスタマイズ) を参照してください。

### <a name="schema"></a>Schema

#### <a name="do-not-delete-person-or-group-resource-types"></a>Person または Group の種類のリソースは削除しない

Person または Group の種類のリソースは Core の種類のリソースとしてマークされていませんが、リソース自体またはそれらに割り当てられた属性は削除すべきではありません。 MIM ポータルのユーザー インターフェイス (UI) では Person または Group のリソースの種類とその属性が存在している必要があります。

#### <a name="do-not-modify-the-core-attributes"></a>Core 属性は変更しない

すべてのリソースの種類には 13 の Core 属性が割り当てられています。 いかなる種類のリソースへも関係は変更しないでください。 13 の Core 属性は次のとおりです。

-   CreatedTime

-   Creator

-   DeletedTime

-   説明

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   ロケール

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

監査要件への依存関係を持つスキーマ リソースを削除しない

そのリソースの監査要件がまだある場合、そのスキーマ リソースは削除しないようにする必要があります。

#### <a name="making-regular-expressions-case-insensitive"></a>正規表現の大文字小文字を区別する

FIM では、正規表現の一部の大文字と小文字を区別すると便利な場合があります。 ?!: を使用すると、グループ内の大文字小文字は無視できます。 たとえば従業員の種類では、次を使用します。

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>メンバー属性の計算

同期エンジンに公開されるメンバー属性は、実際に ComputedMembers にマッピングされています。 これは、条件を使用するメンバーと手動で選択されたメンバーの組み合わせです。 (Filter、ExplicitMembers および ComputedMembers の) 3 つの属性をすべて追加した場合でも、メンバー属性の動的な計算、グループとセット以外の種類のリソースでは起こりません。

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>文字列の先頭と末尾の空白は無視される

FIM では、先頭と末尾にスペースを入れ文字列を入力できますが、FIM システムではこれらのスペースは無視されます。 先頭と末尾にスペースを入れ文字列を送信すると、同期エンジンと Web サービスではそれらのスペースは無視されます。

#### <a name="empty-strings-do-not-equal-null"></a>空の文字列は Null と同じではない

FIM のこのリリースでは、空の文字列は Null と同等ではありません。 空の文字列の入力は有効な値と見なされます。 存在しない場合、Null と見なされます。

### <a name="workflow-and-request-processing"></a>ワークフローおよび要求の処理

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>MIM 2016 に同梱されている既定のワークフローは削除しない

FIM 2010 では次のワークフローが同梱されており、削除されるべきではありません。

-   有効期限のワークフロー

-   管理者向けフィルター検証ワークフロー

-   非管理者向けフィルター検証ワークフロー

-   グループの有効期限通知ワークフロー

-   グループ検証ワークフロー

-   所有者の承認ワークフロー

-   パスワード リセット アクション ワークフロー

-   パスワード リセット AuthN ワークフロー

-   所有者の承認を使用した申請元の検証

-   所有者の承認を使用しない申請元の検証

-   登録に必要なシステム ワークフロー

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>2 つ以上の ApprovalActivities を並列で実行しない

2 つ以上の ApprovalActivities は並列に実行しないようにする必要があります。 実行すると、承認のフェーズで要求が動かなくなってしまう可能性があります。 複数の承認には、より多数の承認者を承認に含めるか、2 つのアクティビティを連続して並べます。

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>承認アクティビティでは MIM リソース データは変更されない

承認ワークフローのワークフローの一環として、関数エバリュエーター アクティビティなど、MIM リソースを変更するアクティビティを使用するのは避けてください。 処理の承認ポイントで要求がコミットされていないため、ID 情報に対して行われるすべての変更は、要求が拒否されている可能性があるにもかかわらず、適用されます。

### <a name="understanding-fim-service-partitions"></a>FIM サービス パーティションを理解する

FIM の目的は、さまざまな FIM クライアントで開始される、FIM 同期サービスや構成済みのビジネス ポリシーに応じたセルフ サービス コンポーネントなどの要求を処理することです。 仕様により、各 FIM サービス インスタンスは、FIM サービスのパーティションとも呼ばれる、1 つ以上の FIM サービス インスタンスで構成されている論理グループに属します。 すべての要求の処理に FIM サービスを 1 つのみ使用している場合、処理の遅延が発生する可能性があります。 一部の処理はセルフ サービスの操作に適した既定のタイムアウト値を超過する場合があります。 この問題を解決するには、FIM サービスのパーティションが助けとなる場合があります。 その他の情報については、「Understanding FIM Service Partitions」 (FIM サービス パーティションを理解する) を参照してください。

