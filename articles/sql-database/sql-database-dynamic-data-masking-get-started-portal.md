---
title: Azure portal:動的データ マスク
description: Azure Portal での SQL Database 動的データ マスクの使用方法
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 03/04/2018
ms.openlocfilehash: a8098b31c6b389b640fc03e756da44c70d9f3a70
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722120"
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Azure Portal で SQL Database 動的データ マスクを使用する

この記事では、Azure Portal で[動的データ マスク](sql-database-dynamic-data-masking-get-started.md)を実装する方法を示します。 [Azure SQL Database コマンドレット](https://docs.microsoft.com/powershell/module/az.sql/)または [REST API](https://docs.microsoft.com/rest/api/sql/) を使って動的データ マスクを実装することもできます。

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Azure Portal を使用してデータベースの動的データ マスクを設定する

1. Azure Portal ([https://portal.azure.com](https://portal.azure.com)) を開きます。
2. マスクする機微なデータを含むデータベースの設定ページに移動します。
3. **[動的データ マスク]** タイルをクリックして、 **[動的データ マスク]** 構成ページを起動します。

   * この方法に代わって、下にスクロールして **[操作]** セクションを表示し、 **[動的データ マスク]** をクリックすることもできます。

     ![ナビゲーション ウィンドウ](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)

4. **動的データ マスク**構成ページには、推奨エンジンがマスク対象として推奨したデータベース列がいくつか表示される場合があります。 推奨を受け入れるには、1 つまたは複数の列の **[マスクの追加]** をクリックします。その列の既定タイプに基づきマスクが作成されます。 マスク機能を変更できます。その場合、マスク ルールをクリックし、マスク フィールド形式を別の形式に変更します。 必ず **[保存]** をクリックして設定を保存します。

    ![ナビゲーション ウィンドウ](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)

5. データベースの列にマスクを追加するには、 **[動的データ マスク]** 構成ページの一番上にある **[マスクの追加]** をクリックし、 **[マスク ルールの追加]** 構成ページを開きます。

    ![ナビゲーション ウィンドウ](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)

6. **[スキーマ]** 、 **[テーブル]** 、 **[列]** を選択し、マスクする指定のフィールドを定義します。
7. 機密データのマスク カテゴリの一覧から **[マスク フィールド形式]** を選択します。

    ![ナビゲーション ウィンドウ](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)

8. データ マスク ルール ページの **[保存]** をクリックして、動的データ マスク ポリシーのマスク ルールのセットを更新します。
9. マスクから除外し、マスクされていない機密データへのアクセスを与える SQL ユーザーまたは AAD の ID を入力します。 ユーザーをセミコロンで区切った一覧にします。 管理者特権を持つユーザーは常にマスクされていない元のデータにアクセスできます。

    ![ナビゲーション ウィンドウ](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    > [!TIP]
    > アプリケーションの特権を持つユーザーに対してアプリケーション レイヤーがデリケートなデータを表示できるようにするには、アプリケーションストアでデータベースの照会に使用される SQL ユーザーまたは AAD の ID を追加します。 デリケートなデータの公開を最小限に抑えるには、この一覧に含める特権ユーザーの数を最小限にすることを強くお勧めします。

10. データ マスク構成ページの **[保存]** をクリックして、新しいまたは更新されたマスク ポリシーを保存します。

## <a name="next-steps"></a>次のステップ

* 動的データ マスクの概要については、「[動的データ マスク](sql-database-dynamic-data-masking-get-started.md)」をご覧ください。
* [Azure SQL Database コマンドレット](https://docs.microsoft.com/powershell/module/az.sql/)または [REST API](https://docs.microsoft.com/rest/api/sql/) を使って動的データ マスクを実装することもできます。
