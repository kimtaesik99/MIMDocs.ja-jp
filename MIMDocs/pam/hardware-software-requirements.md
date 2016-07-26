---
title: "PAM ソフトウェアの要件 | Microsoft Identity Manager"
description: "Privileged Access Management の展開を成功させるためのハードウェアとソフトウェアの要件を確認する"
keywords: 
author: kgremban
manager: femila
ms.date: 07/15/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 82a9085c-9667-4b3b-8079-657eab1d1e58
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: ae4c40c73dd9d5860f42e00765a7e34e8ca397a9
ms.openlocfilehash: 75a748f7035cfb10e833e4fdbfdc208b5245d3ea


---

# ハードウェアとソフトウェアの要件

Privileged Access Management では、基礎となるソフトウェア プラットフォームの要件以外にハードウェアの要件はありません。 十分なメモリまたはディスク領域と、ネットワーク接続が確保されていることを確認してください。

この記事では、基本的な展開の最小要件を提供します。 この要件は、パフォーマンス、スケーラビリティ、高可用性をデモンストレーションするためのものではありません。また、大規模なエンタープライズ環境や運用環境向けの推奨される展開トポロジを示すものでもありません。

## ソフトウェア パッケージからのインストール

次のソフトウェアは、TechNet Evaluation Center または MSDN からダウンロードできます。  
- Microsoft Identity Manager 2016
  - サービスとポータル: MIM サービスおよび MIM ポータル用のインストーラーと PAM シナリオ用のインストーラーが含まれています
  - アドインと拡張機能: 要求元 PowerShell コマンドレットのインストーラーが含まれています

次のソフトウェアは、GitHub からダウンロードできます。  
- PAMSamplePortal: REST API のサンプル Web アプリケーションが含まれています

## 必須ソフトウェア

- Windows Server 2012 R2  
- Windows 8.1 Enterprise または Windows 10 Enterprise  
- SQL Server 2012 Service Pack 1 または SQL Server 2014  

## 評価版ソフトウェア

Windows、SQL Server、Windows Server のライセンスを持っていない場合は、評価版をダウンロードできます。

### TechNet Evaluation Center

- [Windows Server 2012 R2](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2)  
- [Windows 8.1 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-8-1-enterprise)  
- [Windows 10 Enterprise](https://www.microsoft.com/evalcenter/evaluate-windows-10-enterprise)  

### Microsoft ダウンロード センター

- [SQL Server](https://www.microsoft.com/download/details.aspx?id=29066)  
- [SharePoint Foundation 2013 SP1 とその前提条件](https://www.microsoft.com/download/details.aspx?id=42039)

## ハードウェア要件

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



<!--HONumber=Jul16_HO3-->


