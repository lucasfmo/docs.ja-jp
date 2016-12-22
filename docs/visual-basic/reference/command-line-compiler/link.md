---
title: "/link (Visual Basic) | Microsoft Docs"
ms.custom: ""
ms.date: "12/15/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "l compiler option [Visual Basic]"
  - "EmbedInteropTypes"
  - "embed interop types [COM Interop]"
  - "-link compiler option [Visual Basic]"
  - "/link compiler option [Visual Basic]"
  - "link compiler option [Visual Basic]"
  - "-l compiler option [Visual Basic]"
  - "/l compiler option [Visual Basic]"
ms.assetid: 1885f24a-86f5-486c-a064-9fb7e455ccec
caps.latest.revision: 27
caps.handback.revision: 27
author: "stevehoag"
ms.author: "shoag"
manager: "wpickett"
---
# /link (Visual Basic)
[!INCLUDE[vs2017banner](../../../csharp/includes/vs2017banner.md)]

指定のアセンブリ内の COM 型情報をコンパイル中のプロジェクトで使用できるようにします。  
  
## 構文  
  
```  
/link:fileList  
' -or-  
/l:fileList  
```  
  
## 引数  
  
|||  
|-|-|  
|語句|定義|  
|`fileList`|必ず指定します。  アセンブリのファイル名をコンマで区切ったリストです。  ファイル名に空白が含まれる場合は、二重引用符 \(" "\) で囲む必要があります。|  
  
## 解説  
 `/link` オプションを使用すると、埋め込み型情報を含むアプリケーションを配置できます。  このアプリケーションは、ランタイム アセンブリを参照せずに、埋め込み型情報を実装するランタイム アセンブリ内の型を使用できます。  ランタイム アセンブリのバージョンが多数公開されている場合、埋め込み型情報を含むアプリケーションは、再コンパイルしなくても各種バージョンと共に使用できます。  例については、「[チュートリアル: マネージ アセンブリからの型の埋め込み](../Topic/Walkthrough:%20Embedding%20Types%20from%20Managed%20Assemblies%20\(C%23%20and%20Visual%20Basic\).md)」を参照してください。  
  
 `/link` オプションは、COM 相互運用機能を使用している場合に特に便利です。  COM 型を埋め込むと、ターゲット コンピューターにプライマリ相互運用機能アセンブリ \(PIA: Primary Interop Assembly\) がなくてもアプリケーションを実行できるようになります。  `/link` オプションを使用すると、コンパイラによって、COM 型情報が参照先の相互運用機能アセンブリから結果としてコンパイルされるコードに埋め込まれます。  COM 型は、CLSID \(GUID\) 値によって識別されます。  そのため、同じ CLSID 値を持つ同じ COM 型がインストールされているターゲット コンピューターでアプリケーションを実行できます。  たとえば、Microsoft Office を自動化するアプリケーションの例を考えてみます。  通常、Office のようなアプリケーションは異なるバージョン間で同じ CLSID 値を保持するので、.NET Framework 4 以降がターゲット コンピューターにインストールされていて参照先の COM 型に含まれるメソッド、プロパティ、またはイベントが作成アプリケーションで使用される限りは、そのアプリケーションで参照先の COM 型を使用できます。  
  
 `/link` オプションで埋め込まれるのは、インターフェイス、構造体、およびデリゲートだけです。  COM クラスの埋め込みはサポートされていません。  
  
> [!NOTE]
>  コードで埋め込み COM 型のインスタンスを作成する場合は、適切なインターフェイスを使用してインスタンスを作成する必要があります。  コクラスを使用して埋め込み COM 型のインスタンスを作成しようとすると、エラーが発生します。  
  
 [!INCLUDE[vsprvs](../../../csharp/includes/vsprvs_md.md)] で `/link` オプションを設定するには、アセンブリ参照を追加して、`Embed Interop Types` プロパティを **true** に設定します。  `Embed Interop Types` プロパティの既定値は **false** です。  
  
 別の COM アセンブリ \(アセンブリ B\) を参照する COM アセンブリ \(アセンブリ A\) にリンクする場合、次のいずれかの条件に該当するときは、アセンブリ B にもリンクする必要があります。  
  
-   アセンブリ A の型がアセンブリ B の型を継承しているか、アセンブリ B のインターフェイスを実装している場合。  
  
-   アセンブリ B の戻り値の型やパラメーターの型を持つフィールド、プロパティ、イベント、またはメソッドを呼び出す場合。  
  
 [\/libpath](../../../visual-basic/reference/command-line-compiler/libpath.md) を使用して、1 つ以上のアセンブリ参照があるディレクトリを指定します。  
  
 [\/reference](../../../visual-basic/reference/command-line-compiler/reference.md) コンパイラ オプションと同様に、`/link` コンパイラ オプションでは、頻繁に使用される [!INCLUDE[dnprdnshort](../../../csharp/getting-started/includes/dnprdnshort_md.md)] アセンブリを参照する Vbc.rsp 応答ファイルが使用されます。  コンパイラで Vbc.rsp ファイルを使用しない場合は、[\/noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md) コンパイラ オプションを使用します。  
  
 `/link` の省略形は `/l` です。  
  
## ジェネリックと埋め込み型  
 以下のセクションでは、相互運用型を埋め込むアプリケーションでジェネリック型を使用する場合の制限事項について説明します。  
  
### ジェネリック インターフェイス  
 相互運用機能アセンブリから埋め込まれたジェネリック インターフェイスは使用できません。  これを次の例に示します。  
  
 [!code-vb[VbLinkCompiler#1](../../../visual-basic/reference/command-line-compiler/codesnippet/VisualBasic/link_1.vb)]  
  
### ジェネリック パラメーターを含む型  
 型が相互運用機能アセンブリから埋め込まれたジェネリック パラメーターを含む型は、その型が外部アセンブリからの型である場合は使用できません。  ただし、これはインターフェイスには当てはまりません。  たとえば、<xref:Microsoft.Office.Interop.Excel> アセンブリで定義されている <xref:Microsoft.Office.Interop.Excel.Range> インターフェイスについて考えてみます。  ライブラリによって <xref:Microsoft.Office.Interop.Excel> アセンブリから相互運用型が埋め込まれ、型が <xref:Microsoft.Office.Interop.Excel.Range> インターフェイスであるパラメーターを含むジェネリック型を返すメソッドが公開される場合は、次のコード例に示すように、そのメソッドがジェネリック インターフェイスを返す必要があります。  
  
 [!code-vb[VbLinkCompiler#2](../../../visual-basic/reference/command-line-compiler/codesnippet/VisualBasic/link_2.vb)]  
[!code-vb[VbLinkCompiler#3](../../../visual-basic/reference/command-line-compiler/codesnippet/VisualBasic/link_3.vb)]  
[!code-vb[VbLinkCompiler#4](../../../visual-basic/reference/command-line-compiler/codesnippet/VisualBasic/link_4.vb)]  
  
 次の例では、クライアント コードで <xref:System.Collections.IList> ジェネリック インターフェイスを返すメソッドをエラーなしで呼び出すことができます。  
  
 [!code-vb[VbLinkCompiler#5](../../../visual-basic/reference/command-line-compiler/codesnippet/VisualBasic/link_5.vb)]  
  
## 使用例  
 ソース ファイル `OfficeApp.vb` と、`COMData1.dll` および `COMData2.dll` からの参照アセンブリをコンパイルし、`OfficeApp.exe` を生成する場合のコード例です。  
  
```vb#  
vbc /link:COMData1.dll,COMData2.dll /out:OfficeApp.exe OfficeApp.vb  
```  
  
## 参照  
 [Visual Basic Command\-Line Compiler](../../../visual-basic/reference/command-line-compiler/index.md)   
 [チュートリアル: マネージ アセンブリからの型の埋め込み](../Topic/Walkthrough:%20Embedding%20Types%20from%20Managed%20Assemblies%20\(C%23%20and%20Visual%20Basic\).md)   
 [\/reference](../../../visual-basic/reference/command-line-compiler/reference.md)   
 [\/noconfig](../../../visual-basic/reference/command-line-compiler/noconfig.md)   
 [\/libpath](../../../visual-basic/reference/command-line-compiler/libpath.md)   
 [コンパイル コマンド ラインのサンプル](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)   
 [Introduction to COM Interop](../../../visual-basic/programming-guide/com-interop/introduction-to-com-interop.md)