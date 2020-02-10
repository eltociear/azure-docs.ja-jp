---
title: Azure Application Insights の使用量とコストを管理する | Microsoft Docs
description: Application Insights でテレメトリの量を管理し、コストを監視します。
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: DaleKoetke
ms.author: dalek
ms.date: 11/27/2019
ms.reviewer: mbullwin
ms.openlocfilehash: b0a800a95d00e482b2342911111f43cfadb5a9c6
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845643"
---
# <a name="manage-usage-and-costs-for-application-insights"></a>Application Insights の使用量とコストを管理する

> [!NOTE]
> この記事では、Application Insights のコストを理解し、それを制御する方法について説明します。  関連記事の「[使用量と推定コストの監視](https://docs.microsoft.com/azure/azure-monitor/platform/usage-estimated-costs)」では、さまざまな価格モデルに対する複数の Azure 監視機能全体の使用量と推定コストを表示する方法について説明します。

Application Insights は、Azure とオンプレミスのどちらでホストされているかに関係なく、Web アプリケーションの可用性、パフォーマンス、使用状況の監視に必要なものすべてを使えるように設計されています。 Application Insights では、.NET、Java、Node.js など、一般的な言語とフレームワークがサポートされています。また、Azure DevOps、Jira、PagerDuty などの DevOps プロセスおよびツールと統合できます。 アプリケーションの監視コストがどのように決まるかを把握することが重要です。 この記事では、アプリケーションの監視コストを発生させるものを確認し、それらを予防的に監視および制御する方法について説明します。

Application Insights の課金のしくみについてご質問がある場合は、[フォーラム](https://social.msdn.microsoft.com/Forums/home?forum=ApplicationInsights&filter=alltypes&sort=lastpostdesc)に投稿してください。

## <a name="pricing-model"></a>価格モデル

[Azure Application Insights][start] の価格は**従量課金制**モデルであり、取り込まれたデータの量に基づき、データを長期保有してもかかる場合があります。 Application Insights の各リソースは個々のサービスとして課金され、Azure サブスクリプションの課金内容に加えられます。 データ ボリュームは、Application Insights がアプリケーションから受信した圧縮されていない JSON データ パッケージのサイズとして測定されます。 [Live Metrics Stream](../../azure-monitor/app/live-stream.md) を使用するためのデータ ボリューム料金はありません。

[複数ステップ Web テスト](../../azure-monitor/app/availability-multistep.md)に対しては、追加料金が発生します。 複数ステップ Web テストは、一連のアクションを実行する Web テストです。 単一ページの "*ping テスト*" については、個別の料金はかかりません。 Ping テストと複数ステップ テストからのテレメトリについては、アプリの他のテレメトリと同じ料金が請求されます。

## <a name="estimating-the-costs-to-manage-your-application"></a>アプリケーションを管理するためのコストの見積もり 

Application Insights をまだ使用していない場合は、[Azure Monitor 料金計算ツール](https://azure.microsoft.com/pricing/calculator/?service=monitor)を使用して、Application Insights の使用コストを見積もることができます。 まず、[検索] ボックスに「Azure Monitor」と入力し、結果として表示される Azure Monitor タイルをクリックします。 ページを下にスクロールして Azure Monitor に移動し、[種類] ドロップダウンから [Application Insights] を選択します。  知りたいことは Application Insights によるアプリケーションの監視で収集されるデータの量なので、ここでは 1 か月間に収集することが予想されるデータの GB 数を入力できます。 

これに対応する方法は 2 つあります。1 つは、ASP.NET SDK で利用できる既定の監視とアダプティブ サンプリングを使用する方法で、もう 1 つは他の似た顧客の実績に基づいてデータ インジェストを推定する方法です。

### <a name="data-collection-when-using-sampling"></a>サンプリングを使用する場合のデータ収集

ASP.NET SDK の[アダプティブ サンプリング](sampling.md#adaptive-sampling)を使用すると、データ ボリュームが自動的に調整されて、既定の Application Insights 監視に対して指定されている最大トラフィック レート内に維持されます。 アプリケーションが少量のテレメトリを生成している場合 (デバッグ時や使用量が少ない場合など)、量が構成されている秒あたりイベント数レベルを下回っている限り、サンプリング プロセッサによって項目がドロップされることはありません。 量の多いアプリケーションの場合、既定のしきい値である 5 イベント/秒では、アダプティブ サンプリングによって 1 日あたりのイベント数は 432,000 に制限されます。 標準的な平均イベント サイズである 1 KB を使用すると、アプリケーションがホストされているノードごとに、1 か月 (31 日) あたり 13.4 GB のテレメトリに対応します (サンプリングは各ノードに対してローカルに行われるため)。 

アダプティブ サンプリングがサポートされていない SDK の場合は、[インジェスト サンプリング](https://docs.microsoft.com/azure/azure-monitor/app/sampling#ingestion-sampling)を使用できます。この機能では、保持するデータの割合に基づいて、または Web サーバーと Web ブラウザーから送信されるトラフィックを減らすために [ASP.NET、ASP.NET Core、および Java の Web サイトでは固定レートのサンプリング](sampling.md#fixed-rate-sampling)で、Application Insights によってデータが受信されたときにサンプリングされます

### <a name="learn-from-what-similar-customers-collect"></a>似た顧客の収集を参考にする

Application Insights 用の Azure Monitoring 料金計算ツールでは、"アプリケーションのアクティビティに基づいてデータ量を見積もる" 機能を有効にした場合、アプリケーションに関する情報を (クライアント側テレメトリを収集する場合、1 か月あたりの要求数とページ ビュー数) を入力すると、類似アプリケーションによって収集されたデータ量の中央値と 90 パーセンタイル量が表示されます。 これらのアプリケーションでの Application Insights の構成は範囲が広いので (既定の[サンプリング](../../azure-monitor/app/sampling.md)を使用しているものや、サンプルを使用していないものなど)、サンプリングを使用して、取り込まれるデータの量が中央値のレベルよりはるかに少なくなるように制御できます。 しかし、これは、他の似た顧客が行っていることを理解するための出発点となります。 

## <a name="understand-your-usage-and-estimate-costs"></a>ご自分の使用量を理解してコストを見積もる

Application Insights では、最近の使用パターンに基づいてコストがどのようになるかを簡単に理解できるようになっています。 作業を開始するには、Azure portal で Application Insights リソースの **[使用量と推定コスト]** ページに移動します。 

![価格の選択](./media/pricing/pricing-001.png)

A. 該当の月のデータ量を確認します。 これには、サーバーおよびクライアント アプリから、また可用性テストから ([サンプリング](../../azure-monitor/app/sampling.md)の後に) 受信され保持されたすべてのデータが含まれます。  
B. [複数ステップ Web テスト](../../azure-monitor/app/availability-multistep.md)で別料金が発生しています。 (これには、データ ボリューム料金に含まれるシンプルな可用性テストは含まれません。)  
C. 過去 1 か月のデータ ボリュームの傾向を表示します。  
D. データ インジェストの[サンプリング](../../azure-monitor/app/sampling.md)を有効にします。   
E. 1 日のデータ ボリュームの上限を設定します。  

(この記事のスクリーンショットに表示されている価格はすべて、例を示す目的でのみ使用されていることに注意してください。 お客様の通貨およびリージョンでの現在の価格については、[Application Insights の価格][pricing]に関するページをご覧ください。)

Application Insights の使用量をさらに詳しく調査するには、 **[メトリック]** ページを開き、"データ ポイントの量" という名前のメトリックを追加してから、 *[Apply splitting]\(分割の適用\)* オプションを選択して、"テレメトリ項目の種類" でデータを分割します。 

Application Insights の課金は Azure の課金内容に加えられます。 Azure Portal の **[課金]** セクションか [Azure Billing Portal](https://account.windowsazure.com/Subscriptions) で、Azure の課金内容の詳細を確認できます。 

![左側のメニューで [課金] を選択します](./media/pricing/02-billing.png)

### <a name="using-data-volume-metrics"></a>データ ボリューム メトリックの使用
<a id="understanding-ingested-data-volume"></a>

データ ボリュームについてさらに詳しく把握するには、Application Insights リソースの **[メトリック]** を選択して、新しいグラフを追加します。 グラフのメトリックとして、 **[ログ ベースのメトリック]** の **[データ ポイントの量]** を選択します。 **[分割の適用]** をクリックし、 **[`Telemetryitem` の種類]** でグループを選択します。

![メトリックを使用してデータ ボリュームを確認する](./media/pricing/10-billing.png)

### <a name="queries-to-understand-data-volume-details"></a>クエリを実行してデータ ボリュームの詳細を理解する

たとえば、次のクエリを実行すると、`systemEvents` テーブルを使用して、過去 24 時間に取り込まれたデータ ボリュームを確認できます。

```kusto
systemEvents 
| where timestamp >= ago(24h)
| where type == "Billing" 
| extend BillingTelemetryType = tostring(dimensions["BillingTelemetryType"])
| extend BillingTelemetrySizeInBytes = todouble(measurements["BillingTelemetrySize"])
| summarize sum(BillingTelemetrySizeInBytes)
```

または、過去 30 日間のデータ量 (バイト単位) のデータ種類別グラフを表示するには、次のようにします。

```kusto
systemEvents 
| where timestamp >= startofday(ago(30d))
| where type == "Billing" 
| extend BillingTelemetryType = tostring(dimensions["BillingTelemetryType"])
| extend BillingTelemetrySizeInBytes = todouble(measurements["BillingTelemetrySize"])
| summarize sum(BillingTelemetrySizeInBytes) by BillingTelemetryType, bin(timestamp, 1d) | render barchart  
```

このクエリは、データ ボリュームに対するアラートを設定するために、[Azure ログ アラート](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log)で使用できることに注意してください。  

テレメトリ データの変化についてさらに詳しく調べるため、次のクエリを使用して、種類別のイベント数を確認してみましょう。

```kusto
systemEvents 
| where timestamp >= startofday(ago(30d))
| where type == "Billing" 
| extend BillingTelemetryType = tostring(dimensions["BillingTelemetryType"])
| summarize count() by BillingTelemetryType, bin(timestamp, 1d) | render barchart  
```

バイト単位のボリュームで示されているのと同様の変化が数にも見られる場合は、数が増えているイベントのデータの種類に焦点を当てることができます。  たとえば、依存関係の数が増加したことがわかった場合は、次のクエリを使用して、増加の原因となっている操作を把握します。

```kusto
dependencies 
| where timestamp >= startofday(ago(30d))
| summarize count() by operation_Name, bin(timestamp, 1d)  
| render barchart  
```


## <a name="viewing-application-insights-usage-on-your-azure-bill"></a>Azure の請求書での Application Insights の使用状況の表示 

Azure では、[Azure Cost Management と課金](https://docs.microsoft.com/azure/cost-management/quick-acm-cost-analysis?toc=/azure/billing/TOC.json)ハブに便利な機能が多数用意されています。 たとえば、"コスト分析" 機能を使用すると、Azure リソースに対するご自分の支出を表示できます。 リソースの種類 (Application Insights の場合は microsoft.insights/components) でフィルターを追加すると、支出を追跡できます。

[Azure portal から使用状況をダウンロード](https://docs.microsoft.com/azure/billing/billing-download-azure-invoice-daily-usage-date#download-usage-in-azure-portal)することで、使用状況をさらに詳しく理解できます。
ダウンロードしたスプレッドシートでは、Azure リソースごとに、1 日あたりの使用量を確認できます。 この Excel スプレッドシートでは、Application Insights のリソースからの使用量を検索することができます。それには、まず、[測定カテゴリ] 列でフィルター処理を行って "Application Insights" と "Log Analytics" を表示し、次に [インスタンス ID] 列に対するフィルター ("contains microsoft.insights/components") を追加します。  すべての Azure Monitor コンポーネントに対して 1 つのログ バックエンドがあるため、ほとんどの Application Insights の使用量は、メーターでは Log Analytics の測定カテゴリで報告されます。  Application Insights の測定カテゴリで報告されるのは、従来の価格レベルおよび複数ステップ Web テストでの Application Insights リソースのみです。  使用量は "消費量" 列に表示され、各エントリの単位は "測定単位" 列に表示されます。  詳細については、「[Microsoft Azure の課金内容を確認する](https://docs.microsoft.com/azure/billing/billing-understand-your-bill)」を参照してください。 


## <a name="managing-your-data-volume"></a>データ ボリュームの管理 

送信するデータの量は、次の手法で管理できます。

* **サンプリング**:サンプリングを使用すると、メトリックのひずみを最小に抑えて、サーバーおよびクライアント アプリから送信されるテレメトリの量を減らすことができます。 サンプリングは、送信するデータの量を調整するために使用できる主要なツールです。 [サンプリング機能の詳細については、こちらを参照してください](../../azure-monitor/app/sampling.md)。

* **Ajax 呼び出しの制限**: 各ページ ビューで、[報告できる Ajax 呼び出しの数を制限](../../azure-monitor/app/javascript.md#configuration)できます。Ajax レポートを無効にすることもできます。

* **不要なモジュールの無効化**: [ApplicationInsights.config を編集](../../azure-monitor/app/configuration-with-applicationinsights-config.md)し、不要なコレクション モジュールを無効にします。 たとえば、パフォーマンス カウンターや依存関係のデータが重要ではないと判断した場合などに検討します。

* **事前集計メトリック**: TrackMetric への呼び出しをアプリに配置した場合、平均計算と測定のバッチの標準偏差を受け入れるオーバーロードを使用して、トラフィックを減らすことができます。 または、[事前集計パッケージ](https://www.myget.org/gallery/applicationinsights-sdk-labs)を使用することもできます。
 
* **日次上限**:Azure portal で Application Insights リソースを作成する場合、日次上限は 100 GB/日に設定されます。 Visual Studio から Application Insights リソースを作成する場合の既定値は小 (32.3 MB/日) です。 日次上限の既定値は、テストを容易にするために設定されます。 アプリを実稼働環境にデプロイする前に、ユーザーが日次上限を上げることになります。 

    高トラフィック アプリケーション用に最大値の引き上げを要求する場合を除き、最大の上限は 1,000 GB/日です。 
    
    日次上限に関する警告メールは、Application Insights リソースの次のロールのメンバーであるアカウントに送信されます: "ServiceAdmin"、"AccountAdmin"、"CoAdmin"、"Owner"。

    日次上限を設定する場合はご注意ください。 意図は、"*日次上限に達しない*" ようにすることです。 日次上限に達した場合、その日の残りの時間についてデータが失われ、アプリケーションを監視することはできません。 日次上限を変更するには、 **[日次ボリューム上限]** オプションを使用します。 このオプションには、 **[使用量と推定コスト]** ウィンドウでアクセスできます (これについてはこの記事で後ほど詳しく説明します)。
    
    Application Insights には使用できなかったクレジットがある一部のサブスクリプションの種類の制限を除去しました。 これまでは、サブスクリプションに使用制限がある場合は、[日次上限] ダイアログに、使用制限を解除して日次上限を 32.3 MB/日から引き上げる方法が表示されました。
    
* **スロットル**:スロットルにより、データ速度は、インストルメンテーション キーごとに 1 分間で平均して 1 秒あたり 32,000 イベントに制限されます。 アプリから送信されるデータ量は分単位で評価されます。 1 分間で平均して 1 秒あたりのレートを超える場合、一部の要求がサーバーから拒否されます。 SDK はデータをバッファー処理し、その再送信を試みます。 急激な増加を数分間に分散させます。 アプリが常にスロットル レートを超えてデータを送信した場合、一部のデータが破棄されます (ASP.NET、Java、JavaScript SDK はこの方法でデータの再送信を試みますが、その他の SDK は調整されたデータを単に破棄します)。スロットルが発生した場合、この状況が発生したことを通知する警告が表示されます。

## <a name="manage-your-maximum-daily-data-volume"></a>ご自分のデータの 1 日の最大ボリュームを管理する

日次ボリューム上限を使用すると、収集されるデータを制限できます。 ただし、上限に達した場合は、その日の残りの時間についてアプリケーションから送信されたすべてのテレメトリが失われます。 アプリケーションが日次上限に達することは "*望ましくありません*"。 日次上限に達した後は、アプリケーションの正常性とパフォーマンスを追跡できません。

日次ボリューム上限を使用する代わりに、[サンプリング](../../azure-monitor/app/sampling.md)を使用して、データ ボリュームを目的のレベルに調整してください。 その後、アプリケーションが予期せず大量のテレメトリの送信を開始した場合に、"最後の手段" としてのみ日次上限を使用します。

### <a name="identify-what-daily-data-limit-to-define"></a>定義する日次データ制限を明らかにする

Application Insights の使用量と推定コストを確認し、データ インジェストの傾向および日次のデータ ボリュームの上限をどう定義するか理解します。 上限に達した後はリソースを監視できなくなるので、慎重に検討してください。 

### <a name="set-the-daily-cap"></a>1 日の上限を設定する

日次上限を変更するには、Application Insights リソースの **[構成]** セクションで、 **[使用量と推定コスト]** ページから **[日次上限]** を選択します。

![テレメトリの日次ボリューム上限の調整](./media/pricing/pricing-003.png)

[日次上限を変更するために Azure Resource Manager で変更する](../../azure-monitor/app/powershell.md)プロパティは `dailyQuota` です。  Azure Resource Manager を使用して、`dailyQuotaResetTime` と日次上限の `warningThreshold` を設定することもできます。 

## <a name="sampling"></a>サンプリング
[サンプリング](../../azure-monitor/app/sampling.md)は、テレメトリがアプリに送信される速度を低下させる一方で、診断検索中に関連イベントを見つける機能を保持する方法です。 適切なイベント カウントも保持されます。

サンプリングは、料金を削減し、月間クォータ内で維持する効果的な方法です。 サンプリング アルゴリズムはテレメトリの関連項目を保持するので、たとえば、Search を使用する場合は、特定の例外に関連する要求を検出できます。 アルゴリズムは、要求レート、例外レート、およびその他のカウントについてメトリックス エクスプローラーに正しい値が表示されるように正しいカウントも保持します。

サンプリングの形式にはいくつかあります。

* [アダプティブ サンプリング](../../azure-monitor/app/sampling.md)は、ASP.NET SDK の既定値です。 アダプティブ サンプリング は、アプリが送信するテレメトリの量を自動的に調整します。 Web アプリの SDK で自動的に動作して、ネットワーク上のテレメトリのトラフィックを削減します。 
* *インジェスト サンプリングabc* は、アプリからのテレメトリが Application Insights サービスに入る時点で動作します。 インジェスト サンプリングは、アプリから送信されるテレメトリの量には影響しませんが、サービスによって保持される量が削減されます。 インジェスト サンプリングを使用すると、ブラウザーや他の SDK からのテレメトリによって使用されるクォータを削減できます。

インジェスト サンプリングを設定するには、 **[価格]** ウィンドウに移動します。

![クォータと価格のウィンドウで、[サンプル] タイルを選択して、サンプリングの割合を選択する](./media/pricing/pricing-004.png)

> [!WARNING]
> **[データのサンプリング]** ウィンドウでは、インジェスト サンプリングの値だけを制御します。 アプリで Application Insights SDK によって適用されているサンプリング レートは反映されません。 受信テレメトリが既に SDK でサンプリングされている場合、インジェスト サンプリングは適用されません。
>

適用されている場所に関係なく、実際のサンプリング レートを検出するには、[Analytics クエリ](analytics.md) を使用します。 クエリは次のようになります。

    requests | where timestamp > ago(1d)
    | summarize 100/avg(itemCount) by bin(timestamp, 1h)
    | render areachart

保持されている各レコードで、`itemCount` は、それが表す元のレコードの数を示します。 これは、1 + 以前に破棄されたレコードの数と同じです。 

## <a name="change-the-data-retention-period"></a>データ保持期間の変更

Application Insights リソースの既定の保持期間は 90 日です。 Application Insights リソースごとに異なる保持期間を選択できます。 使用可能な保持期間の完全なセットは、30 日、60 日、90 日、120 日、180 日、270 日、365 日、550 日、または 730 日です。 

保持期間を変更するには、ご利用の Application Insights リソースから **[使用量と推定コスト]** ページに移動し、 **[データ保持期間]** オプションを選択します。

![テレメトリの日次ボリューム上限の調整](./media/pricing/pricing-005.png)

この保持期間は `retentionInDays` パラメーターを使用して [PowerShell でプログラムによって設定](powershell.md#set-the-data-retention)することもできます。 さらに、データ保持を 30 日間に設定すると、`immediatePurgeDataOn30Days` パラメーターを使用してより古いデータの即時の消去をトリガーできます。これは、コンプライアンス関連のシナリオに役立つ可能性があります。 この消去機能は、Azure Resource Manager 経由でのみ公開されます。また、使用するときは細心の注意を払う必要があります。 データ ボリュームの上限の 1 日あたりのリセット時間は、Azure Resource Manager を使用して、`dailyQuotaResetTime` パラメーターを設定することで構成できます。 

## <a name="data-transfer-charges-using-application-insights"></a>Application Insights の使用でのデータ転送料金

Application Insights にデータを転送する場合、データ帯域幅の料金が発生する場合があります。 [Azure 帯域幅の価格ページ](https://azure.microsoft.com/pricing/details/bandwidth/)で説明されているように、2 つのリージョンに存在する Azure サービス間のデータ転送は、通常の料金で送信データ転送として課金されます。 受信データ転送は無料です。 ただし、この料金は、Application Insights のデータ インジェストのコストと比へると非常に小さい (数パーセント) ものです。 そのため、Log Analytics のコスト管理では、ご自分で取り込まれたデータ ボリュームに注目する必要があり、それについて理解するためのガイダンスが[こちら](https://docs.microsoft.com/azure/azure-monitor/app/pricing#managing-your-data-volume)に用意されています。   

## <a name="limits-summary"></a>制限の概要

[!INCLUDE [application-insights-limits](../../../includes/application-insights-limits.md)]

## <a name="disable-daily-cap-e-mails"></a>日次上限メールを無効にする

日次ボリューム上限メールを無効にするには、Application Insights リソースの **[構成]** セクションで、 **[使用量と推定コスト]** ウィンドウから **[日次上限]** を選択します。 上限に達したとき、および調整可能な警告レベルに達したときに、メールを送信する設定があります。 すべての日次上限ボリューム関連メールを無効にする場合は、両方のボックスをオフにします。

## <a name="legacy-enterprise-per-node-pricing-tier"></a>従来の Enterprise (Per Node) 価格レベル

Azure Application Insights の早期導入者は、引き続き次の 2 つの価格レベルをご利用いただけます。Basic と Enterprise。 Basic 価格レベルは前述のとおりで、既定のレベルです。 これには、Enterprise レベルのすべての機能が追加コストなしで含まれます。 Basic レベルでは基本的に、取り込まれるデータの量に基づいて請求されます。 

> [!NOTE]
> レガシ価格レベルの名前が変更されました。 Enterprise 価格レベルの新しい名前は **Per Node** に、Basic 価格レベルの新しい名前は **Per GB** となります。 Azure portal も含め、以下、これらの新しい名前を使用します。  

Per Node (旧 Enterprise) レベルは、料金がノード単位となっており、日単位のデータ利用分が各ノードに割り当てられます。 Per Node 価格レベルでは、含まれる利用分を超えて取り込まれたデータに対して課金されます。 Operations Management Suite を使用している場合は、Per Node レベルを選択する必要があります。 

お客様の通貨およびリージョンでの現在の価格については、「[Application Insights の価格](https://azure.microsoft.com/pricing/details/application-insights/)」をご覧ください。

> [!NOTE]
> 2018 年 4 月に、Azure Monitoring 用の新しい価格モデルを[導入](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/)しました。 このモデルでは、監視サービスのポートフォリオ全体で単純な "従量課金制" モデルを採用しています。 [新しい価格モデル](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs)の詳細、使用パターンに基づいて[このモデルへの移行の影響を評価](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#understanding-your-azure-monitor-costs)する方法、[新しいモデルを有効にする方法](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-usage-and-estimated-costs#azure-monitor-pricing-model)をご確認ください。

### <a name="per-node-tier-and-operations-management-suite-subscription-entitlements"></a>Per Node レベルと Operations Management Suite のサブスクリプションの権利

[以前の発表](https://blogs.technet.microsoft.com/msoms/2017/05/19/azure-application-insights-enterprise-as-part-of-operations-management-suite-subscription/)のとおり、Operations Management Suite E1 および E2 を購入したお客様は、追加コストなしで追加コンポーネントとして Application Insights Per Node を取得できます。 具体的には、Operations Management Suite E1 および E2 の各ユニットには、Application Insights Per Node レベル 1 ノード分の権利が含まれています。 Application Insights の各ノードは、追加コストなしで、1 日あたり最大 200 MB のデータを取り込み (Log Analytics のデータ インジェストを除く)、90 日間保持できます。 レベルについては、この記事で後ほど詳しく説明します。 

このレベルは Operations Management Suite サブスクリプションをお持ちのお客様だけに適用されるので、Operations Management Suite サブスクリプションをお持ちでないお客様にはこのレベルを選ぶオプションは表示されません。

> [!NOTE]
> この権利を取得するには、Application Insights リソースが Per Node 価格レベルである必要があります。 この権利は、ノードとしてのみ適用されます。 Per GB レベルの Application Insights リソースでは、メリットは実現されません。 この権利は、 **[使用量と推定コスト]** ウィンドウに表示される見積もりコストには表示されません。 また、サブスクリプションを 2018 年 4 月の新しい Azure Monitoring 価格モデルに移行した場合、使用可能なレベルは Per GB レベルだけです。 Operations Management Suite サブスクリプションをお持ちの場合、新しい Azure Monitoring 価格モデルへのサブスクリプションの移行はお勧めしません。

### <a name="how-the-per-node-tier-works"></a>Per Node レベルのしくみ

* Per Node レベルでは、アプリに対してテレメトリを送信するノードごとに料金が課金されます。
  * *ノード*とは、アプリをホストする物理または仮想サーバー マシン (または、サービスとしてのプラットフォーム (PaaS) ロール インスタンス) のことです。
  * 開発マシン、クライアントのブラウザー、およびモバイル デバイスはノードとしてカウントされません。
  * テレメトリを送信するコンポーネント (Web サービスやバックエンド ワーカーなど) がアプリに複数ある場合、それらは個別にカウントされます。
  * [ライブ メトリック ストリーム](../../azure-monitor/app/live-stream.md) データは、課金対象としてカウントされません。 サブスクリプション内では、料金はノード単位で計算されます (アプリ単位ではありません)。 12 のアプリに対して 5 つのノードがテレメトリを送信する場合、料金は 5 ノード分になります。
* 料金の見積りは月単位で計算されますが、実際の料金は、ノードがアプリからテレメトリを送信した時間分しか課金されません。 1 時間あたりの料金は、1 か月あたりの見積り額を 744 で割ったものです (1 か月 31 日の時間数)。
* 検出された各ノードに、1 日あたり 200 MB のデータ量の割り当てが (時間単位の精度で) 与えられます。 未使用のデータ割り当て分が日をまたいで繰り越されることはありません。
  * Per Node の価格レベルを選んだお客様には、テレメトリをサブスクリプション内の Application Insights リソースに送信するノードの数に基づいて、日単位のデータ利用分が提供されます。 したがって、終日データを送信する 5 つのノードがある場合、サブスクリプション内のすべての Application Insights リソースに 1 GB の許容量が適用されます。 無料データ利用分はすべてのノードで共有されるため、一部のノードのデータ送信量が他のノードより多くても問題はありません。 特定の日に、Application Insights リソースがサブスクリプションの日単位のデータ利用分を超えるデータを受信した場合は、1 GB ごとに超過データ料金が適用されます。 
  * 1 日あたりの無料データ利用分は、各ノードが 1 日にテレメトリを送信する時間数 (UTC を使用) を 24 で割った値に、200 MB を掛け合わせて計算します。 つまり、4 つのノードが、1 日 24 時間のうち 15 時間テレメトリを送信した場合、その日のデータ量は ((4 &#215; 15) / 24) &#215; 200 MB = 500 MB になります。 データ超過分 1 GB あたり 2.30 米国ドルの料金ですので、その日ノードが 1 GB のデータを送信した場合、超過データ料金は 1.15 米国ドルになります。
  * Per Node レベルの 1 日あたりのデータ利用分は、Per GB レベルが選択されたアプリケーションとは共有できません。 無料データの未使用分は、翌日への引き継ぎはできません。 

### <a name="examples-of-how-to-determine-distinct-node-count"></a>個別のノード カウントを決定する方法の例

| シナリオ                               | 日単位の合計ノード数 |
|:---------------------------------------|:----------------:|
| 1 つのアプリケーションで 3つの Azure App Service インスタンスと 1 つの仮想サーバーが使用されている。 | 4 |
| 3 つのアプリケーションが 2 つの VM で実行され、これらのアプリケーションの Application Insights リソースが、同じサブスクリプションの Per Node レベルにある。 | 2 | 
| 4 つのアプリケーションの Application Insights リソースが同じサブスクリプションにあり、各アプリケーションはピーク外の時間帯 16 時間は 2 つのインスタンス、ピーク時間帯 8 時間は 4 つのインスタンスを実行している。 | 13.33 | 
| Cloud Services に 1 つの Worker ロールと 1 つの Web ロールが含まれ、それぞれ 2 つのインスタンスを実行している。 | 4 | 
| 5 つのノードがある Azure Service Fabric クラスターが 50 のマイクロサービスを実行し、各マイクロサービスが 3 つのインスタンスを実行している。 | 5|

* 正確なノード カウントは、アプリケーションで使用している Application Insights SDK によって異なります。 
  * SDK バージョン 2.2 以降では、Application Insights [Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) と [Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) の両方が、各アプリケーション ホストをノードとして報告します。 例としては、物理サーバーと VM ホストのコンピューター名やクラウド サービスのインスタンス名があります。  唯一の例外は、[.NET Core](https://dotnet.github.io/) と Application Insights Core SDK のみを使用するアプリケーションです。 その場合は、ホスト名が使用できないためすべてのホストに対して 1 つのノードが報告されます。 
  * 以前のバージョンの SDK では、[Web SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) は、新しいバージョンの SDK と同じように動作しますが、[Core SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) は実際のアプリケーション ホストの数に関係なく 1 つのノードのみ報告します。 
  * アプリケーションで SDK を使用して**ロール インスタンス**をカスタム値に設定すると、既定ではノード カウントの決定に同じ値が使用されます。 
  * クライアント コンピューターやモバイル デバイスから実行されているアプリで新しいバージョンの SDK を使用している場合は、(クライアント コンピューターやモバイル デバイスの数が多いため) ノード カウントで大きい値が返される可能性があります。 

## <a name="automation"></a>Automation

Azure Resource Management を使用して、価格レベルを設定するスクリプトを記述することができます。 方法については、[こちら](powershell.md#price)をご覧ください。


## <a name="next-steps"></a>次のステップ

* [サンプリング](../../azure-monitor/app/sampling.md)

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[start]: ../../azure-monitor/app/app-insights-overview.md
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/
[pricing]: https://azure.microsoft.com/pricing/details/application-insights/
