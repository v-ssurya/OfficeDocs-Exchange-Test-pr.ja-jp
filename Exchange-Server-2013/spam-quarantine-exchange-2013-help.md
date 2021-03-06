﻿---
title: 'スパム検疫: Exchange 2013 Help'
TOCTitle: スパム検疫
ms:assetid: 4535496f-de6a-43df-8e53-c9a97f65cccc
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/Aa997692(v=EXCHG.150)
ms:contentKeyID: 49896226
ms.date: 05/23/2018
mtps_version: v=EXCHG.150
ms.translationtype: MT
---

# スパム検疫

 

_**適用先:**Exchange Server 2013_

_**トピックの最終更新日:**2012-10-12_

多くの組織では、法規制上の要件から、正当なすべての電子メール メッセージを保存または配信する義務があります。Microsoft Exchange Server 2013 のスパム検疫は、正当なメッセージの消失の危険を減らすためのコンテンツ フィルター エージェントの機能です。スパム検疫は、スパムと識別され、組織内のユーザーのメールボックスに配信されないようにする必要があるメッセージの一時的な格納場所を提供します。

コンテンツ フィルター エージェントによってスパムと識別されたメッセージは、配信不能レポート (NDR) にラップされ、組織内のスパム検疫メールボックスに配信されます。スパム検疫メールボックスに配信されるメッセージを管理し、適切な操作を実行します。たとえば、メッセージを削除したり、スパム対策フィルターで誤ってスパムとフラグが付けられたメッセージを本来の宛先にルーティングすることができます。さらに、指定された時間の経過後にスパム検疫メールボックスがメッセージを自動的に削除するように構成できます。

**目次**

Spam confidence level

Spam quarantine

## Spam Confidence Level

外部のユーザーが、スパム対策機能を実行する Exchange を実行しているサーバーに電子メール メッセージを送信した場合、スパム対策機能はメッセージの特性を累積的に評価し、次のように対処します。

  - スパムであると疑われるメッセージをフィルターで排除します。

  - メッセージがスパムかどうかの確率に基づいて、レベルが割り当てられます。このレベルは、SCL (Spam Confidence Level) レベルと呼ばれるメッセージ プロパティとしてメッセージに格納されます。

スパム検疫は、SCL レベルを使用して、メールがスパムである可能性を判断します。SCL レベルは 0 から 9 の数値です。0 はスパムである可能性が最も低いことを、9 はスパムである可能性が最も高いことを意味します。

特定の SCL レベルが割り当てられたメールについて、削除、拒否、または検疫を行うように構成できます。これらのいずれか処理を起動するレベルを、*SCL による検疫のしきい値*と呼びます。コンテンツ フィルターの処理で、SCL による検疫のしきい値に基づいてコンテンツ フィルター エージェントが処理を行うように構成できます。たとえば、次のような条件を設定できます。

  - SCL による削除のしきい値を 8 に設定する。

  - SCL による拒否のしきい値を 7 に設定する。

  - SCL による検疫のしきい値を 6 に設定する。

  - SCL による迷惑メール フォルダーのしきい値を 5 に設定する。

上記の SCL しきい値に基づき、SCL が 6 の電子メールがスパム検疫メールボックスに配信されます。

詳細については、「[コンテンツ フィルターの管理](manage-content-filtering-exchange-2013-help.md)」を参照してください。

ページのトップへ

## スパム検疫

既定のスパム対策エージェントすべてを実行している Exchange サーバーによってメッセージが受信されると、コンテンツ フィルターが次のように適用されます。

  - SCL レベルが、SCL による検疫のしきい値以上で、SCL による削除のしきい値や SCL による拒否のしきい値よりも小さい場合、メッセージはスパム検疫メールボックスに送られます。

  - SCL レベルがスパム検疫のしきい値よりも小さい場合、メッセージは受信者の受信トレイに配信されます。

メッセージ管理者は、Microsoft Outlook を使用してスパム検疫メールボックスを監視し、誤ってスパムと判断されたメッセージがないかどうかを確認します。誤ってスパムと判断されたメッセージが見つかった場合、管理者は受信者のメールボックスにメッセージを送信できます。

メッセージ管理者は、以下のいずれかの条件に当てはまる場合、スパム対策スタンプを確認できます。

  - フィルターによって誤ってスパムと判断され、スパム検疫メールボックスに送られるメッセージの数が多すぎる。

  - 拒否または削除されるスパムの数が十分ではない。

詳細については、「[スパム対策スタンプ](anti-spam-stamps-exchange-2013-help.md)」を参照してください。

組織に送られるスパムに対して、より正確にフィルターが適用されるように SCL 設定を調整できます。詳細については、「[Spam Confidence Level のしきい値](spam-confidence-level-threshold-exchange-2013-help.md)」を参照してください。

スパム検疫を使用するには、以下の手順を実行する必要があります。

1.  コンテンツ フィルターが有効になっていることを確認します。

2.  スパム検疫のために専用のメールボックスを作成します。

3.  スパム検疫メールボックスを指定します。

4.  SCL による検疫のしきい値を構成します。

5.  スパム検疫メールボックスを管理します。

6.  必要に応じて、SCL による検疫のしきい値を調整します。

詳細な手順については、「[スパム検疫メールボックスの構成](configure-a-spam-quarantine-mailbox-exchange-2013-help.md)」を参照してください。

ページのトップへ

