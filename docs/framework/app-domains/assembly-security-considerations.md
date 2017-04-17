---
title: "アセンブリのセキュリティに関する考慮事項 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-bcl"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "アセンブリ [.NET Framework], セキュリティ"
  - "アセンブリ [.NET Framework], 署名"
  - "アセンブリ [.NET Framework], 厳密な名前を付けた"
  - "付与 (アクセス許可の), アセンブリ"
  - "整合性 (アセンブリとの)"
  - "名前 [.NET Framework], アセンブリ"
  - "名前 [.NET Framework], 厳密な名前"
  - "アクセス許可 [.NET Framework], アセンブリ"
  - "セキュリティ [.NET Framework], アセンブリ"
  - "Signcode"
  - "署名 (アセンブリに)"
  - "厳密な名前を付けたアセンブリ, セキュリティの注意事項"
ms.assetid: 1b5439c1-f3d5-4529-bd69-01814703d067
caps.latest.revision: 14
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 11
---
# アセンブリのセキュリティに関する考慮事項
アセンブリを作成する場合は、アセンブリの実行に必要となるアクセス許可セットを指定できます。  アセンブリに対して特定のアクセス許可を付与するかどうかは、証拠に基づいて決定されます。  
  
 証拠には 2 つの使い方があります。  
  
-   入力された証拠をローダーによって収集された証拠とマージして、ポリシーの解決に使用される最終的な証拠のセットを作成します。  この形式を使用するメソッドには、**Assembly.Load**、**Assembly.LoadFrom**、および **Activator.CreateInstance** があります。  
  
-   入力された証拠を変更せずに、ポリシーの解決に使用する最終的な証拠のセットとして使用します。  この形式を使用するメソッドには、**Assembly.Load\(byte\[\]\)** および **AppDomain.DefineDynamicAssembly\(\)** があります。  
  
 アセンブリが実行されるコンピューターで設定されている[セキュリティ ポリシー](../../../docs/framework/misc/code-access-security-basics.md)により、オプションのアクセス許可を与えることもできます。  発生する可能性があるセキュリティ例外をすべて処理するコードを作成するには、次のいずれかを行います。  
  
-   コードを実行するために必要なすべてのアクセス許可に対してアクセス許可要求を挿入し、アクセス許可が与えられなかった場合に生じる読み込み時エラーをあらかじめ処理しておく。  
  
-   コードを実行するために必要なアクセス許可を取得するためのアクセス許可要求は使用せず、アクセス許可が与えられなかった場合のセキュリティ例外を処理できるように準備しておく。  
  
    > [!NOTE]
    >  セキュリティは複雑な分野で、選択できるオプションも数多くあります。  詳細については、「[セキュリティの基本概念](../../../docs/standard/security/key-security-concepts.md)」を参照してください。  
  
 アセンブリの読み込み時に、セキュリティ ポリシーへの追加情報としてアセンブリの証拠が使用されます。  セキュリティ ポリシーは、エンタープライズとコンピューターの管理者、およびユーザー ポリシー設定によって確立され、すべてのマネージ コードが実行されるときに与えられるアクセス許可セットを決定します。  セキュリティ ポリシーは、アセンブリの発行者 \(署名ツールで生成されたシグネチャがある場合\)、アセンブリのダウンロード元の Web サイトおよびゾーン \(Internet Explorer の用語\)、またはアセンブリの厳密な名前に対して設定できます。  たとえば、コンピューター管理者は、Web サイトからダウンロードされ、所定のソフトウェア企業の署名のあるすべてのコードについて、コンピューター上のデータベースへのアクセスは許可するが、ディスクへの書き込みは許可しない、というセキュリティ ポリシーを設定できます。  
  
## 厳密な名前付きアセンブリと署名ツール  
 アセンブリの署名は、2 つの異なる補完的な方法で行うことができます。厳密な名前を使用するか、.NET Framework Version 1.0 と 1.1 の [File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または .NET Framework のそれ以降のバージョンの [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) を使用する方法です。  厳密な名前を使用してアセンブリに署名すると、アセンブリ マニフェストを格納しているファイルに公開キー暗号化が追加されます。  厳密な名前による署名では、名前の一意性の検証を支援し、名前の悪用を防止し、参照が解決されたときに呼び出し元に ID を提供できます。  
  
 しかし、厳密な名前そのものには信頼性がないため、信頼性という点では [File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) および [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) が重要になります。  2 つの署名ツールでは、発行者が第三者機関に対して自分の身元を証明し、証明書を取得する必要があります。  取得した証明書はファイルに埋め込まれ、管理者はその証明書を使用してコードの正当性を信頼するかどうかを判断します。  
  
 アセンブリには、厳密な名前と [File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) を使用して作成されたデジタル署名の両方を与えるか、またはそのいずれか一方だけを適用できます。  2 つの署名ツールでは、署名できるファイルは一度に 1 つだけです。マルチファイル アセンブリの場合は、アセンブリ マニフェストを格納しているファイルに署名します。  厳密な名前はアセンブリ マニフェストが格納されているファイルに保存されますが、[File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) を使用して作成された署名は、アセンブリ マニフェストが格納されているポータブル実行可能 \(PE: Portable Executable\) ファイルの予約された専用スロットに保存されます。  [File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) を使用するアセンブリに対する署名は、[File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) で生成された署名に依存する信頼階層が既に存在する場合や、またはポリシーがキー部分だけを使用し、信頼階層はチェックしていない場合に、\(厳密な名前と組み合わせて、または単独で\) 使用できます。  
  
> [!NOTE]
>  アセンブリに対して厳密な名前と署名ツールの署名の両方を使用する場合は、厳密な名前を先に割り当てる必要があります。  
  
 共通言語ランタイムは、ハッシュ検査も実行します。アセンブリ マニフェストには、アセンブリを構成するすべてのファイルのリストが格納されており、マニフェスト作成時の状態の各ファイルのハッシュも含まれています。  各ファイルが読み込まれたときに、その内容がハッシュされ、マニフェストに格納されているハッシュ値と比較されます。  2 つのハッシュが一致しない場合、アセンブリは読み込まれません。  
  
 厳密な名前と[File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または[SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) を使用する署名によって整合性が保証されるため、これら 2 種類のアセンブリ証拠に基づいてコード アクセス セキュリティ ポリシーを設定できます。  厳密な名前と [File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) を使用する署名では、デジタル署名と証明書によって整合性が保証されます。  以上で述べた技術 \(ハッシュ検査、厳密な名前、[File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db) または [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md) を使用する署名\) をすべて組み合わせて使用することで、アセンブリがどのような方法によっても変更されていないことが保証されます。  
  
## 参照  
 [厳密な名前付きアセンブリ](../../../docs/framework/app-domains/strong-named-assemblies.md)   
 [共通言語ランタイムのアセンブリ](../../../docs/framework/app-domains/assemblies-in-the-common-language-runtime.md)   
 [SignTool.exe \(署名ツール\)](../../../docs/framework/tools/signtool-exe.md)   
 [File Signing Tool \(Signcode.exe\)](http://msdn.microsoft.com/ja-jp/2d299154-34ea-41ba-ad12-17075bb7e1db)