---
title: "データ コントラクトの XML および ADO.NET の種類 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: c2ce8461-3c15-4c41-8c81-1cb78f5b59a6
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# データ コントラクトの XML および ADO.NET の種類
[!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] のデータ コントラクト モデルでは、XML を直接表す特定の型がサポートされています。  シリアライザーは、これらの型を XML にシリアル化するとき、型の XML コンテンツを書き出しますが、それ以上の処理は行いません。  サポートされている型は、<xref:System.Xml.XmlElement>、<xref:System.Xml.XmlNode> の配列 \(`XmlNode` 型自体はサポートされていません\)、および <xref:System.Xml.Serialization.IXmlSerializable> を実装した型です。  データベース プログラミングでは、<xref:System.Data.DataSet> 型、<xref:System.Data.DataTable> 型、および型指定されたデータセットが一般的に使用されます。  これらの型は `IXmlSerializable` インターフェイスを実装するので、データ コントラクト モデルでシリアル化可能です。  この型に関する考慮事項について、このトピックの最後に説明します。  
  
## XML 型  
  
### XmlElement  
 `XmlElement` 型は、その XML コンテンツを使用してシリアル化されます。  たとえば、次の型を使用するとします。  
  
 [!code-csharp[DataContractAttribute#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#4)]
 [!code-vb[DataContractAttribute#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#4)]  
  
 これは、次のように XML にシリアル化されます。  
  
```  
<MyDataContract xmlns="http://schemas.contoso.com">  
    <myDataMember>  
        <myElement xmlns="" myAttribute="myValue">  
            myContents  
        </myElement>  
    </myDataMember>  
</MyDataContract>  
```  
  
 ラッパー データ メンバー要素 `<myDataMember>` が引き続き存在している点に注意してください。  データ コントラクト モデルにはこの要素を削除する方法がありません。  このモデルを処理するシリアライザー \(<xref:System.Runtime.Serialization.DataContractSerializer> と <xref:System.Runtime.Serialization.NetDataContractSerializer>\) では、このラッパー要素に特別な属性が出力されることがあります。  この特別な属性には、標準の XML スキーマ インスタンスの "nil" 属性 \(`XmlElement` を `null` に設定できるようにする属性\) や "type" 属性 \(`XmlElement` をポリモーフィックに使用できるようにする属性\) などがあります。  また、"Id"、"Ref"、"Type"、および "Assembly" は、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] に固有の XML 属性です。  これらの属性が出力されるのは、オブジェクト グラフの保存モードを有効にするか <xref:System.Runtime.Serialization.NetDataContractSerializer> を使用する場合に `XmlElement` をサポートするためです   \(オブジェクト グラフの保存モードの[!INCLUDE[crabout](../../../../includes/crabout-md.md)] 「[シリアル化と逆シリアル化](../../../../docs/framework/wcf/feature-details/serialization-and-deserialization.md)」を参照してください\)。  
  
 `XmlElement` の配列またはコレクションを使用できます。これらは、他の配列またはコレクションとして処理されます。  つまり、コレクション全体のラッパー要素と、配列に含まれる `XmlElement` ごとに個別のラッパー要素 \(上記の例の `<myDataMember>` と同様\) が存在することになります。  
  
 逆シリアル化時には、デシリアライザーによって受信 XML から `XmlElement` が作成されます。  有効な親 <xref:System.Xml.XmlDocument> は、デシリアライザーによって提供されます。  
  
 `XmlElement` に逆シリアル化される XML フラグメントでは、先祖要素のプレフィックス定義に依存するのではなく、使用するすべてのプレフィックスを定義しておく必要があります。  これは、`DataContractSerializer` を使用して、`DataContractSerializer` 以外の別のソースから XML にアクセスする場合にのみ考慮する必要があります。  
  
 `DataContractSerializer` を使用する場合、`XmlElement` の割り当てはポリモーフィックに行えますが、割り当てることができるのは <xref:System.Object> 型のデータ メンバーに対してのみです。  <xref:System.Collections.IEnumerable> が実装されていても、`XmlElement` をコレクション型として使用したり、<xref:System.Collections.IEnumerable> データ メンバーに割り当てることはできません。  すべてのポリモーフィックな割り当てと同様に、`DataContractSerializer` は、生成される XML にデータ コントラクト名を出力します。この場合、コントラクト名は "http:\/\/schemas.datacontract.org\/2004\/07\/System.Xml" 名前空間の "XmlElement" です。  
  
 `NetDataContractSerializer` を使用する場合、`XmlElement` の有効なポリモーフィック割り当て \(`Object` または `IEnumerable` への割り当て\) がすべてサポートされます。  
  
 `XmlElement` から派生した型がポリモーフィックに割り当てられているかどうかに関係なく、このような型と上記の 2 つのシリアライザーを一緒に使用しないでください。  
  
### XmlNode の配列  
 <xref:System.Xml.XmlNode> の配列の使用は、`XmlElement` の使用に非常に似ています。  `XmlNode` の配列を使用すると、`XmlElement` を使用する場合よりも高い柔軟性が得られます。  たとえば、データ メンバーがラップする要素内に複数の要素を作成できます。  データ メンバーがラップする要素内には、XML コメントなどの要素以外のコンテンツを挿入することもできます。  さらに、ラップする側のデータ メンバー要素にも属性を挿入できます。  これらはすべて、`XmlNode` の配列に `XmlNode` の特定の派生クラス \(<xref:System.Xml.XmlAttribute>、`XmlElement`、<xref:System.Xml.XmlComment> など\) を追加することによって実現できます。  たとえば、次の型を使用するとします。  
  
 [!code-csharp[DataContractAttribute#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#5)]
 [!code-vb[DataContractAttribute#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#5)]  
  
 シリアル化すると、生成される XML は次のコードのようになります。  
  
```  
<MyDataContract xmlns="http://schemas.contoso.com">  
  <myDataMember myAttribute="myValue">  
     <!--myComment-->  
     <myElement xmlns="" myAttribute="myValue">  
 myContents  
     </myElement>  
     <myElement xmlns="" myAttribute="myValue">  
       myContents  
     </myElement>  
  </myDataMember>  
</MyDataContract>  
```  
  
 データ メンバー ラッパー要素 `<myDataMember>` には、1 つの属性、1 つのコメント、および 2 つの要素が含まれています。  これらが、シリアル化された 4 つの `XmlNode` インスタンスです。  
  
 結果的に無効な XML になる `XmlNode` の配列は、シリアル化できません。  たとえば、最初のインスタンスが `XmlElement` で、次のインスタンスが <xref:System.Xml.XmlAttribute> という 2 つの `XmlNode` インスタンスの配列は、無効です。このシーケンスは、有効な XML インスタンスに対応していません \(属性を関連付ける場所がありません\)。  
  
 `XmlNode` の配列の逆シリアル化時には、ノードが作成され、そのノードに受信 XML からの情報が追加されます。  有効な親 <xref:System.Xml.XmlDocument> は、デシリアライザーによって提供されます。  ラッパー データ メンバー要素にあるすべての属性を含めて、すべてのノードが逆シリアル化されます。ただし、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] シリアライザーによってその要素に配置される属性 \(ポリモーフィック割り当てを示すための属性など\) を除きます。  XML フラグメントですべての名前空間プレフィックスを定義するときの注意点は、`XmlElement` の逆シリアル化に適用されるのと同様に、`XmlNode` の配列の逆シリアル化にも適用されます。  
  
 オブジェクト グラフの保存を有効にしてシリアライザーを使用した場合、オブジェクトの等価性は、個別の `XmlNode` インスタンスではなく、`XmlNode` 配列のレベルでのみ保存されます。  
  
 1 つ以上のノードが `null` に設定された `XmlNode` の配列をシリアル化しないようにしてください。  配列メンバー全体を `null` にすることはできますが、配列に格納された個々の `XmlNode` を null にすることはできません。  配列メンバー全体が null の場合、ラッパー データ メンバー要素には、配列メンバー全体が null であることを示す特別な属性が含まれます。  逆シリアル化時にも、配列メンバー全体が null になります。  
  
 `XmlNode` の正規の配列だけが、シリアライザーによって特別に処理されます。  `XmlNode` を含む他のコレクション型として宣言されたデータ メンバーや、`XmlNode` から派生した型の配列として宣言されたデータ メンバーには、特別な処理は行われません。  このため、これらのデータ メンバーは、シリアル化に関するその他の条件のいずれかを満たしている場合を除き、通常はシリアル化できません。  
  
 `XmlNode` の配列の配列またはコレクションを使用できます。  コレクション全体のラッパー要素と、外側の配列またはコレクションに含まれる `XmlNode` の配列ごとに個別のラッパー要素 \(上記の例の `<myDataMember>` と同様\) が存在することになります。  
  
 `IEnumerable` の `Object` または `Array` の <xref:System.Array> 型のデータ メンバーに `XmlNode` インスタンスを追加しても、そのデータ メンバーが `XmlNode` インスタンスの `Array` として扱われることはありません。  配列の各メンバーは個別にシリアル化されます。  
  
 `DataContractSerializer` を使用する場合、`XmlNode` の配列をポリモーフィックに割り当てることができますが、その対象は `Object` 型のデータ メンバーだけです。  `IEnumerable` が実装されていても、`XmlNode` の配列をコレクション型として使用したり、`IEnumerable` データ メンバーに割り当てることはできません。  すべてのポリモーフィックな割り当てと同様に、`DataContractSerializer` は、生成される XML にデータ コントラクト名を出力します。この場合、コントラクト名は "http:\/\/schemas.datacontract.org\/2004\/07\/System.Xml" 名前空間の "ArrayOfXmlNode" です。  `DataContractSerializer` を使用する場合は、`XmlNode` 配列の有効な割り当てがサポートされます。  
  
### スキーマの考慮事項  
 XML 型のスキーマ マッピングの詳細については、「[データ コントラクト スキーマの参照](../../../../docs/framework/wcf/feature-details/data-contract-schema-reference.md)」を参照してください。  ここでは、重要な点の概要について説明します。  
  
 `XmlElement` 型のデータ メンバーは、次の匿名型を使用して定義された要素にマップされます。  
  
```  
<xsd:complexType>  
   <xsd:sequence>  
      <xsd:any minOccurs="0" processContents="lax" />  
   </xsd:sequence>  
</xsd:complexType>  
```  
  
 `XmlNode` の Array 型のデータ メンバーは、次の匿名型を使用して定義された要素にマップされます。  
  
```  
<xsd:complexType mixed="true">  
   <xsd:sequence>  
      <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax" />  
   </xsd:sequence>  
   <xsd:anyAttribute/>  
</xsd:complexType>  
```  
  
## IXmlSerializable インターフェイスを実装する型  
 `IXmlSerializable` インターフェイスを実装する型は、`DataContractSerializer` で完全にサポートされます。  これらの型には、スキーマを制御するために必ず <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 属性を適用する必要があります。  
  
 `IXmlSerializable` を実装する型には、任意のコンテンツを表す型、1 つの要素を表す型、および従来の <xref:System.Data.DataSet> 型の 3 種類があります。  
  
-   コンテンツ型では、`XmlSchemaProviderAttribute` 属性によって指定されたスキーマ プロバイダー メソッドが使用されます。  このメソッドから `null` が返されることはなく、属性の <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> プロパティは既定値 `false` のままになります。  これは、`IXmlSerializable` 型の最も一般的な使用方法です。  
  
-   要素型は、`IXmlSerializable` 型が自身のルート要素名を制御する必要があるときに使用します。  型を要素型としてマークするには、<xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 属性の <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> プロパティを `true` に設定するか、スキーマ プロバイダー メソッドから null を返します。  スキーマ プロバイダー メソッドの使用は、要素型ではオプションです。`XmlSchemaProviderAttribute` でメソッド名の代わりに null を指定できます。  ただし、`IsAny` が `true` で、スキーマ プロバイダー メソッドが指定されている場合、メソッドは null を返す必要があります。  
  
-   従来の <xref:System.Data.DataSet> 型は、`XmlSchemaProviderAttribute` 属性でマークされていない `IXmlSerializable` 型です。  これらの型は、スキーマ生成に関して <xref:System.Xml.Serialization.IXmlSerializable.GetSchema%2A> メソッドに依存しています。  以前のバージョンの .NET Framework では、このパターンが `DataSet` 型に使用され、型指定されたデータセットからクラスが派生されます。ただし、現在、このパターンは使用されなくなっており、従来の型に対応することだけを目的としてサポートされています。  このパターンに依存せず、必ず `XmlSchemaProviderAttribute` を `IXmlSerializable` 型に適用してください。  
  
### IXmlSerializable コンテンツ型  
 `IXmlSerializable` を実装しており、以前に定義したコンテンツ型である型のデータ メンバーをシリアル化すると、シリアライザーはそのデータ メンバーのラッパー要素を書き込み、<xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> メソッドに制御を渡します。  <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> 実装により、ラッパー要素に属性が追加されるなど、任意の XML が書き込まれることがあります。  `WriteXml` の実行後、シリアライザーは要素を閉じます。  
  
 `IXmlSerializable` を実装しており、以前に定義したコンテンツ型である型のデータ メンバーを逆シリアル化すると、デシリアライザーはそのデータ メンバーのラッパー要素に XML リーダーを配置し、<xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> メソッドに制御を渡します。  このメソッドは、開始タグと終了タグを含む、要素全体を読み取る必要があります。  `ReadXml` コードでは、要素が空の場合も忘れずに処理してください。  また、`ReadXml` の実装では、特定の方法で名前が付けられたラッパー要素に依存しないようにしてください。  シリアライザーによって選択される名前は、異なる場合があります。  
  
 `IXmlSerializable` コンテンツ型は、たとえば <xref:System.Object> 型のデータ メンバーなどに、ポリモーフィックに割り当てることができます。  また、型インスタンスを null にすることもできます。  さらに、`IXmlSerializable` 型は、オブジェクト グラフの保存を有効にして <xref:System.Runtime.Serialization.NetDataContractSerializer> で使用することも可能です。  これらのすべての機能を実現するには、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] シリアライザーが特定の属性 \(XML スキーマ インスタンス名前空間の "nil" と "type"、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 固有の名前空間の "Id"、"Ref"、"Type"、および "Assembly"\) をラッパー要素に追加する必要があります。  
  
#### ReadXml を実装するときに無視する属性  
 `ReadXml` コードに制御を渡す前に、デシリアライザーは、XML 要素を調べ、前述の特別な XML 属性を検出し、その属性に従って動作します。  たとえば、"nil" が `true` の場合は、null 値が逆シリアル化され、`ReadXml` は呼び出されません。  ポリモーフィズムが検出された場合は、要素のコンテンツが別の型と同様に逆シリアル化されます。  ポリモーフィックに割り当てられた型の `ReadXml` 実装が呼び出されます。  どの場合も、これらの特別な属性がデシリアライザーによって処理されるため、`ReadXml` 実装ではこれらの属性を無視する必要があります。  
  
### IXmlSerializable コンテンツ型のスキーマに関する考慮事項  
 `IXmlSerializable` コンテンツ型のスキーマをエクスポートすると、スキーマ プロバイダー メソッドが呼び出されます。  このスキーマ プロバイダー メソッドには、<xref:System.Xml.Schema.XmlSchemaSet> が渡されます。  このメソッドは、有効なスキーマをスキーマ セットに追加できます。  スキーマ セットには、スキーマをエクスポートした時点で既に認識されていたスキーマが格納されます。  スキーマ プロバイダー メソッドは、スキーマ セットに項目を追加する必要があるときに、適切な名前空間を持つ <xref:System.Xml.Schema.XmlSchema> がそのセットに既に存在するかどうかを確認する必要があります。  存在する場合、スキーマ プロバイダー メソッドは新しい項目を既存の `XmlSchema` に追加する必要があります。  存在しない場合、新しい `XmlSchema` インスタンスを作成する必要があります。  これは、`IXmlSerializable` 型の配列を使用する場合に重要です。  たとえば、`IXmlSerializable` 型を名前空間 "B" の "A" 型としてエクスポートする場合、スキーマ プロバイダー メソッドが呼び出される前に、"B" が "ArrayOfA" 型を保持するためのスキーマがスキーマ セットに既に存在している可能性があります。  
  
 型を <xref:System.Xml.Schema.XmlSchemaSet> に追加する以外に、コンテンツ型のスキーマ プロバイダー メソッドは null 以外の値を返す必要があります。  このメソッドは、特定の `IXmlSerializable` 型で使用するスキーマ型の名前を指定する <xref:System.Xml.XmlQualifiedName> を返すことができます。  この修飾名は、その型のデータ コントラクト名および名前空間としても使用されます。  スキーマ プロバイダー メソッドは、スキーマ セットにまだ存在していない型であっても、復帰時に返すことができます。  ただし、関連するすべての型がエクスポートされる \(<xref:System.Runtime.Serialization.XsdDataContractExporter> の関連するすべての型に対して <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> メソッドが呼び出され、<xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A> プロパティにアクセスする\) までに、その型がスキーマ セットに存在している必要があります。  関連するすべての `Export` 呼び出しが実行される前に `Schemas` プロパティにアクセスすると、<xref:System.Xml.Schema.XmlSchemaException> が発生する可能性があります。  エクスポート プロセス[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[クラスからのスキーマのエクスポート](../../../../docs/framework/wcf/feature-details/exporting-schemas-from-classes.md)」を参照してください。  
  
 スキーマ プロバイダー メソッドは、使用する <xref:System.Xml.Schema.XmlSchemaType> を返すこともできます。  その型は、匿名の場合とそうでない場合があります。  匿名の場合、`IXmlSerializable` 型のスキーマは、`IXmlSerializable` 型がデータ メンバーとして使用されるたびに匿名型としてエクスポートされます。  `IXmlSerializable` 型には、データ コントラクト名と名前空間が引き続き保持されます   \(決定方法は、「[データ コントラクト名](../../../../docs/framework/wcf/feature-details/data-contract-names.md)」で説明されているとおりです。ただし、<xref:System.Runtime.Serialization.DataContractAttribute> 属性を使用して名前をカスタマイズすることはできません\)。匿名でない場合、型は `XmlSchemaSet` に含まれている型のいずれかである必要があります。  これは、型の `XmlQualifiedName` を返す場合と同じです。  
  
 さらに、型のグローバル要素宣言がエクスポートされます。  型に <xref:System.Xml.Serialization.XmlRootAttribute> 属性が適用されていない場合、その要素にはデータ コントラクトと同じ名前と名前空間が使用され、その "nillable" プロパティは true に設定されます。  ただし、スキーマ名前空間 \("http:\/\/www.w3.org\/2001\/XMLSchema"\) だけは例外です。型のデータ コントラクトがこの名前空間にある場合は、スキーマ名前空間に新しい要素を追加することが禁止されているため、対応するグローバル要素は、空白の名前空間に属することになります。  型に `XmlRootAttribute` 属性が適用されている場合、グローバル要素宣言は、<xref:System.Xml.Serialization.XmlRootAttribute.ElementName%2A>、<xref:System.Xml.Serialization.XmlRootAttribute.Namespace%2A>、および <xref:System.Xml.Serialization.XmlRootAttribute.IsNullable%2A> の各プロパティを使用してエクスポートされます。  `XmlRootAttribute` が適用された場合の既定値は、データ コントラクト名、空白の名前空間、および true に設定された "nillable" です。  
  
 同じグローバル要素宣言の規則が、従来のデータセット型に適用されます。  `XmlRootAttribute` は、カスタム コードによって追加されたグローバル要素宣言をオーバーライドできません。これには、スキーマ プロバイダー メソッドを使用して `XmlSchemaSet` に追加された場合と、従来のデータセット型に対して `GetSchema` を使用して追加された場合があります。  
  
### IXmlSerializable 要素型  
 `IXmlSerializable` 要素型には、`true` に設定された `IsAny` プロパティか、`null` を返すスキーマ プロバイダー メソッドのいずれかが含まれています。  
  
 要素型のシリアル化と逆シリアル化は、コンテンツ型のシリアル化と逆シリアル化に非常に似ています。  ただし、重要な違いがいくつかあります。  
  
-   `WriteXml` の実装では、要素 \(これには複数の子要素が含まれている可能性もありますが\) を 1 つだけ出力することが想定されています。  この 1 つの要素の外側にある属性、複数の兄弟要素、またはこれらが混在したコンテンツを出力することはできません。  要素は空であってもかまいません。  
  
-   `ReadXml` の実装では、ラッパー要素の読み取りは想定されていません。  読み取ることが想定されているのは、`WriteXml` で生成される要素 1 つのみです。  
  
-   要素型を一様にシリアル化する場合 \(データ コントラクトのデータ メンバーとしてシリアル化する場合など\) は、コンテンツ型の場合と同様に、`WriteXml` を呼び出す前にラッパー要素が出力されます。  ただし、`DataContractSerializer` コンストラクターまたは `NetDataContractSerializer` コンストラクターによるシリアライザーの構築時にルート名と名前空間が明示的に指定されていない限り、トップ レベルで要素型をシリアル化しても、通常は `WriteXml` で書き出される要素を囲むラッパー要素が出力されることはありません。  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [シリアル化と逆シリアル化](../../../../docs/framework/wcf/feature-details/serialization-and-deserialization.md).  
  
-   構築時にルート名と名前空間を指定せずにトップ レベルで要素型をシリアル化した場合、<xref:System.Runtime.Serialization.XmlObjectSerializer.WriteStartObject%2A> と <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteEndObject%2A> では基本的に何も実行されず、<xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObjectContent%2A> によって `WriteXml` が呼び出されます。  このモードでは、シリアル化されるオブジェクトは null にできず、ポリモーフィックに割り当てることができません。  また、オブジェクト グラフの保存を有効化できず、`NetDataContractSerializer` も使用できません。  
  
-   構築時にルート名と名前空間を指定せずにトップ レベルで要素型を逆シリアル化したときに、要素の先頭を検出できた場合は、<xref:System.Runtime.Serialization.XmlObjectSerializer.IsStartObject%2A> が `true` を返します。  `verifyObjectName` パラメーターが `true` に設定されている <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A> は、実際にオブジェクトを読み取る前の動作が `IsStartObject` と同様です。  その後、`ReadObject` は制御を `ReadXml` メソッドに渡します。  
  
 要素型の場合も、エクスポートされるスキーマは、前のセクションで説明した `XmlElement` 型に対するスキーマと同じです。ただし、スキーマ プロバイダー メソッドは、コンテンツ型と同様、追加のスキーマを <xref:System.Xml.Schema.XmlSchemaSet> に追加できます。  要素型には `XmlRootAttribute` 属性を使用することはできないので、グローバル要素宣言は要素型に対して出力されません。  
  
### XmlSerializer との相違点  
 `IXmlSerializable` インターフェイス、`XmlSchemaProviderAttribute` 属性、および `XmlRootAttribute` 属性は、<xref:System.Xml.Serialization.XmlSerializer> でも認識されます。  ただし、データ コントラクト モデルでの処理方法に違いがあります。  重要な違いを以下にまとめます。  
  
-   スキーマ プロバイダー メソッドは、`XmlSerializer` で使用できるようにするためにパブリックにする必要がありますが、データ コントラクト モデルで使用するためにパブリックにする必要はありません。  
  
-   スキーマ プロバイダー メソッドは、データ コントラクト モデルで `IsAny` が true の場合に呼び出されますが、`XmlSerializer` では呼び出されません。  
  
-   コンテンツ型または従来のデータセット型に `XmlRootAttribute` 属性がない場合、`XmlSerializer` は、グローバル要素宣言を空白の名前空間にエクスポートします。  データ コントラクト モデルで通常使用される名前空間は、前に説明したとおりデータ コントラクトの名前空間です。  
  
 両方のシリアル化技術で使用する型を作成する場合には、これらの違いに注意してください。  
  
### IXmlSerializable スキーマのインポート  
 `IXmlSerializable` 型から生成されたスキーマをインポートする場合、次のような状況が考えられます。  
  
-   生成されたスキーマが、「[データ コントラクト スキーマの参照](../../../../docs/framework/wcf/feature-details/data-contract-schema-reference.md)」に記載された有効なデータ コントラクト スキーマである場合があります。  この場合は、スキーマを通常どおりにインポートでき、通常のデータ コントラクト型が生成されます。  
  
-   生成されたスキーマが、有効なデータ コントラクト スキーマではない場合があります。  たとえば、スキーマ プロバイダー メソッドによって、データ コントラクト モデルでサポートされていない XML 属性を含むスキーマが生成されることがあります。  この場合、スキーマを `IXmlSerializable` 型としてインポートできます。  このインポート モードは、既定では有効化されていませんが、たとえば、[ServiceModel メタデータ ユーティリティ ツール \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) の `/importXmlTypes` コマンド ライン スイッチを使用することによって、簡単に有効化できます。  詳細については、「[クラスを作成するためのスキーマのインポート](../../../../docs/framework/wcf/feature-details/importing-schema-to-generate-classes.md)」を参照してください。  型インスタンスを処理するには、XML を直接操作する必要があります。  `XmlSerializer` の使用方法に関するトピックを参照し、より広い範囲のスキーマをサポートする別のシリアル化技術を使用することを検討することもできます。  
  
-   新しい型を生成する代わりに、プロキシ内の既存の `IXmlSerializable` 型を再利用できます。  この場合、「型を作成するためのスキーマのインポート」で説明する参照型の機能を使用して、再利用する型を示すことができます。  これは、Svcutil.exe で `/reference` スイッチを使用して、再利用する型を含むアセンブリを指定することに相当します。  
  
## データ コントラクトでの任意の XML の表現  
 `XmlElement` 型、`XmlNode` の配列、および `IXmlSerializable` 型を使用すると、任意の XML をデータ コントラクト モデルに挿入できます。  `DataContractSerializer` と `NetDataContractSerializer` は、プロセスには介入せずに、使用している XML ライターにこの XML コンテンツを渡します。  ただし、XML ライターは、出力する XML に対して特定の制限を適用することがあります。  次に重要な例を具体的に示します。  
  
-   通常、XML ライターでは、別のドキュメントの書き込み中の XML ドキュメントの宣言 \(\<?xml version\=’1.0’ ?\> など\) は許可されません。  完全な XML ドキュメントを取得して、それを `XmlNode` データ メンバーの `Array` としてシリアル化したりすることはできません。  これを実行するには、ドキュメントの宣言を取り除くか、独自のエンコード方法でこの宣言を表現する必要があります。  
  
-   [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] に用意されたすべての XML ライターは、XML 処理命令 \(\<?  … ?\>\)  と文書型定義 \(\<\!  … \>\) を SOAP メッセージで使用できないため、これらを拒否します。  この場合も、独自のエンコード機構を使用して、この制限に対処できます。  生成される XML に XML 処理命令と文書型定義を含める必要がある場合は、この 2 つをサポートする XML ライターを使用するカスタム エンコーダーを作成できます。  
  
-   `WriteXml` を実装するときは、XML ライターで <xref:System.Xml.XmlWriter.WriteRaw%2A> メソッドを呼び出さないようにします。  [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] では、バイナリを含むさまざまな XML エンコードを使用します。その結果をどのエンコードでも使用できるように `WriteRaw` を使用することは非常に困難か不可能です。  
  
-   `WriteXml` を実装するとき、[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] で提供される XML ライターでサポートされていない <xref:System.Xml.XmlWriter.WriteEntityRef%2A> メソッドと <xref:System.Xml.XmlWriter.WriteNmToken%2A> メソッドを使用しないようにします。  
  
## DataSet、Typed DataSet、および DataTable の使用  
 データ コントラクト モデルでは、この 3 つの型の使用が完全にサポートされています。  これらの型を使用する場合は、次の点に注意してください。  
  
-   この 3 つの型 \(特に、<xref:System.Data.DataSet> と型指定された派生クラス\) のスキーマは、一部の [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] 以外のプラットフォームと相互運用できない場合があります。また、このようなプラットフォームで使用しても、結果的に使いにくい場合もあります。  また、`DataSet` 型の使用はパフォーマンスに影響を与えることがあります。  さらに、将来的にアプリケーションのバージョン管理が通常より難しくなる可能性もあります。  コントラクトでは、`DataSet` 型の代わりに、明示的に定義されたデータ コントラクト型を使用することを検討してください。  
  
-   `DataSet` スキーマまたは `DataTable` スキーマをインポートする際には、これらの型を参照することが重要です。  Svcutil.exe コマンド ライン ツールを使用すると、System.Data.dll アセンブリ名を `/reference` スイッチに渡すことによって、この参照を実現できます。  型指定されたデータセット スキーマをインポートする場合、その型指定されたデータセットの型を参照する必要があります。  Svcutil.exe を使用して、型指定されたデータセットのアセンブリの場所を `/reference` スイッチに渡します。  型を参照する方法[!INCLUDE[crabout](../../../../includes/crabout-md.md)]、「[クラスを作成するためのスキーマのインポート](../../../../docs/framework/wcf/feature-details/importing-schema-to-generate-classes.md)」を参照してください。  
  
 データ コントラクト モデルでは、型指定された DataSet のサポートが制限されています。  型指定された DataSet は、シリアル化と逆シリアル化が可能であり、スキーマをエクスポートすることもできます。  ただし、データ コントラクト スキーマのインポートでは、既存のスキーマの再利用のみが行われるため、型指定された新しい DataSet 型をスキーマから生成することはできません。  Svcutil.exe ツールで `/r` スイッチを使用して、型指定された既存の DataSet を指し示すことができます。  型指定されたデータセットを使用するサービスで、`/r` スイッチを指定せずに Svcutil.exe を使用しようとすると、代替のシリアライザー \(XmlSerializer\) が自動的に選択されます。  DataContractSerializer を使用することも、スキーマから DataSet を生成することも必要である場合は、\(サービスで `/d` スイッチを指定した Xsd.exe ツールを使用して\) 型指定された DataSet 型を生成し、型をコンパイルした後に、Svcutil.exe で `/r` スイッチを使用して、それらの型を指し示します。  
  
## 参照  
 <xref:System.Runtime.Serialization.DataContractSerializer>   
 <xref:System.Xml.Serialization.IXmlSerializable>   
 [データ コントラクトの使用](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)   
 [データ コントラクト シリアライザーでサポートされる型](../../../../docs/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer.md)