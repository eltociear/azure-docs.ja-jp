---
title: インクルード ファイル
description: インクルード ファイル
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/16/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8c82e3d2c978a7980d02bc686346acdcb21dd591
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/31/2019
ms.locfileid: "66425753"
---
|  | **ポイント対サイト** | **サイト間** | **ExpressRoute** |
| --- | --- | --- | --- |
| **Azure でサポートされるサービス** |Cloud Services および Virtual Machines |Cloud Services および Virtual Machines |[サービス一覧](../articles/expressroute/expressroute-faqs.md#supported-services) |
| **一般的な帯域幅** |ゲートウェイの SKU に基づく |一般的には 1 Gbps 未満のアグリゲート |50 Mbps、100 Mbps、200 Mbps、500 Mbps、1 Gbps、2 Gbps、5 Gbps、10 Gbps |
| **サポート対象プロトコル** |Secure Sockets Tunneling Protocol (SSTP)、OpenVPN、IPsec |IPsec |VLAN、NSP の VPN テクノロジー (MPLS、VPLS など) 経由の直接接続 |
| **ルーティング** |RouteBased (動的) |ポリシー ベース (静的ルーティング) およびルート ベース (動的ルーティング VPN) がサポートされます。 |BGP |
| **接続の弾力性** |アクティブ/パッシブ |アクティブ/パッシブまたはアクティブ/アクティブ |アクティブ/アクティブ |
| **一般的な使用例** |プロトタイプの作成、クラウド サービスおよび仮想マシンの開発/テスト/ラボ シナリオ |クラウド サービスおよび仮想マシンの開発/テスト/ラボ シナリオおよび小規模実稼動ワークロード |すべての Azure サービス (検証済み一覧)、エンタープライズクラスおよびミッション クリティカルなワークロード、バックアップ、ビッグ データ、DR サイトとしての Azure へのアクセス |
| **SLA** |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |[SLA](https://azure.microsoft.com/support/legal/sla/) |
| **料金** |[料金](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[料金](https://azure.microsoft.com/pricing/details/vpn-gateway/) |[料金](https://azure.microsoft.com/pricing/details/expressroute/) |
| **テクニカル ドキュメント** |[VPN Gateway のドキュメント](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[VPN Gateway のドキュメント](https://azure.microsoft.com/documentation/services/vpn-gateway/) |[ExpressRoute のドキュメント](https://azure.microsoft.com/documentation/services/expressroute/) |
| **FAQ** |[VPN Gateway に関する FAQ](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[VPN Gateway に関する FAQ](../articles/vpn-gateway/vpn-gateway-vpn-faq.md) |[ExpressRoute の FAQ](../articles/expressroute/expressroute-faqs.md) |
