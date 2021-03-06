﻿---
title: 'Windows Server バックアップを使用して Exchange のバックアップを復元する: Exchange 2013 Help'
TOCTitle: Windows Server バックアップを使用して Exchange のバックアップを復元する
ms:assetid: 2d0f31dc-eb32-451a-8852-591269026506
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/Dd876864(v=EXCHG.150)
ms:contentKeyID: 48269310
ms.date: 05/23/2018
mtps_version: v=EXCHG.150
ms.translationtype: MT
---

# Windows Server バックアップを使用して Exchange のバックアップを復元する

 

_**適用先:**Exchange Server 2013_

_**トピックの最終更新日:**2014-06-18_

Windows Server Backup を使用して、Exchange データベースのバックアップと復元を行うことができます。Exchange には、ボリューム シャドウ コピー サービス (VSS) ベースで Exchange データのバックアップの作成と復元ができる Windows Server Backup のプラグインが組み込まれています。詳細については、「[Windows Server バックアップを使用した Exchange データのバックアップと復元](using-windows-server-backup-to-back-up-and-restore-exchange-data-exchange-2013-help.md)」を参照してください。

## 始める前に把握しておくべき情報

  - 予想所要時間 : 1 分にデータの復元にかかる時間を合計した時間

  - この手順を実行する際には、あらかじめアクセス許可を割り当てる必要があります。必要なアクセス許可の一覧については、以下を参照してください。「[受信者のアクセス許可](recipients-permissions-exchange-2013-help.md)」の「メールボックス回復」。

  - Windows Server Backup 機能はローカル コンピューターにインストールする必要があります。

  - データベースを元の場所に復元する場合、データベースをダーティ シャットダウン状態のままにして、システムによってマウントできるようにできます。(回復用のデータベースを使用する準備などで) 代わりの場所に復元する場合は、Exchange サーバー データベース ユーティリティ (Eseutil.exe) を使用して手動でクリーン シャットダウン状態にする必要があります。


> [!TIP]
> 問題がある場合は、Exchange のフォーラムで質問してください。 次のフォーラムにアクセスしてください。<A href="https://go.microsoft.com/fwlink/p/?linkid=60612">Exchange Server</A>、 <A href="https://go.microsoft.com/fwlink/p/?linkid=267542">Exchange Online</A>、 または <A href="https://go.microsoft.com/fwlink/p/?linkid=285351">Exchange Online Protection</A>。.



## Windows Server バックアップを使用して Exchange のバックアップを復元する

1.  Windows Server Backup を起動します。

2.  **\[ローカル バックアップ\]** を選択します。

3.  操作ウィンドウで **\[回復...\]** をクリックすると、回復ウィザードが開始します。

4.  **\[作業の開始\]** ページで、以下のいずれかの操作を行います。
    
      - 回復するデータをローカル サーバーにバックアップした場合は、**\[このサーバー (サーバー名)\]** を選択してから **\[次へ\]** をクリックします。
    
      - 回復するデータが他のサーバーからのデータである場合、または回復するバックアップが別のコンピューターにある場合は、**\[別のサーバー\]** を選択し、**\[次へ\]** をクリックします。**\[バックアップの場所の種類指定\]** ページで、**\[ローカル ドライブ\]** または **\[リモート共有フォルダー\]** を選択し、**\[次へ\]** をクリックします。**\[ローカル ドライブ\]** を選択した場合は、**\[バックアップの場所の選択\]** ページでバックアップを含むドライブを選択し、**\[次へ\]** をクリックします。**\[リモート共有フォルダー\]** を選択した場合は、**\[リモート フォルダーの指定\]** ページでバックアップ データの UNC パスを入力し、**\[次へ\]** をクリックします。

5.  **\[バックアップの日付の選択\]** ページで、回復するバックアップの日時を選択し、**\[次へ\]** をクリックします。

6.  **\[回復の種類の選択\]** ページで、**\[アプリケーション\]** を選択し、**\[次へ\]** をクリックします。
    

    > [!NOTE]
    > 選択肢として<STRONG>[アプリケーション]</STRONG> が使用できない場合は、復元用に選択したバックアップはフォルダー レベルのバックアップで、ボリューム レベルのバックアップではないことが示されます。Windows Server Backup で Exchange データをバックアップする場合は、ボリューム レベルでバックアップを実行する必要があります。



7.  **\[アプリケーションの選択\]** ページで、Exchange が **\[アプリケーション\]** フィールドで選択されていることを確認します。**\[詳細の表示\]** をクリックして、バックアップのアプリケーション コンポーネントを表示します。回復するバックアップが最新のものである場合は、**\[アプリケーション データベースのロールフォワード回復を実行しない\]** チェック ボックスが表示されます。コミットしていないトランザクション ログをすべてコミットすることで、Windows Server Backup が回復するデータベースをロール フォワードしないようにするには、このチェック ボックスをオンにします。**\[次へ\]** をクリックします。

8.  **\[回復オプションの指定\]** ページで、データの回復先の場所を次のように指定し、**\[次へ\]** をクリックします。
    
      - Exchange データを直接元の場所に復元する場合は、**\[元の場所に回復する\]** を選択します。このオプションを使用する場合、どのデータベースを復元するかを選択することはできません。ボリューム上でバックアップされたデータベースはすべて元の場所に復元されます。
    
      - 個別のデータベースおよびファイルを指定の場所に回復する場合は、**\[別の場所に回復する\]** を選択します。別の場所を指定するには、**\[参照\]** をクリックします。このオプションを使用すると、どのデータベースを復元するかを選択できます。回復したデータ ファイルは、その後回復用データベースに移動したり、手動で元の場所に移動したり、[データベースの移植性](database-portability-exchange-2013-help.md) を使用して Exchange 組織内の別の場所にマウントすることができます。データベースを代替の場所に回復する場合、回復したデータベースはダーティ シャットダウン状態になります。復元処理が完了したら、Eseutil.exe を使用してデータベースを手動でクリーン シャットダウン状態にする必要があります。

9.  **\[確認\]** ページで、回復の設定を確認し、**\[回復\]** をクリックします。

10. **\[回復の進行状況\]** ページで、回復操作の状態と進行状況を確認できます。

11. 回復操作が完了したら、**\[閉じる\]** をクリックします。

## 正常な動作を確認する方法

**\[回復の進行状況\]** ページに、回復処理が正常に完了したかどうかが示されます。さらにデータを正常に復元できたことを確認するには、次のいずれかを実行します。

  - バックアップ先のディレクトリを調べ、復元されたデータがあることを確認します。

  - Windows Server バックアップを実行したサーバーで、バックアップ ログを表示して、ジョブが正常に完了したことを確認します。

  - イベント ビューアーを開いて、復元完了イベントがアプリケーション イベント ログに記録されたことを確認します。

