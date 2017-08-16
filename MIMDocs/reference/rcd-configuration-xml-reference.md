---
title: "リソース コントロールの表示構成 XML のリファレンス | Microsoft Docs"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>リソース コントロールの表示構成 XML のリファレンス

リソース コントロールの表示構成 (RCDC) リソースは、Microsoft Identity Manager 2016 SP1 (MIM) のデータ ストアに格納された他のリソースが、エンド ユーザーのユーザー インターフェイス (UI) にどのように表示されるかを制御するのに使用するユーザー定義のリソースです。 各 RCDC リソースには XML 構成ファイルが含まれており、このファイルを変更して UI テキストと UI コントロールを追加、変更、または削除できます。 MIM 2016 SP1 では既定の RCDC リソースがいくつか提供されていますが、カスタム リソース用にカスタムの RCDC リソースを作成することもできます。 FIM ポータルで RCDC の UI を使用する方法の詳細については、FIM ドキュメントの「[Introduction to Configuring and Customizing the FIM Portal (FIM ポータルの構成およびカスタマイズの概要)](http://go.microsoft.com/fwlink/?LinkID=165848)」をご覧ください。


## <a name="known-issues"></a>既知の問題

リソース コントロールの表示構成に含まれる多くのコントロールでは、既定値がサポートされていません。

今回のリリースでは、リソース コントロールに含まれるコントロールで既定値の設定はサポートされていません (オプション ボタン コントロールは除きます)。 ドロップダウン ボックスのこの問題を回避するには、どの値とも関連付けられていない既定値を指定して、ユーザーに強制的に選択を変更させます。 他のコントロールでこの問題を回避するには、承認ワークフローを使用して要求の送信時に既定値を指定する必要があります。

## <a name="basic-structure"></a>基本構造

RCDC リソースの XML データは、単一の XML 要素 **ObjectControlConfiguration** で構成されています。

>[!NOTE]
すべての XSD スキーマについては、このドキュメントで後述している「付録 A: 既定の XSD スキーマ」をご覧ください。

ObjectControlConfiguration 要素の XSD スキーマを次に示します。

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



**ObjectControlConfiguration** 要素には、次のものが含まれています。

1.  **ObjectDataSource**: この要素は、リソース コントロール (RC) で使用されるデータ ソース クラスの TypeName を指定します。 説明とスキーマの定義については、このドキュメントの次の「データ ソース」セクションをご覧ください。 **ObjectControlConfiguration** 要素には、**ObjectDataSource** 要素のノードを最大 32 個含めることができます。

2.  **XmlDataSource**: これは、概要ページのデザインを指定するのに最もよく使用される単純なデータ ソースです。 説明とスキーマの定義については、このドキュメントの次の「データ ソース」セクションをご覧ください。 **ObjectControlConfiguration** 要素には、**XmlDataSource** 要素のノードを最大 32 個含めることができます。

3.  **Panel**: 管理者は Panel 要素内の要素を変更して、RCDC ページのレイアウトをカスタマイズできます。 詳細については、このドキュメントで後述する「パネル」セクションをご覧ください。 **ObjectControlConfiguration** 要素には、Panel 要素は 1 つしか含めることができません。

4.  **Events**: 管理者は、カスタマイズされた分離コードを入力することはできません。この機能は制限されています。 これは、状態の変更に基づき、パネルやコントロールによって生成されるイベントです。 詳細については、このドキュメントで後述する「イベント」セクションをご覧ください。 **ObjectControlConfiguration** 要素には、必要に応じて 1 つの **Events** 要素を含めることができます。 通常、カスタム **イベント**の使用はサポートされていません (後の改良で特別に開発された場合は除きます)。

## <a name="data-sources"></a>データ ソース

Microsoft Identity Manager では、UI コンポーネントにデータをバインドする手段としてデータ ソースが使用されます。 これにより、プレゼンテーション層からデータを容易に分離できます。 RCDC リソース構成データには、**ObjectDataSource** と **XmlDataSource** の 2 種類のデータ ソースが含まれています。

-   **ObjectDataSources** は、RC にデータを提供する Microsoft .NET クラスを指定します。 使用可能な ObjectDataSources の種類には、決められたセットが用意されており、管理者は RCDC を作成するときにこのセットから選択して使用できます。

-   **XMLDataSources** は XML ベースのデータを簡単に作成する方法を提供します。管理者はこれを使用してカスタマイズしたデータを提供できます。 XML データは RCDC で直接指定する必要があります (定義済みの、組み込みの XML 構造を使用する場合を除きます)。 組み込みの XML 構造は、RC の概要ページの作成に使用されます。

RCDC では、これらのデータ ソースを RCDC で指定された UI コントロールの属性にバインドして、UIを作成できます。

### <a name="objectdatasource"></a>ObjectDataSource

Microsoft Identity Manager では、次の表に示すように、すべてのリソースの種類で使用可能な (注記されているものを除く)、一般的なデータ ソースの種類が提供されています。

| TypeName                        | 説明     | 両方向のバインドのサポート | サポートされるバインド構文        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | これは、作成、編集、または表示される FIM 2010 リソースを表します。 バインド文字列のパスは属性名です。 リソースの種類を、RCDC の ConfigurationData 属性ではなく、TargetObjectType 属性で指定する点に注意してください。 | ○                     | [AttributeName] その名前で指定されたオブジェクト属性の値。    |
| PrimaryResourceDeltaDataSource  | このデータ ソースは、FIM 2010 リソースの元の状態と現在の状態を比較する差分 XML を作成します。 生成された差分 XML は、RC の概要コントロールによって、ユーザーが送信する要求に対して UI を表示するのに使用されます。                                    | いいえ                      | DeltaXml: </br> これは、概要コントロールとともに、差分の表示に使用されます。                                                 |
| PrimaryResourceRightsDataSource | このデータ ソースは、FIM 2010 リソースの各属性にインライン権限を提供します。 これにより RC は、送信前に該当する属性でユーザーが持つ権限を特定してから、その属性の UI を適切に表示できます。                     | いいえ                      | [AttributeName]                                                                                         |
| SchemaDataSource                | このデータ ソースは、リソースの種類の情報のほか、スキーマ関連の情報 (表示名、説明、属性が必須かどうかなど) にアクセスするために使用されます。                                                                                             | いいえ                      | [AttributeName].**Required** ブール値 (属性に有効な値が含まれる必要があるかを示す)。 <br/> [AttributeName].**DisplayNameString** 値 (バインドの表示名を示します)。 <br/>[AttributeName].**DescriptionString** 値 (バインドの説明を示します)。 <br/>[AttributeName].StringRegexString 値 (バインドの文字列の正規表現を示します)。 <br/>[AttributeName].**DisplayName** <br/> [AttributeName].**Description** <br/> [AttributeName]. [AttributeName].**IntergerValueMinimum** <br/>[AttributeName].**IntergerValueMaximum** <br/>[AttributeName].**LocalizedAllowedValues**|
| DomainDataSource                | このデータ ソースは、ドメイン構成リソースに基づき、ドメインの列挙型を提供します。 このデータ ソースは、グループ リソースおよびユーザー リソースの RCDC でのみ使用できることに注意してください。                                                                           | ○                     | ドメイン           |

3 つのデータ ソースを UocTextBox コントロールにバインドし、グループの Description 属性を編集する RCDC スニペットの例を次に示します。

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

**XMLDataSource** を使用すると、RCDC が特定のリソースで使用するカスタム データを指定できます。 この場合、XML データを RCDC で指定する必要があります。 別の方法として、このデータ ソースを使用して組み込みの XML データ構造を参照し、概要ページの UI を表示できます。 RCDC で定義するときに使用する **XMLDataSource** の種類を制御します。


| TypeName                 | 説明   | | |
|--------------------------|------------|
| **XMLDataSource**            | このデータ ソースは、XML データを表します。 XSL 形式または埋め込みの XSL 形式のいずれかを指定できます。 <br/>**XSL 形式:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll<my:XmlDataSource my:Name=" <br/>summaryTransformXsl"my:Parameters=”Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl”> </my:XmlDataSource><br/> **埋め込みの XSL 形式:** <br/> <my:XmlDataSource my:Name="RequestStatusTransformXsl"> <br/> <xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl="urn:schemas-microsoft-com:xslt"><br/></xsl:stylesheet></my:XmlDataSource>                       |いいえ | ```Xpath[;namespaces]``` <br/> Where:Xpath は、必要なノード (通常は "/" (ルート)) を選択するための、XML の有効な XPath です。 <br/>名前空間はプレフィックス=URI 文字列の省略可能なリストです。名前空間内の XML に対して XPath が機能する必要がある場合は、セミコロンで区切ります。 |
| **ReferenceDeltaDataSource** | このデータ ソースは、複数値を持つ参照属性の差分を表します。 グループとセットの RCDC でのみ使用されます。 <br/> このデータ ソースはグループまたはセットに限定されませんが、このような差分を送信するには RCDC ホストでコードを変更する必要があります。 現時点でこのデータ ソースが認識されるのは、グループとセットのホストのみです。  | Yes                      | [AttributeName].Add - [AttributeName] は参照属性を表し、返されるデータは差分を追加したものになります。 <br/> 例: [ReferenceAttribute].Add <br/>例: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName].Remove - [AttributeName] は参照属性を表し、返されるデータは差分を削除したものになります。 <br/> DeltaXml |
|**RequestDetailsDataSource**| このデータ ソースは、要求オブジェクトの RequestParameter 属性を表します。 このパラメーターは、複数値を持つ属性ごとに表示される属性値の最大数を設定します。要求の RCDC でのみ使用されます。 ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| いいえ | DeltaXml |
|**RequestStatusDataSource**| このデータ ソースは、要求オブジェクトの **RequestStatusDetails** 属性を表します。 <br/>要求の RCDC でのみ使用されます。  | いいえ | DeltaXml |

-   カスタムの XML データ ソースを定義します。

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   概要コントロールの組み込みの XSL を使用するには、データ ソースを次のように定義します。

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 カスタム リソースの種類用に RCDC を作成する場合は、このメソッドを使用してカスタム リソースの概要ページを自動的に表示できます。

組み込みの XSL を使用し、PrimaryResourceDeltaDataSource と XMLDataSource を使って RCDC の概要タブを作成する方法の例を次に示します。

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

別の方法として、ユーザーは前に指定した XmlDataSource 要素を次の形式に置き換えて、概要ページのカスタマイズされたレイアウトを定義できます。 参考として、このドキュメントで後述する「付録 B: 既定の概要 XSL」に、FIM 2010 の既定の概要 XSL を掲載していますのでご覧ください。

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>データ ソースのスキーマ
2 種類のデータ ソースの XSD スキーマを次に示します。

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>イベント
イベントは、コントロールの状態の変化を定義します。 イベントがトリガーされた後の動作を定義するためにカスタマイズされた関数 (ハンドラー) を作成することはできないため、この機能の拡張性は限定的です。 同じ Events 要素を、Panel 要素でも使用できます。 詳細については、このドキュメントで後述する「パネル」セクションをご覧ください。 Events 要素の XSD スキーマを次に示します。

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


イベントは空の要素であり、次の 2 つの属性があります。

**属性:**

1.  **Name**: これは、イベントの一意の名前です。 **ObjectControlConfiguration** でサポートされるイベントは、読み込みイベントのみです。 このイベントは、ページが最初に読み込まれるときにトリガーされます。

2.  **Handler**: これは、ハンドラーの一意の名前です。 イベントがトリガーされると、通常はコントロールの状態の変化に対応するためにプログラムのメソッドが呼び出されます。 既存のコントロールから既存のハンドラーを削除したり、新しいハンドラーを作成したり、既存または新しいコントロールにアタッチしたりすることはサポートされていません。

サンプル:

Events 要素の例を次に示します。
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**パネル**


Panel 要素は、RCDC レイアウトの中核をなす要素です。 Panel 要素の XSD スキーマを次に示します。

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

この要素には、繰り返し現れる要素であるグループが含まれます。 詳細については、このドキュメントの「グループ化」セクションをご覧ください。

Panel 要素には、次の 4 つの属性が含まれます。

1.  **Name**: パネルの名前です。 これは必須の、文字列型の属性です。

2.  **DisplayAsWizard**: この属性は、現在使用されていません。 リソース レイアウトがウィザード モードまたはタブ モードの場合は、RCDC で対応する VerbContext 属性によって制御されます。 0 (作成モード) に設定されている場合も、ウィザード モードです。 それ以外の場合は、タブ モードです。 詳細については、このドキュメントの「Introduction to Configuring and Customizing the FIM Portal (FIM ポータルの構成およびカスタマイズの概要)」をご覧ください。

3.  **Caption**: この属性は、現在使用されていません。 ユーザーは、ヘッダー情報のみを含むグループを含めることで、ページのキャプションを指定できます。 詳細については、このドキュメントの「グループ化」セクションをご覧ください。

4.  **AutoValidate**: これは省略可能なブール値の属性です。 true に設定されている場合、現在のタブの各コントロールに対して検証がトリガーされます。 既定では、この属性が指定されない場合、true に設定されます。 これは RegularExpression プロパティと組み合わせて使用できます。 詳細については、このドキュメントの別のセクションで後述する “RegularExpression“ をご覧ください。

## <a name="grouping"></a>グループ化

Grouping 要素は、パネル全体のレイアウトを定義します。 これは、個々のコントロールを異なるセクションとタブにグループ化するコンテナーとして機能します。 Grouping 要素の XSD スキーマを次に示します。

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


**グループ**には、次の 3 つの種類があります。

1.  **ヘッダー グループ**: ヘッダー グループは省略可能です。 **パネル**には、ヘッダー グループを 1 つだけ含めることができます。 ヘッダー グループは、パネル上部にキャプションとして表示されます。
    このグループでは、UocCaptionControl を 1 つだけ使用できます。 ヘッダー グループの例については、「サンプル」セクションをご覧ください。

2.  **コンテンツ グループ**: 少なくとも 1 つのコンテンツ グループが必要です。 パネルには複数のコンテンツ グループを含めることができます。 コンテンツ グループは、RCDC ページの主要なコンテンツとして表示されます。 各コンテンツ グループは同じパネルの 1 つのタブとして表示され、1 から 256 個のコントロールを保持できます。 **コンテンツ グループ**の例については、下記の「サンプル」セクションをご覧ください。

3.  **概要グループ**: 概要グループは省略可能です。 パネルには、概要グループを 1 つだけ含めることができます。 概要グループは、パネルの最後のタブとして表示されます。 概要グループでは **UocHtmlSummary** コントロールを 1 つだけ使用して、ユーザーが要求を送信する前に行った変更を表示できます。 概要グループの例については、下記の「サンプル」セクションをご覧ください。

各種類のグループには、次の要素が含まれます。

1.  **Help**: この要素は、タブにヘルプ テキストを提供します。 これを使用して、タブにヘルプ ファイルへのリンクを追加することもできます。

2.  **Controls**: この要素の詳細については、このドキュメントの「Control」セクションをご覧ください。 グループの種類に応じて、各グループに 1 から 256 個のコントロールを含める必要があります。

3.  **Events**: この要素の詳細については、このドキュメントの「イベント」セクションをご覧ください。 各グループは、オプションとして 1 つのイベントを持つことができます。 Grouping 要素でサポートされているイベントは次のとおりです。

    - **BeforeLeave**: このイベントは、ユーザーがコンテンツ グループのタブから離れる準備ができたときにトリガーされます。
    - **AfterEnter**: このイベントは、ユーザーがコンテンツ グループのタブに入る準備ができたときにトリガーされます。

属性:

1.  **Name**: これは、グループの必須の名前です。 この**名前**は、**パネル**内で一意である必要があります。

2.  **Caption**: **キャプション**は、ヘッダー グループのヘッダーのキャプションとして表示されます。 コンテンツ グループまたは概要グループのタブのキャプションとして表示されます。

3.  **Description**: 省略可能な文字列属性である **Description** は、コンテンツ グループで使用される場合にのみ機能します。 この要素を使用すると、同じタブ内の情報について、エンド ユーザーに詳細情報を提供できます。

  >[!NOTE]
  この属性を概要グループで使用すると、XML は無効とみなされます。 この属性をヘッダー グループで使用すると、XML は有効とみなされますが、無視されます。

4.  **Enabled**: 省略可能なブール型の属性である Enabled は、指定されない場合 true に設定されます。 Enabled を false に設定すると、エンド ユーザーに無効なタブが表示されます。 この属性は、コンテンツ グループでのみ機能します。

  >[!NOTE]
  この属性を概要グループで使用すると、XML は無効とみなされます。 この属性をヘッダー グループで使用すると、XML は有効とみなされますが、無視されます。

5.  **Visible**: この属性を false に設定すると、RCDC ページのタブまたは見出しを非表示にできます。 既定では、この省略可能なブール型の属性は true に設定されています。 この属性は、コンテンツ グループでのみ機能します。

  >[!NOTE]
  パネルにコンテンツ グループが 1 つしか含まれていない場合、この機能は動作しません。 パネルに複数のコンテンツ グループが含まれている場合は、上記のように動作します。

6.  **IsHeader**: これは省略可能なブール型の属性で、グループがヘッダー グループかどうかを定義します。 この属性が指定されない場合は、false に設定されます。

7.  **IsSummary**: これは省略可能なブール値の属性で、グループが概要グループかどうかを定義します。 この属性が指定されない場合は、false に設定されます。

![RCD 構成 XML](media/rcd-configuration-xml-reference/image005.jpg)

次の XML サンプル コードでは、前述したヘッダー グループが作成されます。 ヘッダー グループは、"Sample Header Grouping (サンプル ヘッダー グループ)" と表示されている領域です。

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![RCD 構成 XML](media\rcd-configuration-xml-reference/image007.jpg)

次の XML サンプル コードでは、前述したコンテンツ グループが作成されます。 コンテンツ グループは、"**Sample Content Grouping (サンプル コンテンツ グループ)**" と表示されている、左端のタブです。

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![RCD 構成 XML](media/rcd-configuration-xml-reference/image010.jpg)

次の XML サンプル コードでは、前述した概要グループが作成されます。 概要グループは、"**Summary (概要)**" と表示されている、右端のタブです。

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>ヘルプ

Help 要素をオプションとして、Grouping 要素や Control 要素に含めることができます。 グループで使用される場合は、最初にこの要素が使用される必要があります。 テキスト形式のヘルプをエンド ユーザーに提供し、正確な情報を提供できます。 Help 要素の XSD スキーマを次に示します。

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
Help 要素の例を次に示します。
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>Control

Grouping 要素には、1 つまたは複数の Control 要素が含まれています。 コントロールは、RCDC の主要な要素です。 Grouping 要素に含まれるさまざまな Control 要素を定義して、Grouping 要素をカスタマイズできます。 Control 要素の XSD スキーマを次に示します。

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

コントロールには、次の要素が含まれています。

1.  **Help**: この要素は無視されます。 これはグループでのみ機能します。

2.  **CustomProperties**: この要素はサポートされていません。

3.  **Options**: この要素は、**UocDropDownList** コントロールまたは **UocRadioButtonList** コントロールとの組み合わせでのみ使用します。 その他のコントロールでは機能しません。 この要素の構造については、このドキュメントの「オプション」セクションをご覧ください。 コントロールのコンテキストでどのように使用されるかについては、個々のコントロールをご覧ください。

4.  **Buttons**: この要素は、**UocListView** コントロールとの組み合わせでのみ使用します。 その他のコントロールでは機能しません。 詳細については、このドキュメントの「UocListView」セクションをご覧ください。

5.  Properties: この要素は、コントロールの追加の動作を指定するためにすべてのコントロールで使用します。 この要素の詳細については、このドキュメントの「プロパティ」セクションをご覧ください。

6.  **Events**: この要素の構造については、このドキュメントで前述した「イベント」セクションをご覧ください。 コントロールに使用するイベントを決めるには、個々のコントロールの定義をご覧ください。

コントロールには、次の属性が含まれています。

1.  **Name**: これは、コントロールの名前です。 コントロールの名前は、各パネル内で一意である必要があります。 これは必須の、文字列型の属性です。

2.  **TypeName**: この属性は、コントロールの種類を指定します。 これは必須の、文字列型の属性です。 各コントロールの名前については、このドキュメントの「個々のコントロール」セクションをご覧ください。

3.  **Caption**: この属性を使用して、コントロールのキャプションを含めることができます。
    通常、キャプションは、コントロールによって表示または入力されるデータの表示名です。 キャプションの値を明示的に指定したり、スキーマ属性の表示名の情報とバインドしたりできます。 キャプションは、通常サイズのコントロールの左端に表示されます。 コントロールを全画面表示にすると、キャプションはコントロールの上に表示されます。 これは省略可能な、文字列型の属性です。 データ ソースを属性またはプロパティ値とバインドする方法について詳しくは、「プロパティ」セクションをご覧ください。

   次の例は、キャプションを明示的に使用する方法を示しています。

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     次の例は、データ ソースでキャプションを使用する方法を示しています。 ドキュメントで前述したデータ ソースのテンプレートを使用している場合、データ ソースは schema です。 属性の DisplayName を Caption 属性とバインドすることをお勧めします。

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Enabled: これは省略可能な、ブール型の属性です。 この属性値を false に設定すると、ユーザーはコントロールを無効にできます。 既定値は true に設定されています。

5.  Visible: これは省略可能な、ブール型の属性です。 この属性を使用して、コントロール全体を非表示にできます。 既定値は true に設定されています。

6.  Description: この省略可能な文字列型の属性を使用すると、コントロールに入力する内容や、コントロールの機能をエンド ユーザーが理解するのに役立つ説明を含めることができます。 説明に使用する値を明示的に指定したり、スキーマ属性の説明情報とバインドしたりできます。 <br/>説明は、通常サイズのコントロールの左端の、キャプションの下に表示されます。 コントロールを全画面表示にすると、説明はコントロール上部の、キャプションの下に表示されます。 データ ソースを属性またはプロパティ値とバインドする方法について詳しくは、このドキュメントの「プロパティ」セクションをご覧ください。

7.  次の例は、説明を明示的に使用する方法を示しています。
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  この例は、データ ソースで説明を使用する方法を示しています。 ドキュメントで前述したデータ ソースのテンプレートを使用している場合、データ ソースは **schema** です。 属性の**説明**を Description 属性とバインドすることをお勧めします。
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: この属性は、コントロールを全画面表示にするかどうかを示します。 これは省略可能な、ブール型の属性です。 既定値は false に設定されています。

    >[!NOTE]
    この属性を true に設定すると、Caption 属性と Description 属性は無効になります。 展開されたコントロールにキャプションを提供するには、UocLabel コントロールを使用する必要があります。
9. **Hint**: これは省略可能な、文字列型の属性です。 Hint 属性のテキストは、エンド ユーザーがコントロールの有効な入力を知るうえで役立ちます。 ヒントはコントロールの下に表示されます。

10.  **AutoPostback**: これは省略可能な、ブール型の属性です。 既定値は false です。 false に設定すると、ページを更新してもコントロールが更新されない場合があります。 AutoPostback の詳細については、Microsoft ASP.NET UI コントロールの、同じ名前のプロパティをご覧ください。

11. **RightsLevel**: これは省略可能な、文字列型の属性です。 この属性は、データ ソースのインライン権限とのみバインドできます。 ユーザーの権限に基づき、コントロールは動的に有効または無効になります。 データ ソースを属性またはプロパティ値とバインドする方法について詳しくは、このドキュメントの「プロパティ」セクションをご覧ください。

    この例は、データ ソースで **RightsLevel** 属性を使用する方法を示しています。 ドキュメントで前述したデータ ソースのテンプレートを使用している場合、データ ソースは **rights** です。 パスとして属性名を使用します。

### <a name="properties"></a>[プロパティ]

プロパティを使用して、各コントロールの動作をさらにカスタマイズできます。 プロパティは、空の要素です。 Property 要素の XSD スキーマを次に示します。
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



すべてのプロパティには、次の 2 つの必須の属性があります。

1.  **Name**: この文字列型の属性は、プロパティの一意の名前です。
    コントロールごとにプロパティが異なります。 すべてのコントロールで使用できる共通のプロパティがいくつかあります。 特定のコントロールで使用できる名前について詳しくは、「共通プロパティ」と「個々のコントロール」セクションをご覧ください。

2.  **Value**: これは、プロパティの値です。 値のデータ型は、割り当てられているプロパティによって異なります。 特定のプロパティで許可されている値の形式については、次のセクションをご覧ください。

一部のプロパティは、データ ソースの情報とバインドできます。 これを行うには、次の文字列の形式を使用する必要があります。 データ ソースとバインドする方法について詳しくは、「個々のコントロール」セクションで取り上げている個々のプロパティをご覧ください。

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**例:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>共通プロパティ
-----------------

このドキュメントで指定されるすべての RCDC コントロールは、次の共通のプロパティを持つことができます。 さまざまなコントロールに固有の他のプロパティと、これらのプロパティは併用できます。

1.  Required: このプロパティは、フィールドが必須フィールドまたは省略可能なフィールドのいずれかであることを示します。 必須フィールドには値を入力する必要があります。 文字列の入力の場合、空の値はサポートされていません。 省略可能なフィールドは、空のままでもかまいません。 必須フィールドに値が入力されないと、入力コントロールの上部にエラー メッセージが表示されます。 フィールドが必須かまたは省略可能かを明示的に指定できます。 フィールドを、属性とリソースの種類との間の、特定のバインドに関するスキーマ情報にバインドすることもできます。 既定では、このプロパティが指定されない場合、コントロールは省略可能な入力コントロールになります。

   このプロパティに明示的な値を使用する例を次に示します。

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   これは、このプロパティに動的なデータ ソースを使用する例です。 ドキュメントの前のセクションに示したデータ ソースのテンプレートを使用している場合、データ ソースは schema です。 パスとして \<属性名\>.Required を使用します。

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**: このプロパティを true に設定すると、エンド ユーザー側では、このコントロールは読み取り専用モードになります。 これは省略可能な、ブール型の属性です。
    既定値は false に設定されています。 ただし、コントロールのデータ バインドに対してユーザーが持つ権限の種類によっては、このプロパティの動作が上書きされる場合があります。 たとえば、ユーザーにフィールドを更新する権限がなく、フィールドがインライン権限とバインドされている場合は、このプロパティが false に設定されていても、データは読み取り専用モードでユーザーに表示されます。

3.  **RegularExpression**: このプロパティは、コントロールの値が受ける制限を指定します。 このプロパティ値の形式は、標準の .NET StringRegex でサポートされている形式です。 詳細については、「[.NET Framework Regular Expressions (.NET Framework の正規表現)](http://go.microsoft.com/fwlink/?LinkId=165361)」をご覧ください。 コントロールを使用して値を入力すると、ユーザーが現在のページから移動しようとしたときに、このプロパティに指定された制限に照らし合わせて値がチェックされます。
    無効な入力を含むコントロールの上部に、エラー メッセージが表示されます。 ユーザーは、文字列の正規表現を明示的に指定できます。 また、特定の属性のスキーマ情報とバインドすることもできます。 既定では、このプロパティが指定されない場合、コントロールは入力された文字列に対して何の制限もチェックしません。
    このプロパティに明示的な値を使用する例を次に示します。

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    これは、このプロパティに動的なデータ ソースを使用する例です。 ドキュメントで前述したデータ ソースのテンプレートを使用している場合、データ ソースは schema です。 パスとして <attribute name>.StringRegex を使用します。
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visible: これは省略可能な、ブール型の属性です。 この属性を使用して、コントロール全体を非表示にできます。 既定値は true に設定されています。

### <a name="options"></a>オプション

Options 要素には、1 つまたは複数のオプション サブノードが含まれます。 Options 要素は、UocRadioButtonList コントロールおよび UocDropDownList コントロールとのみ使用します。 これらの使用方法の詳細については、「個々のコントロール」セクションをご覧ください。 Options 要素の XSD スキーマを次に示します。

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


属性:

1.  Value: これは必須の、文字列型の属性です。 Value 属性は、同じコントロール内で一意である必要があります。 A から Z の文字のみを使用でき、大文字と小文字は区別されません。

2.  Caption: この必須の属性は、各オプションの表示名です。

3.  Hint: これは省略可能な属性です。 この属性を使用して、エンド ユーザーに詳細情報とヒントを提供できます。

### <a name="environment-variables"></a>環境変数

次の表の環境変数は、どの RCDC 構成でも使用できます。

| 変数 | 説明 |
|--------|--------|
| `<LoginID>`       | 現在ログインしているユーザーの ID を表示します。           |
| `<LoginDomain>`   | 現在ログインしているユーザーのドメインを表示します。       |
| `<Today>  `       | 現在の日付と時刻を表示します。                                |
| `<FromToday_nnn>` | 現在の日付、nnn、時刻を表示します。 nnn は整数です。  |
| `<ObjectID> `     | RCDC プライマリ リソースの ID です。                                     |
| `<Attribute_xxx>` | RCDC プライマリ リソースの指定された属性である xxx を返します。 |

### <a name="debugging-xml-configuration-files"></a>XML 構成ファイルのデバッグ


RCDC の XML 構成ファイルを作成または変更する場合、Microsoft Visual Studio® などのエディターを使用して、XSD ファイルに対して XML を検証することでエラーを削減できます。 詳細については、「[An Introduction to the XML Tools in Visual Studio 2005 (Visual Studio 2005 の XML ツールの概要)](http://go.microsoft.com/fwlink/?LinkID=74512)」をご覧ください。

### <a name="customizing-a-help-file"></a>ヘルプ ファイルのカスタマイズ

新しいリソースと属性を作成したら、FIM ポータルの既存のヘルプ ファイルを、カスタマイズされたリソースの内容で更新します。 FIM ポータルのヘルプ ファイルは .htm 形式であり、手動で編集できます。

>[!IMPORTANT]
カスタム属性の作成方法について詳しくは、FIM 2010 ドキュメントの「Introduction to Custom Resource and Attribute Management (カスタムのリソースおよび属性の管理の概要)」をご覧ください。

>[!IMPORTANT]
このセクションでは、HTML の基本的な書式と編集に関する説明は行いません。 ヘルプ ファイルを変更するには、HTML の編集に習熟している必要があります。


**ヘルプ ファイルの保存先**: Microsoft Identity Management 2016 SP1 ポータルのヘルプ ファイルはすべて、MIM サービス サーバーの次のフォルダーに保存されています。

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**適切なヘルプ ファイルを見つける方法**: FIM ポータルのヘルプ ファイルはすべて、グローバル一意識別子 (GUID) を使用して名前が付けられています。 カスタム リソースで使用する適切なファイルを見つけるには、次の手順を実行します。

1.  FIM ポータルのポータル ページで、カスタマイズするヘルプ ファイルを開きます。

2.  ヘルプ ファイルを右クリックして、**[プロパティ]** をクリックします。

3.  'URL Address' フィールドの `<GUID\>.htm` ファイルを強調表示し、コピーします。

4.  ヘルプ ファイルが保存されているフォルダーに移動して、ファイルを検索します。

**新しい属性に内容を追加する**: 既存の Grouping 要素 (タブ) 内で新しい属性に説明内容を追加するには、次の手順を実行します。

1.  適切なヘルプ ファイルを特定し、その場所を見つけます。

2.  HTML エディターを使用して、ファイルを開きます。

3.  内容を追加する場所を見つけます。 次の例に示すように、通常、これは追加するパラグラフ内になります。

`<p xmlns="">A new paragraph with customized information.</p>`

次の例に示すように、既存のリストに項目を挿入することになる場合もあります。
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**新しい Grouping 要素に内容を追加する**: FIM ポータル ページの大半には、複数の Grouping 要素 (タブ) が含まれており、付随するヘルプ ファイルには、各 Grouping 要素と関連した、ブックマークの付いたセクションがあります。 HTML 内のブックマークは、このセクションで指定します。 たとえば、FIM ポータルの [ユーザーの作成] ページで使用されるヘルプ ファイルの、[勤務先情報] タブの HTML を次に示します。

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

これは、**ユーザー作成の構成** RCDC で使用される構成データのXML ファイルで、Grouping 要素 **WorkInfo** によって参照されます。 ファイル名 `\<GUID\>.htm` とブックマークが、my:Link パラメーターで指定されていることに注意します。

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**単純なコントロールのサンプル**

次の図は、異なるモードの、単純なテキストボックス コントロールの各種サンプルを示しています。

例:

![](media/rcd-configuration-xml-reference/image016.gif)

次のコード セグメントでは、すべての属性とプロパティに明示的なテキストを使用する、1 つ目のテキストボックス コントロールが作成されます。

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


次のコード セグメントでは、動的なバインド技法を使用してコントロールを別のデータ ソースとリンクさせる、2 つ目のテキストボックス コントロールが作成されます。

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


次のコード セグメントでは、3 つ目の展開されたラベルとテキストボックス コントロールが作成されます。

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

次のコード セグメントでは、4 つ目の無効なテキストボックス コントロールが作成されます。
このコントロールでは、無効な状態と有効な状態との明らかな違いは見られませんが、ユーザーはテキスト ボックスにデータを入力できなくなります。

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>個々のコントロール

### <a name="uocbutton"></a>UocButton

**名前**: UocButton

**説明**: これは、特定のアクションをトリガーするのに使用できる単純なボタン コントロールです。 ただし、独自のハンドラーを指定できないため、このコントロールの用途は限られます。

**プロパティ**:

1.  **すべての共通プロパティ**: このプロパティについて詳しくは、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  **Text**: このプロパティは、ボタンに表示されるテキストを指定します。 これは省略可能な、文字列型の属性です。 テキストは、明示的な文字列値を取ります。

イベント:

   • **OnButtonClicked**: ボタンがクリックされると、このイベントが生成されます。

例:

![](media/rcd-configuration-xml-reference/image017.png)


次の XML セグメントでは、単純なボタンが作成されます。

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**名前**: UocCaptionControl

**説明**: このコントロールは、RCDC ページのキャプションを表示するのに使用されます。 このコントロールは、ヘッダー グループの単一のコントロールとしてのみ使用するように設計されています。
他のコンテキストで使用すると、表示に関する問題やポータル エラーが発生する可能性があります。

**モード**: 読み取り専用 (OneWay)

**プロパティ:**

1.  **すべての共通プロパティ:** 詳細については、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  **MaxHeight:** このプロパティは、キャプション セクションのアイコンの高さの最大値を指定します。 このプロパティは省略可能です。 このプロパティは、ピクセル単位で整数の値を取ります。 既定値は 32 ピクセルです。

例:

![](media/rcd-configuration-xml-reference/image020.jpg)

次のコード セグメントでは、**ヘッダー キャプション**が作成されます。
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


このコード セグメントでは、**明示的なコンテンツ キャプション**が作成されます。

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

次のコード セグメントでは、**表示名**の動的なキャプションが作成されます。
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**名前**: UocCheckBox

説明: これは、単純なチェックボックス コントロールです。 ユーザーはこのコントロールを、ブール型のデータとバインドすることをお勧めします。 このコントロールは、バインドされているデータに基づいて、読み取り専用コントロールまたは更新可能なコントロールとして使用できます。

>[!NOTE]
今回のリリースでは、ブール値の属性を表示するために編集モードでチェック ボックス コントロールを使用する際、この属性が事前に割り当てられた値を持っていない場合は、編集モードで **[OK]** をクリックするとリソース コントロールによって **false** の値が属性に追加されます。 これを回避するには、存在しない場合を **false** と同じである等しいとみなすブール値の属性を必ず作成するか、ブール値の属性にラジオ ボタンなどの他のコントロールを使用します。

**プロパティ**:

1.  **すべての共通プロパティ:** 詳細については、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  **DefaultValue**: これは省略可能な、ブール型のプロパティです。 既定値は false に設定されています。 このフィールドは、チェック ボックスの既定の動作を指定します。
    これは明示的に指定できます。

3.  **Checked**: これは省略可能な、ブール型のプロパティです。 既定値は false に設定されています。 DefaultValue とともに存在する場合、この値によって DefaultValue プロパティが上書きされます。 このフィールドは、チェック ボックスの動作を指定します。 DefaultValue と同様に、これを明示的に指定したり、サーバーからのデータとバインドしたりできます。

4.  **Text**: これは省略可能な、文字列型の属性です。 テキストは、チェック ボックスの右側に表示されます。 このプロパティを使用すると、エンド ユーザーに詳細情報を提供するテキストを指定できます。

**イベント**:

   • CheckedChanged: チェック ボックスの状態が変更されると、このイベントが生成されます。

次の例では、カスタム リソースの種類と **IsConfigurationType** 属性の間で、カスタム バインドが作成されます。 カスタム リソースの種類の RCDC では、XML が使用されます。

例:

![](media/rcd-configuration-xml-reference/image022.png)

次のコード セグメントでは、上記の図に "Dynamic Check Box (動的なチェック ボックス)" と表示されているように、**動的なチェック ボックス**が作成されます。 通常この種類のバインドは、明示的なチェック ボックスよりも多くの用途に使用できて便利です。 この属性は、現在のリソースの種類に属している必要があります。

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**説明**: これは、特殊な文字列の書式設定をサポートする、複数行のテキストボックス コントロールです。 複数値を持つエントリ間の各値は、テキスト ボックス内でセミコロン (;)、または改行で区切られます。 このコントロールを、複数値を持つ、短い文字列型および整数型のデータとバインドすることをお勧めします。 このコントロールでは、読み取り専用モードと更新可能モードの両方がサポートされます。

**プロパティ**:

1.  **すべての共通プロパティ:** 詳細については、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  **DataType**: これは必須の、文字列型の属性です。 これを明示的に、**文字列、整数**、または **DateTime** 型に指定できます。 属性を、スキーマ属性の **DataType** プロパティとバインドすることもできます。 複数値を持つ参照型は、**UOCListView** または **UOCIdentityPicker** によって処理する必要があります。 複数値を持つブール値のデータ型は、サポートされていません。

3.  **Rows**: これは省略可能な、整数型の属性です。 ボックスの高さを文字数で定義できます。 既定値は 1 に設定されています。

4.  **Columns**: これは省略可能な、整数型の属性です。 ボックスの幅を文字数で定義できます。 既定値は次の値に設定されています
    20.

5.  **Value**: これは省略可能な、文字列型の属性です。 この属性は、データ ソースとのみバインドできます。

イベント:

   • **ValueListChanged**: コントロールの現在の値が変更されると、このイベントがトリガーされます。

次の例では、**AMultiValueString** という名前の複数値を持つ文字列型の属性が作成されて、カスタム リソースの種類にバインドされます。 この例は、このバインドの作成後にのみ動作します。

**例:**

![](media/rcd-configuration-xml-reference/image024.jpg)

次のコード セグメントでは、上記の図が作成されます。

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**名前**: UocDateTimeControl

**説明**: これはテキストボックス コントロールと似ていますが、**説明**では特定の形式のみが受け入れられます。 読み取り専用モードでは、ラベルのように表示されます。 サポートされている入力文字列の形式については、このセクションの **DateTimeFormat** プロパティをご覧ください。

**プロパティ**:

1.  **すべての共通プロパティ:** 詳細については、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  **DateTimeFormat**: これは省略可能な、文字列型の属性です。 DateTime または DateOnly の形式がサポートされています。 既定値は、DateTime 形式に設定されています。
    a. DateTime 形式: この属性は、mm/dd/yyyy hh:mm:ss AM の形式です。

      <[!NOTE]
      ユーザーによる指定の違いに関係なく、**DateTime** と **DateOnly** の両方の形式がサポートされます。
3.  **Value**: これは省略可能な、文字列型の属性です。 この属性を、リソースのデータ ソースとバインドします。 この属性の値は、適切な日時形式に準拠する必要があります。

イベント:

   • **DateTimeChanged**: DateTime 値が変更されると、このイベントが発生します。

例:

![](media/rcd-configuration-xml-reference/image027.jpg)

次のコード セグメントでは、1 つ目の **DateTime** コントロールが作成されます。

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


次のコード セグメントでは、2 つ目の **DateTime** コントロールが作成されます。 「データ ソース」セクションのサンプル コードを使用している場合、**ExpirationTime** 属性はすべてのリソースの種類にバインドされます。 このため、次のコードで使用できます。

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**名前**: UocDropDownList

説明: これは、単純なドロップダウン ボックス コントロールです。 通常このコントロールは、定義された一連の選択肢からオプションを選択する場合に使用されます。 このコントロールには、文字列、整数、日時、ブール値のデータ型を指定できます。

**プロパティ**:

1.  **すべての共通プロパティ:** 詳細については、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  **ValuePath**: ItemSource から Value 属性を取得するプロパティです。 ItemSource を Custom に指定すると、値のパスが Value に設定されます。 これは、このドキュメントで後ほど定義される Option 要素の Value フィールドとバインドします。

3.  **CaptionPath**: ItemSource から Value 属性を取得するプロパティです。 ItemSource を Custom に指定すると、値のパスが Caption に設定されます。 これは、ドキュメントで後ほど定義される Option 要素の Caption フィールドとバインドします。

4.  **HintPath**: ItemSource から Value 属性を取得するプロパティです。 ItemSource を Custom に指定すると、値のパスが Hint に設定されます。 これは、ドキュメントで後ほど定義される Option 要素の Hint フィールドとバインドします。

5.  **ItemSource**: リストの選択肢を定義する ListControlItems のコレクションです。 ユーザーは、これを明示的に Custom に設定し、Option 要素を使用して文字列値を指定できます。

6.  **SelectedValue**: 現在選択されている値です。 これは必須の文字列型プロパティです。 このプロパティは、データ ソースの文字列データとバインドします。

イベント:

  • SelectedIndexChanged: ドロップダウン ボックスの選択が変更されると、このイベントが発生します。

オプション:

Option 要素の構造については、このドキュメントの 「オプション」セクションをご覧ください。

1.  **Value**: 単一の Option 要素の値は、コントロールがバインドされているデータ ソースの有効な入力である、任意の文字列に設定できます。

2.  **Caption**: キャプションは、任意の文字列値に設定できます。

3.  **Hint**: ヒントは、任意の文字列値に設定できます。

例:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
サンプルが機能するようにするには、既存の文字列型の属性 **Scope** を、RCDC が適用されるカスタム リソースの種類とバインドする必要があります。


このコード セグメントでは、ドロップダウン リストが作成されます。

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

名前: UocFileDownload

説明: このコントロールには、ハイパーリンクが含まれています。 ハイパーリンクをクリックすると、Windows の [ファイルの保存] ページが表示されます。 ユーザーは、ローカル ドライブにファイルを保存できます。
Internet Explorer で表示できるファイル形式の場合は、[開く] オプションもサポートされます。 このコントロールの使用が推奨されるデータ型は、書式設定された文字列 (XML) およびバイナリ型です。

>[!NOTE]
今回リリースされた Microsoft Identity Manager 2016 SP1 では、ユーザーはファイルを開いた Internet Explorer ウィンドウを閉じてから、ページを更新する必要があります。 Internet Explorer ウィンドウの更新後にダウンロードを開始して、元のウィンドウで同じファイルをもう一度保存したり開いたりできます。


［プロパティ］:

1.  **すべての共通プロパティ:** 詳細については、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  **Text**: これは省略可能な文字列型の属性で、ハイパーリンクのテキストを定義します。 ユーザーはこのプロパティに明示的な文字列を指定できます。

3.  **Value**: これは必須の属性です。 内容がダウンロードされるサーバーで、属性のバインドを指定します。

4.  **PromptedFileName**: これは省略可能な、文字列型の属性です。 ダウンロードしたファイルを保存するときに、ユーザーに提示されるファイル名です。

5.  **ContentType**: これは必須の、文字列型の属性です。 データが保存されているファイルの種類です。 サポートされている文字列のオプションは、テキストまたはバイナリの 2 つです。 テキストの場合、戻り値は長い文字列とみなされます。
    それ以外のバイナリの場合、戻り値は byte[] とみなされます。 テキストが選択されると、ユーザーはオプションとしてサフィックスを追加して、テキストが含まれる形式の種類を指定できます。 たとえば、text/xml が有効です。


>[!NOTE]
このコントロールにバインドされている値が空の場合、コントロールには、ダウンロード アクションをトリガーするハイパーリンクはありません。 これは、ダウンロードするものがないためです。


**イベント**:

このコントロールのイベントはありません。

**例**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
このサンプル ファイルをアップロードする前に、ユーザーはカスタム リソースの種類と既存の ConfigurationData 属性の間にバインドを作成する必要があります。


次のコード セグメントでは、上記の図のファイル ダウンロード コントロールが作成されます。

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**名前**: UocFileUpload

**説明**: このコントロールには、ローカル ファイルがアップロードされる場所を表示するテキスト ボックス、[ファイルの参照] ボタン、[アップロード] ボタンが含まれています。 エンド ユーザーが [参照] ボタンをクリックすると、Windows の [ファイルを開く] ウィンドウが表示されます。 エンド ユーザーは、ローカル ドライブでファイルを 1 つ選択し、アップロードできます。 ファイルを選択すると、テキスト ボックスにファイルの保存先が表示されます。 [アップロード] ボタンをクリックすると、クライアント側のローカル データ ソースにファイルがアップロードされます。 ファイルの内容は、まだサーバーに送信されていません。 このコントロールの使用が推奨されるデータ型は、書式設定された文字列 (XML) またはバイナリ型です。

>[!NOTE]
アップロードの進行状況や状態は表示されません。 ファイルがローカル データ ソースにアップロードされると、テキスト ボックスがクリアされます。


［プロパティ］:

1.  すべての共通プロパティ: 詳細については、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  Value: これは必須の属性です。 データがアップロードされるサーバー上の、スキーマ属性のバインドを指定します。

3.  ContentType: これは省略可能な、文字列型の属性です。 サーバーに保存されるファイルのデータ型です。 テキストまたはバイナリに設定できます。 このプロパティが指定されない場合、既定値はバイナリです。

4.  MaxFileSize: これは省略可能な、文字列型の属性です。 MaxFileSize は、アップロードされるファイルの最大サイズを定義します。 既定では、このプロパティが指定されない場合、最大サイズは 1 メガバイト (MB) です。

5.  PromptedForNoValue: これは省略可能な、文字列型の属性です。 ファイルがアップロードされないときにユーザーに表示されるテキストを定義します。

イベント:

   • FileUploaded: ファイルが正常にアップロードされると、このイベントが生成されます。

例:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
次のサンプル コードが機能するようにするには、ABinaryAttribute という名前の新しいバイナリ型の属性を作成し、カスタム リソースの種類とこの属性の間で新しいバインドを作成する必要があります。


次のコード セグメントでは、上記の図のアップロード コントロールが作成されます。
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

名前: UocFilterBuilder

説明: これは複雑なコントロールで、ユーザーは MIM 2016 の XPath 式を表示できます。 一部の XPath 式はサポートされていません。 フィルター ビルダーの使用方法については、フィルター ビルダーのヘルプをご覧ください。

［プロパティ］:

1.  すべての共通プロパティ: 詳細については、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  PermittedObjectTypes: これは、フィルター ビルダーの select ステートメントに表示されるリソースの種類のリストを定義します。 フィルター ビルダーの使用方法については、フィルター ビルダーのヘルプをご覧ください。 文字列は ResourceTypeA, ResourceTypeB の形式で、リソースの種類ごとにコンマ (,) で区切られます。

3.  Value: これは、フィルター ビルダーの表示で使用される値です。
    XPath 式を含む文字列型のデータとのバインドのみがサポートされています。 このコントロールのバインドには、Filter 属性が推奨されます。

4.  PreviewButtonVisible: これは省略可能な、ブール型のプロパティです。 このプロパティを false に設定すると、[プレビュー] ボタンがユーザーに表示されません。 既定値は true に設定されています。 このボタンをリストビュー コントロールと組み合わせて使用し、XPath 式の結果をプレビューすることができます。

5.  ExcludeGroupMembership: これはブール値のプロパティです。 このプロパティを true に設定すると、\<グループ オブジェクト\>のメンバーである\<参照属性\> (ResourceID など) を使用したフィルターは作成できません。 つまり、このプロパティを true に設定すると、グループ メンバーシップのディレクトリを使用するフィルターは作成できません。

6.  PreviewButtonCaption: これは省略可能な文字列です。 PreviewButtonVisible を true に設定すると、このプロパティを使用して、ボタンにカスタマイズされたテキストを指定できます。 テキストは [プレビュー] ボタンに表示されます。

イベント:

   • OnFilterChanged: フィルター ビルダーの内容が変更されると、このイベントがトリガーされます。

例:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
このサンプル コードを使用する前に、既存の Filter 属性とカスタム リソースの種類の間で新しいバインドを作成します。


UOCLabel コントロール、PermittedObjectTypes での単純なフィルター ビルダー、リスト ビューのプレビューを含むサンプル コードを次に示します。 リスト ビューの ListFilter プロパティとフィルター ビルダーの Value プロパティが同じデータ ソースの属性を指し、互いにリンクする必要があります。

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

名前: UocHtmlSummary

説明: このコントロールを使用して、RCDC ページの概要ページを定義できます。
エンド ユーザーが要求を送信すると、この概要ページが表示されます。 このコントロールは概要グループでのみ使用でき、グループの唯一のコントロールである必要があります。 提供されているサンプル コードを使用することを強くお勧めします。

>[!NOTE]
このコントロールでは、大規模なテストは行われていません。


［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  ModificationsXml: このプロパティは {Binding Source=delta, Path=DeltaXml} の形式で、差分は構成ヘッダーの ObjectDataSource で定義される必要があります。

3.  TransformXsl: このプロパティは通常 {Binding Source=summaryTransformXsl, Path=/} の形式で、summaryTransformXsl は構成ヘッダーの XmlDataSource で定義されます。

このコントロールの実例については、このドキュメント前半の「グループ化」セクションの「概要グループ」をご覧ください。

### <a name="uochyperlink"></a>UocHyperLink

名前: UocHyperLink

説明: これは、単純なハイパーリンク コントロールです。 このコントロールを使用して、情報をハイパーリンクとして表示できます。

［プロパティ］:

1.  すべての共通プロパティ: 詳細については、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  ObjectReference: これは省略可能な、参照型のプロパティです。 このプロパティで定義されている GUID によって有効なリソースが参照されると、ハイパーリンクはリソースにアクセスする方法をエンド ユーザーに提供します。 これは、NavigateUrl プロパティ (下記をご覧ください) と相互に排他的です。

3.  Text: これは省略可能な、文字列型のプロパティです。 ハイパーリンクとして表示されるテキストを定義するには、このプロパティを使用します。

4.  NavigateUrl: これは省略可能な、文字列型のプロパティです。 ハイパーリンクがリンクする完全なパスの URL を定義するには、このプロパティを使用します。 これは、ObjectReference プロパティ (上記をご覧ください) と相互に排他的です。

例:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
これをリンクするリソースの有効な GUID が必要です。 この例では、2 つ目のハイパーリンクが有効な GUID で作成されます。 1 つ目は、任意の Web サイトを指定できます。


次のコード セグメントでは、リダイレクト先のハイパーリンクが作成されます。

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

次のコード セグメントでは、リソースを参照するハイパーリンクが作成されます。 明示的な参照を {Binding Source=object, Path=Creator} の式で置き換えて、これをデータ ソースとバインドできます。 これは、リソースのマネージャーが存在し、参照型の値の場合にのみ有効です。

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

名前: UocIdentityPicker

説明: このコントロールは、解決ボックス (オプション) と参照ウィンドウで構成されます。 [解決] ボックス (オプション) は、ID を入力するテキスト ボックス (オプション)、ID を解決する [解決] ボタン、ポップアップの参照ウィンドウを表示させる [参照] ボタンで構成されます。 [参照] ウィンドウを使用すると、ユーザーはリストビュー コントロールから ID を選択できます。 [参照] ウィンドウで選択された ID は、[解決] ボックスに反映されます。

［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  UsageKeywords: これは省略可能な、文字列のプロパティです。 SearchScopeConfiguration 構造 (各キーワードが (‘) で区切られます) に対応している使用法キーワードの一覧を提供することで、リソース ピッカーで使用できる検索範囲のリストを定義できます。

3.  Filter: これは省略可能な、文字列のプロパティです。 ユーザーは、リソース ピッカーの範囲を限定する XPath 式を指定して、定義された範囲内に収まる項目のみが表示されるようにできます。 このプロパティは、UsageKeywords プロパティ (上記をご覧ください) と相互に排他的です。 検索範囲が適用される場合、このプロパティは影響を与えません。

4.  ResultObjectType: これは省略可能な、文字列のプロパティです。 リソースの種類を使用して、ポップアップのダイアログボックスのリスト内にリソースを表示します。 これをフィルターとともに使用すると、フィルターによって返されるリソースの種類が特定されるため、ID ピッカーは適切なデータを表示できます。 このプロパティは、UsageKeywords プロパティ (上記をご覧ください) と相互に排他的です。 検索範囲が適用される場合、これは影響を与えません。 このプロパティで許容される文字列は、任意の有効な、単一のリソースの種類名 (Person など) です。
    フィルターによって複数のリソースの種類が返されることが想定される場合は、Resource を使用します。

5.  PreviewTitle: これは、リスト ビューで使用されるプレビューのタイトルです。 このプロパティの詳細については、「UocListView」をご覧ください。

6.  ListViewTitle: これは省略可能な文字列のプロパティです。 このプロパティを使用して、リスト ビューの上部にタイトルとして表示されるテキストを定義できます。

7.  Value: これは省略可能な、文字列のプロパティです。 スキーマ属性とバインドして、値をデータ ソースとリンクさせることをお勧めします。

8.  Mode: これは省略可能な、文字列のプロパティです。 このプロパティを使用して、ID ピッカーで 1 つの値が選択されるか、または複数の ID が選択されるかを定義します。 許可される値は、SingleResult と MultipleResult です。 既定では、SingleResult に設定されています。

9.  ObjectTypes: これは省略可能な、文字列型のプロパティです。 エンド ユーザーが ID ピッカーの [解決] ボックスでエントリを解決するときに参照するリソースの種類のリストを定義できます。 このリストは、コンマ (,) で区切られたリソースの種類名の一覧で構成されます。

10. AttributesToSearch: これは省略可能な、文字列型のプロパティです。 ID ピッカーで項目の解決に使用する属性リストを定義できます。このリストは、コンマ (,) で区切られたスキーマ属性の一覧で構成されます。 たとえば、AttributesToSearch が DisplayName, Alias に設定されている場合、ユーザーは DisplayName=\<検索値\> または Alias=\<検索値\> で項目を検索できます。 ここに入力する属性名は、Value に表示されているデータ ソースのターゲット リソースの種類に有効な属性にすることをお勧めします。 ターゲット リソースの種類は、ObjectTypes フィールドで確認できます。 すべての属性は、ObjectTypes フィールドに示されているどのリソースの種類にも有効である必要があります。

11. ColumnsToDisplay: これは省略可能な、文字列型のプロパティです。 ユーザーは、コンマ (,) 区切りの、スキーマ属性名のリストを指定します。 ここで定義した属性によって、ID ピッカーのリスト ビューの列が構成されます。

12. Rows: これは省略可能な、整数のプロパティです。 Mode が MultipleResult に設定されている場合にのみ、機能します。 このプロパティを使用して、[解決] テキスト ボックスの高さを、文字単位で指定されたサイズに設定します。

13. MainSearchScreenText: これは省略可能な、文字列型のプロパティです。 これは、[参照] ウィンドウで検索が実行されているときに表示される、カスタマイズされたテキストです。

イベント:

 • SelectedObjectChanged: 選択されたリソースがユーザーによって変更されると、このイベントが生成されます。

例:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
このサンプルが機能するようにするには、Manager 属性とこの XML が適用される任意のカスタム リソースの種類の間で新しいバインドを作成する必要があります。


次のコード セグメントでは、Filter プロパティと ResultObjectType プロパティを RCDC の一部として使用することで、シングル モードの ID ピッカーが作成されます。

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



例:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
このサンプル コードが機能するようにするには、ExplicitMember 属性 (複数値を持つ参照属性) をカスタム リソースの種類にバインドする必要があります。 また、Person と Group に設定された UsageKeyword プロパティを使って検索範囲を作成する必要があります。


次のコード セグメントでは、上記の図のコントロールが作成されます。

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

名前: UocLabel

説明: これは単純な、読み取り専用のテキスト ラベル コントロールです。 このコントロールは、読み取り専用のデータの表示に使用することをお勧めします。

［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  Text: これは文字列型の属性です。 このプロパティを定義するには、明示的な文字列値を指定するか、データ ソースとバインドします。 このプロパティの値を割り当てるサンプル バインディングは、{Binding Source=object, Path=\<有効な属性名\> です。

UocLabel コントロールのサンプルについては、「単純なコントロールのサンプル」セクションで、単純なコントロールをご覧ください。

### <a name="uoclistview"></a>UocListView

名前: UocListView

説明: これは、高度なリストビュー コントロールです。 単純なリスト ビュー、単純な検索 (オプション)、高度な検索コントロール (オプション)、選択プレビュー ボックス (オプション)、およびアクション ボタン バーで構成されます。 単純な検索 (オプション)は、検索範囲と単純な検索テキスト ボックスで構成されます。 高度な検索コントロールは、フィルター ビルダーです。 リスト ビューには、プリレンダリング済みのリソース リストが表示されます。 また、このコントロールの検索コントロールから送信された検索結果も表示されます。 アクション ボタン バーでは、リスト ビューの選択に基づいて、どのアクションを実行するかを定義します。 選択プレビュー ボックスには、リスト ビューからどのような項目が選択されたかが表示されます。

>[!IMPORTANT]
UocListView は、単一値の参照属性では機能しません。 複数値を持つ参照属性でのみ機能します。 単一値の参照属性については、このドキュメントの「UocIdentityPicker」をご覧ください。


［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  SelectedValue: これは省略可能な文字列型のプロパティで、通常は GUID 形式の文字列のリストを受け入れる、複数値を持つ参照属性にバインドされます。

3.  PageSize: これは省略可能な、整数のプロパティです。 ユーザーは、リスト ビュー コントロールの 1 ページに収めるエントリの数を指定できます。 既定値は 10 個のエントリです。 正の整数はすべて有効です。

4.  UsageKeyword: これは省略可能な、文字列型のプロパティです。 ユーザーは、リストビューの検索コントロールで使用される検索範囲を定義する、キーワードのリストを指定できます。 FIM 2010 サーバーには、検索範囲のリソースが格納されています。 SearchScopeConfiguration 構造の UsageKeyword という属性が、検索範囲のセットをグループ化するのに使用されます。 リスト ビューでは、このキーワードのリストを使用します。 各キーワードはコンマ (,) で区切られます。
    この UsageKeyword は、リスト ビューに表示される、対応する検索範囲で使用されます。 これは、ShowSearchControl プロパティが true に設定されている場合にのみ有効です。

5.  SearchControlAutoPostback: これは省略可能なブール値のプロパティです。 このプロパティの値を true に設定すると、検索がトリガーされたときに AutoPostBack が実行されます。 既定では、SearchControlAutoPostback は false に設定されています。

6.  EmptyResultText: これは省略可能な、文字列型のプロパティです。 既定では No items (項目なし) に設定されていますが、任意の文字列値に設定できます。 検索結果が空の場合、このテキストが表示されます。

7.  ButtonHeight: これは省略可能な、整数型のプロパティです。 このプロパティの値を、任意の正の整数値に設定します。 このプロパティは、操作バーのボタンの高さをピクセル単位で定義します。 既定値は 32 ピクセルです。

8.  ButtonWidth: これは省略可能な、整数型のプロパティです。 このプロパティの値を、任意の正の整数値に設定します。 このプロパティは、操作バーのボタンの幅をピクセル単位で定義します。 既定値は 32 ピクセルです。

9.  CaptionImageMaxHeight: これは省略可能な、整数型のプロパティです。 このプロパティの値を、任意の正の整数に設定します。 このプロパティは、省略可能なキャプション アイコンの高さの最大値を定義します。 既定値は 32 ピクセルです。

10. CaptionImageMaxWidth: これは省略可能な、整数型のプロパティです。 このプロパティの値を、任意の正の整数に設定します。 このプロパティは、省略可能なキャプション アイコンの幅の最大値を定義します。 既定値は 32 ピクセルです。

11. CaptionImageUrl: これは省略可能な、文字列型のプロパティです。 このプロパティは、キャプション イメージとして表示されるイメージにリンクする URL を定義します。

12. PreviewTitle: これは省略可能な、文字列型のプロパティです。 このプロパティを使用して、選択プレビュー ボックスの上部に表示されるテキストを定義します。

13. EnableSelection: これは省略可能な、ブール型のプロパティです。 このプロパティを使用して、リスト ビューが選択モードかどうかを定義します。 リスト ビューが選択モードの場合、チェック ボックスの列がリスト ビューの左端の列に表示され、選択プレビュー ボックスがリスト ビューの一番下に表示されます。 このプロパティの既定値は true に設定されています。

14. SingleSelection: これは省略可能な、ブール型のプロパティです。 リスト ビューで選択モードが有効な場合、この値を true に設定すると、エンド ユーザーがリストから選択できる項目が 1 つに制限されます。 既定では、このプロパティの値は false に設定されています。 つまり既定では、エンド ユーザーはリストから複数の項目を選択できます。

15. RedirectUrl: これは省略可能な、文字列型のプロパティです。 このプロパティを使用して、リストでハイパーリンクとなっている項目をクリックしたときにリダイレクトされるページを指定します。 この URL には、実行時に実際の値で置き換えられるプレースホルダーを含めることができます。 プレースホルダーは次のとおりです。

     ◦ {0} - objectType

     ◦ {1} - objectID

     ◦ {2} - displayName

16.  ShowTitleBar: これは省略可能な、ブール型のプロパティです。 このプロパティを使用して、タイトル バーを表示するかどうかを指定します。 このプロパティの既定値は false です。

17.  ShowActionBar: これは省略可能な、ブール型のプロパティです。 このプロパティを使用して、操作バーの領域を表示するかどうかを指定します。 このプロパティの既定値は true です。

18.  ShowPreview: これは省略可能な、ブール型のプロパティです。 このプロパティを使用して、プレビュー領域を表示するかどうかを指定します。 このプロパティの既定値は true です。

19.  ShowSearchControl: これは省略可能な、ブール型のプロパティです。 このプロパティを使用して、検索コントロールを表示するかどうかを指定します。 このプロパティの既定値は true です。

20.  ResultObjectType: これは省略可能な、文字列型のプロパティです。 このプロパティを使用して、検索結果のオブジェクトの想定される種類を指定します。 このプロパティの既定値は Resource です。 検索結果に複数のリソースの種類が含まれる場合は、この値を Resource に指定する必要があります。

21.  ColumnsToDisplay: これは省略可能なプロパティです。 このプロパティを使用して、リスト ビューで列として表示される属性を指定します。 このプロパティの既定値は DisplayName、ResourceType です。 各列は、属性のシステム名で表示されます。 各列はコンマ (,) で区切られます。 リスト ビューが選択モードで使用されている場合は、このプロパティの値を指定する必要はありません。 選択モードでは、現在選択されている検索範囲の SearchScopeColumn 属性によって列が設定されます。

22.  ListFilter: これは省略可能な、文字列型のプロパティです。 リスト ビューの表示に使用される XPath であり、ShowSearchControl プロパティが false に設定されている場合にのみ有効です。 この値を指定すると、リスト ビューではこのプロパティ値がクエリに使用され、選択モードではなくなります。 フィルターは、次に示すように、リソースの文字列属性にバインドできます。<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     または、次に示すように、定義済みの環境変数を含む文字列にできます。 <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: これは現在使用されていないプロパティです。 この値は、複数値を持つ参照属性のシステム名にする必要があります。 このプロパティを使用しないことをお勧めします。 たとえば、グループ管理では、次を使用しないでください。

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  次のようにします。`<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: これは省略可能な、文字列型のプロパティです。 このプロパティを使用して、リスト ビューの項目をクリックしたときに、サーバーのポストバックがトリガーされるか、または項目の詳細ビューが表示されるかを指定します。 ModelessDialog と Server の、2 つのオプション値がサポートされています。 既定値は ModelessDialog です。

25.  SearchOnLoad: これは省略可能なブール型のプロパティで、リストビュー コントロールが読み込み時にクエリを実行するかどうかを指定します。 このプロパティは、リスト ビューが選択モードの場合にのみ適用できます。 このプロパティの既定値は true です。 意味のある結果を得るために、ユーザーが通常検索ボックスにテキストを入力することが想定される場合は、これをオフにできます。 この場合、リスト ビューでは、ユーザーに検索を実行する方法を伝えるメッセージが最初に表示されます。 次のプロパティでテキストをカスタマイズできます。

26.  MainSearchScreenText: この省略可能な文字列型のプロパティは、SearchOnload が true に設定されている場合にのみ、適用できます。 このプロパティを使用して、リスト ビューが自動的に検索しない場合にリスト ビューの中央に表示されるテキストをカスタマイズできます。 このプロパティの既定値は、"上の [検索] を使用して目的のリソースを検索してください" です。 値を指定して、シナリオに関連性の高いテキストにできます。

27.  SubSearchScreenText: この省略可能な文字列型のプロパティは、MainSearchScreenText の下に表示されるテキストをカスタマイズするのに使用します。 リスト ビューの使用方法に関して追加の指示を加える場合を除き、このプロパティの値を指定する必要はありません。

リスト ビューを UocFilterBuilder コントロールとともにプレビュー リストとして使用する方法の例については、ドキュメントで前述した UocFilterBuilder のサンプルをご覧ください。 UocListView は、フィルター ビルダーなしでも使用できます。

### <a name="uocnumericbox"></a>UocNumericBox

名前: UocNumericBox

説明: これは、整数値のみを取る単純なテキスト ボックスです。 このコントロールでは、読み取り専用モードと更新可能モードの両方がサポートされます。

［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  MaxValue: これは省略可能な、整数型のプロパティです。 このプロパティを使用して、コントロールのクライアント側の検証を定義します。 エンド ユーザーが入力する値は、この値を超えることはできません。 明示的な整数を入力するか、{Binding Source=schema Path=IntegerMaximum} を使用してデータ ソースの整数データとバインドできます。

3.  MinValue: これは省略可能な、整数型のプロパティです。 このプロパティを使用して、コントロールのクライアント側の検証を定義します。 エンド ユーザーが入力する値は、この値を下回ることはできません。 明示的な整数を入力するか、{Binding Source=schema Path=IntegerMinimum} を使用してデータ ソースの整数データとバインドできます。

4.  DefaultValue: これは省略可能な、整数型のプロパティです。 このプロパティを使用して、コントロールが新しいデータの作成に使用される場合のコントロールの既定値を指定します。 この値は、静的整数にのみ明示的に設定できます。

5.  Value: これは省略可能な、整数型のプロパティです。 データ ソースの整数型データとバインドすると、ページの読み込み時にこの属性の値が表示され、送信後にデータ ソースに値が保存されます。

ハンドラー:

   • TextChanged: コントロール内の現在の値が変更されると、このイベントが生成されます。

例:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
次のサンプル コードでは、1 つ目の数値ボックスが作成されます。 この数値ボックスは、データ ソースやスキーマ情報とリンクしていません。

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

次のサンプル コードでは、2 つ目の数値ボックスが作成されます。

>[!NOTE]
このサンプルが機能するようにするには、まず AnIntegerAttribute という名前の新しい整数型の属性を作成し、カスタム リソースの種類とバインドする必要があります。

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

名前: UocPictureBox

説明: このコントロールは、バイナリ型データの画像の表示に使用されます。 このコントロールはバイナリ型データで使用することをお勧めします。 指定された画像 URL、バイナリ型データ、または画像型のデータを含む属性ソースのいずれかで画像を表示できます。

［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  ImageUrl: これは省略可能な、文字列型のプロパティです。 対象となる画像の URL を入力します。

3.  MaxHeight: これは省略可能な、文字列型のプロパティです。 表示される画像の高さの最大値をピクセル単位で定義します。

4.  MaxWidth: これは省略可能な、文字列型のプロパティです。 表示される画像の幅の最大値をピクセル単位で定義します。

5.  ImageData: これはバイナリ型プロパティです。 このプロパティを使用して、データ ソースを表示画像とバインドします。 バインドするデータ ソースは、バイナリ型である必要があります。
    このフィールドを使用し、byte[] 形式でデータを指定することによって、画像を明示的に設定することもできます。

6.  ImageResource: これは省略可能な、バイナリ型のプロパティです。

7.  AlternativeText: これは省略可能な、文字列型のプロパティです。 このプロパティは、画像が表示できない場合に代替テキストとして表示されます。

サンプル:

>[!NOTE]
このサンプルを使用するには、コントロールとバインドされた既存の画像データが必要です。


次のコード セグメントでは、データ ソースをコントロールとバインドする、ピクチャ ボックス コントロールが作成されます。

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

次のコード セグメントでは、URL イメージをコントロールとバインドする、ピクチャ ボックス コントロールが作成されます。

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

名前: UocRadioButtonList

説明: これは単純な、オプションボタン リストです。 このリストの選択肢は相互に排他的です。 ユーザーが選択できるオプションが 5 つ以下の場合は、このコントロールが推奨されます。 それ以外の場合は、UOCListView の使用をお勧めします。

［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  ValuePath: 値のパスが Value に設定されます。 これは、このドキュメントで定義される Option 要素の Value フィールドとバインドします。

3.  CaptionPath: 値のパスが Caption に設定されます。 これは、このドキュメントで定義される Option 要素の Caption フィールドとバインドします。

4.  HintPath: 値のパスが Hint に設定されます。 これは、このドキュメントで定義される Option 要素の Hint フィールドとバインドします。

5.  SelectedValue: 現在選択されている値です。 これは必須の文字列型プロパティです。 このプロパティは、データ ソースの文字列データとバインドします。

イベント:

1.  SelectedIndexChanged

2.  CheckedChanged

オプション:

このコントロールのオプションには、Option 要素を 2 つしか含めることができません。

1.  Value: 1 つの Option 要素の Value フィールドは、True または False に設定する必要があります。

2.  Caption: 任意の文字列値に設定できます。

3.  Hint: 任意の文字列値に設定できます。

例:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
このサンプルが機能するようにするには、新しいブール値の属性 ABooleanAttribute を作成して、カスタム リソースの種類とバインドする必要があります。

次のコード セグメントでは、上記の図のオプション ボタンのリストが作成されます。

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


名前: UocSimpleRadioButton

説明: これは単純な、オプションボタン コントロールです。 このコントロールの使用は、単純なチェック ボックスと同様です。 2 つのオプション ボタンがあり、テキスト ラベルと並んで表示されます。 このコントロールは、ブール型のデータとバインドすることをお勧めします。

［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、このドキュメントの「共通プロパティ」セクションをご覧ください。

2.  TrueText: これは省略可能な、文字列型のプロパティです。 このテキストは、オプション ボタンが選択されているときに表示されます。

3.  FalseText: これは省略可能な、文字列型のプロパティです。 このテキストは、オプション ボタンが選択されていないときに表示されます。

4.  SelectedItem: これは省略可能な、ブール型のプロパティです。 この値は、オプション ボタンが選択されていることを示します。 これは、データ ソースのブール型のデータとバインドできます。 既定値は false に設定されています。

イベント:

   • CheckedChanged: オプション ボタンの状態が選択済みから未選択、または未選択から選択済みに変更されると、このシグナルが生成されます。

例:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
サンプルが機能するようにするには、新しいブール値の属性 ABooleanAttribute を作成して、カスタム リソースの種類にバインドする必要があります。 RCDC データは、同じカスタム リソースの種類に適用されます。

次のコード セグメントでは、上記の図のオプション ボタンが作成されます。

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

名前: UocTextBox

説明: これは文字列型の入力をサポートする、単純なテキスト ボックスです。 このコントロールを使用して、文字列型データとバインドすることをお勧めします。

［プロパティ］:

1.  すべての共通プロパティ: このプロパティについて詳しくは、ドキュメントの「共通プロパティ」セクションをご覧ください。

2.  MaxLength: これは省略可能な、整数型の属性です。 このプロパティは、入力文字列の最大長を指定します。 このプロパティの既定値は 128 文字です。

3.  Text: これは省略可能な、文字列型のプロパティです。 このテキストは、テキスト ボックスに表示されます。 コントロールの最初の読み込み時に、テキスト ボックスに表示される明示的な文字列を定義するか、または文字列型のスキーマ属性とバインドできます。

4.  Rows: これは省略可能な、整数型のプロパティです。 このプロパティは、テキスト ボックスの高さを文字単位で定義します。 既定値は 1 文字です。

5.  Columns: これは省略可能な、整数型のプロパティです。 このプロパティは、テキスト ボックスの幅を文字単位で定義します。 既定値は 20 文字です。

6.  Wrap: これは省略可能な、ブール型のプロパティです。 このプロパティの値を true に設定すると、テキスト ボックスの右端での折り返し機能が有効になります。 このプロパティの既定値は true に設定されています。

7.  UniquenessValidationXPath: これは省略可能な、文字列型のプロパティです。 有効な FIM XPath フィルター式を取り、ユーザーの入力した値がフィルターの範囲のリソース内で一意であることを確認します。
    たとえば、ユーザーの要求した表示名が、FIM サービス データベースのすべてのメールが有効なセキュリティ グループ内で一意であることを確認するには、XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’` を使用します。 ユーザーがページから移動するときに、検証アクションが実行されます。 このプロパティは、リソース作成の RCDC でのみサポートされています。

8.  UniquenessErrorMessage: これは省略可能な、文字列型のプロパティです。 この文字列は UniquenessValidationXPath の検証に失敗した場合のエラー メッセージの表示に使用され、明示的なテキストまたは文字列リソース変数を指定できます。 このプロパティが指定されない場合、検証の失敗に関する既定のエラー メッセージは、"%VALUE% の値は既に存在します。 別の値を試してください" です。

イベント:

   • TextChanged: テキスト ボックス内のテキストが変更されると、このイベントが生成されます。

このコントロールの完全なサンプルについては、「単純なコントロールのサンプル」セクションをご覧ください。

## <a name="appendix-a-default-xsd-schema"></a>付録 A: 既定の XSD スキーマ

Microsoft Identity Manager 2016 SP1 で利用可能な、既定の全 RCDC で使用できるすべての XSD スキーマを次に示します。

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>付録 B: 既定の概要 XSL

Microsoft Identity Manager 2016 SP1 で利用可能な、すべての概要 XSL を次に示します。
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
