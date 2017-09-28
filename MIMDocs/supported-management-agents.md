---
title: "サポートされているコネクタ | Microsoft Docs"
description: "MIM とご利用の接続先データ ソース間のデータ転送を管理するには、コネクタを使用します。"
keywords: 
author: fimguy
ms.author: fimguy
manager: bhu
ms.date: 09/26/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 8bc2f6d2-9f53-4db6-aee6-a937ae468163
ms.reviewer: 
ms.suite: ems
ms.openlocfilehash: 99e98f3f9cb5e68fde0e3018856bf613c082325d
ms.sourcegitcommit: ba4cd133f7b49752c5470c9fc46e7e302cc99b49
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/28/2017
---
# <a name="connect-to-your-directories"></a>ディレクトリへの接続

コネクタは、特定の接続先データ ソースと Microsoft Identity Manager SP1 (MIM) との間を結びます。 コネクタは、接続先データ ソースから MIM へとデータを移動します。 MIM 内のデータが修正されると、コネクタはデータを接続先データ ソースへとエクスポートし、MIM との同期を確保することもできます。 一般的に、各接続先ディレクトリに対して、1 つ以上のコネクタが存在します。

Forefront Identity Manager では、コネクタは管理エージェントと呼ばれていました。 この用語は今でも記事や製品の一部で使用されていますが、コネクタも管理エージェントも同じ概念を示すことをご承知おきください。

この記事は MIM に含まれていて、サポートされているコネクタについて記載していますが、Extensible Connectivity 2.0 用コネクタでは、さらに他のデータ ソースへの接続も可能になります。 一部のパートナーは、この方法で独自のコネクタを作成しています。コネクタの全リストについては、wiki の「[FIM 2010: Management Agents from Partners](http://social.technet.microsoft.com/wiki/contents/articles/1589.fim-2010-management-agents-from-partners.aspx)」をご覧ください。

## <a name="supported-connectors-in-mim-2016-sp1"></a>MIM 2016 SP1 でサポートされているコネクタ

| 名前 | サポートされている接続先データ ソースのバージョン |
| ---- | ----------------------------------------------- |
| Active Directory ドメイン サービス | Active Directory 2012、2016 |
| Active Directory Lightweight Directory Services (ADLDS) | Active Directory Lightweight Directory Services (ADLDS) |
| Active Directory Global Address List (GAL) | Active Directory Global Address List (GAL) - Exchange 2013、2016 |
| Extensible Connectivity 2.0 | コール ベースまたはファイル ベースのデータソースすべて |
| FIM サービス | FIM サービス管理エージェント (同期サービス) は、インストールされている "Forefront Identity Manager サービス" と同じバージョンである必要がある |
| IBM DB2 Universal Database | IBM DB2 Version 9.5 または 9.7。IBM DB2 OLEDB v9.5 FP5 または v9.7 FP1 |
| IBM Directory Server | IBM Tivoli Directory Server 6.x |
| Novell eDirectory | Novell eDirectory version 8.7.3、8.8.5、8.8.6 |
| Oracle データベース | Oracle Database 10g または11g、64 ビット クライアント |
| Microsoft SQL Server | SQL Server 2012、2014、2016 |
| Oracle (以前の Sun と Netscape) Directory Servers | Sun Directory Server 6.x、7.x、Oracle 11 |
| [Windows PowerShell Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn640417.aspx) | Windows PowerShell 2.0 以上 |
| [Microsoft Azure Active Directory Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn511001.aspx) | Microsoft Azure Active Directory |
| [Generic LDAP Connector for FIM 2010 R2](https://msdn.microsoft.com/en-us/library/dn510997.aspx) | LDAP v3 server (RFC 4510 準拠) |
| [Connector for Lotus Domino](https://msdn.microsoft.com/en-us/library/hh859750.aspx) | Lotus Notes Release v8.5.x |
| [SharePoint Services Connector for FIM 2010 R2 Technical Reference](https://msdn.microsoft.com/en-us/library/dn511003.aspx) | User Profile Service アプリケーション (UPA) 付き SharePoint server 2013 または 2016 |
| [Connector for Web Services](https://www.microsoft.com/en-us/download/details.aspx?id=51495) | SAP ECC 5.0 または 6.0、Oracle PeopleSoft 9.1、Oracle eBusiness 12.1 |
| [Attribute-Value Pair Text File](https://technet.microsoft.com/en-us/library/cc708644(v=ws.10).aspx) | 属性値ペア テキスト ファイル |
| [Delimited Text File](https://technet.microsoft.com/en-us/library/cc720612(v=ws.10).aspx) | 区切りテキスト ファイル |
| [Directory Services Markup Language (DSML)](https://technet.microsoft.com/en-us/library/cc720660(v=ws.10).aspx) | ディレクトリ サービス マークアップ言語 (DSML) 2.0 |
| [Fixed-Width Text File](https://technet.microsoft.com/en-us/library/cc720633(v=ws.10).aspx) | 固定幅テキスト ファイル |
| [LDAP Data Interchange Format (LDIF)](https://technet.microsoft.com/en-us/library/cc708662(v=ws.10).aspx) | LDAP データ交換形式 (LDIF) |

## <a name="related-topics"></a>関連項目

[FIM 2010 R2 の管理エージェント](https://technet.microsoft.com/library/jj133885.aspx)
