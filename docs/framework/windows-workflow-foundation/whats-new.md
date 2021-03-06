---
title: "Windows Workflow Foundation の新機能 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "WF [WF], 新機能"
  - "Windows Workflow Foundation [WF], 新機能"
ms.assetid: 11f96014-001e-41a0-bcc2-d0684a52fa43
caps.latest.revision: 29
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 29
---
# Windows Workflow Foundation の新機能
[!INCLUDE[netfx40_long](../../../includes/netfx40-long-md.md)] の [!INCLUDE[wf](../../../includes/wf-md.md)] では、前のバージョンから開発パラダイムがいくつか変更されました。ワークフローでは、新しい機能のホストの作成、実行、保守、実装が簡単になっています。.NET 3.0 および .NET 3.5 ワークフロー アプリケーションを移行して最新バージョンを使用する方法[!INCLUDE[crabout](../../../includes/crabout-md.md)]、「[移行のガイドライン](../../../docs/framework/windows-workflow-foundation//migration-guidance.md)」を参照してください。  
  
## ワークフロー アクティビティ モデル  
 アクティビティは、現在はワークフロー作成の基本単位であり、<xref:System.Workflow.Activities.SequentialWorkflowActivity> クラスや <xref:System.Workflow.Activities.StatemachineWorkflowActivity> クラスは使用されなくなっています。<xref:System.Activities.Activity> クラスは、ワークフロー動作の基本抽象クラスです。このため、アクティビティの作成者は、基本的なカスタム アクティビティ機能用に <xref:System.Activities.CodeActivity> を実装するか、一定範囲のランタイムを使用するカスタム アクティビティ機能用に <xref:System.Activities.NativeActivity> を実装することができます。また、アクティビティの作成者は <xref:System.Activities.Activity> クラスを使用して、他の <xref:System.Activities.NativeActivity> オブジェクト、<xref:System.Activities.CodeActivity> オブジェクト、<xref:System.Activities.AsyncCodeActivity> オブジェクト、または <xref:System.Activities.DynamicActivity> オブジェクトによって新しい動作を宣言して表します。このオブジェクトは、カスタムに開発されたものである場合と、[ビルトイン アクティビティ ライブラリ](../../../docs/framework/windows-workflow-foundation//net-framework-4-5-built-in-activity-library.md)に含まれているものである場合があります。  
  
## 豊富な複合アクティビティ オプション  
 <xref:System.Activities.Statements.Flowchart> は新しい強力な制御フロー アクティビティです。作成者は、これを使用して任意のループや条件分岐をモデル化できます。<xref:System.Activities.Statements.Flowchart> により、以前は <xref:System.Workflow.Activities.StateMachineWorkflowActivity> でのみ実装が可能であったイベント ドリブン プログラミング モデルを使用できます。手続き型のワークフローでは、<xref:System.Activities.Statements.TryCatch> や <xref:System.Activities.Statements.Switch%601> などの従来のフロー制御構造をモデル化する新しいフロー制御アクティビティを利用できます。  
  
## 拡張ビルトイン アクティビティ ライブラリ  
 アクティビティ ライブラリには、次のような新しい機能があります。  
  
-   <xref:System.Activities.Statements.DoWhile>、<xref:System.Activities.Statements.Pick>、<xref:System.Activities.Statements.TryCatch>、<xref:System.Activities.Statements.ForEach%601>、<xref:System.Activities.Statements.Switch%601>、<xref:System.Activities.Statements.ParallelForEach%601> などの新しいフロー制御アクティビティ  
  
-   <xref:System.Activities.Statements.Assign> などのメンバー データを操作するためのアクティビティと、<xref:System.Activities.Statements.AddToCollection%601> などのコレクション アクティビティ  
  
-   <xref:System.Activities.Statements.TransactionScope> や <xref:System.Activities.Statements.Compensate> などのトランザクションを制御するアクティビティ  
  
-   <xref:System.ServiceModel.Activities.SendContent> や <xref:System.ServiceModel.Activities.ReceiveReply> などの新しいメッセージング アクティビティ  
  
## 明示的なアクティビティ データ モデル  
 [!INCLUDE[netfx40_short](../../../includes/netfx40-short-md.md)] には、データを格納および移動するための新しいオプションがあります。データは、<xref:System.Activities.Variable> を使用してアクティビティに格納できます。データをアクティビティに移動したり、アクティビティから移動したりするときは、特殊な引数型を使用してデータの移動方向が判定されます。これらの型は、<xref:System.Activities.InArgument>、<xref:System.Activities.InOutArgument>、<xref:System.Activities.OutArgument>です。[!INCLUDE[crdefault](../../../includes/crdefault-md.md)][Windows Workflow Foundation のデータ モデル](../../../docs/framework/windows-workflow-foundation//data-model.md).  
  
## ホスティング、永続化、追跡の強化されたオプション  
 [!INCLUDE[netfx40_short](../../../includes/netfx40-short-md.md)] には、次のような永続化の拡張機能があります。  
  
-   <xref:System.ServiceModel.Activities.WorkflowServiceHost>、<xref:System.Activities.WorkflowApplication>、<xref:System.Activities.WorkflowInvoker> などの、ワークフローを実行するための多数のオプションがあります。  
  
-   <xref:System.Activities.Statements.Persist> アクティビティを使用してワークフロー状態データを明示的に永続化できます。  
  
-   ホストは、<xref:System.Activities.ActivityInstance> をアンロードせずに永続化できます。  
  
-   ワークフローは、永続化できないデータを処理するときは非永続化ゾーンを指定できます。このため、非永続化ゾーンを終了するまで永続化は延期されます。  
  
-   トランザクションは、<xref:System.Activities.Statements.TransactionScope> を使用してワークフローに流し込むことができます。  
  
-   追跡は、<xref:System.Activities.Tracking.TrackingParticipant> を使用して、簡単に実行できます。  
  
-   システムのイベント ログへの追跡は <xref:System.Activities.Tracking.EtwTrackingParticipant> を使用して実現されます。  
  
-   保留中のワークフローの再開は、<xref:System.Activities.Bookmark> オブジェクトを使用して管理されるようになりました。  
  
## WF デザイナー エクスペリエンスの容易な拡張  
 新しい WF デザイナーは [!INCLUDE[avalon1](../../../includes/avalon1-md.md)] に基づいて構築されており、Visual Studio の外部で WF デザイナーを再ホストするときに簡単に使用できるモデルです。また、カスタムのアクティビティ デザイナーを作成するための使いやすいメカニズムを備えています。[!INCLUDE[crdefault](../../../includes/crdefault-md.md)][ワークフロー デザイン操作のカスタマイズ](../../../docs/framework/windows-workflow-foundation//customizing-the-workflow-design-experience.md).