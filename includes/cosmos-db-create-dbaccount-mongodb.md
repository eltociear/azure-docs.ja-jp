---
title: インクルード ファイル
description: インクルード ファイル
services: cosmos-db
ms.custom: include file
ms.openlocfilehash: b1dcf395c55e91a98b237f6e3866c97d74b7a97c
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942848"
---
1. 新しいウィンドウで、[Azure Portal](https://portal.azure.com/) にサインインします。
2. 左側のメニューで、 **[リソースの作成]** 、 **[データベース]** の順に選択し、 **[Azure Cosmos DB]** の下にある **[作成]** を選択します。
   
   ![その他のサービスと Azure Cosmos DB が強調表示された Azure portal のスクリーンショット](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. **[Azure Cosmos DB アカウントの作成]** ページで、新しい Azure Cosmos DB アカウントの設定を入力します。 
 
    設定|値|[説明]
    ---|---|---
    サブスクリプション|該当するサブスクリプション|この Azure Cosmos DB アカウントに使用する Azure サブスクリプションを選択します。 
    リソース グループ|新規作成<br><br>その後、ID で指定したものと同じ一意の名前を入力|**[新規作成]** を選択します。 その後、自分のアカウントの新しいリソースグループの名前を入力します。 簡略化するには、ID と同じ名前を使用します。 
    アカウント名|一意の名前を入力します|自分の Azure Cosmos DB アカウントを識別するための一意の名前を入力します。 指定した ID に *mongo.cosmos.azure.com* が付加されて URI が作成されるので、一意の ID を使用してください。<br><br>ID に使用できるのは、英小文字、数字、ハイフン (-) のみです。 長さは 3 文字から 31 文字でなければなりません。
    API|Azure Cosmos DB の MongoDB 用 API|API によって、作成するアカウントの種類が決まります。 Azure Cosmos DB には、5 種類の API が用意されています。ドキュメント データベース用の Core(SQL)、グラフ データベース用の Gremlin、ドキュメント データベース用の Azure Cosmos DB の MongoDB 用 API、Azure Table、Cassandra です。 現在は、API ごとに別のアカウントを作成する必要があります。 <br><br>このクイックスタートでは MongoDB に対応するコレクションを作成するので、**MongoDB** を選択します。|
    Location|ユーザーに最も近いリージョンを選択|Azure Cosmos DB アカウントをホストする地理的な場所を選択します。 データに最も高速にアクセスできる、ユーザーに最も近い場所を使用します。
    Version|3.6|MongoDB ワイヤ プロトコル バージョン 3.6 を選択します。下位互換性を確保するには、3.2 を選択します。

    **[確認および作成]** を選択します。 **[ネットワーク]** セクションと **[タグ]** セクションはスキップできます。 

    ![Azure Cosmos DB の新しいアカウント ページ](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. アカウントの作成には数分かかります。 ポータルに "**Your Cosmos account with wire protocol compatibility for MongoDB is ready (MongoDB に対してワイヤ プロトコルの互換性がある Cosmos アカウントの準備ができました)** " ページが表示されるまで待ちます。

    ![Azure Portal の [通知] ウィンドウ](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
