---
title: Microsoft Identity Manager 2016 |Microsoft Docs
description: "Microsoft Identity Manager 2016 を使用して AD DS でユーザーを作成するプロセスを調べる"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 60b28497f6abba14bd186cf2e2f2ce69b08693bc
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# AD DS にユーザーをプロビジョニングする方法
<a id="how-do-i-provision-users-to-ad-ds" class="xliff"></a>

適用対象: Microsoft Identity Manager 2016 SP1 (MIM)

ID 管理システムの基本要件の 1 つは、外部システムにリソースをプロビジョニングする機能です。

このガイドでは、Microsoft® Identity Manager (MIM) 2016 から Active Directory® ドメイン サービス (AD DS) へのユーザーのプロビジョニング処理に関係する主要な構成要素について説明します。また、シナリオが期待どおりに機能しているかの確認方法についての説明、MIM 2016 を使用して Active Directory ユーザーを管理するための推奨事項、追加情報のリストも提供します。

## 作業を開始する準備
<a id="before-you-begin" class="xliff"></a>


このセクションには、このドキュメントのスコープに関する情報も含まれています。 一般に、「～する方法」のガイドは、関連する『[ファースト ステップ ガイド](http://go.microsoft.com/FWLink/p/?LinkId=190486)』で説明されている、オブジェクトと MIM を同期するプロセスについて、基本的な経験がある読者を対象としています。

### 対象ユーザー
<a id="audience" class="xliff"></a>


このガイドは、既に MIM 同期プロセスのしくみについての基本的な知識があり、実地体験と、特定のシナリオに関するより詳細な概念情報を得ることに興味がある情報技術 (IT) プロフェッショナルを対象としています。

### 前提条件となる知識
<a id="prerequisite-knowledge" class="xliff"></a>


このドキュメントでは、MIM の実行中のインスタンスへのアクセス権があることと、次のドキュメントで説明されている単純な同期のシナリオを構成した経験があることを前提としています。

-   [受信同期の概要](http://go.microsoft.com/FWLink/p/?LinkId=189652)

-   [送信同期の概要](http://go.microsoft.com/FWLink/p/?LinkId=189653)

このドキュメントの内容は、これらの概要ドキュメントの延長としての機能に限定されます。

### スコープ
<a id="scope" class="xliff"></a>


このドキュメントで紹介するシナリオは、基本的なラボ環境の要件に対応するために簡略化されています。 主な目的は、説明されている概念とテクノロジを読者に理解してもらうことです。

このドキュメントは、MIM を使用した AD DS でのグループ管理を含むソリューションを開発するのに役立ちます。

### 時間の要件
<a id="time-requirements" class="xliff"></a>


このドキュメントの手順を完了するには、90 から 120 分必要です。

この予想時間は、テスト環境が既に構成されていていることを前提としており、テスト環境のセットアップに必要な時間は含まれていません。

### サポートの入手
<a id="getting-support" class="xliff"></a>


このドキュメントの内容について質問がある場合、または話し合いたい一般的なフィードバックがある場合は、[Forefront Identity Manager 2010 フォーラム](http://go.microsoft.com/FWLink/p/?LinkId=189654)にメッセージを投稿してください。

## シナリオの説明
<a id="scenario-description" class="xliff"></a>


Fabrikam という架空の企業は、MIM を使用して、会社の AD DS 内のユーザー アカウントを管理することを計画しています。 このプロセスの一環として、Fabrikam 社は AD DS にユーザーをプロビジョニングする必要があります。 初期テストを開始するため、Fabrikam 社は MIM と AD DS で構成される基本的なラボ環境をインストールしました。
このラボ環境で Fabrikam 社は MIM ポータルで手動で作成したユーザーで構成されるシナリオをテストします。 このシナリオの目的は、定義済みのパスワードを使用して有効化したユーザーとして AD DS にユーザーをプロビジョニングすることです。

## シナリオの設計
<a id="scenario-design" class="xliff"></a>


このガイドを使用するには、次の 3 つのアーキテクチャ コンポーネントが必要です。

-   Active Directory ドメイン コントローラー

-   FIM 同期サービスを実行しているコンピューター
-   FIM ポータルを実行しているコンピューター

次の図は、必要な環境を示しています。

![必要な環境](media/how-provision-users-adds/image001.png)


1 台のコンピューターですべてのコンポーネントを実行できます。

>[!NOTE]
MIM のセットアップの詳細については、[FIM のインストール ガイド](http://go.microsoft.com/FWLink/p/?LinkId=165845)を参照してください。

## シナリオのコンポーネントの一覧
<a id="scenario-components-list" class="xliff"></a>


次の表に、このガイドのシナリオの一部であるコンポーネントを一覧表示します。

| ![組織単位](media/how-provision-users-adds/image005.jpg)   | 組織単位                | MIM オブジェクト: プロビジョニングされたユーザーのターゲットとして使用される組織単位 (OU)。                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![ユーザー アカウント](media/how-provision-users-adds/image006.jpg)   | ユーザー アカウント                      | &#183; **ADMA**: AD DS に接続するための十分な権限を持つ Active Directory ユーザー アカウント。<br/> &#183; **FIMMA**: MIM に接続するための十分な権限を持つ Active Directory ユーザー アカウント。
                                                                 |
| ![管理エージェントと実行プロファイル](media/how-provision-users-adds/image007.jpg)  | 管理エージェントと実行プロファイル | &#183; **Fabrikam ADMA**: AD DS とデータを交換する管理エージェント。 <br/> &#183; Fabrikam FIMMA: MIM とデータを交換する管理エージェント。                                                                                 |
| ![同期規則](media/how-provision-users-adds/image008.jpg)  | 同期規則              | Fabrikam グループの送信同期規則: AD DS にユーザーをプロビジョニングする送信同期規則。                                     |
| ![設定](media/how-provision-users-adds/image009.jpg)   | 設定                               | すべての契約社員: 契約社員の EmployeeType 属性値を持つすべてのオブジェクトの動的メンバーシップを持つセット。                                |
| ![ワークフロー](media/how-provision-users-adds/image010.jpg)  | ワークフロー                          | AD プロビジョニング ワークフロー: MIM ユーザーを AD 送信同期規則のスコープに取り込むためのワークフロー。                                |
| ![管理ポリシー規則](media/how-provision-users-adds/image011.jpg)   | 管理ポリシー規則            | AD プロビジョニング管理ポリシー規則: リソースがすべての契約社員のセットのメンバーになったときにトリガーする管理ポリシー規則 (MPR)。 |
| ![MIM ユーザー](media/how-provision-users-adds/image012.jpg) | MIM ユーザー                          | Britta Simon: AD DS にプロビジョニングする MIM ユーザー。                                                                                             |



## シナリオの手順
<a id="scenario-steps" class="xliff"></a>


次の図は、このガイドで取り上げるシナリオの構成要素を示しています。

![シナリオの手順](media/how-provision-users-adds/image013.png)


## 外部システムの構成
<a id="configuring-the-external-systems" class="xliff"></a>


このセクションには、MIM 環境外で作成する必要があるリソースの作成方法も含まれています。

### 手順 1: OU を作成する
<a id="step-1-create-the-ou" class="xliff"></a>


プロビジョニングされているサンプル ユーザーのコンテナーとして OU が必要です。 OU の作成の詳細については、「[新しい組織単位 (OU) を作成する](http://go.microsoft.com/FWLink/p/?LinkId=189655)」を参照してください。

AD DS に MIMObjects という名前の OU を作成します。

### 手順 2: Active Directory ユーザー アカウントを作成する
<a id="step-2-create-the-active-directory-user-accounts" class="xliff"></a>

このガイドのシナリオでは、次の 2 つの Active Directory ユーザー アカウントが必要です。

- **ADMA**: Active Directory 管理エージェントで使用

- **FIMMA**: FIM サービス管理エージェントで使用

どちらの場合も、通常のユーザー アカウントを作成するには十分です。 両方のアカウントの特定の要件の詳細については、このドキュメントで後述します。 ユーザーの作成の詳細については、「[新しいユーザー アカウントを作成する](http://go.microsoft.com/FWLink/p/?LinkId=189656)」を参照してください。


## FIM 同期サービスの構成
<a id="configuring-the-fim-synchronization-service" class="xliff"></a>


このセクションの構成手順では、FIM 同期サービス マネージャーを起動する必要があります。

### 管理エージェントを作成する
<a id="creating-the-management-agents" class="xliff"></a>

このガイドのシナリオでは、次の 2 つの管理エージェントを作成する必要があります。

-   **Fabrikam ADMA**: AD DS 用の管理エージェント

-   **Fabrikam FIMMA**: FIM サービス管理エージェント用の管理エージェント

### 手順 3: Fabrikam ADMA 管理エージェントを作成する
<a id="step-3-create-the-fabrikam-adma-management-agent" class="xliff"></a>

AD DS 用の管理エージェントを構成するときに、AD DS とのデータ交換で管理エージェントによって使用されるアカウントを指定する必要があります。 通常のユーザー アカウントを使用する必要があります。 ただし、AD DS からデータをインポートするには、アカウントに DirSync コントロールからの変更をポーリングする権限が必要です。 管理エージェントで AD DS にデータをエクスポートする場合は、ターゲットの OU に対して十分な権限をアカウントに付与する必要があります。 このトピックの詳細については、「[Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657)」 (ADMA アカウントの構成) を参照してください。

AD DS でユーザーを作成するには、オブジェクトの DN をフローする必要があります。 この他に、姓、名、および表示名もフローして、オブジェクトが確実に検出されるようにすることをお勧めします。

AD DS では、ユーザーが sAMAccountName 属性を使用してディレクトリ サービスにログオンするのがまだ一般的です。 この属性の値を指定しない場合、ディレクトリ サービスによってランダムな値が生成されます。 しかし、これらのランダムな値はわかりにくいため、通常、この属性のわかりやすいバージョンが AD DS へのエクスポートの一部となっています。 ユーザーが AD DS にログオンできるようにするには、エクスポート ロジックに unicodePwd 属性を使用して作成したパスワードを含める必要があります。

>[!Note]                                
unicodePwd として指定した値が、ターゲット AD DS のパスワード ポリシーに準拠していることを確認します。

AD DS アカウントのパスワードを設定するときに、有効なアカウントとしてアカウントを作成する必要もあります。 これは、userAccountControl 属性を設定することで行います。 UserAccountControl 属性の詳細については、「[Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189658)」 (Active Directory で FIM を使用してアカウントを有効または無効にする) を参照してください。

次の表に、構成する必要がある最も重要なシナリオに固有の設定を一覧表示します。

| 管理エージェント デザイナー ページ                          | 構成                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| 管理エージェントを作成する                                 | 1.**管理エージェント:** AD DS  <br/> 2.**名前:** Fabrikam ADMA |
| Active Directory フォレストに接続する                      | 1.**ディレクトリ パーティションの選択:** “DC=Fabrikam,DC=com”   <br/>   2.**[コンテナー]** をクリックして **[Select Containers (コンテナーの選択)]** ダイアログ ボックスを開き、**[MIMObjects]** が選択されている唯一の OU であることを確認します。        |
| オブジェクトの種類を選択する                                     | 既に選択されているオブジェクトの種類に加え、**ユーザー**を選択します。 |
| 属性を選択する                                       | 1.**[すべて表示]** をクリックします。 <br/>   2.次の属性を選択します。 <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

詳細については、ヘルプで次のトピックを参照してください。
- 管理エージェントを作成する
- Active Directory フォレストに接続する
- Active Directory の管理エージェントを使用する
- ディレクトリ パーティションを構成する

>[!Note]
ExpectedRulesList 属性用に構成された属性フロー ルールのインポートがあることを確認します。

### 手順 4: Fabrikam FIMMA 管理エージェントを作成する
<a id="step-4-create-the-fabrikam-fimma-management-agent" class="xliff"></a>

FIM サービス管理エージェントを構成するときに、FIM サービスとのデータ交換で管理エージェントによって使用されるアカウントを指定する必要があります。

通常のユーザー アカウントを使用する必要があります。 このアカウントは、MIM のインストール時に指定したものと同じアカウントである必要があります。 セットアップ時に指定した FIMMA アカウントの名前を決定したり、このアカウントがまだ有効であるかどうかをテストするために使用できるスクリプトについては、「[Using Windows PowerShell to Do a FIM MA Account Configuration Quick Test](http://go.microsoft.com/FWLink/p/?LinkId=189659)」 (Windows PowerShell を使用して FIM MA アカウント構成の簡単なテストを行う) を参照してください、

次の表に、構成する必要がある最も重要なシナリオに固有の設定を一覧表示します。 次の表の情報に基づいて、管理エージェントを作成します。  

| 管理エージェント デザイナー ページ | 構成 |
|------------|------------------------------------|
| 管理エージェントを作成する | 1.**管理エージェント:** FIM サービス管理エージェント <br/> 2.**名前:** Fabrikam FIMMA |
| データベースに接続する     | 以下の設定を使用します。 <br/> &#183; **サーバー:** localhost <br/> &#183; **データベース:** FIMService <br/> &#183; **FIM サービスのベース アドレス:** http://localhost:5725 <br/> <br/> この管理エージェント用に作成したアカウントについての情報を提供します。 |
| オブジェクトの種類を選択する                                     | 既に選択されているオブジェクトの種類に加え、**[Person (ユーザー)]** を選択します。   |
| オブジェクトの種類のマッピングを構成する                          | 既存のオブジェクトの種類のマッピングに加え、**データ ソース オブジェクトの種類**のユーザーから**メタバース** オブジェクトの種類のユーザーへのマッピングを追加します。 |
| 属性フローを構成する                                | 既存の属性フローのマッピングに加え、次の属性フローのマッピングを追加します。 <br/><br/> ![属性フロー](media/how-provision-users-adds/image018.jpg) |




詳細については、ヘルプの次のトピックを参照してください。
-   管理エージェントを作成する

-   Active Directory データベースに接続する

-   Active Directory の管理エージェントを使用する

-   ディレクトリ パーティションを構成する

>[!NOTE]
 ExpectedRulesList 属性用に構成された属性フロー ルールのインポートがあることを確認します。

### 手順 5: 実行プロファイルを作成する
<a id="step-5-create-the-run-profiles" class="xliff"></a>

次の表に、このガイドのシナリオのために作成する必要がある実行プロファイルの一覧を示します。

| 管理エージェント  | 実行プロファイル     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1.フル インポート  <br/> 2.完全な同期 <br/> 3.差分インポート <br/> 4.差分同期 <br/> 5.エクスポート                                                                    |
| Fabrikam FIMMA   | 1.フル インポート <br> 2.完全な同期 <br/> 3.デルタ インポート <br/> 4.差分同期 <br/> 5.エクスポート|                                                                                                                                                                                   

前の表に従って各管理エージェントの実行プロファイルを作成します。


>[!Note]
詳細については、MIM ヘルプで「Create a Management Agent Run Profile」 (管理エージェントの実行プロファイルを作成する) を参照してください。                                                                                                                  


>[!Important]
 環境内でプロビジョニングが有効になっていることを確認します。 これは、「Using Windows PowerShell to Enable Provisioning」 (Windows PowerShell を使用してプロビジョンを有効にする) (http://go.microsoft.com/FWLink/p/?LinkId=189660) のスクリプトを実行して行うことができます。


## FIM サービスの構成
<a id="configuring-the-fim-service" class="xliff"></a>


このガイドのシナリオのため、次の図に示すように、プロビジョニング ポリシーを構成する必要があります。

![プロビジョニング ポリシー](media/how-provision-users-adds/image019.png)

このプロビジョニング ポリシーの目的は、グループを AD ユーザーの送信同期規則のスコープに入れることです。 リソースを同期規則のスコープに入れることで、同期エンジンで構成に従ってリソースを AD DS にプロビジョニングできるようになります。

FIM サービスを構成するには、Windows Internet Explorer® で http://localhost/identitymanagement にアクセスします。 MIM ポータル ページで、プロビジョニング ポリシーを作成するには、[管理] セクションから関連ページに移動します。 構成を確認するには、「[Using Windows PowerShell to document your provisioning policy configuration](http://go.microsoft.com/FWLink/p/?LinkId=189661)」 (Windows PowerShell を使用してプロビジョニング ポリシーの構成を文書化する) のスクリプトを実行する必要があります

### 手順 6: 同期規則を作成する
<a id="step-6-create-the-synchronization-rule" class="xliff"></a>

次の表に、必要な Fabrikam プロビジョニングの同期規則の構成を示します。 次の表のデータに従い、同期規則を作成します。

| 同期規則の構成                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| 名前                                                                                                       | Active Directory ユーザーの送信同期規則                         |                                                          
| 説明                                                                                               |                                                                             |                                                           
| 優先順位                                                                                                | 2                                                                           |                                                           
| データ フローの方向   | 送信             |       
| 依存関係       |         |                                         


| スコープ |                                                                             |                                                           
|--------|-------|
| メタバース リソースの種類 | 個人 |                                                         
| 外部システム                   |Fabrikam ADMA                                                               |                                                       
| 外部システム リソースの種類                                                                              | ユーザー      



| Relationship ||
|------------|---------|
| 外部システムでリソースを作成する                                                                         | True                                                                        |                                                           
| プロビジョニング解除を有効にする                                                                                      | False                                                                       |                                                           

| リレーションシップ条件                                                                                      | |
|------------|----------|
| ILM 属性     | データ ソース属性                                                       |
| データ ソース属性         | sAMAccountName    |

| 最初の送信属性フロー        | |                                                             |
|-------------------|---------------------- |---------------|
| Null の許可                 | Destination                                                                 | ソース                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | **定数:** 512                                         |
| false                                                                     | unicodePwd                    | 定数: P\@\$\$W0rd                                    |

| 永続的な送信属性フロー  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| Null の許可                                                                                                | Destination                                                                 | ソース                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



 >[!NOTE]
 重要 送信先としての DN を持つ属性フローに [初期フローのみ] が選択されていることを確認します。                                                                          

### 手順 7: ワークフローを作成する
<a id="step-7-create-the-workflow" class="xliff"></a>

AD のプロビジョニング ワークフローの目的は、Fabrikam のプロビジョニング同期規則をリソースに追加することです。 次の表に構成を示します。  次の表のデータに従って、ワークフローを作成します。

| ワークフローの構成               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| 名前                                 | Active Directory ユーザーのプロビジョニング ワークフロー                     |
| 説明                          |                                                                 |
| ワークフローの種類                        | 操作                                                          |
| ポリシー更新時に実行                 | False                                                           |

| 同期規則                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| 名前                                 | Active Directory ユーザーの送信同期規則             |
| 操作                               | 追加                                                             |




### 手順 8: MPR を作成する
<a id="step-8-create-the-mpr" class="xliff"></a>

必要な MPR はセット移行の種類で、リソースがすべての契約社員のセットのメンバーになったときにトリガーします。 次の表に構成を示します。  次の表のデータに従って、ワークフローを作成します。

| MPR の構成                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| 名前                                 | AD ユーザー プロビジョニングの管理ポリシー規則                 |
| 説明                          |                                                             |
| 型                                 | セット移行                                              |
| アクセス許可の付与                   | False                                                       |
| 無効                             | False                                                       |

| 移行の定義                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| 移行の種類                      | セットへの移行                                               |
| 移行セット                       | すべての契約社員                                             |

| ポリシー ワークフロー                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| 型                                 | 操作                                                      |
| 表示名                         | Active Directory ユーザーのプロビジョニング ワークフロー                 |




## 環境の初期化
<a id="initializing-your-environment" class="xliff"></a>


初期化フェーズの目的は次のとおりです。

-   同期規則をメタバースに取り込む。

-   Active Directory 構造を Active Directory コネクタ スペースに取り込む。

### 手順 9: 実行プロファイルを実行する
<a id="step-9-run-the-run-profiles" class="xliff"></a>

次の表に、初期化フェーズの一部である実行プロファイルを一覧表示します。  次の表に従って実行プロファイルを実行します。

| [実行]                                                                                                           | 管理エージェント                                      | 実行プロファイル          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | フル インポート          |
| 2                                                                                                             |                                                       | 完全な同期 |
| 3                                                                                                             |                                                       | エクスポート               |
| 4                                                                                                             |                                                       | 差分インポート         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | フル インポート          |
| 6                                                                                                             |                                                       | 完全な同期 |



>[!NOTE]
送信同期規則がメタバースに正常に投影されていることを確認する必要があります。

## 構成のテスト
<a id="testing-the-configuration" class="xliff"></a>


このセクションの目的は、実際の構成をテストすることです。 構成をテストするには、次の手順を実行します。

1.  FIM ポータルでサンプル ユーザーを作成します。

2.  サンプル ユーザーのプロビジョニングの前提条件を確認します。

3.  AD DS にサンプル ユーザーをプロビジョニングします。

4.  ユーザーが AD DS に存在することを確認します。

### 手順 10: MIM でサンプル ユーザーを作成する
<a id="step-10-create-a-sample-user-in-mim" class="xliff"></a>


次の表にサンプル ユーザーのプロパティの一覧を示します。 次の表のデータに基づいてサンプル ユーザーを作成します。

| 属性                              | 値                                                          |
|----------------------------------------|----------------------------------------------------------------|
| 名                             | Britta                                                         |
| 姓                              | Simon                                                          |
| 表示名                           | Britta Simon                                                   |
| アカウント名                           | BSimon                                                         |
| ドメイン                                 | Fabrikam                                                       |
| 従業員タイプ                          | 契約社員                                                     |



### サンプル ユーザーのプロビジョニングの前提条件を確認する
<a id="verify-the-provisioning-requisites-of-the-sample-user" class="xliff"></a>


サンプル ユーザーを AD DS にプロビジョニングするには、2 つの前提条件を満たす必要があります。

1.  ユーザーがすべての契約社員のセットのメンバーであること。

2.  ユーザーが送信同期規則のスコープ内にあるように設定していること。

### 手順 11: ユーザーがすべての契約社員のメンバーであることを確認する
<a id="step-11-verify-the-user-is-a-member-of-all-contractors" class="xliff"></a>

ユーザーがすべての契約社員のセットのメンバーであるかどうかを確認するには、セットを開き、[メンバーの表示] をクリックします。

![ユーザーがすべての契約社員のメンバーであることを確認する](media/how-provision-users-adds/image022.jpg)


### 手順 12: ユーザーが送信同期規則のスコープ内にあることを確認する
<a id="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule" class="xliff"></a>

ユーザーが同期規則のスコープ内にあるかどうかを確認するには、ユーザーのプロパティ ページを開き、[プロビジョニング] タブで [予期される規則一覧] 属性を確認します。 [予期される規則一覧] 属性で AD ユーザーの

送信同期規則が一覧表示されている必要があります。 次のスクリーン ショットは、[予期される規則一覧] 属性の例を示しています。

![同期規則の状態](media/how-provision-users-adds/image023.jpg)

処理のこの時点では、[予期される規則一覧] は [保留] になっています。 これは、同期規則がこのユーザーにまだ適用されていないことを意味します。



### 手順 13: サンプル グループを同期する
<a id="step-13-synchronize-the-sample-group" class="xliff"></a>


テスト オブジェクトに対して最初の同期サイクルを開始する前に、テスト計画で実行される各実行プロファイルの後に予想されるオブジェクトの状態を追跡する必要があります。 テスト計画には、オブジェクトの一般的な状態 (作成済み、更新済み、または削除済み) の横に、想定している属性値も含める必要があります。
テスト計画を使用して、テスト計画の想定を確認します。 手順が期待どおりの結果を返さない場合、想定した結果と実際の結果との不一致を解決するまで、次の手順に進まないでください。

自身の想定を確認するため、最初のインジケーターとして同期の統計情報を使用できます。 たとえば、コネクタ スペースにステージングする新しいオブジェクトを想定していたにも関わらず、インポートの統計で “Adds (追加)” がなしと返された場合、明らかに環境内の何かが想定どおりに動作していません。

![同期の統計情報](media/how-provision-users-adds/image024.jpg)

同期の統計情報から自分のシナリオが期待どおりに動作するかどうかの最初の兆候を得ることができますが、Synchronization Service Manager のコネクタ スペースの検索とメタバース検索機能を使用して、予想される属性値を検証する必要があります。

AD DS にユーザーを同期するには、次の手順を実行します。

1.  FIM MA コネクタ スペースにユーザーをインポートします。

2.  メタバースにユーザーを投影します。

3.  Active Directory コネクタ スペースにユーザーをプロビジョニングします。

4.  ステータス情報を FIM にエクスポートします。

5.  ユーザーを AD DS にエクスポートします。

6.  ユーザーの作成を確認します。

これらのタスクを実行するには、次の実行プロファイルを実行します。

| 管理エージェント | 実行プロファイル  |
|------------------|--------------|
| Fabrikam FIMMA   | 1.差分インポート <br/> 2.差分同期 <br/> 3.エクスポート <br/> 4.差分インポート |
| Fabrikam FIMMA   | 1.エクスポート <br/> 2.デルタ インポート       |


FIM サービス データベースからインポートすると、Britta Simon と、Britta を

AD ユーザーの送信同期規則にリンクする ExpectedRuleEntry オブジェクトが、Fabrikam FIMMA コネクタ スペースにステージングされます。 コネクタ スペースで Britta のプロパティを確認すると、

FIM ポータルで構成した属性値の横に、ExpectedRuleEntry オブジェクトへの有効な参照も表示されます。 その例を次のスクリーンショットに示します。

![コネクタ スペース オブジェクトのプロパティ](media/how-provision-users-adds/image025.jpg)

Fabrikam FIMMA 上で実行する差分同期の目的は、次の複数の操作を実行することです。

-   投影: 新しいユーザー オブジェクトと関連する ExpectedRuleEntry オブジェクトがメタバースに投影されます。

-   プロビジョニング: 新しく投影された Britta Simon オブジェクトが Fabrikam ADMA のコネクタ スペースにプロビジョニングされます。

-   エクスポート属性フロー: エクスポート属性フローは、両方の管理エージェントで発生します。 Fabrikam ADMA に、新しくプロビジョニングされた Britta Simon オブジェクトが新しい属性の値とともに入力されます。 Fabrikam FIMMA では、既存の Britta Simon オブジェクトと関連する ExpectedRuleEntry オブジェクトが投影の結果である属性値で更新されます。

![同期の統計情報](media/how-provision-users-adds/image026.jpg)

同期の統計情報で既に示したように、プロビジョニング アクティビティは、Fabrikam ADMA のコネクタ スペースで行われています。 Britta Simon のメタバース オブジェクトのプロパティを確認すると、このアクティビティが、有効な参照が入力された ExpectedRulesList 属性の結果であることがわかります。

![メタバース オブジェクトのプロパティ](media/how-provision-users-adds/image027.jpg)

Fabrikam FIMMA で次のエクスポート中に、Britta Simon の同期ルールのステータスが [保留] から [適用済み] に更新されます。これは、送信同期規則がメタバースのオブジェクトでアクティブになっていることを示します。

![適用済みの同期規則](media/how-provision-users-adds/image028.jpg)

新しいオブジェクトが ADMA コネクタ スペースにプロビジョニングされたため、この管理エージェントの保留中のエクスポートに、[Add] が 1 つあるはずです。 この目的のために作成されたスクリプトを使用すると、Fabrikam ADMA の保留中のエクスポートで [Add] が 1 つ報告されます。 このスクリプトを使用するには、「[Using Windows PowerShell to Display the Export State of a Management Agent](http://go.microsoft.com/FWLink/p/?LinkId=189664)」 (Windows PowerShell を使用して管理エージェントのエクスポートの状態を表示する) を参照してください。

![管理エージェントの保留中のエクスポート](media/how-provision-users-adds/image029.jpg)

FIM では、エクスポートを実行するたびに、次の差分インポートを行ってエクスポート操作を完了させる必要があります。 前回のエクスポートの実行後に実行する差分インポートは、確認のインポートと呼ばれています。 確認のインポートは、FIM 同期サービスが連続する同期の実行中に適切な更新要件を作成できるようにするために必要です。


このセクションの指示に従って、実行プロファイルを実行します。

>[!IMPORTANT]
各実行プロファイルは、エラーなく正常に実行される必要があります。

### 手順 14: AD DS でプロビジョニングされたユーザーを確認する
<a id="step-14-verify-the-provisioned-user-in-ad-ds" class="xliff"></a>

サンプル ユーザーが AD DS にプロビジョニングされていることを確認するには、FIMObjects OU を開きます。 Britta Simon が FIMObjects OU に配置されているはずです。

![ユーザーの確認は FIMObjects OU で行う](media/how-provision-users-adds/image033.jpg)

[概要]
<a id="summary" class="xliff"></a>
=======

このドキュメントの目的は、MIM のユーザーを AD DS と同期するための主な構成要素を紹介することです。 最初のテストでは、タスクを完了するために必要な最小限の属性を使用して開始し、一般的な手順が期待どおりに機能したら、シナリオにさらに属性を追加します。 複雑さを最小レベルに維持することで、トラブルシューティングのプロセスを簡略化します。

構成をテストするときに、テスト オブジェクトを削除して新しく再作成することがよくあります。 これを入力された 

ExpectedRulesList 属性を持つオブジェクトで行うと、孤立した ERE オブジェクトになる場合があります。
テスト環境からこれらのオブジェクトを削除する方法については、「[A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667)」 (環境から孤立した ExpectedRuleEntry オブジェクトを削除する方法) を参照してください。

同期ターゲットとして AD DS を含む一般的な同期のシナリオでは、MIM はオブジェクトのすべての属性にとって信頼できません。 たとえば、FIM を使用して AD DS 内のユーザー オブジェクトを管理する場合、少なくとも、ドメインと objectSID 属性は AD DS 管理エージェントによって提供される必要があります。
ユーザーが FIM ポータルにログオンできるようにする場合は、アカウント名、ドメイン、および objectSID 属性が必要です。 AD DS からこれらの属性を入力するには、AD DS のコネクタ スペースに追加の受信同期規則が必要です。 属性値の複数のソースを持つオブジェクトを管理する場合は、属性フローの優先順位を正しく構成する必要があります。 属性フローの優先順位が正しく構成されていない場合、同期エンジンによって属性値の入力がブロックされます。 属性フローの優先順位の詳細については、「[About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675)」 (属性フローの優先順位について) の記事を参照してください。

関連項目
<a id="see-also" class="xliff"></a>
=========

その他のリソース
<a id="other-resources" class="xliff"></a>
---------------

[Active Directory で FIM を使用してアカウントを有効または無効にする](http://go.microsoft.com/FWLink/p/?LinkId=189670)

[参照属性について](http://go.microsoft.com/FWLink/p/?LinkId=189671)

[FIM MA アカウントを管理する方法](http://go.microsoft.com/FWLink/p/?LinkId=189672)

[権限のないアカウントの検出 – 第 1 部: ビジョン化](http://go.microsoft.com/FWLink/p/?LinkId=189673)

[コネクタの検出メカニズムの貧者のためのバージョン](http://go.microsoft.com/FWLink/p/?LinkId=189674)

[ADMA アカウントの構成](http://go.microsoft.com/FWLink/p/?LinkId=189657)

[環境から孤立した ExpectedRuleEntry オブジェクトを削除する方法](http://go.microsoft.com/FWLink/p/?LinkId=189667)

[属性フローの優先順位について](http://go.microsoft.com/FWLink/p/?LinkId=189675)

[エクスポートについて](http://go.microsoft.com/FWLink/p/?LinkId=189676)
