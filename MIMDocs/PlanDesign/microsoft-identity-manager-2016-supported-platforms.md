---
title: "サポートされるソフトウェア プラットフォーム | Microsoft Docs"
description: "MIM 2016 の各コンポーネントと互換性のある製品およびバージョンを検索"
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 09/29/2016
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 4978f60d-044d-4e84-8d93-65801fce1144
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 1f545bfb2da0f65c335e37fb9de9c9522bf57f25
ms.openlocfilehash: 55b7dc3c76c5e95153b5839ce1eb6bf4a7997889


---

# <a name="supported-platforms-for-mim-2016"></a>MIM 2016 でサポートされるプラットフォーム

次の表は、Microsoft Identity Manager 2016 のサポートされるプラットフォームと各コンポーネントのバージョンについて説明します。 * が付いたバージョンは、MIM 2016 Service Pack 1 でのみサポートされます。


| **MIM コンポーネント** | **プラットフォーム** | **バージョン** |
|-------------------|--------------|-------------|
| **MIM 同期** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2<br/>Windows Server 2016 * |
|| | MIM 同期データベース | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | ユーザー プロビジョニング、PCNS、および GAL 同期用の Active Directory (省略可能)|Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | メールボックスのプロビジョニングと GAL 同期用の Exchange (省略可能)|Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | 開発環境 (省略可能) | Visual Studio 2012<br/>Visual Studio 2013 |
|| | その他の接続システム (省略可能) | Active Directory ドメイン サービス<br/>Active Directory<br/>ライトウェイト ディレクトリ サービス<br/>SQL Server 2000 以降<br/>SharePoint Server 2013<br/> SharePoint Server 2016 * <br/> 他のサード パーティ製品 |
| **MIM サービス** (PAM シナリオを除く) | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | MIM サービス データベース | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | MIM サービスの承認とグループ管理電子メール用の Exchange (省略可能) | Exchange Server 2007 SP3 (Exchange 管理コンソールがインストール済み)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
| **MIM サービスおよびポータル** (PAM シナリオのみ)| Windows Server | Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | 要塞環境の PAM フォレストの Active Directory | Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | 既存フォレストの Active Directory | Windows Server 2008 <br/> Windows Server 2008 R2 * <br/> Windows Server 2012 * <br/> Windows Server 2012 R2 * <br/> Windows Server 2016 * |
|| | MIM サービス データベース | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 <br/> SQL Server 2016 * |
|| | SharePoint | SharePoint Foundation 2010<br/>SharePoint Foundation 2013 SP1 <br/> SharePoint 2016 * |
|| | MIM サービスの承認とグループ管理電子メール用のメール サーバー (省略可能) | Exchange Server 2007 SP3 (Exchange 管理コンソールがインストール済み)<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 <br/> Exchange Server 2016 * <br/> Exchange Online * |
|| | ブラウザー | すべての主要なブラウザー |
| **MIM サービス レポート** | Windows Server | Windows Server 2012 <br/> Windows Server 2016 * |
|| | データ ウェアハウス | System Center 2012 Service Manager SP1 |
|| | データ ウェアハウス データベース | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 |
| **MIM パスワード リセットと登録ポータル** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 <br/> Windows Server 2016 * |
|| | Web ブラウザー | すべての主要なブラウザー |
| **MIM アドインと拡張機能** | Windows | Windows 7<br/>Windows 8<br/>Windows 8.1<br/>Windows 10 |
|| | Outlook の統合 (省略可能) | Outlook 2007 SP2<br/>Outlook 2010<br/>Outlook 2013 <br/> Outlook 2016 (Windows 10 上) * |
|| | PAM PowerShell Requestor コマンドレット (省略可能) | Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (サーバーと CA の統合) | Windows server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| | 証明機関 | Windows Server 2008 R2 SP1<br/>Windows Server 2012<br/>Windows Server 2012 R2 |
|| | MIM CM データベース | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2<br/>SQL Server 2014 SP1 |
| **MIM Certificate Management** (アプリケーション) | Windows | Windows 8<br/>Windows 8.1<br/>Windows 10 |
| **MIM Certificate Management** (クライアントと Bulk クライアント) | Windows | Windows 7 |
| **MIM BHOLD スイート** | Windows Server | Windows Server 2008 R2 SP1<br/>Windows Server 2012 R2 |
|| | BHOLD データベース | SQL Server 2008 R2 SP3<br/>SQL Server 2012 SP2 <br/> SQL Server 2014 * |
|| | メール サーバー (省略可能) | Exchange Server 2007 SP3<br/>Exchange Server 2010 SP3<br/>Exchange Server 2013 SP1 |
|| | Web ブラウザー | Internet Explorer 7、8、9、10、または 11 と Silverlight |



<!--HONumber=Nov16_HO2-->


