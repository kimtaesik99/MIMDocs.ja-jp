---
title: "要塞環境の計画 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 16ad83ab9a0fbe2b93428cf318b5ef138e2f3783
ms.sourcegitcommit: 2be26acadf35194293cef4310950e121653d2714
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/14/2017
---
# <a name="planning-a-bastion-environment"></a>要塞環境の計画

専用管理フォレストを備えた要塞環境を Active Directory に追加すると、既存の運用環境よりもセキュリティ制御が強化された環境で、組織は管理アカウント、ワークステーション、およびグループを簡単に管理できます。

このアーキテクチャでは、単一のフォレスト アーキテクチャでは不可能な、または簡単に構成できないさまざまな制御が可能になります。 たとえば、運用環境で高い権限が設定された管理フォレストで、特権のない標準ユーザーとしてアカウントをプロビジョニングできるため、より優れた技術で管理を実施できます。 また、このアーキテクチャでは、承認済みホストのみに対してログオン (と資格情報の公開) を制限するための手段として、認証を選択する機能を使用することもできます。 コストや完全な再構築の複雑性が発生することなく、高いレベルの保証が運用フォレストに必要とされる場合に、管理フォレストは運用環境の保証レベルを高める環境を提供できます。

専用管理フォレストだけでなく、その他の手法も使用できます。 これには、管理者の資格情報が公開される場所を制限したり、そのフォレストのユーザーのロールの特権を制限したり、(電子メールや Web 閲覧など) 標準ユーザー アクティビティに使用されるホストで管理タスクが実行されないようにしたりする手法が含まれます。

## <a name="best-practice-considerations"></a>ベスト プラクティスに関する考慮事項

専用管理フォレストとは、Active Directory の管理に利用される、標準のシングル ドメイン Active Directory フォレストのことです。 管理用のフォレストとドメインを使用する利点は、使用事例が限られているため、実稼働フォレストより多くのセキュリティ対策を実施できることです。 さらに、このフォレストは分離されており、組織の既存フォレストを信頼しないため、別のフォレストでセキュリティが侵害されても、その侵害はこの専用フォレストには及びません。

管理フォレスト設計には、次の考慮事項があります。

### <a name="limited-scope"></a>制限付きのスコープ

管理フォレストの価値は、高いレベルのセキュリティ保証があることと、攻撃される面が少ないことにあります。 このフォレストに管理機能やアプリケーションを追加することもできますが、それによってスコープが拡大するため、フォレストとそのリソースの攻撃される面が増えることになります。 目標は、フォレストの機能を制限することによって、攻撃される面を最小化することです。

管理特権をパーティション分割する[階層モデル](tier-model-for-partitioning-administrative-privileges.md)によると、専用管理フォレストのアカウントは単一層にする必要があり、多くの場合、階層 0 または階層 1 になります。 フォレストが階層 1 の場合は、特定のアプリケーション (財務アプリケーションなど) またはユーザー コミュニティ (外部委託 IT ベンダーなど) にスコープを限定することを検討してください。

### <a name="restricted-trust"></a>制限付きの信頼

実稼働 *CORP* フォレストは管理用 *PRIV* フォレストを信頼する必要がありますが、管理用フォレストが実稼働フォレストを信頼する必要はありません。 これにはドメイン信頼またはフォレスト信頼があります。 管理フォレストのドメインは管理対象のドメインとフォレストを信頼しなくても Active Directory を管理できます。ただし、アプリケーションを追加する場合、2 方向の信頼関係、セキュリティ検証、試験が必要になることがあります。

管理フォレストのアカウントが適切な運用ホストのみを使うように強制するには、認証の選択を使う必要があります。 ドメイン コントローラーを維持し、Active Directory の権限を委任する場合は通常、管理フォレストの指定の層 0 管理アカウントにドメイン コントローラーの「ログオン許可」権利を与える必要があります。 詳細については、「[Configuring Selective Authentication Settings (認証の選択設定の構成)](http://technet.microsoft.com/library/cc816580.aspx)」をご覧ください。

## <a name="maintain-logical-separation"></a>論理的な分離の維持

要塞環境が組織の Active Directory において既存または将来のセキュリティ問題の影響を受けないようにするため、要塞環境用のシステムを準備する場合は次のガイドラインに従う必要があります。

- Windows サーバーはドメインに参加したり、既存の環境からソフトウェアまたは設定の配布を利用したりしてはいけません。

- 要塞環境には、独自の Active Directory ドメイン サービスを含める必要があります。それにより、Kerberos、LDAP、DNS、時刻、タイム サービスを要塞環境に提供します。

- MIM では、既存の環境にある SQL データベースのファームを使用しないでください。 SQL Server は、要塞環境内の専用サーバーに展開する必要があります。

- 要塞環境には、Microsoft Identity Manager 2016 を展開する必要があります。その中でも特に、MIM Service コンポーネントと PAM コンポーネントを展開する必要があります。

- 要塞環境のバックアップ ソフトウェアとメディアは、既存フォレストの管理者が要塞環境のバックアップを壊すことがないように、既存フォレストのシステムと分離した状態を保持する必要があります。

- 要塞環境のサーバーを管理するユーザーは、要塞環境の資格情報が漏洩しないように、既存環境の管理者がアクセスできないワークステーションからログインする必要があります。

## <a name="ensure-availability-of-administration-services"></a>管理サービスの可用性の確保

アプリケーション管理を要塞環境に移行する際には、アプリケーションの要件を満たすために十分な可用性を実現する方法を考慮する必要があります。 以下のような技術が含まれます。

- 要塞環境の複数のコンピューターで Active Directory ドメイン サービスを展開します。 定期的な保守管理のために 1 台のサーバーを一時的に再起動する場合でも引き続き確実に認証できるように少なくとも 2 つのサーバーが必要になります。 負荷が高い場合、または複数の地域にまたがるリソースや管理者を管理する必要がある場合は、コンピューターを追加する必要があることがあります。

- 緊急の場合に備えて、既存フォレストと専用管理フォレストに非常事態用アカウント (Break Glass account) を用意します。

- 要塞環境の複数のコンピューターで SQL Server と MIM サービスを展開します。

- 専用管理フォレストでユーザーまたはロールの定義を変更するたびに、AD と SQL のバックアップ コピーを作成して維持します。

## <a name="configure-appropriate-active-directory-permissions"></a>適切な Active Directory アクセス許可の構成

管理フォレストは、Active Directory 管理の要件に基づき、最小の特権に制限されるように構成する必要があります。

- 運用環境を管理するための管理フォレストのアカウントには、管理フォレスト、その中のドメイン、その中のワークステーションに対する管理特権を与えてはいけません。

- 攻撃者や悪意のある内部関係者が監査ログを消去する機会が減るように、管理フォレスト自体の管理特権はオフライン プロセスで厳重に管理する必要があります。 その結果、運用管理者アカウントが与えられたユーザーは自分のアカウントの制約を緩和することができないため、組織に対するリスクが増えることはありません。

- 管理フォレストは、Microsoft Security Compliance Manager (SCM) のドメインに関する考慮事項に従う必要があります。これには、認証プロトコルの強力な構成が含まれます。

要塞環境を構築するとき、Microsoft Identity Manager をインストールする前に、その環境内で管理に使用するアカウントを特定し、作成します。 これには以下が含まれます。

- **非常事態用アカウント**。このアカウントは、要塞環境のドメイン コントローラーにログインするためだけのアカウントにします。

- **"レッド カード" 管理者**。その他のアカウントをプロビジョニングし、予定外のメンテナンスを行います。 このアカウントでは、要塞環境の外にある既存のフォレストやシステムにはアクセスできません。 スマートカードなど、資格情報を物理的に保護する必要があります。このアカウントの利用状況をログに記録する必要があります。

- **サービス アカウント**。Microsoft Identity Manager、SQL Server、その他のソフトウェアで必要になります。

## <a name="harden-the-hosts"></a>ホストのセキュリティ強化

管理フォレストに参加するドメイン コントローラー、サーバー、ワークステーションなどのすべてのホストには最新のオペレーティング システムとサービス パックをインストールし、常に最新の状態を維持しておく必要があります。

- 管理に必要なアプリケーションをワークステーションに事前にインストールしておきます。そうすることで、アプリケーションを利用するアカウントをローカルの管理者グループに入れなくてもアプリケーションをインストールできます。 ドメイン コントローラーの保守管理は、一般的に、RDP とリモート サーバー管理ツールで実行できます。

- 管理者フォレスト ホストはセキュリティ更新プログラムで自動更新する必要があります。 ドメイン コントローラーの保守管理作業が中断される可能性がありますが、修正プログラムの適用されていない部分の脆弱性についてセキュリティ リスクを大幅に軽減できます。

### <a name="identify-administrative-hosts"></a>管理用ホストの識別

システムまたはワークステーションのリスクは、その上で実行される最もリスクの高いアクティビティで計測する必要があります。たとえば、インターネットの閲覧、メールの送受信、他のアプリケーションで未知のコンテンツや信頼されていないコンテンツを利用することなどです。

管理ホストには次のコンピューターが含まれます。

- 管理者の資格情報が物理的に入力されるデスクトップ。

- 管理セッションと管理ツールが実行される管理用の「ジャンプ サーバー」。

- 管理アクションが実行されるすべてのホスト。RDP クライアントを実行し、離れた場所からサーバーおよびアプリケーションを管理する標準ユーザー デスクトップを使用するホストも含まれます。

- 管理の必要があり、RDP と制限付き管理者モードまたは Windows PowerShell リモート処理ではアクセスできないアプリケーションをホストするサーバー。

### <a name="deploy-dedicated-administrative-workstations"></a>専用管理ワークステーションの展開

不便ですが、高い権限を与えられた管理者の資格情報を持つユーザー専用に、セキュリティを強化した別個のワークステーションが必要になる場合があります。 資格情報に付与する特権レベル以上のセキュリティ レベルでホストを提供することが重要です。 付加的な保護のために次の手段を組み込むことを検討してください。

- **ビルドに含まれるすべてのメディアがクリーンであることを検証する**。これは、マスター イメージにインストールされたマルウェアや、ダウンロード中または保存中にインストール ファイルに挿入されたマルウェアを防ぐために役立ちます。

- **セキュリティ ベースライン** 。これを開始点としてセキュリティを構成します。 Microsoft Security Compliance Manager (SCM) を利用すると、ベースラインに合わせた管理ホストを構成できます。

- **セキュア ブート** 。署名のないコードをブート プロセスに読み込もうとする攻撃者またはマルウェアを防ぐために役立ちます。

- **ソフトウェアの制限** 。認定された管理者用ソフトウェアだけが管理ホストで実行されます。 お客様はこの作業に AppLocker と認定アプリケーションのホワイトリストを利用できます。悪意のあるソフトウェアやサポートされていないアプリケーションの実行を防止できます。

- **ボリューム全体の暗号化** 。リモートで使用している管理用ラップトップなどのコンピューターを物理的に紛失した場合のリスクを軽減します。

- **USB の制限**。物理的なウイルス感染を防ぎます。

- **ネットワークの分離** 。ネットワーク攻撃や不注意な管理行動によるリスクから守ります。 ホストのファイアウォールにより、明示的に必要な接続を除き、入ってくるすべての接続をブロックし、外への不必要なインターネット アクセスをブロックします。

- **マルウェア対策** 。既知の脅威やマルウェアを防ぎます。

- **悪用の軽減** 。Enhanced Mitigation Experience Toolkit (EMET) などを利用し、未知の驚異や悪用によるリスクを軽減します。

- **攻撃経路の分析** 。新しいソフトウェアをインストールするときに、新しい攻撃が Windows に侵入するのを防ぎます。 Attack Surface Analyzer (ASA) などのツールにより、ホストの構成設定を評価したり、ソフトウェアや構成の変更で侵入した攻撃ベクトルを特定したりできます。

- **管理者特権**。ローカル コンピューター上のユーザーに付与してはなりません。

- **RestrictedAdmin モード**。外向けの RDP セッションは、ロールで必要とされる場合を除き、RestrictedAdmin モードで行います。 詳細については、「[Windows Server のリモート デスクトップ サービスの新機能](https://technet.microsoft.com/library/dn283323.aspx)」をご覧ください。

以上の対策のいくつかは極端に見えるかもしれませんが、ここ数年、標的を脅かす敵対者の高い能力が一般に知られるようになりました。

## <a name="prepare-existing-domains-to-be-managed-by-the-bastion-environment"></a>既存のドメインを要塞環境で管理するように準備する

MIM では、PowerShell コマンドレットを使って、要塞環境における既存の AD ドメインと専用管理フォレストの間の信頼を確立します。 要塞環境を展開すると、ユーザーやグループが JIT に変換される前に、`New-PAMTrust` コマンドレットと `New-PAMDomainConfiguration` コマンドレットによりドメイン信頼関係が更新され、AD と MIM に必要な成果物が作成されます。

既存の Active Directory トポロジを変更する場合、 `Test-PAMTrust`、 `Test-PAMDomainConfiguration`、 `Remove-PAMTrust` 、および `Remove-PAMDomainConfiguration` コマンドレットを使用して、信頼関係を更新できます。

## <a name="establish-trust-for-each-forest"></a>各フォレストの信頼の確立

`New-PAMTrust` コマンドレットを既存フォレストごとに 1 回実行する必要があります。 これは管理ドメイン内の MIM サービスのコンピューターで呼び出されます。 このコマンドのパラメーターは、既存フォレストの最上位ドメインのドメイン名とそのドメインの管理者の資格情報です。

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

信頼を確立したら、次のセクションで説明するように、要塞環境から管理できるように各ドメインを構成します。

## <a name="enable-management-of-each-domain"></a>各ドメインの管理の有効化

既存ドメインの管理を有効にするには 7 つの要件があります。

### <a name="1-a-security-group-on-the-local-domain"></a>1.ローカル ドメインのセキュリティ グループ

既存ドメインにグループを用意する必要があります。その名前は NetBIOS ドメイン名の後ろにドル記号を 3 つ付けて作ります。たとえば、「*CONTOSO$$$*」のようになります。 グループの範囲は「*ドメイン ローカル*」に、グループの種類は「*セキュリティ*」にする必要があります。 この措置は、このドメインのグループと同じセキュリティ ID を持つグループを専用管理フォレストで作成するために必要です。 このグループを作成するには、既存ドメインの管理者が既存ドメインに参加しているワークステーションで次の PowerShell コマンドを実行します。

```PowerShell
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### <a name="2-success-and-failure-auditing"></a>2.成功と失敗の監査

ドメイン コントローラーに監査のためのグループ ポリシーを設定する場合、監査アカウント管理と監査ディレクトリ サービス アクセスで成功と失敗の両方を監査する必要があります。 これはグループ ポリシー管理コンソールで実行できます。このコンソールは、既存ドメインの管理者によって、既存ドメインに参加しているワークステーション上で実行されます。

3. **[スタート]**  >  **[管理ツール]**  >  **[グループ ポリシーの管理]** の順に移動します。

4. **[フォレスト: contoso.local]**  >  **[ドメイン]**  >  **[contoso.local]**  >  **[ドメイン コントローラー]**  >  **[既定のドメイン コントローラー ポリシー]** の順に移動します。 情報メッセージが表示されます。

    ![既定のドメイン コントローラー ポリシーのスクリーンショット](media/pam-group-policy-management.jpg)

5. **[既定のドメイン コントローラー ポリシー]** を右クリックし、**[編集]** を選びます。 新しいウィンドウが表示されます。

6. [グループ ポリシー管理エディター] ウィンドウの [既定のドメイン コントローラー ポリシー] ツリーで、**[コンピューターの構成]**  >  **[ポリシー]**  >  **[Windows の設定]**  >  **[セキュリティの設定]**  >  **[ローカル ポリシー]**  >  **[監査ポリシー]** の順に移動します。

    ![グループ ポリシー管理エディターのスクリーンショット](media/pam-group-policy-management-editor.jpg)

5. [詳細] ウィンドウで **[アカウント管理の監査]** を右クリックして、**[プロパティ]** を選びます。 **[これらのポリシーの設定を定義する]** をクリックし、**[成功]** と **[失敗]** のチェックボックスをオンにして、**[適用]**、**[OK]** の順にクリックします。

6. [詳細] ウィンドウで **[ディレクトリ サービスのアクセスの監査]** を右クリックして、**[プロパティ]** を選びます。 **[これらのポリシーの設定を定義する]** をクリックし、**[成功]** と **[失敗]** のチェックボックスをオンにして、**[適用]**、**[OK]** の順にクリックします。

    ![成功と失敗ポリシー設定のスクリーン ショット](media/pam-group-policy-management-editor2.jpg)

7. [グループ ポリシー管理エディター] ウィンドウと [グループ ポリシー管理] ウィンドウを閉じます。 PowerShell ウィンドウを起動し、次のように入力して、監査の設定を適用します。

    ```cmd
    gpupdate /force /target:computer
    ```

「コンピューター ポリシーの更新が正常に完了しました。」というメッセージが 数分後に表示されます。

### <a name="3-allow-connections-to-the-local-security-authority"></a>3.ローカル セキュリティ機関への接続を許可する

ドメイン コントローラーで、ローカル セキュリティ機関 (LSA) のために要塞環境からの TCP/IP 接続による RPC を許可する必要があります。 以前のバージョンの Windows Server の場合、LSA の TCP/IP のサポートをレジストリで有効にする必要があります。

```PowerShell
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### <a name="4-create-the-pam-domain-configuration"></a>4.PAM ドメイン構成の作成

`New-PAMDomainConfiguration` コマンドレットを管理ドメイン内の MIM サービスのコンピューターで実行する必要があります。 このコマンドのパラメーターは、既存ドメインのドメイン名とそのドメインの管理者の資格情報です。

```PowerShell
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### <a name="5-give-read-permissions-to-accounts"></a>5.アカウントへの読み取りアクセス許可の付与

ロール ( `New-PAMUser` 、および `New-PAMGroup` コマンドレットを使用する管理者) の確立に使用される要塞フォレストのアカウントと MIM モニター サービスで使用されるアカウントには、そのドメインの読み取りアクセス許可が必要です。

次の手順では、ユーザー *PRIV\administrator* に *CORPDC* ドメイン コントローラー内のドメイン *Contoso* に対する読み取りアクセス許可を有効にします。

1. Contoso ドメイン管理者 (Contoso\Administrator など) として CORPDC にログインします。

2. [Active Directory ユーザーとコンピューター] を起動します。

3. ドメイン **contoso.local** を右クリックし、**[制御の委任]**を選択します。

4. [選択されたユーザーとグループ] タブで、**[追加]** をクリックします。

5. [ユーザー、コンピューター、またはグループの選択] ポップアップの **[場所]** をクリックし、場所を *priv.contoso.local* に変更します オブジェクト名に「*Domain Admins*」と入力し、**[名前の確認]** をクリックします。 ポップアップが表示されたら、ユーザー名に「*priv\administrator*」と入力し、パスワードを入力します。

6. 「Domain Admins」に続けて「*; MIMMonitor*」と入力します。 Domain Admins と MIMMonitor の名前に下線が表示されたら、**[OK]** をクリックし、**[次へ]** をクリックします。

7. 一般的なタスクの一覧で、**[すべてのユーザー情報の読み取り]** を選択し、**[次へ]**、**[完了]** の順にクリックします。

18. [Active Directory ユーザーとコンピューター] を閉じます。

### <a name="6-a-break-glass-account"></a>6.非常事態用アカウント

特権アクセス管理プロジェクトの目的が、ドメイン管理者特権がドメインに永久的に割り当てられるアカウントの数を減らすことであれば、後で信頼関係に問題が発生した場合に備え、ドメインに*非常事態用*アカウントを用意する必要があります。 運用フォレストに緊急アクセスするためのアカウントをドメインごとに配置する必要があります。ログインできるのはドメイン コントローラーに限定します。 サイトを複数所有する組織の場合、冗長性のために追加アカウントが必要なことがあります。

### <a name="7-update-permissions-in-the-bastion-environment"></a>7.要塞環境でのアクセス許可の更新

最後に、そのドメインのシステム コンテナーで *AdminSDHolder* オブジェクトのアクセス許可を確認します。 *AdminSDHolder* オブジェクトにアクセス制御リスト (ACL) があります。これは組み込まれている特権 Active Directory グループのメンバーであるセキュリティ プリンシパルのアクセス許可を制御するために使用されます。 既定のアクセス許可を変更した結果、ドメインで管理特権を持つユーザーが影響を受けるかどうかに注意してください。アカウントが要塞環境にあるユーザーにはこのようなアクセス許可は適用されません。

## <a name="select-users-and-groups-for-inclusion"></a>追加するユーザーとグループの選択

次の手順で PAM ロールを定義し、ユーザーにアクセスが求められるグループとユーザーを関連付けます。 これは、一般的に、要塞環境で管理対象として特定される層のユーザーとグループのサブセットになります。 詳細は、「[Defining roles for Privileged Access Management (Privileged Access Management のロールを定義する)](defining-roles-for-pam.md)」にあります。
