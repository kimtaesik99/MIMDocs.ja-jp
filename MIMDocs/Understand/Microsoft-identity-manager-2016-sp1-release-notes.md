---
title: Microsoft Identity Manager 2016 |Microsoft Identity Manager
description: "クラウドとオンプレミスでより安全でより便利な ID 管理エクスペリエンスを作成する MIM 2016 のしくみを理解します。"
keywords: 
author: barclayn
manager: mbaldwin
ms.date: 09/28/2016
ms.topic: article
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ccdd8a9f-02da-440a-81a8-354800dcd2a8
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 94813519554652a5554af914611d06b8a4d96ea4
ms.openlocfilehash: b791b18fa3775295e9c199086aa11a0d6c6a55e7


---
# Microsoft Identity Manager 2016 Service Pack 1 の新機能 #

Microsoft Identity Manager の通常の提供および更新サイクルの一環として、[Microsoft Identity Manager (MIM) 2016 Service Pack 1 (SP1)](https://msdn.microsoft.com/subscriptions/downloads/?fileid=70212#searchTerm=&Languages=en&PageSize=10&PageIndex=0&FileId=70212) を発表いたします。 このドキュメントでは、このリリースに含まれる更新プログラム、拡張機能、機能、および変更について概要を示します。

MIM SP1 の運用環境への展開中に問題が発生した場合は、Microsoft カスタマー サポートにお問い合わせください。

ご意見もお待ちしています。 製品チームに関してフィードバック、コメント、または懸案事項がありましたら、[mim2016@microsoft.com](mailto:mim2016@microsoft.com) まで電子メールでお知らせください。



## この Service Pack での更新内容 #

### MIM

- **MIM ポータルにおけるブラウザー間のエンド ユーザーのセルフ サービスの互換性:** この Service Pack より、ほとんどの主要なブラウザーをサポートします。 ユーザーは、Edge、Chrome、および Safari から、セルフ サービス グループとプロファイルを管理するために MIM ポータルにアクセスし、操作できるようになりました。

- **Exchange Online の MIM サービスのサポート:** MIM サービスは、長期にわたって承認および通知の電子メールの送受信をサポートしてきました。 SP1 MIM よりも前は、Exchange Server または SMTP のみをサポートしていました。 Service Pack 1 では、MIM サービスは Office 365 Exchange Online のアカウントを使用して依頼と電子メール通知を送受信できます。

- **アップロード時のイメージ ファイル形式の検証:** イメージをポータルにアップロードする際に、MIM によってイメージ ファイルの形式を検証できるようになりました。

### Privileged Access Management (PAM)

- **Windows Server 2016 の機能レベルの PAM "PRIV" (要塞) フォレストのサポート:** MIM PAM サービスは、Windows Server 2016 の Active Directory Domain Services フォレストの機能レベルで実行されているドメイン コント ローラーを使用した環境で構成できます。 構成すると、ユーザーの Kerberos チケットは、ロールのアクティブ化の残りの時間に期間が限定されます。

    >[!Note]
    CORP ドメイン内の Windows Server 2012 R2 のフォレストの機能レベルを維持することを選択した場合は、[KB 2919442](https://support.microsoft.com/en-us/kb/2919442) と [KB 2919355](https://support.microsoft.com/en-us/kb/2919355) を CORP ドメイン コントローラーにインストールすることをお勧めします。

- **"PRIV" (要塞) フォレスト専用のグループに、特権のあるアカウントを昇格:** 管理者は "PRIV" フォレスト専用のグループとユーザーを MIM サービスに通知できるようになりました。 このことを行うと、これらのグループおよびユーザーを PAM ロールに含めることができます。  これらをロールに対してアクティブ化したり、これらに "PRIV" フォレスト内のグループへのメンバーシップを割り当てたりすることができます。

- **PAM の展開スクリプト:** PAM 展開スクリプトを使用すると、管理者が PAM 環境のインストールを合理化できます。

- **認証ポリシー サイロ構成用の PAM コマンドレット:** Service Pack 1 では、要塞フォレストのセキュリティを強化する新しいコマンドレットが導入されています。 これらのコマンドレットによって認証ポリシー サイロが自動的に作成され、認証ポリシー テンプレートにバインドされます。

    >[!Note]
    これらのコマンドレットは、展開スクリプトの一部として自動的に実行されます。


## プラットフォームのサポート
更新されたプラットフォームのサポート情報は、ドキュメント「[MIM 2016 でサポートされるプラットフォーム](/microsoft-identity-manager/plan-design/microsoft-identity-manager-2016-supported-platforms)」に記載されています。  この Service Pack でサポートされている新しいプラットフォームには、SQL Server 2016 や SharePoint 2016 もあります。

## MIM 2016 の一般公開からこのリリースで修正された問題

### PAM
- New-PAMGroup では、PRIV フォレスト内のドメイン ローカル グループの MIM オブジェクトが作成されない
- New-PAMDomainConfiguration が "netdom" エラー メッセージで失敗する
- PAM 監視サービスでは、PRIV フォレスト内のグループに対する警告を記録する

## Service Pack 1 にアップグレードする方法

Microsoft Identity Manager 2016 Service Pack 1 にアップグレードするお客様は、以下で説明されている、展開に適用可能なすべてのサービスについてのガイダンスに従ってください。

>[!Note] Forefront Identity Manager 2010 R2 SP1 またはそれ以前のバージョンを実行しているお客様は、2015 年 8 月にリリースされた Microsoft Identity Manager 2016 に環境をアップグレードしてから、次の手順に従ってください。

始める前に

MIM サービスおよびポータルをアップグレードする前に、MIM 同期エンジンをアップグレードする必要があります。
MIMService と MIM 同期データベースをバックアップする必要があります。

  1. アップグレードする Microsoft Identity Manager コンポーネントをアンインストールします。
  2. アンインストールが完了したら、インストール メディアにあるスプラッシュ ページ "FIMSplash.htm" を開きます。
  3. アップグレードする MIM コンポーネントを選択します。
  4. 画面の指示に従ってインストールを続行します。
    * MIM サービスおよびポータルのインストール: Exchange Online を電子メール アカウントとして選択するときに、次の画面で、Exchange Online のアカウントの電子メール アドレスと資格情報を入力します。



<!--HONumber=Sep16_HO4-->

