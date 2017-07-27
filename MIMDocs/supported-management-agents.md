---
title: "サポートされているコネクタ | Microsoft Docs"
description: "MIM とご利用のディレクトリ間のデータ転送を管理するには、コネクタを使用します。"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b26fe7bc56ab8229054afb1409c3652e81464a3d
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="connect-to-your-directories"></a>ディレクトリへの接続

コネクタは、特定の接続先データ ソースと Microsoft Identity Manager (MIM) との間を結びます。 コネクタは、接続先データ ソースから MIM へとデータを移動します。 MIM 内のデータが修正されると、コネクタはデータを接続先データ ソースへとエクスポートし、MIM との同期を確保することもできます。 一般的に、各接続先ディレクトリに対して、1 つ以上のコネクタが存在します。

Forefront Identity Manager では、コネクタは管理エージェントと呼ばれていました。 この用語は今でも記事や製品の一部で使用されていますが、コネクタも管理エージェントも同じ概念を示すことをご承知おきください。

この記事は MIM に含まれているコネクタについて記載していますが、Extensible Connectivity 2.0 用コネクタでは、さらに他のデータ ソースへの接続も可能になります。 一部のパートナーは、この方法で独自のコネクタを作成しています。コネクタの全リストについては、wiki の「[FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx)」をご覧ください。

## <a name="supported-connectors-in-mim-2016"></a>MIM 2016 でサポートされているコネクタ

| 名前 | サポートされている接続先データ ソースのバージョン |
| ---- | ----------------------------------------------- |
| Active Directory ドメイン サービス | Active Directory 2000、2003、2003 R2、2008、2008 R2、2012 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory Global Address List (GAL) | Active Directory Global Address List (GAL) – Exchange 2000、2003、2007、 2010、2013 |
| Extensible Connectivity 2.0 | コール ベースまたはファイル ベースのデータソースすべて |
| MIM サービス | Microsoft Docs 2016 |
| IBM DB2 Universal Database | IBM DB2 version 9.1、9.5、または9.7。IBM DB2 OLEDB v9.5 FP5 または v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory version 8.7.3、8.8.5、8.8.6 |
| Oracle データベース | Oracle Database 10g または11g、64 ビット クライアント |
| Microsoft SQL Server | SQL Server 2000、2005、2008、2008 R2、2012 |
| Oracle (以前の Sun と Netscape) Directory Servers | Sun Directory Server 6.x、7.x、Oracle 11 |
| [Windows PowerShell Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 以上 |
| [Microsoft Azure Active Directory Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Generic LDAP Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3 server (RFC 4510 準拠) |
| [Connector for Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.0.x または v8.5.x |
| [SharePoint Services Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | User Profile Service アプリケーション (UPA) 付き SharePoint server 2013 または 2016 |
| [Connector for Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 または 6.0、Oracle PeopleSoft 9.1、Oracle eBusiness 12.1 |
| 属性値ペア テキスト ファイル | 属性値ペア テキスト ファイル |
| 区切りテキスト ファイル | 区切りテキスト ファイル |
| ディレクトリ サービス マークアップ言語 (DSML) | ディレクトリ サービス マークアップ言語 (DSML) 2.0 |
| 固定幅テキスト ファイル | 固定幅テキスト ファイル |
| LDAP データ交換形式 (LDIF) | LDAP データ交換形式 (LDIF) |

## <a name="related-topics"></a>関連項目

[FIM 2010 R2 の管理エージェント](https://technet.microsoft.com/library/jj133885.aspx)
