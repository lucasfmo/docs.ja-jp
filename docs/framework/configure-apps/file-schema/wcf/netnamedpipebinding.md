---
title: "&lt;netNamedPipeBinding&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
ms.assetid: 00a8580b-face-47a4-838d-b9fed48e72df
caps.latest.revision: 29
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 29
---
# &lt;netNamedPipeBinding&gt;
コンピューター上のプロセス間通信に適した、セキュリティで保護された信頼できる最適バインディングを定義します。  既定では、信頼のための WS\-ReliableMessaging、転送セキュリティ用トランスポート セキュリティ、メッセージ配信用名前付きパイプ、およびバイナリ メッセージ エンコーディングを持つランタイム通信スタックを生成します。  
  
## 構文  
  
```  
  
<netNamedPipeBinding>  
   <binding   
      closeTimeout="TimeSpan"  
      hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"  
      maxBufferPoolSize="Integer"  
      maxBufferSize="Integer"  
      maxConnections="Integer"   
      maxReceivedMessageSize="Integer"  
            name="string"  
      openTimeout="TimeSpan"   
      receiveTimeout="TimeSpan"  
      sendTimeout="TimeSpan"  
      transactionFlow="Boolean"  
      transactionProtocol="OleTransactions/WS-AtomicTransactionOctober2004"  
            transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"  
      <security mode="None/Transport">  
            <transport  protectionLevel="None/Sign/EncryptAndSign" />  
      </security>  
       <readerQuotas   
            maxArrayLength="Integer"  
            maxBytesPerRead="Integer"  
            maxDepth="Integer"   
            maxNameTableCharCount="Integer"           
            maxStringContentLength="Integer" />  
   </binding>  
</netNamedPipeBinding>  
```  
  
## 属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### 属性  
  
|属性|説明|  
|--------|--------|  
|closeTimeout|クローズ操作が完了するまでの期間を指定する <xref:System.TimeSpan> 値。  この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。  既定値は 00:01:00 です。|  
|hostnameComparisonMode|URI の解析に使用する HTTP ホスト名比較モードを指定します。  この属性は <xref:System.ServiceModel.HostnameComparisonMode> 型で、URI が一致したときにサービスへのアクセスにホスト名を使用するかどうかを指定します。  既定値は <xref:System.ServiceModel.HostnameComparisonMode.StrongWildcard> で、一致しているホスト名を無視します。|  
|maxBufferPoolSize|このバインディングに使用するバッファー プール サイズの上限を指定する整数。  既定は 524,288 バイト \(512 \* 1024\) です。  Windows Communication Foundation \(WCF\) では、多くの部分でバッファーを使用します。  使用するたびに毎回バッファーを作成および破壊すると負荷が高くなります。バッファーのガベージ コレクションも同様です。  バッファー プールを使用すると、バッファーをプールから取得して使用し、作業が終わったらプールに戻すことができます。  これで、バッファーの作成と破棄のオーバーヘッドを回避できます。|  
|maxBufferSize|メッセージをメモリに保存するのに使用するバッファーの最大サイズをバイト単位で指定する正の整数。  バッファーがいっぱいになると、超過データは、バッファーに空きが出るまで、基になるソケットに残されます。  この値が `maxReceivedMessageSize` 属性の値を下回らないようにしてください。  既定値は 65536 です。  詳細については、「<xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement.MaxBufferSize%2A>」を参照してください。|  
|maxConnections|サービスが作成し受け付ける発信\/着信接続数の上限を指定する整数。  この属性により指定された別個の制限に対して、着信接続および発信接続がカウントされます。<br /><br /> 制限を超える着信接続は、制限内に空きができるまでキューに置かれます。<br /><br /> 制限を超える発信接続は、制限内に空きができるまでキューに置かれます。<br /><br /> 既定値は 10 です。|  
|maxReceivedMessageSize|このバインディングで構成されるチャネルで受信可能な最大メッセージ サイズ \(ヘッダーを含む\) をバイト単位で指定する正の整数。  この制限を超えるメッセージの送信者が、SOAP エラーを受信します。  メッセージは受信者によって破棄され、トレース ログにこのイベントのエントリが作成されます。  既定値は 65536 です。|  
|name|バインディングの構成名を格納する文字列です。  この値は、バインディングの ID として使用されるため、一意にする必要があります。  [!INCLUDE[netfx40_short](../../../../../includes/netfx40-short-md.md)] 以降では、バインディングおよび動作に名前を付ける必要はありません。  既定の構成、および名前のないバインディングと動作の詳細については、「[簡略化された構成](../../../../../docs/framework/wcf/simplified-configuration.md)」および「[WCF サービスの簡略化された構成](../../../../../docs/framework/wcf/samples/simplified-configuration-for-wcf-services.md)」を参照してください。|  
|openTimeout|実行中の操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。  この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。  既定値は 00:01:00 です。|  
|receiveTimeout|受信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。  この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。  既定値は 00:10:00 です。|  
|sendTimeout|送信操作が完了するまでの時間間隔を指定する <xref:System.TimeSpan> 値です。  この値は必ず <xref:System.TimeSpan.Zero> 以上である必要があります。  既定値は 00:01:00 です。|  
|transactionFlow|バインディングが WS\-Transactions のフローをサポートするかどうかを指定するブール値です。  既定値は、`false` です。|  
|transactionProtocol|このバインディングで使用されるトランザクション プロトコルを指定します。  有効な値は、次のとおりです。<br /><br /> -   OleTransactions<br />-   WS\-AtomicTransactionOctober2004<br /><br /> 既定値は OleTransactions です。  この属性は <xref:System.ServiceModel.TransactionProtocol> 型です。|  
|transferMode|メッセージが要求や応答をバッファーするか、ストリーミングするかを指定する <xref:System.ServiceModel.TransferMode> 値です。|  
  
### 子要素  
  
|要素|説明|  
|--------|--------|  
|[\<security\>](../../../../../docs/framework/configure-apps/file-schema/wcf/security-of-netnamedpipebinding.md)|バインディングのセキュリティ設定を定義します。  この要素は <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement> 型です。|  
|[\<readerQuotas\>](../Topic/%3CreaderQuotas%3E.md)|このバインドを使用して設定されるエンドポイントにより処理可能な、SOAP メッセージの複雑さに対する制約を定義します。  この要素は <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> 型です。|  
  
### 親要素  
  
|要素|説明|  
|--------|--------|  
|[\<bindings\>](../../../../../docs/framework/configure-apps/file-schema/wcf/bindings.md)|この要素には、標準バインディングおよびカスタム バインディングのコレクションが保持されます。|  
  
## 解説  
 `NetNamedPipeBinding` は、トランスポート セキュリティ、メッセージ配信用の名前付きパイプ、およびバイナリ メッセージ エンコーディングを使用するランタイム通信スタックを既定で生成します。  このバインディングは、コンピューター間通信に適した、WCF \(Windows Communication Foundation\) システム標準の選択肢です。  トランザクションもサポートします。  
  
 `NetNamedPipeBinding` の既定の構成は、`NetTcpBinding` によって提供される構成に似ていますが、それよりも単純です。この理由は、WCF の実装はコンピューター間での使用のみを目的としているので、公開される機能が少ないためです。  最も顕著な違いは、`securityMode` 設定に `None` オプションと `Transport` オプションしか用意されていないことです。  SOAP セキュリティ サポートは、オプションに含まれません。  このセキュリティ動作は、省略可能な `securityMode` 属性を使用して構成できます。  
  
## 使用例  
 次の例では、同じコンピューター上でのプロセス間通信を実現する NetNamedPipeBinding バインディングを示します。  名前付きパイプは、異なるコンピューター間では動作しません。  
  
 バインディングは、クライアントとサービスの構成ファイルに指定されます。  バインディングの種類は、`<endpoint>` 要素の `binding` 属性に指定します。  netNamedPipeBinding バインディングを構成してその設定の一部を変更する場合は、バインディング構成を定義する必要があります。  エンドポイントは、バインディング構成を `bindingConfiguration` 属性を使用して名前で参照します。  この例では、バインディングの構成の名前は Binding1 です。  
  
```  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
               behaviorConfiguration="CalculatorServiceBehavior">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8000/ServiceModelSamples/service"/>  
          </baseAddresses>  
        </host>  
        <!-- this endpoint is exposed at the base address provided by host: net.pipe://localhost/ServiceModelSamples/service  -->  
        <endpoint address="net.pipe://localhost/ServiceModelSamples/service"  
                  binding="netNamedPipeBinding"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />  
        <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
  
    <bindings>  
      <netNamedPipeBinding>  
        <binding   
                 closeTimeout="00:01:00"  
                 openTimeout="00:01:00"   
                 receiveTimeout="00:10:00"   
                 sendTimeout="00:01:00"  
                 transactionFlow="false"   
                 transferMode="Buffered"   
                 transactionProtocol="OleTransactions"  
                 hostNameComparisonMode="StrongWildcard"   
                 maxBufferPoolSize="524288"  
                 maxBufferSize="65536"   
                 maxConnections="10"   
                 maxReceivedMessageSize="65536">  
          <security mode="Transport">  
            <transport protectionLevel="EncryptAndSign" />  
          </security>  
        </binding>  
      </netNamedPipeBinding>  
    </bindings>  
  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="CalculatorServiceBehavior">  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
## 参照  
 <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement>   
 <xref:System.ServiceModel.NetNamedPipeBinding>   
 [\<binding\>](../../../../../docs/framework/misc/binding.md)   
 [バインディング](../../../../../docs/framework/wcf/bindings.md)   
 [システムが提供するバインディングの構成](../../../../../docs/framework/wcf/feature-details/configuring-system-provided-bindings.md)   
 [Using Bindings to Configure Windows Communication Foundation Services and Clients](http://msdn.microsoft.com/ja-jp/bd8b277b-932f-472f-a42a-b02bb5257dfb)