---
title: Microsoft Identity Manager によるデータの処理 | Microsoft Docs
description: 環境内のデータを識別して報告するための Microsoft Identity Manager のデータ処理を理解し、運用の機能と要件に基づいて特定のシステムでアクションを実行します。
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldiwn
ms.date: 05/22/2018
ms.topic: get-started-article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 6bcf9ab26ba38f3c6eefbdb315d4975320a597b9
ms.sourcegitcommit: 66db63fe2813130764e52381f4f9c8e549d77d39
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/22/2018
---
# <a name="microsoft-identity-manager-data-handling"></a>Microsoft Identity Manager によるデータの処理 

この記事では、多くの接続されたデータ ソースで実施または実装する必要のある検索、削除、更新、レポートの操作の決定方法についてのガイダンスを提供します。 削除または更新の方法を決定する前に、ID マネージャー システム (MIM) の現在の設計と構成を理解することが重要です。 ユーザーは以下の質問を検討して答える必要があります。 

- ビジネス プロセスを支援するため、ID 管理にはどのようなデータが必要ですか。
- 現在のデータは MIM のどこに格納されますか。
- このデータはシステムでどのように使われますか。
- このデータは外部パートナーのデータ ソースと共有していますか (エクスポート)。
- データとその処理に対して権限を持っているソースは何ですか。
- どのようなデータ保有およびデータ削除計画が実施されますか。
- データの処理と管理に必要なすべてのテクノロジを明らかにしましたか。

現在の MIM 環境を理解するには、次のツールを使って MIM 環境を文書化できます。または、実装設計ドキュメントを参照してください。
- [MIM Documentor - 現在の構成をエクスポートできます](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>個人データの検索および特定
MIM 内のデータの検索は、構成や設定に依存します。 ほとんどの環境は相互接続されていますが、わかりやすくするために上位レベルのコンポーネントに分けました。

### <a name="synchronization-service"></a>同期サービス

ユーザーに関連する MIM 内のすべてのデータは、Active Directory (AD) や人事データ ソースから派生します。 個人データを検索するときは、まず AD または接続されたデータ ソースを検索することを検討してください。 

信頼できる情報源が不明な場合は、MIM Synchronization Service Manager コンソールからユーザーを追跡できます。メタバース検索バーをクリックすると、データベースに格納されている識別可能な個人データが表示されます。 ユーザーは特定のユーザーまたは属性を検索できます。

- ユーザー オブジェクト データのレビューまたは検索を実行するには
    - 同期サービス クライアントを開きます
        - メタバース デザイナーを使うと、属性フローのインポートと優先順位を参照できます。
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - メタバース検索を使うと、データベース内の任意のオブジェクトと属性を検索できます ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)
 
オブジェクトを検索した後、オブジェクトをクリックするとユーザー プロファイル ページが開きます。 オブジェクトの詳細では、オブジェクト、その属性、最後の変更、信頼できるソース、および以下の例の管理エージェント構成から派生した関連する接続されたデータ ソースに関する包括的詳細が提供されます。

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>サービスおよびポータル/PAM
サービスとポータルまたは PAM のインスタンスがインストールされている場合、ユーザーを検索できることが重要です。 

ポータルをインストールした場合、UI を使って、任意の属性を検索したり、特定のユーザーをクエリしたりできます。

サービス サーバーのみをインストールした場合は (ポータル UI なし)、[FIMAutomation PSSnapin] に基づいて検索構文を実行できます。例は[こちら](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)にあります。

PAM は上記と同じ構文を使用できます。または、[MIMPAM モジュール](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) (具体的には get-pamuser コマンドレット) を使って、PAM 環境内のユーザーを検索できます。

使用可能なデータを検索するための他のレポート オプションは、サービスとポータルです。
- [ハイブリッド レポート](https://docs.microsoft.com/en-us/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [SCSM でのレポート](https://docs.microsoft.com/en-us/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
BHOLD コア サービスの UI を使うと、ユーザーまたは属性を検索できます。 

![BHOLD の検索](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

BHOLD と同期サービスの [Access Management Connector](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-access-management-connector-install) を同期している場合、接続されているユーザー オブジェクトおよび BHOLD コアに送信している属性を表示できます。

また、BHOLD Reporting モジュールを読み込むことができます。

- [BHOLD Reporting](https://docs.microsoft.com/en-us/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>証明書の管理
証明書管理サービスの検索が UI に組み込まれています。 管理者は起動して [ユーザーを検索してユーザー情報を表示または管理] を選びます。  

![CM 検索](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>個人データのエクスポート
MIM 内のエンティティに関連するデータは複数のソースから派生するため、ほとんどのデータは同期サービス データベースに格納されます。 このため、MIM 同期からオブジェクト関連データをエクスポートする必要があります。または、このデータの所有者を決定できます。

### <a name="synchronization-service"></a>同期サービス
データ エクスポート用の同期サービスは、検索 UI からデータを選択し、コピーして csv や推奨される形式に貼り付けるだけです。 このデータをエクスポートするもう 1 つの方法は、ファイル ベースの MA を作成して、関心対象のフラグが設定されたユーザーに関して必要な現在のデータをドロップします。 ファイル ベースの MA の使用例は[こちら](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/)にあります。


### <a name="service-and-portal--pam"></a>サービスおよびポータル/PAM
サービスとポータルおよび PAM を使って、このデータをエクスポートできます。[FIMAutomation PSSnapin] に基づく検索構文を実行し (例は[こちら](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx))、それを [csv](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6) にパイプします。

PAM は上記と同じ構文を使用できます。または、[MIMPAM モジュール](https://docs.microsoft.com/en-us/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1) (具体的には get-pamuser) を使って、PAM 環境内のユーザーを検索し、それを csv にパイプできます。

- [PowerShell を使って MIM サービスを照会する例](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
BHOLD データは、BHOLD レポート モジュールを使って好みの形式にエクスポートできます。

### <a name="certificate-management"></a>証明書の管理
個人データに関連する証明書管理データは、Active Directory に接続されています。 管理者は、Active Directory PowerShell を使ってこのデータをエクスポートできます。

## <a name="updating-personal-data"></a>個人データの更新

通常、MIM ソリューション内のユーザーまたはオブジェクトに関する個人データは、組織の接続されたデータ ソース内のユーザーのオブジェクトから取得されます。 HR ソース、または AD などの別の権限のあるレコード システムの、ユーザー プロファイルに対して行われた変更は、MIM 同期サービスにおいて反映されます。

### <a name="synchronization-service"></a>同期サービス

管理操作を実行するには、管理者は同期操作または[こちら](https://docs.microsoft.com/en-us/previous-versions/mim/jj590183(v%3dws.10))で定義されている管理者の一部である必要があります。

データの更新は、権限のあるソースからの規則を定義することによって行われます。 管理コンソールは、ソースにおいてデータを更新するための権限のあるソースを識別するのに役立ちます。 もう 1 つのオプションは、HR データなどのソースを維持する必要がある場合にデータ更新を制御するための同期規則や規則の拡張を作成することです。 これらは、サポートされている使用可能なオプションです。

属性を更新するさまざまな方法について詳しくは、以下をご覧ください。 

- [規則の拡張機能の使用](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [外部システムとのデータ同期について](https://docs.microsoft.com/en-us/previous-versions/mim/jj133850(v%3dws.10))

### <a name="service-and-portal--pam"></a>サービスおよびポータル/PAM

PAM データを含めるためのサービスおよびポータルは、FIMAutomation または PAM コマンドレットを使って更新できます。 ポータルを使っている場合は、オブジェクトを検索して変更することにより直接更新することもできます。 注意する必要があるのは、構成によっては、ポータルから更新するだけではそれが残っていることを意味しないことです。 権限のあるソースは、全体の構成に大きく依存します。

### <a name="bhold"></a>BHOLD

BHOLD コアのユーザー インターフェイスまたは Access Management Connector を使って、ユーザーを直接更新することができます。

### <a name="certificate-management"></a>証明書の管理

証明書管理サービス内のユーザーはすべて、Active Directory からの反映です。 更新するには、Active Directory を使ってオブジェクトの詳細を変更します。

## <a name="deleting-personal-data"></a>個人データの削除

>[!Note] 
> この記事は、Microsoft Identity Manager から個人データを削除する方法についてのガイダンスを提供し、GDPR での義務をサポートするために使用できます。 GDPR に関する一般情報については、[Service Trust Portal の GDPR セクション](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)をご覧ください。

MIM 内のデータは接続されているデータ ソースから同期されて、常に更新されます。 ターゲット内のオブジェクトが削除されたときに、セキュリティ調査のために MIM 内のオブジェクトのデータを保持できます。 オブジェクトの削除は、接続されているデータ ソースの規則または規則の拡張 (コード) およびオブジェクトの削除ルールに従って構成されます。

### <a name="synchronization-service"></a>同期サービス
同期サービスは、ビジネス プロセスに応じて、さまざまな方法でデータを処理またはデータを削除します。 属性の削除と更新のオプションの理解に役立つ記事を以下に示します。 

- [プロビジョニング解除について](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)
- [規則の拡張機能の使用](https://msdn.microsoft.com/en-us/library/windows/desktop/ms698810(v=vs.100).aspx)
- [MIM のベスト プラクティス](https://docs.microsoft.com/en-us/microsoft-identity-manager/mim-best-practices)

### <a name="service-and-portal--pam"></a>サービスおよびポータル/PAM

サービスおよびポータルには、既定の 30 日間のシステム リソース リテンション期間の構成をお勧めします。 これはサービスに対し、要求データだけでなく、システムからはクリアする必要のあるオブジェクトの、削除のタイミングを示します。 プロセスが発生すると、このオブジェクトにリンクされているすべてのデータが削除され、これにはすべての SSPR 登録データが含まれます。 これは、上記のオブジェクト削除構成に関わります。 オブジェクトの GUID が格納されているテーブルがあります。 ビルド 4.4.1459 でテーブルの全体サイズを小さくするため、FIM_DeleteExpiredSystemObjectsJob というプロセスが追加されました。このプロセスについて詳しくは、[こちら](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden)をご覧ください。

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

BHOLD は、同期サービスに接続されているほとんどのシステムと同様に、HR などのソース オブジェクトが削除されたら削除されるように構成できます。 これは、管理エージェントで構成されます。 そして、同期サービス機能で記述されているオブジェクト削除規則によって制御されます。

もう 1 つのオプションは、BHOLD コア ユーザー インターフェイスから直接ユーザー オブジェクトを削除することです。 セットアップによってはこれでうまくいきますが、ソースで削除されていないとプロビジョニング ロジックがこのユーザーを再作成する場合があることに注意してください。
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>証明書の管理
CM からユーザーを削除するには、Active Directory でユーザーを削除します。

証明書の管理は、ドメインの sAMAccountName で証明書サービスからのプロファイル uid のみを保存します。 AD からユーザーが削除されると、ユーザー キャッシュは登録した証明書に対してのみ存在します。 データベースからは何も削除しないことをお勧めします。環境の動作全体に悪影響がある可能性があります。

## <a name="opt-out-of-telemetry"></a>製品利用統計情報のオプトアウト
FIM/MIM の以前のビルドは、各展開に関する匿名の製品利用統計情報を収集し、そのデータを HTTPS で Microsoft サーバーに転送しました。 このデータは、マイクロソフトが過去に FIM/MIM の以降のバージョンを向上するために使われました。

>[!Note] 
> リリース 4.5.x.x 以降では、データ収集は無効になります。

以前のバージョンでデータ収集を無効にするには、変更モードを実行して、次のプロンプトを選択解除します。

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

または、レジストリを編集し、次の値を 0 に設定します。(Component)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>次の手順 
- [SQL 関連のプライバシー ガイダンスについて](https://docs.microsoft.com/en-us/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Service Trust Portal の GDPR セクション](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 アーカイブ: 開始 - Forefront Identity Manager 2010 の実装](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)