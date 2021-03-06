---
title: "WF 内の制御フロー アクティビティ | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6892885b-f7c5-4aea-8f5e-28863fb4ae75
caps.latest.revision: 16
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 16
---
# WF 内の制御フロー アクティビティ
[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] には、ワークフロー内の実行フローを制御するアクティビティがいくつか用意されています。このようなアクティビティの一部 \(`Switch` や `If`\) は、[!INCLUDE[csprcs](../../../includes/csprcs-md.md)] など、プログラミング環境のアクティビティと似たフロー制御構造を実装しています。一方、その他は新しいプログラミング構造をモデル化しています \(`Pick` など\)。  
  
 `Parallel` および `ParallelForEach` などのアクティビティは、同時実行のために複数の子アクティビティをスケジュールできますが、シングル スレッドのみがワークフローに使用されます。これらのアクティビティのそれぞれの子アクティビティは連続して実行され、連続するアクティビティは前のアクティビティが完了するかアイドルになるまで実行されません。その結果、これらのアクティビティは、ブロック処理の可能性がある複数のアクティビティがインターリーブ形式で実行されるアプリケーションの場合に最も有効です。これらのアクティビティに、アイドルになる子アクティビティがない場合、`Parallel` は `Sequence` とまったく同様に実行され、`ParallelForEach` アクティビティは `ForEach` とまったく同様に実行されます。しかし、非同期アクティビティ \(<xref:System.Activities.AsyncCodeActivity> から派生するアクティビティなど\) またはメッセージング アクティビティが使用されると、子アクティビティがそのメッセージ受信を待っていても、または非同期作業を完了する必要があっても、コントロールは次の分岐にパスします。  
  
## フロー制御アクティビティ  
  
|アクティビティ|説明|  
|-------------|--------|  
|<xref:System.Activities.Statements.DoWhile>|含まれるアクティビティを 1 回実行し、条件が `true` の間はその実行を続行します。|  
|<xref:System.Activities.Statements.ForEach%601>|コレクション内の要素ごとに埋め込みステートメントを順番に実行します。<xref:System.Activities.Statements.ForEach%601> はキーワード `foreach` と似ていますが、言語ステートメントではなくアクティビティとして実装されます。|  
|<xref:System.Activities.Statements.If>|条件が `true` の場合は含まれるアクティビティを実行します。条件が `false` の場合は <xref:System.Activities.Statements.If.Else%2A> プロパティに含まれるアクティビティを実行できます。|  
|<xref:System.Activities.Statements.Parallel>|含まれるアクティビティを並列実行します。|  
|<xref:System.Activities.Statements.ParallelForEach%601>|コレクション内の要素ごとに埋め込みステートメントを並行実行します。|  
|<xref:System.Activities.Statements.Pick>|イベント ベースの制御フロー モデリングを提供します。|  
|<xref:System.Activities.Statements.PickBranch>|<xref:System.Activities.Statements.Pick> アクティビティ内の実行パスを表します。|  
|<xref:System.Activities.Statements.Sequence>|含まれるアクティビティを連続実行します。|  
|<xref:System.Activities.Statements.Switch%601>|指定された式の値に基づいて、実行する複数のアクティビティから 1 つを選択します。|  
|<xref:System.Activities.Statements.While>|条件が `true` である間は、含まれるアクティビティを実行します。|