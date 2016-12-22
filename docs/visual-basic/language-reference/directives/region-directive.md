---
title: "#Region Directive | Microsoft Docs"
ms.custom: ""
ms.date: "12/03/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "vb.Region"
  - "vb.#Region"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "Visual Basic compiler, compiler directives"
  - "#region directive"
  - "region directive (#region)"
  - "#Region keyword"
ms.assetid: 90a6a104-3cbf-47d0-bdc4-b585d0921b87
caps.latest.revision: 14
caps.handback.revision: 14
author: "stevehoag"
ms.author: "shoag"
manager: "wpickett"
---
# #Region Directive
[!INCLUDE[vs2017banner](../../../csharp/includes/vs2017banner.md)]

Visual Basic ファイルのコードのセクションを折りたたんで非表示にします。  
  
## 構文  
  
```  
  
        #Region "identifier_string"  
#End Region  
```  
  
## 指定項目  
  
|||  
|-|-|  
|用語|定義|  
|`identifier_string`|必須。  領域が折りたたまれたときにその領域のタイトルとして機能する文字列です。  既定では、領域は折りたたまれています。|  
|`#End Region`|`#Region` ブロックを終了します。|  
  
## 解説  
 Visual Studio コード エディターのアウトライン機能を使うときに展開または折りたたみの対象となるコード ブロックを指定するには、`#Region` ディレクティブを使用します。  類似した領域をグループ化するために、他の領域内に領域を配置する \(*入れ子*にする\) ことができます。  
  
## 使用例  
 `#Region` ディレクティブの使用例を次に示します。  
  
 [!CODE [VbVbalrConditionalComp#4](../CodeSnippet/VS_Snippets_VBCSharp/VbVbalrConditionalComp#4)]  
  
## 参照  
 [\#If...Then...\#Else Directives](../../../visual-basic/language-reference/directives/if-then-else-directives.md)   
 [アウトライン](/visual-studio/ide/outlining)   
 [How to: Collapse and Hide Sections of Code](../../../visual-basic/programming-guide/program-structure/how-to-collapse-and-hide-sections-of-code.md)