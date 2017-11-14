---
title: "BHOLD SP1 のインストール | Microsoft Docs"
description: "BHOLD SP1 のインストールに関するドキュメント"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c36a9d02e90101b98ade913224e573ed21dc3d5c
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2017
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Microsoft BHOLD スイート SP1 (6.0) のインストール ガイド

Microsoft® BHOLD スイート Service Pack 1 (SP1) はアプリケーションのコレクションで、Microsoft Identity Manager 2016 SP1 (MIM) と一緒に使用することで、有効なロール管理、分析、および構成証明を MIM に追加します。 Microsoft BHOLD スイート SP1 は次のモジュールで構成されます。

- BHOLD コア
- Access Management Connector
- BHOLD FIM/MIM 統合
- BHOLD Model Generator
- BHOLD Analytics
- BHOLD Reporting
- BHOLD 構成


>[!NOTE]
**適用対象:** Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>このドキュメントの内容

このドキュメントでは、ビジネス ニーズに合わせて BHOLD デプロイを計画し、各 BHOLD モジュールをインストールする方法について説明します。 モジュール、関連するハードウェア、インフラストラクチャ、およびソフトウェア要件ごとに、インストール前のネットワーク構成、セットアップ中に必要な情報、およびインストール後の手順 (存在する場合) を詳しく説明します。

## <a name="pre-requisite-knowledge"></a>前提条件となる知識

このドキュメントでは、サーバー コンピューターにソフトウェアをインストールする方法に関する基本的な知識があることを前提としています。 また、Active Directory® Domain Services ドメイン サービス、Microsoft Identity Manager SP1 (FIM)、および Microsoft SQL Server 2008 データベース ソフトウェアの基本的な知識も必要です。 AD DS、FIM などの依存テクノロジをセットアップして構成する方法は、このドキュメントでは説明しません。 Microsoft BHOLD モジュールが実行する機能については、[Microsoft BHOLD スイート概念ガイド](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)を参照してください。

## <a name="audience"></a>対象ユーザー

このドキュメントは、IT プランナー、システム アーキテクト、技術上の意思決定者、コンサルタント、インフラストラクチャ プランナー、および Microsoft BHOLD スイートのデプロイを計画する IT 担当者を対象としています。

## <a name="bhold-infrastructure-considerations"></a>BHOLD インフラストラクチャに関する考慮事項

BHOLD と FIM は、ほとんどの場合、大規模なインフラストラクチャ環境で使用されます。 BHOLD と FIM のアーキテクチャは、特定のビジネス ニーズに合わせて調整できます。 次のセクションでは、実現可能なアーキテクチャ ソリューションをいくつか紹介します。 この概要では、すべてのオプションを詳しく説明することはできませんが、BHOLD をネットワークにデプロイするときに参考になる情報を提供します。
 
このセクションでは、次のトピックを取り上げます。

- 単一サーバー アーキテクチャ
- デュアル サーバー アーキテクチャ
- 2 層アーキテクチャ
- SQL Server の推奨事項

### <a name="single-server-architecture"></a>単一サーバー アーキテクチャ

小規模な組織や開発目的でのデプロイについては、次の図に示すように、SQL Server および AD DS と同じサーバーに BHOLD と FIM をインストールできます。
 
![単一サーバー アーキテクチャ](media/bhold-installation-guide/single.png)

BHOLD スイート SP1 と FIM ポータルを単一サーバーに一緒にインストールする場合、BHOLD と FIM の DNS では異なるホスト別名 (CNAME または A レコード) を作成する必要があります。 これにより、BHOLD サービスと FIM サービスに対して、個別のサービス プリンシパル名 (SPN) を作成できます。 詳細については、「[BHOLD Core Installation](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)」 (BHOLD コアのインストール) をご覧ください。
単一サーバー構成への FIM インストールに関するガイダンスについては、Microsoft TechNet ライブラリの「[Common Configuration for Getting Started Guides](https://technet.microsoft.com/library/ff575965.aspx)」 (最初の一般的な構成ガイド) をご覧ください。

### <a name="dual-server-architecture"></a>デュアル サーバー アーキテクチャ

BHOLD コアおよび FIM をそれぞれ別のサーバーにインストールすると、多層アーキテクチャで実現する複雑なデプロイを必要としない中規模な組織でパフォーマンスと柔軟性が向上します。 次の図は、それぞれ独自のサーバーにインストールされた BHOLD と FIM を示しています。FIM サーバーでは SQL Server も実行され、BHOLD と FIM に対してデータベース サービスを提供しています。 FIM サーバーで実行されている FIM 同期サービスにより、FIM データベースと BHOLD データベースの間で変更が同期されます。 エンドユーザー セルフサービスが必要な場合は、BHOLD FIM 統合モジュールを、FIM サービスおよび FIM ポータルと同じサーバーにインストールする必要があることに注意してください。 BHOLD FIM 統合モジュールでは、FIM サービスと BHOLD FIM 統合モジュールが同じサーバーにインストールされていなければなりません。

![デュアル サーバー アーキテクチャ](media/bhold-installation-guide/dual.png)

>[!IMPORTANT]
BHOLD FIM 統合モジュールのレポート機能では、BHOLD データベースと FIM データベースが、同じ SQL Server インスタンスにインストールされている必要があります。また、BHOLD サービス アカウントには、FIM サービス データベースへのアクセス権が必要です。

### <a name="two-tier-architecture"></a>2 層アーキテクチャ

ほとんどの環境で、特にパフォーマンスが重要な場合は、BHOLD スイート SP1、FIM、および SQL Server はそれぞれ個別のサーバーで実行する必要があります (2 層アーキテクチャ)。 2 層アーキテクチャでは、メモリと CPU リソースが層ごとに専用になります。 次の図は、2 層アーキテクチャを構成する方法の 1 つを示しています。 FIM サーバーで実行されている FIM 同期サービスにより、FIM データベースと BHOLD データベースの間で変更が同期されます。 エンドユーザー セルフサービスが必要な場合は、BHOLD FIM 統合モジュールを、FIM サービスおよびポータルと同じサーバーにインストールする必要があることに注意してください。

![2 層アーキテクチャ](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server の推奨事項

BHOLD を大規模な組織にデプロイする場合は、次のガイドラインに従って、Microsoft SQL Server データベースを設定することを強くお勧めします。

- FIM または BHOLD サービスから切り離されたサーバーに SQL Server をデプロイします。
- ログ ファイルは物理ディスク レベルでデータ ファイルから切り離します。
- RAID を使用してストレージ冗長性を提供している場合は、RAID レベル 10 (1 + 0) を使用します。 RAID レベル 5 は使用しないでください。
- SQL Server を実行しているサーバーに対して 2 GB を超える物理メモリを使用する場合は、必ず正しい設定を構成してください。
- 最適な BHOLD パフォーマンスを実現するには、Microsoft SQL Server 2008 R2 以降を使用します。

SQL Server のベスト プラクティスの詳細については、Microsoft TechNet ライブラリの「[Storage Top 10 Best Practices](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx)」 (ストレージのベスト プラクティス トップ 10) をご覧ください。

### <a name="trusted-certificates-list-update"></a>信頼できる証明書一覧の更新

サービスを開始する前に証明書チェーンを検証するように、Windows を構成できます。 こうしたシステムでは、サービスの実行可能コードが、サーバーの信頼できる証明書一覧 (TCL) に含まれない証明書で署名されている場合、そのサービスは開始できません。 Microsoft BHOLD スイート SP1 ソフトウェアは、Microsoft Root Certificate Authority 2010 証明書のコード署名証明書チェーンを使用してコード署名されています。
インターネットに接続されている場合は、インターネット経由で Microsoft からルート証明書を取得するように、Windows を構成できます。 ただし、接続されていないシステムの場合、Windows Server に含まれるのは、Windows がリリースされる前にルート プログラムに存在していた証明書のみです。 Windows Server 2010 より前の Windows Server リリースでは、こうした証明書に、BHOLD スイート SP1 コード署名証明書チェーンの検証に必要なルート証明書は含まれません。 最新 TCL がない可能性があるシステムに 1 つ以上の Microsoft BHOLD スイート SP1 モジュールをインストールする場合は、その BHOLD スイート SP1 モジュールをインストールする前に、ルート更新パッケージをダウンロードしてインストールするか、グループ ポリシーを使用してルート更新パッケージをインストールする必要があります。 詳細については、[Windows ルート証明書プログラムのメンバー](http://support.microsoft.com/kb/931125)に関するページをご覧ください。

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Windows Server 2012/2016 への BHOLD スイート SP1 のインストールに必要なステップ 

![IIS による BHOLD のインストール](media/bhold-installation-guide/iis-install-bhold.png)

BHOLD スイート SP1 を Windows Server 2012 または 2016 にインストールする場合、BHOLD Web ページは、```C:\Windows\System32\inetsrv\config``` にある applicationHost.config ファイルを変更するまで使用できなくなります。 ```<globalModules>``` セクションで、```preCondition="bitness64``` を、```<add name="SPNativeRequestModule"``` で始まるエントリに追加します。これにより次のようになります。

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

ファイルを編集して保存した後、iisreset コマンドを実行して IIS サーバーをリセットします。


## <a name="upgrading-bhold-suite"></a>BHOLD スイートのアップグレード

既存の BHOLD スイートのインストールをアップグレードすることはできません。 BHOLD モジュールを更新するには、その前に既存の BHOLD スイートのインストールをアンインストールする必要があります。 既存の BHOLD ロール モデルがある場合は、BHOLD データベースをアップグレードし、更新された BHOLD コア モジュールをインストールするときにそれを使用できます。 詳細については、「[Replacing BHOLD Suite with BHOLD Suite SP1](https://technet.microsoft.com/en-us/library/jj874043(v=ws.10).aspx)」 (BHOLD スイートを BHOLD スイート SP1 に置き換える) をご覧ください。


## <a name="next-steps"></a>次のステップ

- [BHOLD 開発者用リファレンス](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD のバージョン履歴](../reference/version-bhold-history.md)
