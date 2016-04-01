---
タイトル: 要塞環境の計画
ms.custom: na
ms.prod: identity manager 2015
ms.reviewer: na
ms.service: active directory
ms.suite: na
ms.technology:
  - active directory ドメイン サービス
ms.tgt_pltfrm: na
ms.topic: 記事
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
作成者: Kgremban
---
# 要塞環境の計画

> [!メモ] {このコンテンツは、ホワイト ペーパーから派生した"Mitigating パス、-ハッシュとその他の資格情報の盗難、バージョン 2"、Microsoft Corporation の Trustworthy Computing グループにより開発された}。

## 概要
専用管理フォレストを備えた要塞環境を Active Directory に追加すると、既存の運用環境よりもセキュリティ制御が強化された環境で、組織は管理アカウント、ワークステーション、およびグループを簡単に管理できます。

このアーキテクチャでは、単一のフォレスト アーキテクチャでは不可能な、または簡単に構成できないさまざまな制御が可能になります。 この手法では、運用環境で高い権限が設定された管理フォレストで、特権のない標準ユーザーとしてアカウントをプロビジョニングできるため、より優れた技術で管理を実施できます。 また、このアーキテクチャでは、承認済みホストのみに対してログオン (と資格情報の公開) を制限するための手段として、認証を選択する機能を使用することもできます。 コストや完全な再構築の複雑性が発生することなく、高いレベルの保証が運用フォレストに必要とされる場合に、管理フォレストは運用環境の保証レベルを高める環境を提供できます。

専用管理フォレストだけでなく、その他の手法も使用できます。 これには、管理者の資格情報が公開される場所を制限したり、そのフォレストのユーザーのロールの特権を制限したり、(電子メールや Web 閲覧など) 標準ユーザー アクティビティに使用されるホストで管理タスクが実行されないようにしたりする手法が含まれます。

## 要塞環境フォレストに関する考慮事項

専用管理フォレストとは、Active Directory 管理機能専用の標準シングル ドメイン Active Directory フォレストのことです。 管理フォレストと管理ドメインは、使用事例が限定されるため、運用フォレストより厳重にセキュリティ強化されることがあります。 さらに、このフォレストは分離されており、組織の既存フォレストを信頼しないため、組織の既存フォレストが侵害されていることが後に判明しても、その侵害はこの専用フォレストには及びません。

管理フォレスト設計には、次の考慮事項があります。

**制限された範囲** – 管理フォレストの価値は、高いレベルのセキュリティ保証と、攻撃される面が少ないため、残存リスクが低いことにあります。 フォレストは追加の管理機能と管理アプリケーションの格納に使用できますが、それぞれの範囲が広がると、それだけフォレストとそのリソースに攻撃される面が増えます。 その目的は、フォレストとその中の管理ユーザーの機能を制限し、攻撃される面を最小に押さえることです。そのため、範囲を広げる場合、そのたびに慎重に考慮する必要があります。 専用管理フォレストのアカウントは単一層にあります (通常は層 0 または層 1)。層 1 の場合、特定の範囲のアプリケーション (財務アプリなど) またはユーザー コミュニティ (外注 IT ベンダーなど) に限定されることがあります。

**制限のある信頼** – 信頼は「管理対象のフォレストまたはドメインから管理フォレストへ」の方向で構成されます。

- 1 方向の信頼は、「運用環境から管理フォレストへ」の方向で必要となります。 これにはドメイン信頼またはフォレスト信頼があります。 管理フォレストのドメインは管理対象のドメインとフォレストを信頼しなくても Active Directory を管理できます。ただし、アプリケーションを追加する場合、2 方向の信頼関係、セキュリティ検証、試験が必要になることがあります。

- 管理フォレストのアカウントを適切な運用ホストへのログオンのみに制限するには、認証の選択を使用する必要があります。 ドメイン コントローラーを維持し、Active Directory の権限を委任する場合は通常、管理フォレストの指定の層 0 管理アカウントにドメイン コントローラーの「ログオン許可」権利を与える必要があります。 参照してください [選択的認証設定を構成する](http://technet.microsoft.com/library/cc755844.aspx) の詳細。

## フォレストの展開

コアの要塞環境には、認証および承認のために Active Directory ドメイン コントローラー サーバーのコレクション、特権アカウント ライフサイクル管理と要求ワークフロー処理のために Microsoft Identity Manager サーバーが含まれています。 また、管理者が認証するための特権管理者ワークステーションが含まれています。

### 論理分離の維持

既存の組織の Active Directory において既存または将来のセキュリティ問題の影響を受けないように、要塞環境のシステムを準備する場合には、次のガイドラインを使用する必要があります。

- Windows サーバーはドメインに参加したり、既存の環境からソフトウェアまたは設定の配布を利用したりしてはいけません。

- 要塞環境には、独自の Active Directory ドメイン サービスを含める必要があります。それにより、Kerberos、LDAP、DNS、時刻、タイム サービスを要塞環境に提供します。 Active Directory ドメイン サービスを提供するサーバーは Windows Server 2012 R2 以降、フォレストの機能レベルは Windows Server 2012 R2 のレベル以降である必要があります。

- Microsoft Identity Manager には、SQL Server 2012 Service Pack 1 または SQL Server 2014 が必要です。 これは要塞環境の専用サーバーに展開する必要があります。MIM は既存環境で SQL データベース ファームを使用してはいけません。

- 要塞環境では、Microsoft Identity Manager 2016 を PAM シナリオのために展開する必要があります。特に、MIM Service と PAM コンポーネントを展開する必要があります。

- 要塞環境のバックアップ ソフトウェアとメディアは、既存フォレストの管理者が要塞環境のバックアップを壊すことがないように、既存フォレストのシステムと分離した状態を保持する必要があります。

- 要塞環境のサーバーを管理するユーザーは、要塞環境の資格情報が漏洩しないように、既存環境の管理者がアクセスできないワークステーションからログインする必要があります。 以下に、専用管理ワークステーションの展開とセキュリティ強化の詳細を記載します。

### 要塞環境から管理サービスを利用できるようにする

既存フォレストのアプリケーション管理が要塞環境に移行するため、要塞環境の展開を計画するには、アプリケーションの要件を満たすのに十分な可用性を実現する方法を考慮する必要があります。 以下のような技術が含まれます。

- 要塞環境の複数のコンピューターで Active Directory ドメイン サービスを展開します。 定期的な保守管理のために 1 台のサーバーを一時的に再起動する場合でも引き続き確実に認証できるように少なくとも 2 つのサーバーが必要になります。 負荷が高い場合、または組織のリソースや管理者の基盤が複数の地域にまたがる場合、コンピューターを追加する必要があることがあります。

- 緊急の場合に備えて、既存フォレストと専用管理フォレストに非常事態用アカウント (Break Glass account) を用意します。

- 要塞環境の複数のコンピューターで SQL Server と MIM サービスを展開します。

- 専用管理フォレストでユーザーまたはロールの定義を変更するたびにバックアップ コピーを更新し、障害復旧のために AD と SQL のバックアップ コピーを作成します。

### 適切な Active Directory アクセス許可を構成する

管理フォレストは、Active Directory 管理の要件に基づき、最小の特権に制限されるように構成する必要があります。

- 運用環境を管理するための管理フォレストのアカウントには、管理フォレスト、その中のドメイン、その中のワークステーションに対する管理特権を与えてはいけません。

たとえば、アリスが要塞環境の運用を担当し、ボブが Exchange 管理ロールの所有者で、キャロルが Exchange 管理者であるとします。その場合、専用管理フォレストでは、ボブとキャロルを専用管理環境の Administrators グループに入れる必要はありません。

- 攻撃者や悪意のある内部関係者が監査ログを消去する機会が減るように、管理フォレスト自体の管理特権はオフライン プロセスで厳重に管理する必要があります。 その結果、運用管理者アカウントが与えられたユーザーは自分のアカウントの制約を緩和することができないため、組織に対するリスクが増えることはありません。

- 管理フォレストは、Microsoft Security Compliance Manager (SCM) のドメインに関する考慮事項に従う必要があります。これには、認証プロトコルの強力な構成が含まれます。

要塞環境を構築するとき、Microsoft Identity Manager をインストールする前に、その環境内で管理に使用するアカウントを特定し、作成します。 これには以下が含まれます。

- **非常事態用アカウント** このアカウントは、要塞環境のドメイン コントローラーにログインするためだけのアカウントにします。

- **「レッド カード」管理者** このユーザー アカウントは、他のアカウントをプロビジョニングし、予定外の保守管理を実行するためのものです。 このアカウントでは、要塞環境の外にある既存のフォレストやシステムにはアクセスできません。 スマートカードなど、資格情報を物理的に保護する必要があります。このアカウントの利用状況をログに記録する必要があります。

- **サービス アカウント** Microsoft Identity Manager、SQL Server、および要塞環境にインストールされているその他のソフトウェアで必要になります。

### SIEM および動作分析ツールと統合する

管理フォレストで異常が発生した場合、警報を出すように管理フォレストの検出コントロールを設計する必要があります。 認定シナリオまたは認定アクティビティの数を制限することで、運用環境より正確にコントロールを調整できます。 たとえば、Active Directory および Microsoft Identity Manager を SIEM システムにエクスポートして分析します。

### ホストのセキュリティ強化

管理フォレストに参加するドメイン コントローラー、サーバー、ワークステーションなどのすべてのホストには最新のオペレーティング システムとサービス パックをインストールし、常に最新の状態を維持しておく必要があります。 さらに、

- 管理に必要なアプリケーションをワークステーションに事前にインストールしておきます。そうすることで、アプリケーションを利用するアカウントをローカルの管理者グループに入れなくてもアプリケーションをインストールできます。 ドメイン コントローラーの保守管理は、一般的に、RDP とリモート サーバー管理ツールで実行できます。

- 管理者フォレスト ホストはセキュリティ更新プログラムで自動更新する必要があります。 ドメイン コントローラーの保守管理作業が中断される可能性がありますが、修正プログラムの適用されていない部分の脆弱性についてセキュリティ リスクを大幅に軽減できます。

更新プログラムが自動で承認されるように Windows Server Update Services を構成できます。 詳細については、更新プログラムの承認に関する「更新プログラムのインストールを自動的に承認する」セクションを参照してください。

#### ワークステーションのセキュリティ強化

管理者が資格情報を入力したり管理タスクを実行したりする任意のホストには、一時的であっても、使用されるアカウントに関連付けられている特権が委任されます。 パスワード、スマートカード PIN、その他の検証コードを物理的に入力したり、物理的な認証デバイスに接続したりすることは、そのコンピューターに資格情報のアクセス許可を与えることになります。 システムのリスクは、その上で実行される最もリスクの高いアクティビティで計測する必要があります。たとえば、インターネットの閲覧、メールの送受信、他のアプリケーションで未知のコンテンツや信頼されていないコンテンツを利用することなどです。

管理ホストには次のコンピューターが含まれます。

- 管理者の資格情報が物理的に入力されるデスクトップ。

- 管理セッションと管理ツールが実行される管理用の「ジャンプ サーバー」。

- 管理アクションが実行されるすべてのホスト。RDP クライアントを実行し、離れた場所からサーバーおよびアプリケーションを管理する標準ユーザー デスクトップを使用するホストも含まれます。

- 管理の必要があり、RDP と制限付き管理者モードまたは Windows PowerShell リモート処理ではアクセスできないアプリケーションをホストするサーバー。

#### 専用管理ワークステーションを展開する

資格情報に委ねられる特権のレベルと同じか、それより上のレベルのセキュリティをホストに与えるには、不便ですが、影響力の大きい管理資格情報を持つユーザー専用に、セキュリティが強化された別個のワークステーションが必要になります。 明確な意図と能力を持った攻撃者を防ぐためのセキュリティを維持するには、次のような対策を追加する必要があります。

- **ビルド内のすべてのメディアが「クリーン」であることを検証する** 。これは、マスター イメージにインストールされたか、ダウンロード中または保存中にインストール ファイルに挿入されたマルウェアを防ぐために役立ちます。

- **セキュリティ ベースライン** 。これを開始点としてセキュリティを構成します。

お客様は Microsoft Security Compliance Manager (SCM) を利用し、管理ホストにベースラインを構成できます。

- **セキュア ブート** 。署名のないコードをブート プロセスに読み込もうとする攻撃者またはマルウェアを防ぐために役立ちます。

この機能は、Unified Extensible Firmware Interface (UEFI) を活用するために Windows 8 で導入されました。

- **ソフトウェアの制限** 。認定された管理者用ソフトウェアだけが管理ホストで実行されます。

お客様はこの作業に AppLocker と認定アプリケーションのホワイトリストを利用できます。悪意のあるソフトウェアやサポートされていないアプリケーションの実行を防止できます。

- **ボリューム全体の暗号化** 。リモートで使用している管理用ラップトップなどのコンピューターを物理的に紛失した場合のリスクを軽減します。

- **USB の制限** 。物理的なウイルス感染を防ぎます。

- **ネットワークの分離** 。ネットワーク攻撃や不注意な管理行動によるリスクから守ります。 ホストのファイアウォールにより、明示的に必要な接続を除き、入ってくるすべての接続をブロックし、外への不必要なインターネット アクセスをブロックします。

- **マルウェア対策** 。既知の脅威やマルウェアを防ぎます。

- **悪用の軽減** 。Enhanced Mitigation Experience Toolkit (EMET) などを利用し、未知の驚異や悪用によるリスクを軽減します。

- **攻撃経路の分析** 。新しいソフトウェアをインストールするときに、新しい攻撃が Windows に侵入するのを防ぎます。

Attack Surface Analyzer (ASA) などのツールを利用すれば、ホストの構成設定を評価したり、ソフトウェアや構成変更で侵入した攻撃を特定したりできます。

- ユーザーにローカル コンピューターの管理者特権は必要ありません。

- 外向けの RDP セッションは、ロールで必要とされる場合を除き、RestrictedAdmin モードで行います。 詳細については、「 [Windows Server のリモート デスクトップ サービスの新機能](https://technet.microsoft.com/en-us/library/dn283323.aspx) 」を参照してください。

以上の対策のいくつかは極端に見えるかもしれませんが、ここ数年、標的を脅かす敵対者の高い能力が一般に知られるようになりました。

### 信頼を構築し、要塞環境から管理できるように既存ドメインを準備する

MIM には、要塞環境で既存の AD ドメインと専用管理フォレストの間の信頼を構築する際に役立つ PowerShell コマンドレットが含まれています。 要塞環境を展開すると、ユーザーやグループが JIT に変換される前に、New-PAMTrust コマンドレットと New-PAMDomainConfiguration コマンドレットによりドメイン信頼関係が更新され、AD と MIM に必要な成果物が作成されます。

既存の Active Directory トポロジを変更する場合、 `Test-PAMTrust`、 `Test-PAMDomainConfiguration`、 `Remove-PAMTrust` 、および `Remove-PAMDomainConfiguration` コマンドレットを使用して、信頼関係を更新できます。

#### 各フォレストの信頼を確立するための手順

New-PAMTrust を既存フォレストごとに 1 回実行する必要があります。 これは管理ドメイン内の MIM サービスのコンピューターで呼び出されます。 このコマンドのパラメーターは、既存フォレストの最上位ドメインのドメイン名とそのドメインの管理者の資格情報です。

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

信頼を確立したら、次のセクションで説明するように、要塞環境から管理できるように各ドメインを構成します。

#### 各ドメインの管理を有効にする手順

既存ドメインの管理を有効にするには 7 つの要件があります。

1. 既存ドメインにグループを用意する必要があります。その名前は NetBIOS ドメイン名の後ろにドル記号を 3 つ付けて作ります。たとえば、「CONTOSO$$$」のようになります。 グループの範囲は「ドメイン ローカル」に、グループの種類は「セキュリティ」にする必要があります。 この措置は、このドメインのグループと同じセキュリティ ID を持つ専用管理フォレストでグループを作成するために必要です。 これは次の PowerShell コマンドで実行できます。既存ドメインの管理者が既存ドメインに参加しているワークステーションで実行します。

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

2. ドメイン コントローラーに監査のためのグループ ポリシーを設定する場合、監査アカウント管理と監査ディレクトリ サービス アクセスで成功と失敗の両方を監査する必要があります。 これはグループ ポリシー管理コンソールで実行できます。このコンソールは、既存ドメインの管理者によって、既存ドメインに参加しているワークステーション上で実行されます。

3. **監査の構成** [スタート]、[管理ツール] の順に選択し、[グループ ポリシーの管理] を起動します。

4. [フォレスト: contoso.local]、[ドメイン]、[contoso.local]、[ドメイン コントローラー]、[既定のドメイン コントローラー ポリシー] の順に移動します。 情報メッセージが表示されます。

![pam-group-policy-management-editor](./media/pam-group-policy-management.jpg)

5. [既定のドメイン コントローラー ポリシー] を右クリックし、右クリック メニューで [編集...] を選択します。 新しいウィンドウが表示されます。

6. [グループ ポリシー管理エディター] ウィンドウの [既定のドメイン コントローラー ポリシー] ツリーで、[コンピューターの構成]、[ポリシー]、[Windows の設定]、[セキュリティの設定]、[ローカル ポリシー]、[監査ポリシー] の順に展開します。

![pam-group-policy-management-editor](./media/pam-group-policy-management-editor.jpg)

5. 詳細ウィンドウで [アカウント管理の監査] を右クリックして、右クリック メニューで [プロパティ] を選択します。 [これらのポリシーの設定を定義する] をクリックし、[成功] と [失敗] のチェックボックスをオンにして、[適用]、[OK] の順にクリックします。

6. 詳細ウィンドウで [ディレクトリ サービスのアクセスの監査] を右クリックして、右クリック メニューで [プロパティ] を選択します。 [これらのポリシーの設定を定義する] をクリックし、[成功] と [失敗] のチェックボックスをオンにして、[適用]、[OK] の順にクリックします。

![pam-group-policy-management-editor2](./media/pam-group-policy-management-editor2.jpg)


7. [グループ ポリシー管理エディター] ウィンドウと [グループ ポリシー管理] ウィンドウを閉じます。 PowerShell ウィンドウを起動し、次のように入力して、監査の設定を適用します。

```
gpupdate /force /target:computere
```

「コンピューター ポリシーの更新が正常に完了しました。」というメッセージが 数分後に表示されます。

8. ドメイン コントローラーで、LSA のために要塞環境から TCP/IP 接続による RPC を許可する必要があります。 以前のバージョンの Windows Server の場合、LSA の TCP/IP のサポートをレジストリで有効にする必要があります。

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

9. `New-PAMDomainConfiguration` コマンドレットを管理ドメイン内の MIM サービスのコンピューターで実行する必要があります。 このコマンドのパラメーターは、既存ドメインのドメイン名とそのドメインの管理者の資格情報です。

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

10. ロール ( `New-PAMUser` 、および `New-PAMGroup` コマンドレットを使用する管理者) の確立に使用される要塞フォレストのアカウントと MIM モニター サービスで使用されるアカウントには、そのドメインの読み取りアクセス許可が必要です。

たとえば、「CORPDC」が既存ドメイン「CONTOSO」のドメイン コントローラーで、「PRIV\\Administrator」が読み取りアクセス許可を必要とする要塞環境のユーザーの場合、次の手順により、管理者別または監視サービス別にドメインの読み取りアクセスが許可されます。

11. **CORPDC で、PRIV 管理者と監視サービスによる AD に対する読み取りアクセス権を有効にします。** Contoso ドメイン管理者 (Contoso\Administrator など) として CORPDC にログインします。

12. [Active Directory ユーザーとコンピューター] を起動します。

13. ドメイン contoso.local を右クリックし、[制御の委任] を選択します。

14. [選択されたユーザーとグループ] タブで、[追加] をクリックします。

15. [ユーザーの選択]、[コンピュータ]、または [グループ] ポップアップの [場所] をクリックし、場所を priv.contoso.local に変更します。 オブジェクト名に「Domain Admins」と入力し、[名前の確認] をクリックします。 ポップアップが表示されたら、ユーザー名に「priv\administrator」と入力し、パスワードを入力します。

16. 「Domain Admins」に続けて「; MIMMonitor」と入力します。 Domain Admins と MIMMonitor の名前に下線が表示されたら、[OK] をクリックし、[次へ] をクリックします。

17. 一般的なタスクの一覧で、[すべてのユーザー情報の読み取り] を選択し、[次へ] をクリックして [完了] をクリックします。

18. [Active Directory ユーザーとコンピューター] を閉じます。

> [!メモ] {特権アクセス管理プロジェクトの目標をドメインに割り当てられているドメインの管理者特権を持つアカウントの数を減らす場合は、必要があります、 *ガラスを中断* 以降の信頼関係で問題がある場合に、ドメイン内のアカウントです。 運用フォレストに緊急アクセスするためのアカウントをドメインごとに配置する必要があります。ログインできるのはドメイン コントローラーに限定します。 サイトを複数所有する組織の場合、冗長性のために追加アカウントを用意することがあります。}

19. 最後に、そのドメインのシステム コンテナーで *AdminSDHolder* オブジェクトのアクセス許可を確認します。  *AdminSDHolder* オブジェクトにアクセス制御リスト (ACL) があります。これは組み込まれている特権 Active Directory グループのメンバーであるセキュリティ プリンシパルのアクセス許可を制御するために使用されます。 既定のアクセス許可を変更した結果、ドメインで管理特権を持つユーザーが影響を受けるかどうかに注意してください。アカウントが要塞環境にあるユーザーにはこのようなアクセス許可は適用されません。

### 追加するユーザーとグループの選択

次の手順で PAM ロールを定義し、ユーザーにアクセスが求められるグループとユーザーを関連付けます。 これは、一般的に、要塞環境で管理対象として特定される層のユーザーとグループのサブセットになります。 詳細についてで [Privileged Access Management の役割を定義する](defining-roles-for-pam.md)です。


<!--HONumber=Mar16_HO1-->

