---
title: Azure IoT Central でのアーキテクチャの概念 - エネルギー | Microsoft Docs
description: この記事では、Azure IoT Central エネルギー アプリ テンプレートのアーキテクチャに関連する主要な概念を紹介します。
author: op-ravi
ms.author: omravi
ms.date: 10/22/2019
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: abjork
ms.openlocfilehash: cab593990369bb8a4135e82534a2eb409b574e96
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/04/2019
ms.locfileid: "73582762"
---
# <a name="azure-iot-central---smart-meter-app-architecture"></a>Azure IoT Central - スマート メーター アプリのアーキテクチャ

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

この記事では、スマート メーター監視アプリ テンプレートのアーキテクチャの概要について説明します。 次の図は、IoT Central プラットフォームを使用した Azure 上のスマート メーター アプリに一般的に使用されるアーキテクチャを示しています。

> [!div class="mx-imgBorder"]
> ![スマート メーターのアーキテクチャ](media/concept-iot-central-smart-meter/smart-meter-app-architecture.png)

このアーキテクチャは、次のコンポーネントで構成されます。 一部のソリューションでは、ここに記載されているすべてのコンポーネントが必要ではない場合があります。

## <a name="smart-meters-and-connectivity"></a>スマート メーターと接続性 

スマート メーターは、すべてのエネルギー アセットの中でも最も重要なデバイスの 1 つです。 これを使ってエネルギー消費データを記録し、監視やその他のユース ケース (請求書や需要応答など) のために公共事業体に伝達できます。 メーターの種類に基づいて、ゲートウェイ、またはその他の中間デバイス (エッジ デバイス、ヘッドエンド システムなど) を介して IoT Central に接続できます。 直接接続できないデバイスを接続するには、IoT Central デバイス ブリッジを構築します。 IoT Central デバイス ブリッジはオープン ソース ソリューションであり、詳細については[こちら](https://docs.microsoft.com/azure/iot-central/core/howto-build-iotc-device-bridge)で確認できます。 


## <a name="iot-central-platform"></a>IoT Central プラットフォーム

Azure IoT Central は、IoT ソリューションの構築を簡素化し、IoT の管理、運用、開発の負担とコストを削減するために役立つプラットフォームです。 IoT Central を使用すると、大規模なモノのインターネット (IoT) アセットを簡単に接続、監視、管理できます。 スマート メーターを IoT Central に接続すると、アプリ テンプレートにはデバイス モデル、コマンド、ダッシュボードなどの組み込みの機能が使用されます。 また、アプリ テンプレートには、ほぼリアルタイムのメーター データの監視、分析、ルール、視覚化など、ウォーム パス シナリオにも IoT Central ストレージが使用されます。 


## <a name="extensibility-options-to-build-with-iot-central"></a>IoT Central を使用して構築する拡張オプション
IoT Central プラットフォームには、次の 2 つの拡張オプションがあります。継続的データ エクスポート (CDE) と API。 顧客とパートナーは、特定のニーズに合わせてこれらのオプションのいずれかを選択し、ソリューションをカスタマイズすることができます。 たとえば、あるパートナーは Azure Data Lake Storage (ADLS) を使用して CDE を構成しました。 バッチ処理、監査、報告などの目的で、長期的なデータ保持やその他のコールド パス ストレージ シナリオに ADLS が使用されています。 

## <a name="next-steps"></a>次の手順

* アーキテクチャについて学習したので、次は[スマート メーター アプリを無料で作成します](https://apps.azureiotcentral.com/build/new/smart-meter-monitoring)
* IoT Central の詳細については、[IoT Central の概要](https://docs.microsoft.com/azure/iot-central/)に関する記事を参照してください
