---
title: "PAM ソフトウェアの要件 | Microsoft Docs"
description: "Privileged Access Management の展開を成功させるためのハードウェアとソフトウェアの要件を確認する"
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/15/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: bfc73723bdd3a49529522f78ac056939bb8025a3
ms.openlocfilehash: 2985215821db843d2f90d8a34250a8ca6a84b592
ms.lasthandoff: 05/02/2017


---

# <a name="hardware-and-software-requirements"></a>ハードウェアとソフトウェアの要件

Privileged Access Management では、基礎となるソフトウェア プラットフォームの要件以外にハードウェアの要件はありません。 十分なメモリまたはディスク領域と、ネットワーク接続が確保されていることを確認してください。

この記事では、基本的な展開の最小要件を提供します。 この要件は、パフォーマンス、スケーラビリティ、高可用性をデモンストレーションするためのものではありません。また、大規模なエンタープライズ環境や運用環境向けの推奨される展開トポロジを示すものでもありません。

## <a name="installing-from-software-packages"></a>ソフトウェア パッケージからのインストール

次のソフトウェアは、TechNet Evaluation Center または MSDN からダウンロードできます。  
- Microsoft Identity Manager 2016
  - サービスとポータル: MIM サービスおよび MIM ポータル用のインストーラーと PAM シナリオ用のインストーラーが含まれています
  - アドインと拡張機能: 要求元 PowerShell コマンドレットのインストーラーが含まれています

次のソフトウェアは、GitHub からダウンロードできます。  
- PAMSamplePortal: REST API のサンプル Web アプリケーションが含まれています

## <a name="required-software"></a>必須ソフトウェア

- Windows Server 2012 R2  
- Windows 8.1 Enterprise または Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 または SQL Server 2014  

## <a name="evaluation-software"></a>評価版ソフトウェア

Windows、SQL Server、Windows Server のライセンスを持っていない場合は、評価版をダウンロードできます。

### <a name="technet-evaluation-center"></a>TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### <a name="microsoft-download-center"></a>Microsoft ダウンロード センター

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 とその前提条件](https://www.microsoft.com/download/details.aspx?id=42039)

## <a name="hardware-requirements"></a>ハードウェア要件

PAM の各コンポーネントについては、ソフトウェア製品のシステム要件を参照してください。

CORPDC の場合:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx) またはそれ以前のバージョン

CORPWKSTN の場合:  
- [Windows 8.1](http://windows.microsoft.com/windows-8/system-requirements)

PRIVDC の場合:  
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)

PAMSRV の場合:
- [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418.aspx)  
- [SQL Server 2012](https://msdn.microsoft.com/library/ms143506(sql.110).aspx) または [SQL Server 2014](https://msdn.microsoft.com/en-us/library/ms143506(v=sql.120).aspx)

