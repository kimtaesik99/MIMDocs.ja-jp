---
title: "MIM サービスの動的なログ記録 | Microsoft Docs"
description: "管理サービスを再起動することなく MIM サービスの動的なログ記録を有効にする"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/18/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.openlocfilehash: 96dcd03616a0b63fc7cc9806446ebb36b3c81fa0
ms.sourcegitcommit: 8edd380f54c3e9e83cfabe8adfa31587612e5773
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/19/2017
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>MIM SP1 (4.4.1436.0)  サービスの動的なログ記録
4.4.1436.0 では、新しいログ機能を導入しました。 これにより、管理者およびサポート エンジニアは、管理サービスを再起動しなくてもログ記録を有効にすることができます。

このバージョンをインストールすると、Microsoft.ResourceManagement.Service.exe.config というファイルに次に示す新しい行が含められます。

*   第 6 行: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   第 8 行:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   第 266 行``</system.diagnostics> ``

![強調表示されたセクションは、新しい動的なログ エントリを示します。](media/mim-service-dynamic-logging/screen01.png)

動的なログ レベルは、[こちら](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3) で確認することができます。

- Critical = 既定レベルのサービスでは、重大なイベントのみを書き込む
- 更新された第 8 行 (dynamicLogging mode="true" loggingLevel="Critical") には優先されるログ値が指定されています。

第 266 行に指定された動的なログ記録に関する構成: Microsoft.ResourceManagement.Service.exe.config

![強調表示されたセクションは、さまざまなログ記録領域が指定された行を示します。](media/mim-service-dynamic-logging/screen02.png)

既定では、ログを記録する場所は、**C:\Program Files\Microsoft Forefront Identity Manager\2010\Service** となります。FIM サービス アカウントでは、動的なログを生成するために、この場所への書き込みアクセス許可を必要とします。

![ログの内容を格納するフォルダーの場所](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 予期しないエラーが発生した場合 (構成ファイル Microsoft.ResourceManagement.Service.exe.config 内の構文エラーまたはその他の間違い)、パス %TMP%、%TEMP%、または %USERPROFILE% (先頭にある) を持つファイル Microsoft.ResourceManagement.Service.exe_Emergency.log に、対応するエラー メッセージが書き込まれます。  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

トレースの内容を表示するには、[サービス トレース ビューアー ツール](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx) を使用します。

 ![サービス トレース ビューアーのスクリーンショット](media/mim-service-dynamic-logging/screen04.png)
