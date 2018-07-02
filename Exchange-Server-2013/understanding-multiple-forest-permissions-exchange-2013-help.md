﻿---
title: '複数フォレストのアクセス許可について: Exchange 2013 Help'
TOCTitle: 複数フォレストのアクセス許可について
ms:assetid: 8241033f-e201-4799-b17c-4f120c6e6445
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/Dd298099(v=EXCHG.150)
ms:contentKeyID: 49896341
ms.date: 04/24/2018
mtps_version: v=EXCHG.150
ms.translationtype: HT
---

# 複数フォレストのアクセス許可について

 

_**適用先:** Exchange Server 2013_

_**トピックの最終更新日:** 2015-04-07_

多くの組織では、組織内でセキュリティ境界を作成するために複数のフォレストを展開します。管理者は複数のフォレストを使用することで、組織内でリソースにアクセスする人数を最小限に抑えたり、事業部をセグメント化したりするなど、自身の要件に適合するようにこれらのセキュリティ境界を定義できます。

Microsoft Exchange Server 2013 では 2 種類の複数フォレスト トポロジがサポートされています。

  - **クロスフォレスト**   クロスフォレストのトポロジでは複数のフォレストを存在させることができます。それぞれのフォレストは独自に Exchange をインストールしています。

  - **リソース フォレスト**   リソース フォレストのトポロジでは、1 つの Exchange フォレストと 1 つ以上のアカウント フォレストがあります。

このトピックでは、アカウント フォレストまたはその他のリソース フォレストのうち、ユニバーサル セキュリティ グループ (USG) および Exchange 2013 がインストールされているフォレスト外のユーザーを含むフォレストを、外部フォレストと呼びます。

複数のフォレスト トポロジでアクセス許可を構成するには、リンクされるメールボックスの作成時に、フォレストの信頼およびグローバル アドレス一覧 (GAL) の同期を正しく構成する必要があります。Exchange 2013 フォレストは、リンクされた役割グループに関連付けられる USG およびリンクされたメールボックスに関連付けられるユーザーを含む外部フォレストを信頼する必要があります。

Exchange 2013 では、役割ベースのアクセス制御 (RBAC) アクセス許可モデルが使用されます。管理者がメンバーとなっている管理役割グループ、およびエンド ユーザーが割り当てられている管理役割割り当てポリシーによって、各管理者およびエンド ユーザーが実行できる範囲が決定されます。複数フォレストのアクセス許可について理解するには、RBAC について理解しておく必要があります。RBAC、役割グループ、および役割割り当てポリシーの詳細については、以下のトピックを参照してください。

  - [役割ベースのアクセス制御について](understanding-role-based-access-control-exchange-2013-help.md)

  - [管理役割グループについて](understanding-management-role-groups-exchange-2013-help.md)

  - [管理役割の割り当てポリシーについて](understanding-management-role-assignment-policies-exchange-2013-help.md)

アクセス許可の管理に関連する管理タスクについては、「[アクセス許可](permissions-exchange-2013-help.md)」を参照してください。

**目次**

複数のフォレスト トポロジでのアクセス許可

境界間のアクセス許可

境界間のアクセス許可の構成

## 複数のフォレスト トポロジでのアクセス許可

RBAC によって 1 つのフォレスト内のすべての Exchange オブジェクトにアクセス許可が適用されます。各フォレスト内の RBAC 構成は、他のすべてのフォレストから独立して構成されます。1 つのフォレストに役割グループを作成する場合、その役割グループは他のフォレスト内には存在せず、その役割グループが付与するアクセス許可は、役割グループが作成されたフォレストに対してのみ適用されます。たとえば、メールボックス作成のアクセス許可を付与する役割グループのメンバーは、その役割グループを含むフォレスト内にのみメールボックスを作成できます。

複数の Exchange フォレストがあり、各フォレスト内で同様にアクセス許可を構成する場合、各フォレストに同じ構成を明示的に適用する必要があります。たとえば、2 つの Exchange 2013 フォレストがあり、法務部門のアクセス許可を管理する "Compliance Management/コンプライアンス管理" 役割グループを作成する場合、次の手順を実行する必要があります。

  - 各フォレストで、Compliance Management という名前の役割グループを作成します。管理者がいずれかの Exchange フォレストとは別の外部フォレストにいる場合、両方の役割グループをリンクされた役割グループとして作成します。役割グループの詳細については、「境界間のアクセス許可」のセクションを参照してください。

  - 各フォレストで、新しい役割グループと使用する役割の間に、役割割り当てを作成します。

  - 新しい役割割り当ての一部として、各フォレスト内のサーバー オブジェクトと受信者オブジェクトを含む管理スコープをオプションで追加します。

  - リンクされた役割グループとして役割グループを作成した場合は、外部フォレストの関連付けられている USG にメンバーを追加します。

次の図では、Exchange 2013 フォレスト内で構成される役割グループが、個々のフォレストにどのように結合されているかについて示しています。Exchange 2013 フォレスト A の Organization Management という役割グループによって、同じフォレスト内のメールボックスおよびサーバーの管理だけを目的としたアクセス許可が付与されています。同様に、Exchange 2013 フォレスト B の役割グループによって、そのフォレスト内のメールボックスおよびサーバーに限定したアクセス許可が付与されています。

この図ではまた、カスタム役割グループ A が各フォレストに作成されています。これらは同じ名前で作成されていますが、それぞれ個別のエンティティを持っています。実際、この図が示すとおり、それぞれの役割グループに対して、各フォレストによって異なる管理役割を割り当てることができます。Exchange 2013 フォレスト A のカスタム役割グループ A にはメールボックス検索およびメッセージ追跡の役割が割り当てられており、Exchange 2013 フォレスト B のカスタム役割グループ A にはメールボックス検索および保有管理の役割が割り当てられています。

各フォレストで作成される管理スコープは、最終的にフォレストによって結合されます。各フォレストで作成されるサーバー スコープは、そのフォレストのメンバーであるサーバーしか含むことができません。サーバー スコープ A は、Exchange 2013 フォレスト A 内のサーバーしか含むことができず、サーバー スコープ B は、Exchange 2013 フォレスト B 内のサーバーしか含むことができません。同様に、Exchange 2013 フォレスト B の受信者スコープは、Exchange 2013 フォレスト B 内のメールボックスしか含むことができません。

**RBAC とフォレストのスコープ境界の関係**

![RBAC とフォレスト境界範囲の関係](images/Dd298099.220da7c3-cbb8-42ac-9d58-084d996bf837(EXCHG.150).gif "RBAC とフォレスト境界範囲の関係")

## 境界間のアクセス許可

RBAC が付与するアクセス許可によってユーザーが表示または変更できるのは、特定のフォレスト内の Exchange オブジェクトに限られます。ただし、フォレスト内の Exchange オブジェクトを表示および変更するためのアクセス許可を、フォレスト外のユーザーに対して付与することはできます。境界間のアクセス許可を使用することで、各フォレストに対して個別にタスク実行の認証を行わなくても、1 つのフォレスト内で Exchange 管理アカウントを集中管理できます。


> [!NOTE]
> ただし Exchange フォレスト外のユーザーに付与されるアクセス許可は、特定の Exchange フォレストに対してのみ適用されます。たとえば、外部フォレストのユーザーが、フォレスト A にある Organization Management というリンクされた役割グループのメンバーである場合、そのユーザーが管理できるのは、フォレスト A 内に含まれる Exchange オブジェクトのみです。ユーザーに各フォレストを管理するアクセス許可を付与するには、そのユーザーを各 Exchange フォレストのリンクされた役割グループのメンバーにする必要があります。



境界間のアクセス許可ではまた、そのメールボックスが Exchange フォレスト内にあり、そのユーザー アカウントがアカウント フォレスト内に存在するユーザーのメールボックスに、役割割り当てポリシーを適用することもできます。Exchange 2013 では、以降で説明するリンクされた役割グループとリンクされたメールボックスを使用して、境界間のアクセス許可がサポートされます。

## 管理用のアクセス許可

管理アクセス許可は、リンクされた役割グループおよびリンクされたメールボックスを使用して、フォレスト境界間で付与されます。

リンクされた役割グループは、Exchange 2013 組織内で作成され、外部フォレストのフォレスト境界間で USG にリンクされます。リンクされた役割グループのリンク先の USG は、次のいずれかになることができます。

  - リンクされた役割グループを特定の目的で使用するための専用 USG

  - 複数の Exchange 2013 フォレスト内のリンクされた役割グループにリンクする USG

  - 他の Exchange 2013 のフォレストの役割グループ USG

  - Exchange Server 2007 の管理者役割または Exchange 2010 の役割グループに関連付けられている USG

リンクされた役割グループのリンク先の USG は、他のフォレストにある必要があります。同じフォレスト内で、リンクされた役割グループと USG をリンクすることはできません。

次の図は、アカウント フォレスト内の USG が、1 つ以上の Exchange 2013 リソース フォレストの役割グループと関連付けできることを示しています。アカウント フォレスト内の USG のメンバーは、USG 経由で役割グループのメンバーとなります。

**他のフォレストの USG に関連付けられているリンクされた役割グループ**

![リンクされた役割グループと USG の関係](images/Dd298099.23f245e8-b80f-4082-8628-ee6701259be6(EXCHG.150).gif "リンクされた役割グループと USG の関係")

リンクされた役割グループを作成する際、Exchange 2013 フォレストのリンクされた役割グループに対して役割を割り当てます。リンクされた役割グループに対して役割を関連付ける割り当ては、必要に応じて管理スコープを含むことができます。これらのスコープは、リンクされた役割グループが作成されたフォレストに限定されます。

リンクされた役割グループのメンバーシップは、外部フォレストの USG との間で、互いにメンバーを追加および削除することで管理されます。この USG にメンバーを追加すると、リンクされた役割グループが Exchange 2013 フォレストで割り当てられているアクセス許可がメンバーに付与されます。複数のリンクされた役割グループを同じ USG にリンクした場合、その USG のメンバーに対しては、各 Exchange 2013 フォレスト内でリンクされている各役割グループに対して割り当てられているアクセス許可が付与されます。

Exchange 2013 フォレストから、リンクされた役割グループのメンバーシップを管理することはできません。

フォレスト境界間で管理アクセス許可を割り当てる 2 番目の方法は、リンクされたメールボックスを使用することです。別々の Exchange 2013 リソース フォレストで Exchange 2013 展開を使用するアカウント フォレスト内のユーザーの場合、リンクされたメールボックスを各ユーザーで構成する必要があります。リンクされたメールボックスは、Exchange 2013 フォレスト内の役割グループに、メンバーとして追加できます。リンクされたメールボックスが役割グループのメンバーとなると、そのリンクされたメールボックス、およびそのリンクされたメールボックスに関連付けられているアカウント フォレスト内のユーザーには、役割グループが提供するアクセス許可が付与されます。

次の図は、アカウント フォレスト内のユーザー、このユーザーに関連付けられているリンクされたメールボックス、およびこのユーザーがメンバーとなっている役割グループの間の関係を示しています。

**役割グループのメンバーである、リンクされたメールボックスに関連付けられているアカウント フォレスト内のユーザー**

![役割グループとリンクされたメールボックスの関係](images/Dd298099.e59242f6-5c63-4114-90f7-6b6c2478570c(EXCHG.150).gif "役割グループとリンクされたメールボックスの関係")

リンクされた役割グループおよびリンクされたメールボックスには共に、フォレスト境界間で管理アクセス許可の割り当てに使用する際の利点と欠点があります。次の表にそのうちの一部を示します。

### リンクされた役割グループおよびリンクされたメールボックスの利点と欠点

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>リンクされた役割グループまたはリンクされたメールボックス</th>
<th>利点</th>
<th>欠点</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>リンクされた役割グループ</p></td>
<td><p>アカウント フォレスト内または他の Exchange 2013 リソース フォレスト内の 1 つの USG に対して、複数の Exchange フォレストから、複数のリンクされた役割グループを関連付けることができます。これにより、1 つのフォレスト内の少数の USG によって、複雑な Exchange フォレスト トポロジを管理できます。</p></td>
<td><p>正規の役割グループをリンクされた役割グループに変更することはできません。リンクされた役割グループを手動で作成してから、フォレスト境界間で付与するアクセス許可を持つ、正規の各役割グループと置き換える必要があります。詳細については、「境界間のアクセス許可の構成」を参照してください。</p></td>
</tr>
<tr class="even">
<td><p>リンクされたメールボックス</p></td>
<td><p>リンクされたメールボックスによって、Exchange フォレスト内の既存の役割グループを使用できます。リンクされたメールボックスは、通常のメールボックス、USG、および同じ Exchange フォレスト内のユーザーに同様に、既存の役割グループにメンバーとして追加されます。</p></td>
<td><p>アカウント フォレスト内の 1 ユーザーにリンクされている、リンクされたメールボックスを使用して複数の Exchange 2013 フォレストでアクセス許可を付与する場合に、ユーザーに対して付与されるアクセス許可を変更する場合は、各 Exchange 2013 フォレストで役割グループのメンバーシップを変更する必要があります。</p></td>
</tr>
</tbody>
</table>


複数の Exchange リソース フォレストを持つ予定がある場合は、リンクされた役割グループを使用してフォレスト境界間でアクセス許可を付与することをお勧めします。

## エンド ユーザーのアクセス許可

役割割り当てポリシーを使用して、個々のメールボックスにエンド ユーザーのアクセス許可が割り当てられます。Exchange 2013 がリソース フォレストにインストールされている場合は、リンクされたメールボックスがリソース フォレストに作成され、アカウント フォレスト内のユーザー アカウントに関連付けられます。

リンクされたメールボックスが作成されると、そのメールボックスは、通常のメールボックスと同様に既定の役割割り当てポリシーに割り当てられます。役割割り当てポリシーによって、そのメールボックスに付与されるエンド ユーザーのアクセス許可が決定されます。これらのアクセス許可によって、ユーザーは以下の機能およびその他の機能に関連する設定を表示および変更できます。

  - エンド ユーザーのプロファイル情報

  - エンド ユーザーのボイスメール

  - エンド ユーザーの配布メンバーシップおよび所有権

役割割り当てポリシーがリンクされたメールボックスに割り当てられると、リンクされたメールボックスに関連付けられているアカウント フォレスト内のユーザーに対して、そのユーザーが使用できる機能を管理するためのアクセス許可が付与されます。このアクセス許可は、リンクされたメールボックスのある Exchange フォレスト内のリソースに対してのみ適用されます。次の図は、アカウント フォレスト内のエンド ユーザー、そのユーザーに関連付けられているリンクされたメールボックス、およびそのリンクされたメールボックスに割り当てられている役割割り当てポリシーの間の関係を示しています。また、アカウント フォレスト内の管理ユーザーに関連付けられているリンクされたメールボックスは、役割割り当てポリシーの他にも、複数の役割グループに関連付けることができます。

**それぞれに役割割り当てポリシーが割り当てられているリンクされたメールボックスに関連付けられているアカウント フォレスト内のユーザー**

![役割グループと割り当てポリシーの関係](images/Dd298099.785b9b35-4292-43ce-917b-117d0174742e(EXCHG.150).gif "役割グループと割り当てポリシーの関係")

ページのトップへ

## 境界間のアクセス許可の構成

複数のフォレスト トポロジで、境界間のアクセス許可を構成するには、外部フォレストの USG とリンクする各役割グループで、リンクされた役割グループを作成する必要があります。つまり、組み込みの役割グループそれぞれに対し、リンクされた役割グループを作成する必要があるということです。以下の操作を実行する必要があります。

1.  作成予定のリンクされている各役割グループに対し、外部フォレストで USG を作成します。アクセスを許可するメンバーをこの USG に追加します。

2.  組み込みの役割グループごとに、リンクされた役割グループを作成します。リンクされた役割グループを作成すると、以下の処理が行われます。
    
      - 組み込みの役割グループに割り当てられているものと同じ役割が、新規のリンクされた役割グループに割り当てられます。
    
      - リンクされた役割グループは、外部フォレストの USG に関連付けられます。

3.  作成したカスタム役割グループに対して、リンクされた役割グループを作成します。

4.  必要に応じて、新しいリンクされた役割グループにカスタム スコープを割り当てます。

これらの手順の実行方法の詳細については、以下のトピックを参照してください。

  - [組み込みの役割グループをミラーするリンクされた役割グループを作成する](create-linked-role-groups-that-mirror-built-in-role-groups-exchange-2013-help.md)

  - [リンクされた役割グループの管理](manage-linked-role-groups-exchange-2013-help.md)

  - [役割グループの管理](manage-role-groups-exchange-2013-help.md)

リンクされた役割グループと関連付けられている USG を変更する必要がある場合は、「[リンクされた役割グループの管理](manage-linked-role-groups-exchange-2013-help.md)」を参照してください。

リンクされたメールボックスが作成されると、役割割り当てポリシーに自動的に割り当てられます。リンクされたメールボックスに割り当てられている役割割り当てポリシーを変更するか、メールボックス作成時に既定で割り当てられる役割割り当てポリシーを変更できます。詳細については、以下のトピックを参照してください。

  - [メールボックスの割り当てポリシーを変更する](change-the-assignment-policy-on-a-mailbox-exchange-2013-help.md)

  - [役割の割り当てポリシーの管理](manage-role-assignment-policies-exchange-2013-help.md)
