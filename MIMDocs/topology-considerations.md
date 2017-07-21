---
title: "展開用のトポロジ ガイド | Microsoft Docs"
description: "MIM 2016 コンポーネントを理解し、これらを環境内に展開する方法についての提案を得ます。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1695cc2df766df3c38a0e1393f6f974102f9fd36
ms.sourcegitcommit: 0cb8269f07a5f419d2d1cd760d9cc78b8a1c8aa9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/14/2017
---
# <a name="topology-considerations"></a>トポロジに関する考慮事項
Microsoft Identity Manager (MIM) コンポーネントは、同じサーバー上または複数の構成の複数のサーバー間に展開することができます。 展開用に選択したトポロジは、MIM で達成できるパフォーマンスに影響します。 この記事では、実装を検討できる複数の展開トポロジについて説明します。


>[!NOTE]
これらのオプションは、MIM Sync、MIM Service、および MIM Portal のみを使用して ID 管理を行う展開に適用されます。  MIM CM や MIM BHOLD Suite を使用する展開、または特権アクセス管理のための展開には、別の展開オプションが用意されています。


## <a name="mim-components"></a>MIM コンポーネント
展開トポロジを設計する場合は、各コンポーネントの動作や、それらのすべての相互作用を理解することが重要です。

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**MIM ポータル** - パスワードのリセット、グループの管理、および管理操作のためのインターフェイスです。
    -
- **MIM サービス** - MIM 2016 ID 管理機能を実装する Web サービスです。
- **MIM 同期サービス** - 他の ID システムとデータを同期します。
- **Microsoft SQL Server** - MIM サービスと MIM 同期サービスは、どちらも SQL データベースにデータを格納します。

次の表に、各 MIM コンポーネントをホストするためのオプションを示します。 これらは同じコンピューターでホストすることも、複数のサーバーとクラスター間で分散させることもできます。

| | MIM ポータル | MIM サービス | MIM 同期サービス | SQL Server |
| --- | --- | --- | --- | --- |
| 同じコンピューター | [はい] | ○ | ○ | [はい] |
| 別のサーバー | Yes | ○ | ○ | Yes |
| ネットワーク負荷分散クラスター | [はい] | Yes | | |
| サーバー クラスター | | | | Yes |


## <a name="multitier-topology"></a>多層トポロジ
多層トポロジは、最もよく使用されるトポロジです。 最大限の柔軟性を提供します。 MIM ポータル、MIM サービス、およびデータベースは、層に分離され、複数のコンピューターに展開されます。 このトポロジでは、さまざまなコンポーネントの MIM のスケーリングの柔軟性を高めます。 たとえば、ネットワーク負荷分散 (NLB) クラスターに他のサーバーを追加することで MIM ポータルを水平方向にスケーリングできます。 同様に、NLB クラスターを使用して、必要に応じてクラスター内のコンピューター (ノード) の数を増やすことで、MIM サービスをスケーリングできます。

多層トポロジでは、各 SQL データベースをホストする専用のコンピューター (MIM サービス用に 1 台と MIM 同期サービス用にもう 1 台) が割り当てられます。 SQL データベースをホストするコンピューターのパフォーマンスのスケーラビリティは、ハードウェアを追加またはアップグレードすることで向上することができます。たとえば、CPU をアップグレードする、CPU を追加する、ランダム アクセス メモリ (RAM) を増設またはアップグレードする、またはハードドライブ構成をアップグレードして読み取りおよび書き込みアクセスを増やして待機時間を短縮する方法があります。

![MIM 多層トポロジの図](media/MIM-topo-multitier.png)

この構成では、MIM 同期サービスとそのデータベースは、同じコンピューター上でホストされています。 ただし、MIM 同期サービスとそのデータベースが別のコンピューターでホストされていて、これらの間に 1 ギガビットの専用ネットワーク接続がある場合は、同等のパフォーマンスを実現することができます。


## <a name="multitier-topology-with-multiple-mim-services"></a>複数の MIM サービスを含む多層トポロジ
外部システムとのデータの同期には時間がかかり、その間システムには大きな負荷がかかります。 同期の構成によりワークフローを含むポリシーがトリガーされる場合、これらのポリシーは、エンドユーザーのワークフローとリソースの競合をします。 このような問題は、プロセスの完了を待機しているエンド ユーザーによりリアルタイムで行われるパスワードのリセットなど、認証ワークフローで顕著になります。 エンド ユーザーの操作に MIM サービスの 1 つのインスタンスを提供し、別のポータルを管理データの同期に提供することで、エンド ユーザーの操作の応答性を向上することができます。

![複数の MIM 多層トポロジの図](media/MIM-topo-multitier-multiservice.png)

標準の多層トポロジと同様に、NLB クラスターを使用し、必要に応じてクラスター内のノードの数を増やすことで、MIM ポータルのパフォーマンスを向上することができます。

MIM 同期サービスと MIM サービス データベースをホストしている SQL Server を実行しているコンピューターは、MIM 展開の全体的なパフォーマンスに大きな影響を及ぼします。 そのため、SQL Server ドキュメントのデータベース パフォーマンスを最適化するための推奨事項に従います。 詳細については、次のドキュメントを参照してください。

## <a name="see-also"></a>関連項目
- テスト ビルドとパフォーマンス テストの結果に関する詳細は、ダウンロード可能な「[Forefront Identity Manager (FIM) 2010 Capactity Planning Guide (Forefront Identity Manager (FIM) 2010 容量計画ガイド)](http://go.microsoft.com/fwlink/?LinkId=200180)」を参照してください。
