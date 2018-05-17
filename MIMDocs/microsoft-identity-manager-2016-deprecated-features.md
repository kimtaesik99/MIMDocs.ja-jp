---
title: MIM の非推奨の機能と今後の計画 | Microsoft Docs
description: この記事では MIM Identity Manager 2016 SP1 の非推奨の機能を示します。
keywords: ''
author: barclayn
ms.author: davidste
manager: mbaldwin
ms.date: 2/28/2018
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 50f7b135ce0d5a46ea08068a7658b229759d2b50
ms.sourcegitcommit: 24bb3e82f55971696bdefa6c240f1a27f856e110
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="deprecated-features"></a>非推奨の機能

この記事では Microsoft Identity Manager 2016 SP1 の非推奨の機能について説明します。 Microsoft Identity Manager に残っている機能は引き続きサポートされます。 このような機能は今後のリリースで削除される可能性があるため、新しい展開では推奨されません。  開発者には、新しいアプリケーションまたはソリューションで非推奨の機能を使用しないことをお勧めします。

>[!NOTE]
Microsoft Identity Manager SP1 で削除された機能には ** が付いています。 <br>
サポートの詳細については、[Microsoft Identity Manager のライフサイクル](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)に関するページを参照してください


## <a name="bhold"></a>BHOLD 

お客様が Microsoft BHOLD Suite コンポーネントの新しい展開を開始することはお勧めしません。 BHOLD の既存の展開は引き続きサポートされます。 Azure AD で、[アクセス レビュー](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview)が提供されるようになりました。これにより、BHOLD 構成証明キャンペーン機能の一部が置き換えられます。

## <a name="certificate-management"></a>証明書の管理 
| **[カテゴリ]**                | 
  **非推奨の機能**              | **置換およびコメント**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| 管理エージェント | **FIM 証明書の管理 | MIM 2016 では FIM 証明書の管理エージェントが削除されています。                                                             |

## <a name="service-and-portal"></a>サービスおよびポータル

| **[カテゴリ]**                | 
  **非推奨の機能**              | **置換およびコメント**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| プログラムによる構成 | Web サービスの構成インターフェイス (ma-data と mv-data) | FIM サービスの Web サービスを介して FIM 同期サービスを構成する機能は、次のバージョンで削除される予定です。
|

## <a name="synchronization-service"></a>同期サービス 

| **[カテゴリ]**                | 
  **非推奨の機能**              | **置換およびコメント**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| プログラムによる構成 | Web サービスの構成インターフェイス | FIM サービスを介して FIM 同期サービスを構成する機能は、次のバージョンで削除される予定です。                                                          |
| 管理エージェント           | 組み込みの MA                        | MIM 2016 では、次の MA が削除されています。 </br> 1. **FIM 証明書管理の MA </br>2. **Lotus Notes の MA</br> 3. ** SAP R/3 の MA </br> Lotus Notes と SAP R/3 の MA は新しいバージョンに置き換えられています。 詳細については、[最新のコネクタ バージョンのリリース履歴とダウンロード](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)に関するページを参照してください                                                                                                                                                                                                                                              |
| 管理エージェント           | ECMA1                               | ECMA1/XMA 拡張性フレームワークは、ECMA 2.0 によって置き換えられました。 ECMA2.0 コネクタを使用した既存の ECMA1 管理エージェントの更新が必要です。                                                                                                                                          |
| 管理エージェント           | コネクタのアウト プロセスの実行      | この機能は置き換えられません。 同期サービスでは常に同じプロセスのコネクタが呼び出されます。 他のプロセスの開始と管理は、コネクタの役割です。 |
| 管理エージェント           | パーティションの表示名の構成    | この機能は置き換えられません。 このオプションは、WMI インターフェイス内のパーティションに代替名を提供するためにのみ使用されました。                                                                                                                                                                       |
| 実行プロファイル                | 複合プロファイル                   | 複合プロファイルの差分インポート/同期、フル インポート/差分同期、およびフル インポート/同期は削除されます。 代わりに実行プロファイルを 2 つの手順で使用する必要があります。 

>[!NOTE]
複合の実行プロファイルは、既存のディスコネクタが大量にあることでパフォーマンスが影響を受ける環境でのみ保持してください。


| **[カテゴリ]**                | 
  **非推奨の機能**              | **置換およびコメント**           |
|--------|-------|---|    
| 属性の優先順位 | 複数の支配的/同等の優先順位                       | 同等の優先順位は削除されます。 この機能に代わるものはありません。 代わりに、手動で優先順位を構成する必要があります。 環境内に FIM サービス管理エージェントが展開されている場合は、この機能を引き続き使用することができます。 この管理エージェントでは、宣言型プロビジョニングで非優先のエクスポートを避けるための手動の優先順位が提供されません。 |
| 結合規則           | オブジェクトの種類 "Any" での結合                             | この機能は置き換えられません。 すべての結合規則で、結合しようとしているメタバース オブジェクトの種類が明示的に定義されます。       |
| 属性フロー      | エクスポートされた値の "allow nulls\(null を許可\)" の選択解除            | この機能は置き換えられません。 "allow nulls\(null を許可\)" が常に選択されます。 現在の環境で "allow nulls\(null を許可\)" が選択されていることを確認する必要があります。  |
| 属性フロー      | “属性を呼び出さない“                            | この機能は置き換えられません。 属性は常に呼び出されます。これがベスト プラクティスです。  |
| ルール エクステンション      | アウト プロセスのメタバースと MA のルール エクステンション実行 | この機能は置き換えられません。 メタバースと属性フローの規則は、同期エンジンと同じプロセスで実行されます。       |
| ルール エクステンション      | トランザクションのプロパティ                                | この機能は置き換えられません。 このユーティリティ クラスを使用して受信、プロビジョニング、送信の同期でデータを渡すことは避けてください。  |
| ルール エクステンション      | ExchangeUtils: Create55\* メソッド                     | Exchange 5.5 サーバーのオブジェクトを作成するメソッドは削除されます。        |
| インターフェイス            | Mms_Metaverse                                        | すべての ClmUtils クラス メンバーは、次のバージョンで削除されます。   |

## <a name="next-steps"></a>次の手順
詳細については、下記のリンクをクリックしてください。

- Microsoft Identity Manager は、その前身である Forefront Identity Manager に密接に関連しています。 現在も FIM を使用している場合や、追加のドキュメントを参照する必要がある場合は、[FIM 2010 R2 のドキュメントのロードマップ](https://technet.microsoft.com/library/jj133885.aspx)をご覧ください。
- [MIM 展開時のトポロジに関する考慮事項](topology-considerations.md) この記事では、実装を検討できる複数の展開トポロジについて説明します。
- [容量計画ガイド](capacity-planning-guide.md) このガイドとテスト環境を併せて活用し、展開に適した範囲を理解してください。
