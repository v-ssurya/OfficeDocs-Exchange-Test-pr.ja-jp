﻿---
title: '名前空間の不整合の DNS サフィックス検索一覧を構成する: Exchange 2013 Help'
TOCTitle: 名前空間の不整合の DNS サフィックス検索一覧を構成する
ms:assetid: cfa715ac-7b69-47c3-b206-933ec2cf677b
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/Bb847901(v=EXCHG.150)
ms:contentKeyID: 49896484
ms.date: 04/24/2018
mtps_version: v=EXCHG.150
ms.translationtype: HT
---

# 名前空間の不整合の DNS サフィックス検索一覧を構成する

 

_**適用先:**Exchange Server 2013_

_**トピックの最終更新日:**2016-12-09_

ここでは、グループ ポリシー管理コンソール (GPMC) を使用してドメイン ネーム システム (DNS) サフィックス検索一覧を構成する方法について説明します。Microsoft Exchange 2013 の一部のシナリオでは、名前空間の不整合がある場合、複数の DNS サフィックスを含むように DNS サフィックス検索一覧を構成する必要があります。

## 始める前に把握しておくべき情報

  - 予想所要時間 : 10 分

  - この手順を実行するには、使用するアカウントに Domain Admins グループのメンバーシップが委任されている必要があります。

  - GPMC をインストールするコンピューター上に, .NET Framework 3.0 がインストールされていることを確認します。
    

    > [!NOTE]
    > 現在 Microsoft ダウンロード センターからダウンロードできる GPMC の最新バージョンは、32 ビット バージョンの Windows Server 2003 および Windows XP オペレーティング システムで動作し、32 ビットおよび 64 ビットのドメイン コントローラーにあるグループ ポリシー オブジェクトをリモートで管理できます。このバージョンの GPMC には 64 ビット バージョンは含まれておらず、32 ビット バージョンは 64 ビット プラットフォームでは実行できません。32 ビット バージョンの Windows Server 2008&nbsp;と 32 ビット バージョンの Windows Vista&nbsp;のいずれにも、32 ビット バージョンの GPMC が含まれています。64 ビット バージョンの Windows Server 2008&nbsp;と 64 ビット バージョンの Windows Vista&nbsp;のいずれにも、64 ビット バージョンの GPMC が含まれています。



  - このトピックの手順で使用可能なキーボード ショートカットについては、「[Exchange 管理センターのキーボード ショートカット](keyboard-shortcuts-in-the-exchange-admin-center-exchange-online-protection-help.md)」を参照してください。


> [!TIP]
> 問題がある場合は、Exchange のフォーラムで質問してください。 次のフォーラムにアクセスしてください。<A href="https://go.microsoft.com/fwlink/p/?linkid=60612">Exchange Server</A>、 <A href="https://go.microsoft.com/fwlink/p/?linkid=267542">Exchange Online</A>、 または <A href="https://go.microsoft.com/fwlink/p/?linkid=285351">Exchange Online Protection</A>。



## GPMC を使用して DNS サフィックス検索一覧を構成するには

1.  ドメイン内の 32 ビット コンピューターに、GPMC with Service Pack 1 (SP1) をインストールします。ダウンロードについては、「[Group Policy Management Console with Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=100126)」を参照してください。
    

    > [!NOTE]
    > ドメインに Windows Server 2008&nbsp;または Windows Vista を実行しているコンピューターがある場合、この手順を省略できます。



2.  **\[スタート\]** \> **\[プログラム\]** \> **\[管理ツール\]** \> **\[グループ ポリシーの管理\]** の順にクリックします。

3.  **\[グループ ポリシーの管理\]** で、グループ ポリシーを適用するフォレストとドメインを展開します。**\[グループ ポリシー オブジェクト\]** を右クリックし、**\[新規作成\]** をクリックします。

4.  **\[新しい GPO\]** で、ポリシーの名前を入力し、**\[OK\]** をクリックします。

5.  手順 4. で作成した新しいポリシーを右クリックし、**\[編集\]** をクリックします。

6.  **\[グループ ポリシー管理エディター\]** で、**\[コンピューターの構成\]**、**\[ポリシー\]**、**\[管理用テンプレート\]**、**\[ネットワーク\]** を順に展開し、**\[DNS クライアント\]** をクリックします。

7.  **\[DNS サフィックス検索一覧\]** を右クリックし、**\[すべてのタスク\]** をクリックした後、**\[編集\]** をクリックします。

8.  **\[DNS サフィックス検索一覧のプロパティ\]** ページで、**\[有効\]** を選択します。**\[DNS サフィックス\]** ボックスに、不整合なコンピューターのプライマリ DNS サフィックス、DNS ドメイン名、および Exchange が相互運用する可能性がある他のサーバー (監視サーバーやサード パーティ アプリケーション用のサーバーなど) の追加の名前空間を入力します。**\[OK\]** をクリックします。

9.  **\[グループ ポリシーの管理\]** で、**\[グループ ポリシー オブジェクト\]** を展開し、手順 4 で作成したポリシーを選択します。**\[スコープ\]** タブで、不整合のあるコンピューターのみに適用されるようにポリシーの範囲を設定します。

## 正常な動作を確認する方法

移行が正常に完了したことを確認するには、次の手順を実行します。

  - Exchange 2013 をインストールしたら、組織の内部および外部に電子メール メッセージを送信できることを確認します。

## 詳細情報

[Windows Server Group Policy](https://go.microsoft.com/fwlink/p/?linkid=100128)

[グループ ポリシー](https://go.microsoft.com/fwlink/?linkid=268043)

[名前空間の不整合のシナリオ](disjoint-namespace-scenarios-exchange-2013-help.md)

