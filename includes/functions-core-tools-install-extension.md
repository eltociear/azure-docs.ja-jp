---
title: インクルード ファイル
description: インクルード ファイル
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 94cac0932da5880e5e7b8a8fac3870b5bc464af9
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/31/2019
ms.locfileid: "75564775"
---
## <a name="register-extensions"></a>拡張機能を登録する

HTTP およびタイマートリガーを除き、ランタイム バージョン 2.x およびそれ以降の関数バインディングは、拡張パッケージとして実装されます。 Azure Functions ランタイム バージョン 2.x では、関数で使用するバインディングの型に対応した拡張機能を明示的に登録する必要があります。 この例外は HTTP バインドとタイマー トリガーで、これらは拡張機能を必要としません。

バインド拡張機能を個別にインストールするか、host.json プロジェクト ファイルに拡張機能のバンドルの参照を追加することができます。 拡張機能のバンドルは、複数のバインディングの種類を使用するときに、パッケージの互換性の問題を発生する可能性をなくします。 これはバインド拡張機能を登録するための推奨される方法です。 また、拡張機能のバンドルにより、.NET Core 2.x SDK をインストールする必要もなくなります。 

### <a name="extension-bundles"></a>拡張機能のバンドル

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

詳細については、「[Azure Functions バインド拡張機能を登録する](../articles/azure-functions/functions-bindings-register.md#extension-bundles)」を参照してください。 function.json ファイルへのバインドを追加する前に、host.json に拡張機能のバンドルを追加する必要があります。

### <a name="register-individual-extensions"></a>個々の拡張機能を登録する

バンドル内にない拡張機能をインストールする必要がある場合は、特定のバインド用の個々の拡張機能パッケージを手動で登録できます。 

> [!NOTE]
> `func extensions install` を使用して拡張機能を手動で登録するには、.NET Core 2.x SDK がインストールされている必要があります。

関数に必要なすべてのバインドを含むように *function.json* ファイルを更新した後、プロジェクト フォルダーで以下のコマンドを実行します。

```bash
func extensions install
```

コマンドは、*function.json* ファイルを読み取って必要なパッケージを確認して、パッケージをインストールして、拡張プロジェクトを再構築します。 現在のバージョンで新しいバインドが追加されますが、既存のバインドは更新されません。 新しいバージョンをインストールするときに、`--force` オプションを使用して既存のバインドを最新バージョンに更新します。
