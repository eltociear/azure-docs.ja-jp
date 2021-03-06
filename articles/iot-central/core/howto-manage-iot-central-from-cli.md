---
title: Azure CLI から IoT Central を管理する | Microsoft Docs
description: この記事では、CLI を使用して IoT Central アプリケーションを作成し、管理する方法について説明します。 CLI を使用して、アプリケーションの表示、変更、および削除を行えます。
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 1051ea91378cc2e2facec7e34f6d303297b91ce8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75454065"
---
# <a name="manage-iot-central-from-azure-cli"></a>Azure CLI から IoT Central を管理する

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central アプリケーション マネージャー](https://aka.ms/iotcentral) Web サイト上で IoT Central アプリケーションを作成および管理するのではなく、[Azure CLI](/cli/azure/) を使用してアプリケーションを管理できます。

## <a name="prerequisites"></a>前提条件

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

ローカル コンピューター上で Azure CLI を実行する場合は、「[Azure CLI のインストール](/cli/azure/install-azure-cli)」をご覧ください。 Azure CLI をローカルで実行する場合は、この記事にあるコマンドを試行する前に、**az login** コマンドを使用して Azure にサインインします。

## <a name="create-an-application"></a>アプリケーションの作成

[az iotcentral app create](/cli/azure/iotcentral/app#az-iotcentral-app-create) コマンドを使用して、Azure サブスクリプション内に IoT Central アプリケーションを作成します。 次に例を示します。

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku S1 --template "iotc-demo@1.0.0" \
  --display-name "My Custom Display Name"
```

これらのコマンドでは、最初に、米国東部の場所にアプリケーション用のリソース グループを作成します。 次の表では、**az iotcentral app create** コマンドに使用されるパラメーターを説明しています。

| パラメーター         | [説明] |
| ----------------- | ----------- |
| resource-group    | そのアプリケーションを含むリソース グループ。 サブスクリプションにこのリソース グループが既に存在している必要があります。 |
| location          | このコマンドでは既定で、リソース グループの場所が使用されます。 現時点では、IoT Central アプリケーションは、**米国**、**オーストラリア**、**アジア太平洋**、または **ヨーロッパ** の場所で作成できます。 |
| name              | Azure portal 内のアプリケーションの名前。 |
| subdomain         | アプリケーションの URL のサブドメイン。 この例では、アプリケーションの URL は https://mysubdomain.azureiotcentral.com です。 |
| sku               | 現在使用できる値は **S1** (Standard レベル) のみです。 「[Azure IoT Central の価格](https://azure.microsoft.com/pricing/details/iot-central/)」を参照してください。 |
| template          | 使用するアプリケーション テンプレート。 詳細については、後の表を参照してください。 |
| display-name      | UI に表示されるアプリケーションの名前。 |

**一般公開されている機能を備えたアプリケーション テンプレート**

| テンプレート名            | [説明] |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | 独自のデバイス テンプレートおよびデバイスにデータを入力するための空のアプリケーションを作成します。


**パブリック プレビュー機能を備えたアプリケーション テンプレート**

| テンプレート名            | [説明] |
| ------------------------ | ----------- |
| iotc-pnp-preview@1.0.0   | 独自のデバイス テンプレートとデバイスにデータを入力するための空のプラグ アンド プレイ プレビュー アプリケーションを作成します。 |
| iotc-condition@1.0.0     | ストア内分析 (条件監視テンプレート) を含むアプリケーションを作成します。 このテンプレートを使用して、ストア環境に接続して監視します。 |
| iotc-consumption@1.0.0   | 水消費量監視テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、水流の監視と制御を行います。 |
| iotc-distribution@1.0.0  | デジタル配布テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、主要な資産と作業をデジタル化することで、倉庫からの出荷の効率を向上させます。 |
| iotc-inventory@1.0.0     | スマート インベントリ管理テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、入荷、製品の移動、循環棚卸、およびセンサーの追跡を自動化します。 |
| iotc-logistics@1.0.0     | 接続された物流テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、場所と条件を監視して、航空輸送、船舶輸送、および陸上輸送での出荷をリアルタイムで追跡します。 |
| iotc-meter@1.0.0         | スマート メーター監視テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、エネルギー消費量やネットワークの状態を監視し、カスタマー サポートとスマート メーターの管理を向上させるための傾向を識別します。  |
| iotc-patient@1.0.0       | 患者の継続的なモニタリング テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、患者の治療、再入院、および病名を管理します。 |
| iotc-power@1.0.0         | ソーラー パネル監視テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、ソーラー パネルの状態やエネルギー生成の傾向を監視します。 |
| iotc-quality@1.0.0       | 水質監視テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、水質をデジタルに監視します。|
| iotc-store@1.0.0         | ストア内分析 (チェックアウト テンプレート) を備えたアプリケーションを作成します。 このテンプレートを使用して、ストア内のチェックアウト フローの監視と管理を行います。 |
| iotc-waste@1.0.0         | 接続された廃棄物管理テンプレートを含むアプリケーションを作成します。 このテンプレートを使用して、ゴミ箱を監視し、現場作業員を派遣します。 |

> [!NOTE]
> 現在、プレビュー アプリケーション テンプレートは、**ヨーロッパ**と**米国**のリージョンでのみ利用できます。

## <a name="view-your-applications"></a>アプリケーションを表示する

[az iotcentral app list](/cli/azure/iotcentral/app#az-iotcentral-app-list) コマンドを使用して、IoT Central アプリケーションを一覧表示し、メタデータを表示します。

## <a name="modify-an-application"></a>アプリケーションの変更

[az iotcentral app update](/cli/azure/iotcentral/app#az-iotcentral-app-update) コマンドを使用して、IoT Central アプリケーションのメタデータを更新します。 アプリケーションの表示名を変更する場合の例を次に示します。

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>アプリケーションの削除

[az iotcentral app delete](/cli/azure/iotcentral/app#az-iotcentral-app-delete) コマンドを使用して、IoT Central アプリケーションを削除します。 次に例を示します。

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>次のステップ

ここでは、Azure CLI から Azure IoT Central アプリケーションを管理する方法について説明しました。推奨される次の手順は以下のとおりです。

> [!div class="nextstepaction"]
> [アプリケーションを管理する](howto-administer.md)
