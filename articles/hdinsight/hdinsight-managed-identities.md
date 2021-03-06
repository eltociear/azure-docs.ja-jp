---
title: Azure HDInsight のマネージド ID
description: Azure HDInsight でのマネージド ID の実装の概要を説明します。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/20/2019
ms.openlocfilehash: 934c99c25ca37526ac31fd9bbaf58623fdbdf166
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76764396"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Azure HDInsight のマネージド ID

マネージド ID とは、資格情報が Azure によって管理されている Azure Active Directory (Azure AD) に登録されている ID です。 マネージド ID を使用すると、Azure AD でサービス プリンシパルを登録したり、証明書などの資格情報を保持したりする必要はありません。

Azure HDInsight では、必要に応じて、マネージド ID を使用して Azure AD ドメイン サービスにアクセスしたり、 Azure Data Lake Storage Gen2 のファイルにアクセスしたりします。

マネージド ID には、ユーザー割り当てとシステム割り当ての 2 種類があります。 Azure HDInsight では、ユーザー割り当てマネージド ID が使用されます。 ユーザー割り当てマネージド ID は、スタンドアロン Azure リソースとして作成されます。その後、これを 1 つ以上の Azure サービス インスタンスに割り当てることができます。 これに対して、システム割り当てマネージド ID は Azure AD 内で作成され、自動的に特定の Azure サービス インスタンスで直接有効化されます。 システム割り当てマネージド ID の有効期間は、そのマネージド ID が有効になっているサービス インスタンスの有効期間に関連付けられます。

## <a name="hdinsight-managed-identity-implementation"></a>HDInsight のマネージド ID 実装

Azure HDInsight では、マネージド ID はクラスターのノードごとにプロビジョニングされます。 ただし、これらの ID コンポーネントは HDInsight サービスのみが使用できます。 現在、HDInsight クラスター ノードにインストールされているマネージド ID を使用してアクセス トークンを生成するためのサポートされている方法はありません。 一部の Azure サービスでは、マネージド ID は、ユーザーが他の Azure サービスと独自にやり取りするためのアクセス トークンの取得に使用できるエンドポイントで実装されます。

## <a name="create-a-managed-identity"></a>マネージド ID の作成

マネージド ID は、次のいずれかの方法で作成できます。

* [Azure Portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

マネージド ID を構成するための残りの手順は、その ID が使用されるシナリオによって異なります。

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Azure HDInsight におけるマネージド ID のシナリオ

マネージド ID は、Azure HDInsight のさまざまなシナリオで使用されます。 セットアップと構成の詳細な手順については、次の関連ドキュメントをご覧ください。

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-assigned-managed-identity)
* [Enterprise セキュリティ パッケージ](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Kafka Bring Your Own Key (BYOK)](kafka/apache-kafka-byok.md#get-started-with-byok)

## <a name="next-steps"></a>次のステップ

* [Azure リソースのマネージド ID とは](../active-directory/managed-identities-azure-resources/overview.md)
