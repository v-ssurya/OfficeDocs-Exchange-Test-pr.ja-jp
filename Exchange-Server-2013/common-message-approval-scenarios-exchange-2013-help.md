﻿---
title: '一般的なメッセージの承認シナリオ: Exchange Online Help'
TOCTitle: 一般的なメッセージの承認シナリオ
ms:assetid: 5c13a07e-c21d-4502-a9f9-fb801197e1dd
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/Dd298007(v=EXCHG.150)
ms:contentKeyID: 49896268
ms.date: 05/22/2018
mtps_version: v=EXCHG.150
ms.translationtype: HT
---

# 一般的なメッセージの承認シナリオ

 

_**適用先:**Exchange Online, Exchange Server 2013_

組織では、法的またはコンプライアンス要件を満たすために、または特定のビジネス ワークフローを実装するために特定の種類のメッセージの承認が必要になることがあります。以下に、Exchange を使用してセットアップできる一般的なシナリオの例を示します。

  - 例 1: 大きな配布グループに対するメール ストームを回避する

  - 例 2: 承認用にメッセージを送信者の上司に転送する

  - 例 3: メッセージ承認チェーンのセットアップ

  - 例 4: いくつかの基準のいずれかと一致するメッセージの転送

  - 例 5: 機密情報を含むメッセージの転送

## 例 1: 大きな配布グループに対するメール ストームを回避する

大きな配布グループへのメッセージを制御するために、そのグループに送信されるメッセージを承認するようにモデレーターに要求することができます。　承認を必要とするメッセージに関する基準が存在しない場合、これをセットアップする最も簡単な方法は、メッセージの承認が必要なグループを構成することです。

この例では、送信者が Legal Team という配布グループのメンバーである場合を除き、All Employees グループに対するすべてのメッセージを承認する必要があります。

![配布グループのメッセージの承認の設定](images/Dd298007.77721509-93f9-4a90-8d77-986db2b0acf4(EXCHG.150).png "配布グループのメッセージの承認の設定")

特定の配布グループに対してメッセージの承認が必要になるようにするには、Exchange 管理センター (EAC) で、**\[受信者\]** \> **\[グループ\]** に移動し、配布グループを編集し、**\[メッセージの承認\]** を選択します。

## 例 2: 承認用にメッセージを送信者の上司に転送する

以下に、上司の承認が必要な場合があるメッセージの一般的なタイプを示します。

  - ユーザーから特定の配布グループまたは受信者に送信されるメッセージ

  - 外部ユーザーまたはパートナーに送信されるメッセージ

  - 2 つのグループ間で送信されるメッセージ

  - 特定の顧客の名前など、特定の内容で送信されるメッセージ

  - 研修中の社員によって送信されるメッセージ

承認用にメッセージを送信することが必要になるようにするには、まず **\[モデレーターにメッセージを送信する\]** テンプレートを使用してルールを作成し、次のスクリーンショットに示されているように、メッセージを送信者の上司に配信することを選択します。

![テンプレートを使用して新しいルールを作成する](images/Dd298007.051a5653-1a09-4db4-908f-48b56cc8d13f(EXCHG.150).png "テンプレートを使用して新しいルールを作成する")

次に、承認を必要とするメッセージを定義します。

Garth Fort という研修中の社員によって組織外の受信者に送信されるすべてのメッセージが上司の承認を必要とするという例を以下に示します。

![ユーザーの上司にメッセージを転送するルール](images/Dd298007.7f94c22e-b5ba-45a3-9ccd-31996b6c863a(EXCHG.150).png "ユーザーの上司にメッセージを転送するルール")

始めに、EAC \> **\[メール フロー\]** \> **\[ルール\]** に移動し、**\[モデレーターにメッセージを送信する\]** テンプレートを使用して新しいルールを作成します。


> [!IMPORTANT]
> 既定では、送信者の上司への転送など、一部の条件やアクションは <STRONG>[新しいルール]</STRONG> ページでは表示されません。すべての条件やアクションを表示するには、<STRONG>[その他のオプション]</STRONG> を選択します。



## 例 3: メッセージ承認チェーンのセットアップ

メッセージが複数の承認レベルを必要とするようにできます。たとえば、特定の顧客に対するメッセージが最初に顧客関係マネージャーによって承認され、続いてコンプライアンス責任者によって承認されることが必要になるようにできます。または、費用のレポートが 2 つのレベルのマネージャーによって承認されることが必要になるようにできます。

この種類の複数レベルの承認を作成するには、承認レベルごとに 1 つのトランスポート ルールを作成します。各ルールでは、以下のようにメッセージ内に同じパターンが存在します。

  - 最初のルールはメッセージを最初の承認者に転送します。最初の承認者がメッセージを受け入れると、メッセージは自動的に 2 番目のルールの承認者に配信されます。

  - 承認要求を受け取ったときにチェーン内のすべての承認者が **\[承認\]** を選択した場合、チェーン内の最後の承認が完了すると、元のメッセージは目的の受信者に送信されます。

  - 承認要求を受け取ったときに承認チェーン内のいずれかの承認者が **\[拒否\]** を選択した場合、送信者は拒否メッセージを受け取ります。

  - 承認要求のいずれかが有効期限内 (Exchange Online の場合は 2 日、Exchange Server 2013 の場合は 5 日) に承認されなかった場合、送信者は有効期限切れメッセージを受け取ります。

以下の例では、Blue Yonder Airlines という顧客がおり、この顧客宛のすべてのメッセージを顧客関係マネージャーとコンプライアンス責任者の両方に承認してもらうということを前提とします。承認者ごとに 1 つずつ、合計 2 つのルールを作成します。最初のルールは最初のレベルの承認者に適用されます。2 番目のルールは 2 番目のレベルの承認者に適用されます。

![承認の 2 つのレベルに使用される 2 つのルール](images/Dd298007.29686c05-eaa0-42b9-86ad-d577f656392c(EXCHG.150).png "承認の 2 つのレベルに使用される 2 つのルール")

最初のルールは、件名またはメッセージ内に会社名 Blue Yonder Airlines が含まれるすべてのメッセージを識別し、これらのメッセージを Blue Yonder Airlines を担当する内部顧客関係マネージャー Garret Vargas に送信します。

![最初のレベルの承認者のルール](images/Dd298007.e22d1c04-85c5-4227-88e6-b118d5593350(EXCHG.150).png "最初のレベルの承認者のルール")

2 番目のルールは、これらのメッセージをコンプライアンス責任者 Tony Krijnen に送信します。

![同じ条件の、第 2 レベルの承認のルール](images/Dd298007.5d888786-8e48-4459-ab86-8a4b9a016d58(EXCHG.150).png "同じ条件の、第 2 レベルの承認のルール")

## 例 4: いくつかの基準のいずれかと一致するメッセージの転送

トランスポート ルール内では、ルールと一致するにはルール内で構成されたすべての条件を満たしている必要があります。同じアクションをいずれかの条件を満たした場合に適用するには、条件ごとに個別のルールを作成する必要があります。

これを行うには、EAC の **\[ルール\]** ページで、最初の条件に対してルールを作成します。次に、ルールを選択して **\[コピー\]** を選択し、新しいルールの条件を変更して 2 番目の条件と一致させます。

「OR」条件を指定した複数のルールを作成する場合には、メッセージが承認者に複数回送信されることがないように注意してください。これを回避するには、最初のルールと一致したメッセージが無視されるように 2 番目のルールに例外を追加します。

たとえば、1 つのルールではメッセージの件名または添付ファイルに「販売見積」が含まれているかどうか確認できません。メッセージが承認者に複数回送信されないようにするため、件名またはメッセージの本文に「販売見積」があるかどうかを最初のルールで確認する場合は、添付ファイルの内容に「販売見積」があるかどうかを確認する 2 番目のルールでは最初のルールの基準を例外に含めることが必要です。

![2 番目のルールの例外を使用する](images/Dd298007.c39bbdcf-c619-4f84-8922-114ad1da824d(EXCHG.150).png "2 番目のルールの例外を使用する")


> [!NOTE]
> 既定では、例外は <STRONG>[新しいルール]</STRONG> ページでは表示されません。すべての条件やアクションを表示するには、<STRONG>[その他のオプション]</STRONG> を選択します。



## 例 5: 機密情報を含むメッセージの転送

[データ損失防止](technical-overview-of-dlp-data-loss-prevention-in-exchange.md)(DLP) 機能がある場合、多くの種類の機密情報が事前定義されています。DLP を使用すると、メッセージに機密情報条件が含まれている場合に検知できます。DLP の有無にかかわらず、組織に固有な特定の機密情報パターンを識別する条件を作成できます。

以下に、機密情報を含むメッセージが承認を必要とする例を示します。この例では、クレジット カード番号が含まれるメッセージで承認が必要です。

![機密情報を含むメールを転送するルール](images/Dd298007.7ec1ca74-5d20-42ea-a9ee-3a8b25beb7df(EXCHG.150).png "機密情報を含むメールを転送するルール")

## 関連項目


[メッセージ承認の管理](manage-message-approval-exchange-2013-help.md)

