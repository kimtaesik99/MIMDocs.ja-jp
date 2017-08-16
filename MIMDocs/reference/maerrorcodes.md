---
title: "管理エージェントの実行エラー コード | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 511bd9ae4e303a2142c3f101c3880dc8cb46be3d
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2017
---
# <a name="management-agent-run-error-codes"></a>管理エージェントの実行エラー コード

以下の表では、Microsoft Identity Manager (MIM) 2016 内の Synchronization Service Manager ユーザー インターフェイスに表示されるエラー コードと、その各々のエラーについての説明を示します。

## <a name="connection-errors"></a>接続エラー

| エラー                  | 説明                                    |
|--------------------------|--------------------------------------------|
| failed=connection         | 接続先ディレクトリへの接続が、認証以外の理由で失敗しました。 たとえば、ネットワークが使用できない、または対象サーバーがオフラインである、などです。                                                                                                                                                                                                                                                                                                                      |
| dropped-connection        | 管理エージェントと接続先ディレクトリの間の接続が存在しません。 管理エージェントは、複数のインスタンスで、接続先ディレクトリへの再接続を試行します。                                                                                                                                                                                                                                                                                                         |
| failed-authentication     | 指定された資格情報を使用して認証することができません。                                                                                                                                                                                                                                                                                                                                                                                                                          |
| failed-permission         | 接続先ディレクトリ内のコンテナーにアクセスする権限がありません。 このエラーは、別の接続されたディレクトリ コンテナーを検索するライトウェイト ディレクトリ アクセス プロトコル (LDAP) 管理エージェントでのみ発生します。                                                                                                                                                                                                                                                              |
| failed-search            | コンテナーまたはテーブルの検索が、予期しないエラーで失敗しました。                                                                                                                                                                                                                                                                                                                                                                                                                            |
| warning-no-watermark      | フル インポートを行うときに、管理エージェントが透かしを読み取ることができません。 このエラーは、最初の管理エージェントの構成が完了し、接続されたディレクトリの変更ログが有効になっているときに、Sun ONE Directory Server 5.1 (旧称 iPlanet Directory Server) の管理エージェントでのみ発生します。 後で、接続されたディレクトリの変更ログをオフにした際に管理エージェントの構成が更新されていない場合は、フル インポートが行われるとこの警告が発生します。 |
| no-start-partition-delete | この管理エージェント用に最初に構成された LDAP パーティションが存在しません。 パーティションが削除されたか、パーティションが削除され同じ名前で再作成されたときに、このエラーが返されます。 後者の場合は、名前は同じでも、エラー パーティション GUID は変わります。                                                                                                                                                                           |

## <a name="discovery-errors"></a>検出エラー

| エラー                   | 説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| missing-change-type      | ファイルベースおよびデータベースの管理エージェントや、Sun および Netscape のディレクトリ サーバー用の管理エージェントにより実行される差分インポート中に、変更型の列値 (追加、変更、削除) が存在しない場合に、このエラーが返されます。                                                                                                                                                                                                                                          |
| invalid-change-type      | ファイルベースおよびデータベースの管理エージェントや、Sun および Netscape のディレクトリ サーバー用の管理エージェントにより実行される差分インポート中に、変更型の列値が有効な変更型の一覧と合致しない場合に、このエラーが返されます。 また、変更型フィールドが存在するがその値が追加でない場合にも、LDAP データ交換形式 (LDIF) フル インポートから返されます。                                                                                  |
| multi-valued-change-type | ファイルベースの管理エージェントや Sun および Netscape のディレクトリ サーバー用の管理エージェントにより実行される差分インポート中に、変更型に複数の値が存在する場合に、このエラーが返されます。                                                                                                                                                                                                                                                                                                        |
| need-full-object        | ファイルベースの管理エージェントによる差分インポート実行中に、またはファイルベースの管理エージェントから再開するときに、このエラーが返されます。 これは、管理エージェントが、コネクタ スペースに配置できないオブジェクトに対する変更を送信したことを示します。 同期エンジンは、オブジェクトのすべての属性の現在値を要求しています。 これはファイルからのインポートであるため、そうした情報は入手できません。 フル インポートによって、この問題を解決できます。 |
| missing-dn              | ドメイン名の値がない場合に、ファイルベースの管理エージェント (つまり、LDIF、DSML、または構成されているドメイン名属性を持つフラット ファイルの管理エージェント) に対してこのエラーが返されます。 また、Sun ONE Directory Server の変更ログが破損し、ドメイン名の属性が見つからない場合もこれが返されます。 これは、管理エージェントは要素を読み取り、それを解析できたが、そのオブジェクトのドメイン名の値はなかったことを示します。                           |
| dn-not-ldap-conformant   | LDAP、LDIF、DSML、または構成されているドメイン名属性を持つフラット ファイルの管理エージェントが、LDAP 仕様に準拠していないドメイン名の値を報告したときに、このエラーが返されます。                                                                                                                                                                                                                                                                                |
| invalid-dn              | ドメイン名が FIM 制約を満たしていないことを管理エージェントが報告したときに、このエラーが返されます。以下が含まれます。   <br/>&#176; FIM で許可されていない 1 つまたは複数の文字 <br/>&#176; 空の相対識別名 (別名 RDN) <br/>&#176; FIM の最大値を超える相対識別名 <br/>&#176; ドメイン名の階層レベル数が FIM の最大値を超えている                                                                                                                                                                                                                                                                                                                                                            |
| missing-anchor-component | 1 つまたは複数のアンカー構築ルール属性に値がないためにアンカーを構築できないときに、ファイルベースおよびデータベースの管理エージェントや Sun および Netscape のディレクトリ サーバーの管理エージェントによって、このエラーが返されます。   |
| multi-valued-anchor-component | アンカー構築ルール属性に複数の値があるためにアンカーを構築できない場合に、Sun および Netscape のディレクトリ サーバーの管理エージェントによって、このエラーが返されます。 |   
|anchor-too-long | アンカー構築によって FIM の最大制限サイズを超えるアンカーが生成されるときに、ファイルベースおよびデータベースの管理エージェントや Sun および Netscape のディレクトリ サーバーの管理エージェントによって、このエラーが返されます。|
|duplicate-object | ファイルベースおよびデータベースの管理エージェントによるフル インポートで、この実行中に同じアンカーを持つオブジェクトが既に同期エンジンに報告済みであるときに、このエラーが返されます。|

>[!Note]
コネクタ スペース オブジェクトの陳腐化は、現在の実行ステップの完了が、成功、同期エラー付き完了、警告付き完了、または一時的完了である場合にのみ発生します。

|エラー| 説明 |
|----|----|  
|missing-object-class | 変更ログが破損している場合は、ファイルベースの管理エージェント (つまり、DSML、LDIF、または構成済みのオブジェクトのクラス属性を持つフラット ファイルの管理エージェント) によって、または Sun および Netscape のディレクトリ サーバーの管理エージェントに対して、このエラーが返されます。 これは、管理エージェントが、オブジェクト クラス属性の値を読み取れないことを示します。 |
|missing-object-type | 破損したドロップ ファイルからインポートの再開を実行するときに、このエラーが返されます。 このエラーは、通常の操作中に発生するものではありません。        |
|unmappable-object-type  | マッピングのプレフィックスのいずれにも一致しない、オブジェクト クラスの値のセットを持つオブジェクトを読み取るときに、ファイルベースの管理エージェントによってこのエラーが返されます。 |
|parse-error | エントリを解析できない場合に、差分モードでの Sun および Netscape のディレクトリ サーバーの管理エージェントと、ファイルベースの管理エージェントによって、このエラーが返されます。 エラーを見つけやすいように、`<entry-number>`要素 (ほとんどの場合 `<line-number>` と `<column-number>`) が表示されます。 `<attribute-name>` 要素が存在する可能性があります。 Sun および Netscape のディレクトリ サーバーの管理エージェントは、これが発生したときに実行を終了します。 ファイルベースの管理エージェントは、検出エラーを記録して、続行します。 |   
|read-error | 特定のオブジェクトの読み取りで一般的なエラーがある場合に、呼び出しベースの管理エージェントによってこのエラーが返されます。 これは一般的に、実行を終了させます。 接続されたデータ ソースのエラー要素が存在し、それを使用して問題のトラブルシューティングを行うことができます。    |
|staging-error | このエラーはほとんどの管理エージェントによって返されます。 これは、同期エンジンがコネクタ スペースで差分をステージできなかったことを示します。 サーバーでは、問題に関する情報を提供してトラブルシューティングに使用できるイベント ログを作成します。 ほとんどの管理エージェントは、エラーが記録されたときにインポート実行を続行しますが、変更ログ処理内のギャップがコネクタ スペース内に不整合な状態を発生させる可能性があるため、Sun および Netscape の管理エージェントの差分実行は停止します。 このエラーは、通常の操作中に発生するものではありません。  |
|invalid-modification-type | LDIF 管理エージェントでの差分インポート中に、オブジェクト レベルの変更タイプが標準の LDIF 変更タイプのいずれでもない、または objectclass に nonreplace 変更タイプ (add: objectclass や delete: objectclass など) がある場合に、このエラーが返されます。    |
|conflicting-modification-types        | LDIF 管理エージェントによってこのエラーが返されます。下記のように、異なる属性レベルの変更タイプが同じレコード内で見つかった (この場合は、競合する型を生成した属性の名前が報告される) か、同じファイル内に次のような複数の置換 LDIF 差分が表示されることを示します。    <br/> [LDIF ファイルの例](./media/maerrorcodes/conflict-modification-types.jpg) |
|multi-single-mismatch | FIM で単一値の属性として定義されている属性に対して、1 つの値の追加や 1 つの値の削除が複数報告される場合は、ファイルベースの管理エージェントによってこのエラーが返されます。 このエラーは、FIM で格納された接続されたデータ ソース スキーマが正しく指定されていない (ファイルベースの管理エージェント)、または現在のスキーマに対して古い、ということを示している可能性があります。 エラーのコンテキストを提供する `<attribute-name>` 要素が含まれています。    |
|invalid-attribute-value | スキーマで宣言されている属性型に準拠していない属性値が読み込まれたときに、呼び出しベースの管理エージェントによってこのエラーが返されます。 エラーのコンテキストを提供する `<attribute-name>` 要素が含まれています。  |
|invalid-base64-value| 無効な base64 文字列を検出したときに、LDIF、DSML、Sun および Netscape のディレクトリ サーバーの管理エージェントによってこのエラーが返されます。     |
|invalid-numeric-value | 数値を解析できないときに、ファイルベースの管理エージェントおよび LDAP 用の管理エージェントによってこのエラーが返されます。 エラーのコンテキストを提供する `<attribute-name>` 要素が含まれています。 |
|invalid-boolean-value | ブール値を解析できないときに、ファイルベースの管理エージェントおよび LDAP 用の管理エージェントによってこのエラーが返されます。 エラーのコンテキストを提供する `<attribute-name>` 要素が含まれています。  |
|reference-value-not-ldap-conformant | ドメイン名の値がLDAP 仕様に準拠していないときに、LDAP、LDIF、DSML、またはフラット ファイル (構成されているドメイン名属性を持つ) の管理エージェントによってこのエラーが返されます。 このエラー メッセージには、エラーのコンテキストを提供する `<attribute-name>` 要素が含まれています。|
|invalid-reference-value | ドメイン名が FIM 制約を満たしていないときに、管理エージェントによってこのエラーが返されます。以下が含まれます。  <br/>&#176; FIM で許可されていない 1 つまたは複数の文字   <br/>&#176; 空の相対識別名 (別名 RDN)  <br/>&#176; FIM の最大値を超える相対識別名  <br/>&#176; FIM の最大値を超えたドメイン名の階層レベル数                |
|unsupported-value-type | ファイルで指定された値の型が属性の型と互換性がない場合に、DSML または LDIF 管理エージェントによってこのエラーが返されます。以下が含まれます。    <br/>&#176; 文字列以外の属性、または予約済みのキーワード (dn、objectclass、changetype など) に、URI または URL 値が指定されている。  <br/>&#176; base64 値が changetype 属性に指定されている。  <br/>&#176; 非 ASCII 文字を含む文字列値がバイナリ属性に指定されている。 |

## <a name="synchronization-errors"></a>同期エラー

| エラー               |説明      |
|---------|-------------|
| extension-dll-exception  | このエラーは、ルール拡張が例外を引き起こした場合に発生します。 このエラーが発生した場合は、\<exception-error-info\> 要素を見て、例外のコール スタックを調べます。 場合によっては `<rule-error-info>` が存在し、エラー発生時にどのようなルールが処理されていたかに関する追加情報を提供します。        |
| extension-dll-crash | このエラーは、ルール拡張を実行しているプロセスが予期せず終了したときに発生します。 このエラーは、ルール拡張がプロセス外で実行されている場合にのみ発生します。 このエラー値について考えられる原因は、ルール拡張がアクセス違反を発生させるコードを呼び出していることです。 |
| extension-dll-timeout                          | 顧客が拡張タイムアウトを構成し、単一の顧客の拡張コードのエントリ ポイントでの呼び出しが、構成されたタイムアウトを超える場合に、このエラーが発生します。 `<exception-error-info>` は、タイムアウトしたときにどのようなエントリ ポイントが呼び出されていたかに関するコンテキスト情報を提供します。 場合によっては `<rule-error-info> が存在し、エラー発生時にどのようなルールが処理されていたかに関する追加情報を提供します。 拡張機能を実行しているプロセスをデバッグしているときはタイムアウトは強制実行されないため、注意してください。  |              
| extension-projection-object-type-not-set        | ルール拡張内の **IMASynchronization.ShouldProjectToMV** メソッドの実装がメタバース オブジェクト型を指定していないときに、このエラーが発生します。                    |
| extension-projection-invalid-object-type        | ルール拡張内の **IMASynchronization.ShouldProjectToMV** メソッドの実装で、送信メタバース オブジェクト型の値を Synchronization Service Manager の Metaverse Designer の一覧にない値に設定したときに、このエラーが発生します。 メソッドが指定したオブジェクト型の値のいずれかを使用していることを確認してください。   |
| extension-join-resolution-invalid-object-type   | ルール拡張内の **IMASynchronization.ResolveJoinSearch** メソッドの実装によって、送信メタバース オブジェクト型の値が Synchronization Service Manager の Metaverse Designer の一覧にない値に設定されたときに、このエラーが発生します。 メソッドによって、送信メタバース オブジェクト型の値が、リストされているオブジェクト型の値のいずれかに設定されていることを確認してください。   |
| extension-join-resolution-index-out-of-bounds   | ルール拡張内の **IMASynchronization.ResolveJoinSearch** メソッドの実装によって、負のインデックス値またはメタバース オブジェクトの数より大きいインデックス値が設定されるときに、このエラーが発生します。                     |
| extension-provisioning-call-limit-reached       | 1 つのオブジェクトの同期中に **IMASynchronization.Provision** メソッドの呼び出しが 10 回を超えたときに、このエラーが発生します。 プロビジョニング メソッド内のユーザー ロジックがオブジェクトのプロビジョニング解除を行い、メタバース オブジェクトの変更を引き起こし、その結果プロビジョニングの新しい呼び出しを起こすような属性再呼び出しがある場合は、このメソッドは複数回呼び出すことができます。 無限プロビジョニング ノートが起きるのを防ぐために、プロビジョニング メソッドに対する 10 回の呼び出し制限が設定されます。                                                                                                                                               |
| extension-deprovisioning-invalid-result         | **IMASynchronization.Deprovision** メソッドの実装で無効な DeprovisionAction 列挙値を返すときに、このエラーが発生します。 メソッドが有効な値を返すことを確認します。        |
| extension-entry-point-not-implemented           | ルール拡張が **EntryPointNotImplementedException** 例外をスローするときに、このエラーが発生します。 |
| extension-unexpected-attribute-value            | ルール拡張が **UnexpectedDataException** 例外をスローするときに、このエラーが発生します。            |
| flow-multi-values-to-single-value              | Synchronization Service Manager で構成されたインポートまたはエクスポート属性のフロー ルールで、複数の値を持つ属性を単一値の属性へ流し込もうとしたときに、このエラーが発生します。 このエラーは Synchronization Service Manager で構成されている直接のフロー ルールに対してのみ返されます。 フロー ルールで、複数値を単一値の属性へ流し込むルール拡張を使用している場合は、**TooManyValuesException** 例外がスローされます。          |
| cs-attribute-type-mismatch                     | インポートされた属性の型が、管理エージェントのスキーマで指定された属性の型と一致しないときに、このエラーが発生します。 このエラーの原因の 1 つとして、格納されている接続されたデータ ソース スキーマが、接続されたデータ ソースの実際のスキーマで期限切れになっていることが考えられます。 格納されている接続されたデータ ソース スキーマを最新の状態にするには、Synchronization Service Manager を使用してスキーマを更新します。|
| join-object-id-must-be-single-valued            | Synchronization Service Manager の管理エージェントのプロパティで指定された結合ルールでメタバース オブジェクトを結合するために使用されるデータ ソースの属性値に、複数の値が含まれているときに、このエラーが発生します。 結合ルールで使用されるデータ ソースの属性値には、単一値しか含めることができません。     |
| dn-index-out-of-bounds       | Synchronization Service Manager 管理エージェントのプロパティで構成されたインポート属性フローにおいて使用される識別名コンポーネント インデックス値が、ソース オブジェクトの識別名内のコンポーネントの数より大きいときに、このエラーが発生します。      |
| connector-filter-rule-violation                | 追加または名前変更のプロビジョニング操作を行うか属性フローをエクスポートし、コネクタ フィルター構成の結果としてコネクタ オブジェクトがフィルターされたディスコネクタ オブジェクトになる場合に、このエラーが発生します。 この値は、明示的なコネクタ オブジェクトでは発生しません。    |
| unsupported-container-delete   | 管理エージェントが、プロビジョニング解除中にコンテナー オブジェクトを削除しようとしています。 FIM 管理エージェントでは、子オブジェクトを持つコンテナー オブジェクトを削除できません。            |
| ambiguous-import-flow-from-multiple-connectors   | メタバース オブジェクトに接続されているソース管理エージェントのもとに複数のコネクタがあり、宣言型のインポート属性フロー ルールが定義されているときに、このエラーが発生します。 複数のコネクタがある管理エージェントで属性をメタバース オブジェクトにインポートするには、管理エージェントのプロパティでダイレクト規則を構成するのではなく、ルール拡張を使用してフロー ルールを定義します。                                                                                                                                                                                                          |
| ambiguous-export-flow-to-single-valued-attribute | Synchronization Service Manager の管理エージェントのプロパティで構成されたエクスポート フロー ルールが、メタバース オブジェクトから単一値の属性に複数の値を流し込もうとしたときに、このエラーが発生します。       |
| cannot-parse-object-id | Synchronization Service Manager の管理エージェントのプロパティで指定された結合ルールにあるメタバース オブジェクトを検索するのに使用される文字列値が、正しいグローバル一意識別子 (GUID) 形式ではありません。 GUID の形式は {nnnnnnnn-nnnn-nnnnnnnn-nnnnnnnnnnnn} で、n は 16 進数です。 |
| unexported-container-rename  | IMVSynchronization.Provision メソッドまたは IMASynchronization.Deprovision メソッドの実装が、エクスポートされていない子オブジェクトを 1 つまたは複数持つコンテナー オブジェクトの名前を変更しようとしています。   |
| mv-constraint-violation           | 直接インポート属性フローが発生して、コネクタ スペースからの属性値がメタバース属性の長さ制限を超えているときに、このエラーが発生します。 |
| locking-error-needs-retry   | 複数の管理エージェントが、同じコネクタ スペース オブジェクトを同期しようとしています。 管理エージェントを再実行します。     |
| unique-index-violation              | ユーザーが、メタバース テーブル内の属性に一意のインデックスを手動で設定しようとしています。 メタバース テーブルを手動で構成しないでください。    |
| encryption-key-lost                            | FIM を実行しているサーバーに暗号化キー セットがありません。  |
| unexpected-error                              | 同期エンジンがメタバースに変更を適用しようとすると (プロビジョニングとエクスポート属性フローを含む)、このエラーが発生します。 このエラーは、メタバースへの変更適用の実行中にのみ発生します。 詳細については、イベント ログを確認してください。  |
| exported-change-not-reimported                 | 管理エージェントにエクスポートされる変更が、この管理エージェント インポート実行中に再確認されていないときに、このエラーが発生します。 ユーザーまたは FIM 外部で動作するシステム プロセスが、エクスポート属性フロー ルールが接続されたデータ ソース オブジェクトに値を流し込もうとしているという構成上の問題を含む方法で、接続されたデータ ソースのデータを変更しました。しかし接続されたデータ ソースは、管理エージェントにエラーを報告せずに、自動的に値を別の値にリセットします。 \<change-not-reimported\> 要素は、どの変更が再確認されていないかを示します。 |
| cannot-parse-dn-component                      | このエラーは、LDAP スタイルの識別名 (別名 DN) が構成されている任意の管理エージェントによって返されるもので、コネクタ スペースからメタバースへの同期が失敗しました。 識別名コンポーネントが、送信先属性の型に対して正しい形式ではないため、dncomponent マッピングで解析できません。|
| missing-partition-for-run-step                 | このエラーは、実行プロファイルで指定されたパーティションが見つからないことを示します。 パーティションが削除または名前変更されていないかを確認します。  |

## <a name="export-errors"></a>エクスポート エラー

| エラー                    |説明     |
|--------------------------|------------|
| cd-missing-object         | 接続されたデータ ソースにオブジェクトの変更がエクスポートされたが、接続されたデータ ソースにそのオブジェクトが見つからないときに、このエラーが返されます。 これは、呼び出しベースの管理エージェントに対してのみ返されます。 このエラーの原因は、ユーザーまたは外部プロセスが、FIM 外部で接続されたデータ ソースからオブジェクトを削除したことです。  |
| cd-existing-object        | 接続されたデータ ソースに追加がエクスポートされたが、接続されたデータ ソースにオブジェクトが既に存在するときに、このエラーが返されます。 これは、呼び出しベースの管理エージェントとリレーショナル データベースの管理エージェントに対してのみ返されます。 |
| duplicate-anchor          | 新しくプロビジョニングされたオブジェクトのアンカーが一意でない場合に、このエラーが返されます。 これは、呼び出しベースおよびデータベースの管理エージェントと Sun および Netscape のディレクトリ サーバーの管理エージェントに対してのみ返されます。 このエラーが発生した場合は、アンカー構築規則を確認して、オブジェクトごとに一意のアンカー値が定義されていることを確認します。      |
| ambiguous-update          | アンカーが一意ではないために管理エージェントが更新または削除の差分を適用できない場合にこのエラーが返されます。 これは、Microsoft SQL Server と Oracle データベースの管理エージェントに対してのみ返されます。 このエラーが発生した場合は、アンカー構築規則を確認して、オブジェクトごとに一意のアンカー値が定義されていることを確認します。 |
| password-policy-violation | パスワード属性が、接続されたデータ ソースの管理者により定義されたパスワード ポリシーを満たしていない値に設定または変更されたときに、Active Directory および Active Directory のグローバル アドレス一覧 (GAL) の管理エージェントによってこのエラーが返されます。 |
| password-set-disallowed   | パスワードの暗号化が暗号化なしまたは 128 ビット Secure Sockets Layer (SSL) に設定されており、管理者がこのシナリオでパスワードの設定を許可する上書きを明示的に行っていないときに、Active Directory Application Mode (ADAM) の管理エージェントによってこのエラーが返されます。      |
| kerberos-time-skew        | パスワード属性を設定または変更する際に、FIM サーバーのコンピューターの時刻がドメイン コントローラーの時刻と 5 分以上異なる場合は、Active Directory および Active Directory のグローバル アドレス一覧 (GAL) の管理エージェントによってこのエラーが返されます。  |
| kerberos-no-logon-server  | Active Directory および Active Directory のグローバル アドレス一覧 (GAL) の管理エージェントがパスワードの属性を設定または変更しようとし、ログオン資格情報のドメイン部分のサーバーを解決できないときに、このエラーが返されます。 NetBIOS または DNS 構成が間違っている可能性があります。  |
| encryption-not-enabled    | パスワード属性が設定中または変更中で、管理エージェントが接続されたデータ ソースへの通信に使用している接続が適切な暗号化メカニズム (128 ビット SSL または TLS) で構成されていないときに、Active Directory Application Mode (ADAM) の管理エージェントによってこのエラーが返されます。 ADAM では、パスワード設定に 128 ビット SSL または TLS のいずれかの構成が必要です。   |
| invalid-dn               | 新しくプロビジョニングされたオブジェクトをエクスポートするか既存のオブジェクトを名前変更する場合と、識別名が接続されたデータ ソースの名前付け要件と互換性がない場合に、LDAP と Windows NT 4.0 の管理エージェントによってこのエラーが返されます。|
| schema-violation          | オブジェクト変更をエクスポートして、接続されたデータ ソース スキーマにない属性を追加するとき、または、スキーマで必要なオブジェクトから属性を削除するとき、LDAP の管理エージェントによってこのエラーが返されます。 FIM では、接続されたデータ ソース スキーマの格納されたコピーが自身のルールによって確認されるため、これらの操作の実行は許可されません。 ただしこの問題は、接続されたデータ ソース スキーマに対して FIM スキーマが期限切れである場合に発生することがあります。 この問題が発生した場合は、ユーザー インターフェイスを使用して、管理エージェントのスキーマを更新します。             |
| constraint-violation      | 追加、変更、または削除のエクスポートが、接続されたデータ ソースの強制的な制約に違反しているときに、LDAP およびデータベースの管理エージェントによってこのエラーが返されます。 LDAP 管理エージェントについての一般的な原因としては、単一値の属性へ複数の値を設定している、文字列およびバイナリ属性でフィールド幅制限を超えている、数値属性の範囲制限に違反している、などがあります。 データベース管理エージェントについては、参照整合性、ルール、データベースに定義されている可能性がある制約など、多くの原因が考えられます。 |
| syntax-violation                   | 属性の値が特定の値制約に違反しているときに、LDAP と Windows NT 4.0 の管理エージェントによってこのエラーが返されます。 たとえば、エクスポートされる値に無効な文字が含まれている場合です。 |
| modify-naming-attribute             | 名前付け属性 (多くのオブジェクト型に使用する CN など) が、相対識別名 (別名 RDN) の値と競合している値に設定されているときに、LDAP の管理エージェントによってこのエラーが返されます。 定義が不十分なエクスポート属性フロー ルールが原因で、または新しくプロビジョニングされたオブジェクトに初期値を設定するスクリプト コード内のエラーが原因で、このエラーが発生することがあります。         |
| insufficient-field-width            | 追加または変更をオブジェクトにエクスポートする際に属性の値が列の幅を超えている場合は、固定幅テキスト ファイルの管理エージェントによってこのエラーが返されます。   |
| insufficient-columns                | 追加または変更をオブジェクトにエクスポートする際に、複数値属性の値の数が属性複数値用に構成された列の数を超える場合は、固定幅の区切りテキスト ファイルの管理エージェントによってこのエラーが返されます。 |
| permission-issue                    | 接続されたデータ ソースに対して操作を実行するための十分なアクセス権が管理エージェントにないために、追加、変更、または削除のエクスポートが失敗する場合は、LDAP と Windows NT 4.0 の管理エージェントによってこのエラーが返されます。     |
| dn-attributes-failure               | 対応する接続されたデータ ソース オブジェクトがない参照値に対して、追加または変更をエクスポートして設定を行う場合は、Active Directory、Active Directory グローバル アドレス一覧 (GAL)、Active Directory Application Mode (ADAM) の管理エージェントによってこのエラーが返されます。 このエラーが発生する場合は、コネクタ スペース オブジェクト ビューアーを使用して、参照属性に対するどの変更が正常にエクスポートされなかったかを判断します。 |
| non-existent-parent                 | 接続されたデータ ソースに親オブジェクトが存在しないために、追加または名前変更のエクスポートのいずれかが失敗したときに、LDAP の管理エージェントによってこのエラーが返されます。       |
| code-page-conversion                | FIM を実行しているサーバー内に Unicode で格納されている属性値からエクスポート ファイルのコード ページへの変換が変換エラーのため失敗したときに、ファイルベースの管理エージェントによってこのエラーが返されます。    |
| no-export-to-this-object-type       | プロビジョニング操作の実施を試みた、またはコンピューター オブジェクトの属性フローをエクスポートしたときに、Windows NT 4.0 の管理エージェントによってこのエラーが返されます。 この型のオブジェクトではエクスポート操作は許可されませんが、この型のオブジェクトでインポートを行うことはできます。  |
| missing-provisioning-attribute       | 新しくプロビジョニングされたオブジェクトをエクスポートする際に、新しいオブジェクトのプロビジョニングに必要な特定の属性がルール拡張によって設定されていない場合は、Lotus Notes の管理エージェントによってこのエラーが返されます。 |
| invalid-provisioning-attribute-value | 新しくプロビジョニングされたオブジェクトをエクスポートする際に、ルール拡張で設定されたプロビジョニングの特定の属性が無効である (たとえば、特定の値の範囲内にない) 場合は、このエラーが返されます。       |
|provision-to-secondary-nab                 | このエラーは Lotus Notes の管理エージェントに固有なもので、人や認証オブジェクトをセカンダリの Lotus Notes アドレス帳にプロビジョニングしようとしたときに発生します。 Lotus Notes では、セカンダリのアドレス帳には連絡先のプロビジョニングのみが許可されています。  |                                                           
| missing-anchor-component                   | 新しくプロビジョニングされたオブジェクトをエクスポートする際に、アンカー構築に必要な値が使用できないためにアンカーが生成できない場合は、このエラーが返されます。 考えられる原因は、プロビジョニング中に属性が設定されていない場合 (Sun または Netscape のディレクトリ サーバーの管理エージェント、データベースおよびファイルベースの管理エージェントで)、または、アンカーを自動インクリメント列から構築したときに、接続されたデータ ソースから読み取ることができない場合 (Active Directory、Sun および Netscape ディレクトリ サーバーの管理エージェント、データベース管理エージェントで) です。 |
| multi-valued-anchor-component               | アンカー構築に使用される属性のうちの 1 つが複数の値を持っているために、新しくプロビジョニングされたオブジェクトのアンカーを構築できないとき、Sun および Netscape のディレクトリ サーバーの管理エージェントによってこのエラーが生成されます。 アンカー構築に使用される属性は、接続されたデータ ソース スキーマでは複数値として定義できますが、FIM 内の実際のオブジェクトでは単一の値しか持つことができません。                                                                                                                                                                           |
| anchor-too-long                           | アンカー構築によって FIM の最大制限サイズを超えるアンカーが生成されるときに、ファイルベースおよびデータベースの管理エージェントや Sun および Netscape のディレクトリ サーバーの管理エージェントによって、このエラーが返されます。 コネクタ スペース内の 1 つの属性のアンカー値の最大長は、398 文字です。 アンカーが複数の属性で構築される場合は、追加の属性ごとに 2 文字を減算します。 たとえば、3 つの属性で構築されたアンカー (sn+location+telephoneNumber) は、392 文字の制限があります。                                    |
| invalid-attribute-value                    | このエラーは、接続されたデータ ソースにとって無効な文字を含む属性値をフローしようとしたときに発生します。 たとえば、固定幅テキスト ファイル、区切られたテキスト ファイル、および属性値ペア テキスト ファイルの管理エージェントにエクスポートされる属性値には、CR、LF、または EOF 文字を含めることはできません。       |
| encryption-key-lost          | このエラーは、通常の操作の一部として発生するものではありません。 これは、FIM で、オブジェクトの読み込み時にコネクタ スペースに格納された暗号化済み属性の値を解読できないことを示します。 FIM によって使用される暗号化キーのセットがコンピューターに存在していないことを示している可能性があります。 このエラーは、Active Directory、Active Directory グローバル アドレス一覧 (GAL)、Sun および Netscape のディレクトリ サーバー、Lotus Notes、Windows NT 4.0 など、パスワード属性が含まれる任意の管理エージェントによって生成される場合があります。    |
| locking-error-needs-retry                  | このエラーは、複数の管理エージェントが同時に同じコネクタ スペース オブジェクトの同期を試みた場合にのみ発生します。 このエラーが発生した場合は、もう一度エクスポートを実行してください。     |
| cd-error                                  | 接続されたデータ ソースに特殊なエラーの種類があるときに、このエラーが返されます。 このエラーは \<cd-error\> 要素を伴い、そこに含まれる情報はトラブルシューティングに役立ちます。|
| unexpected-error                           | このエラーは、変更のエクスポートが試みられてそれにより誤動作が生じたときに返されます。 このエラーが発生した場合は、イベント ログを検索し、問題のトラブルシューティングに役立つ詳細情報を確認します。 |
| no-export-to-this-object-type              | プロビジョニング操作の実施を試みたとき、またはコンピューター オブジェクトの属性フローをエクスポートしたときに、Windows NT 4.0 の管理エージェントによってこのエラーが返されます。 Windows NT 4.0 の管理エージェントでは、この型のオブジェクトのエクスポート操作はサポートされていません。|  
| certifier-ou-not-configured                | 新しいユーザーまたはコンテナーをプロビジョニングする際に、\_MMS_Certifier 属性に指定した認証名が、適切に構成された認証コンテナーの名前ではない場合は、Lotus Notes の管理エージェントによってこのエラーが返されます。 各認証コンテナーは、プロビジョニングで使用する前に、Synchronization Service Manager を使用して構成する必要があります。 |
| temporary- certifier-file-creation-failure | 新しいユーザーまたはコンテナーがプロビジョニングされ、認証ファイルの作成プロセスがなんらかの理由 (ディスク領域やアクセス権の不足など) で失敗するときに、Lotus Notes の管理エージェントによってこのエラーが返されます。 認証ファイル作成の FIM プロセスは、認証コンテナーの認証情報 (\_MMS_Certifier 属性で指定) をフェッチするもので、Lotus Notes の管理エージェントの MAData フォルダーに認証ファイルを一時的に作成し、Notes API で使用できるようにします。                                                                               |
| unexpected-provisioning-attribute           | 新しくプロビジョニングされたオブジェクトをエクスポートする際に、他のプロビジョニング属性の値と互換性がないために、ユーザー拡張機能によって設定されたプロビジョニングの特定の属性を含めることができないときに、Lotus Notes の管理エージェントによってこのエラーが返されます。 たとえば、このエラーは以下のような場合に表示されます。  <br/>&#176; 連絡先 (_MMS_IDRegType=0) を作成し、次の属性のいずれかを指定する: \_MMS_Certifier、\_MMS_OU、\_MMS_Password、\_MMS_IDStoreType、\_MMS_IDPath、または MailFile  <br/>&#176; 米国のユーザーまたは国際ユーザーを作成したが、ID ファイル (_MMS_IDStoreType=0) の作成を指定せず、\_MMS_IDPath または MailFile 属性を指定する。OU (認証者) を作成し、\_MMS_OU 属性を指定する  <br/>&#176; O (認証者) を作成し、\_MMS_Certifier 属性を指定する