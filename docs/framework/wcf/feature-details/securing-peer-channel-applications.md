---
title: "セキュリティによるピア チャネル アプリケーションの保護 | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: d4a0311d-3f78-4525-9c4b-5c93c4492f28
caps.latest.revision: 13
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 13
---
# セキュリティによるピア チャネル アプリケーションの保護
[!INCLUDE[vstecwinfx](../../../../includes/vstecwinfx-md.md)] での他のバインディングと同様に、`NetPeerTcpBinding` には、既定で有効にされているセキュリティがあり、トランスポート ベースのセキュリティとメッセージ ベースのセキュリティが提供されます。  このトピックでは、これら 2 種類のセキュリティについて説明します。  セキュリティの種類は、バインディング仕様のセキュリティ モード タグ \(<xref:System.ServiceModel.NetPeerTcpBinding.Security%2A> `Mode`\) で指定します。  
  
## トランスポート ベースのセキュリティ  
 ピア チャネルでは、トランスポートをセキュリティで保護するための 2 種類の認証資格情報がサポートされます。両方とも、関連する `ChannelFactory` で `ClientCredentialSettings.Peer` プロパティを設定する必要があります。  
  
-   パスワード。  クライアントは、接続の認証に秘密パスワードの知識を利用します。  この種類の資格情報を使用する場合、`ClientCredentialSettings.Peer.MeshPassword` は、有効なパスワードと、必要に応じて `X509Certificate2` インスタンスを転送する必要があります。  
  
-   証明書。  特定のアプリケーション認証を使用します。  この種類の資格情報を使用する場合は、`ClientCredentialSettings.Peer.PeerAuthentication` で <xref:System.IdentityModel.Selectors.X509CertificateValidator> の具体的な実装を使用する必要があります。  
  
## メッセージ ベース セキュリティ  
 メッセージ セキュリティを使用すると、アプリケーションで送信メッセージに署名できるので、メッセージが信頼されたパーティから送信されたこと、および転送中にメッセージが改ざんされていないことをすべての受信パーティが確認できます。  現在、ピア チャネルでは、X.509 資格情報メッセージの署名のみがサポートされます。  
  
## ベスト プラクティス  
  
-   ここでは、ピア チャネル アプリケーションをセキュリティで保護するためのベスト プラクティスについて説明します。  
  
### ピア チャネル アプリケーションでセキュリティを有効にする  
 ピア チャネル プロトコルは分散的な性質のため、セキュリティで保護されていないメッシュ内でメッシュ メンバーシップ、機密性、およびプライバシーを強化することは困難です。  クライアントとリゾルバー サービス間の通信をセキュリティで保護することも重要です。  PNRP \(Peer Name Resolution Protocol\) では、セキュリティで保護された名前を使用してスプーフィングや他の一般的な攻撃を回避してください。  カスタム リゾルバー サービスをセキュリティで保護するには、メッセージ ベース セキュリティおよびトランスポート ベース セキュリティの両方を含むリゾルバー サービスにアクセスするためにクライアントが使用する接続でセキュリティを有効にします。  
  
### 可能な限り強力なセキュリティ モデルを使用する  
 たとえば、メッシュの各メンバーを個々に識別する必要がある場合は、証明書ベースの認証モデルを使用します。  これが不可能な場合は、セキュリティ保護のための最新の推奨事項に従ってパスワード ベースの認証を使用します。  推奨事項には、信頼されたパーティとのパスワードの共有、セキュリティで保護された媒介を使用するパスワードの転送、パスワードの頻繁な変更、強力なパスワード \(8 文字以上で、1 文字以上の大文字\/小文字、数字、特殊文字を含める\) の確認などがあります。  
  
### 自己署名された証明書を受け入れない  
 サブジェクト名に基づいて証明書の資格情報を受け入れないでください。  証明書はだれもが作成でき、検証する名前をだれもが選択できることに注意してください。  スプーフィングの可能性を回避するには、発行元証明機関 \(信頼された発行者またはルート証明機関\) の資格情報に基づいて証明書を検証します。  
  
### メッセージ認証を使用する  
 メッセージ認証を使用して、メッセージが信用されるソースから送信されていること、および送信中にメッセージが改ざんされていないことを確認します。  メッセージ認証を使用しないと、悪質なクライアントがメッセージを使用して容易に他人になりすましたり、メッセージを容易に改ざんしたりできます。  
  
## ピア チャネルのコード例  
 [ピア チャネルのシナリオ](../../../../docs/framework/wcf/feature-details/peer-channel-scenarios.md)  
  
## 参照  
 [ピア チャネルのセキュリティ](../../../../docs/framework/wcf/feature-details/peer-channel-security.md)   
 [ピア チャネル アプリケーションの構築](../../../../docs/framework/wcf/feature-details/building-a-peer-channel-application.md)