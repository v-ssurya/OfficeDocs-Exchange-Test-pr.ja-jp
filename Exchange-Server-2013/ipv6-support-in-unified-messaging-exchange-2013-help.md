﻿---
title: 'ユニファイド メッセージングでの IPv6 サポート: Exchange 2013 Help'
TOCTitle: ユニファイド メッセージングでの IPv6 サポート
ms:assetid: 91242c85-ce4e-422a-954e-ab623d3d6939
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/JJ150536(v=EXCHG.150)
ms:contentKeyID: 48269788
ms.date: 04/24/2018
mtps_version: v=EXCHG.150
ms.translationtype: HT
---

# ユニファイド メッセージングでの IPv6 サポート

 

_**適用先:**Exchange Server 2013, Exchange Server 2016_

_**トピックの最終更新日:**2015-04-07_

インターネット プロトコル Version 6 (IPv6) は、インターネット プロトコル (IP) の最新バージョンです。IPv6 は、以前のバージョンの IP である IPv4 の多くの短所を取り除くことを目的としています。Microsoft Exchange Server 2010 では、IPv6 がサポートされるのは、IPv4 も使用されている場合のみです。純粋な IPv6 Exchange 環境はサポートされていません。IPv6 アドレスおよび IP アドレスの範囲は、Exchange 2010 を実行しているコンピューターで IPv6 と IPv4 の両方が有効化されており、ネットワークで両方の IP アドレス バージョンがサポートされている場合にのみ使用できます。ただし、IPv4 と IPv6 はまったく異なるプロトコルであるため、IPv4 ネットワークは IPv6 ネットワークと直接通信することができず、その逆も同様です。この短所に対処するために、ネットワーク管理者は IPv4 ネットワークと IPv6 ネットワーク間で情報をルーティングできる、ルーターなどのデバイスを展開する必要があります。Exchange 2010 が IPv4 と IPv6 の両方を使用して展開されている場合、ユニファイド メッセージング (UM) を除くすべてのサーバーの役割は、IPv6 アドレスを使用するデバイス、サーバー、およびクライアントとの間でデータを送受信できます。Exchange 2013 では、ユニファイド メッセージングは、Exchange 2007 および Exchange 2010 のトランスポート サーバーの役割、クライアント アクセス サーバーの役割、メールボックス サーバーの役割のような独立したサーバーの役割ではありません。UM 関連のコンポーネントおよび音声認識サービスは、クライアント アクセス サーバーとメールボックス サーバーでのみ実行されます。

Exchange 2013 では、UM アーキテクチャが変更されており、IPv4 と IPv6 および他の Exchange 機能をサポートするために Unified Communications Managed API (UCMA) v4.0 を必要とします。このため、ユニファイド メッセージングのコンポーネントおよびサービスがあるクライアント アクセス サーバーとメールボックス サーバーの両方は、IPv6 ネットワークを完全にサポートします。

## IPv6 サポート

Exchange 2010 Service Pack 1 (SP1) 以降、ユニファイド メッセージング サーバーの役割は、基盤となるセッション開始プロトコル (SIP) 通知および音声認識処理に関して UCMA 2.0 に依存していました。UCMA 2.0 は UM の音声認識機能の主要コンポーネントです。UCMA 2.0 には、SIP スタック、メディア スタック、および自動音声認識 (ASR) と音声合成 (TTS) により生成された音声合成に対応する音声エンジンが含まれています。

Exchange 2010 では、UM に UCMA 2.0 が必要でしたが IPv4 だけがサポートされ、IPv6 はサポートされていませんでした。このため、UM を除くすべてのサーバーの役割にデュアル スタック (IPv4 と IPv6) の実行が必要でした。Exchange 2013 の場合、UCMA 4.0 が UM で使用され、Exchange 2013 をクライアント アクセス サーバーとメールボックス サーバーにインストールするために必要になります。UCMA 4.0 は、新機能および IPv6 をサポートするために必要です。

Exchange 2013 の新機能 (IPv6 など) をサポートするために現在 UM で UCMA 4.0 を使用するいくつかの理由は以下のとおりです。

  - いくつかの政府機関では、使用している製品に IPv6 のサポートが必要である。

  - 現在、UM は、デュアル スタック (IPv4 と IPv6) または IPv6 のみを実行する、ルーター、IP ゲートウェイ、IP PBX、セッション ボーダー コントローラー (SBC) などのハードウェア デバイスとの互換性を必要とする。

  - Exchange 2013 では、Microsoft Exchange ユニファイド メッセージング サービスをメールボックス サーバー上で実行し、Microsoft Exchange ユニファイド メッセージング呼び出しルーター サービスをクライアント アクセス サーバー上で実行する。また、Exchange 2013 のメールボックス サーバーの役割とクライアント アクセス サーバーの役割で IPv4 と IPv6 の両方が必要である。

  - オンライン サービスにより、クライアントは IPv4 または IPv6 を使用してそのサービスに接続できる。

  - パブリック IPv4 アドレス空間が枯渇しつつある。Exchange Server 2013 Enterprise の場合、UM は常にプライベート IPv4 アドレス空間を使用して展開できる内部 SIP ピアと通信するため、これは UM にとって実際上問題にならない。ただし、ホストされている Exchange UM の場合、ユーザーの機器は IPv6 を使用してホストされている UM に接続する必要がある。

UM とトランスポートの一部分を除き、クライアント アクセス サーバーまたはメールボックス サーバーが IPv4 と IPv6 を使用できるデュアル スタック モードで実行されると、Exchange 2013 は 組織内の Exchange 2010 サーバーに接続できます。つまり、ユーザーは IPv4 と IPv6 の両方のスタック アドレスが構成されて実行されているコンピューターに、Exchange 2013 をインストールできます。これにより、IPv6 クライアント、その他の Exchange サーバー (Exchange Server 2010 を含む) は Exchange 2013 に直接接続できます。

UM は、デュアル スタック モードで実行している Windows サーバーで動作します。HTTP などのプロトコルではトランスポートの種類が無視され、UM では相互に依存しない VoIP (Voice over IP) プロトコル (SIP/RTP/STUN/TURN/ICE を含む) が使用されるためです。これには、UM が IP アドレスの一覧を SIP ピア (IP ゲートウェイ、IP PBX、SBC など) にアドバタイズおよび通知する、メディア ネゴシエーション (RTP/SRTP) が含まれています。

## UM 用に IPv6 をサポートするとは、どういうことですか?

Exchange 2013 UM が IPv6 をサポートできるようにするには、Enterprise と Online の両方の UM 管理者が、UM を IPv6 対応デバイス (ルーター、IP ゲートウェイ、IP PBX、Office Communications Server 2007 R2 および Microsoft Lync Server など) に接続するときに、IPv6 を活用できる必要があります。ただし、以前のバージョンの Exchange との相互運用性と後方互換性のために IPv6 を使用できない場合、管理者は追加の構成変更を行う必要はなく、代わりに IPv4 を使用できます。

Exchange 2013 Enterprise では、ソフトウェアとファームウェアで IPv6 をサポートしない SIP ピア (IP ゲートウェイ、IP PBX、および SBC) に対して、UM は直接通信する必要があります。このため、UM は、IPv4 をサポートしている SIP ピア、さらに重要な IPv6 と直接通信できる必要があります。ホストされている Exchange 2013 では、UM は SBC、Lync Server 2010、または Lync Server 15 を使用してユーザーの機器と通信します。ホスト型の Exchange 2013 環境では、SBC、Lync Server などの IPv6 SIP 対応クライアントを展開する可能性が高いため、IPv6 から IPv4 への変換処理を行います。

## IPv6 用の UM デバイスのサポート

UM コンポーネントおよびサービスを実行する Exchange 2013 のメールボックス サーバーおよびクライアント アクセス サーバーは IPv6 をサポートするため、IP ゲートウェイ、IP PBX、および SBC のベンダーも IPv6 をサポート可能にする必要があります。IPv6 用のデバイスのサポートに影響を与えるいくつかの問題があります。

  - IPv6 をサポートできる IP ゲートウェイ、IP PBX、および SBC はありますが、まだ IPv6 と UM を使用してテストされていません。将来、このサポートが追加されることもありますが、ハードウェア ベンダーに依存しています。

  - いくつかの IP ゲートウェイには、現在 IPv6 のサポートがありません。

  - 一部の SBC には IPv4-IPv6 機能がありますが、SRTP (Secure Real-time Transport Protocol)/SDES (Session Description Protocol Security) をサポートしていないため、現在 UM に対しては機能しません。

  - デュアル スタックと純粋な IPv6 をサポートしていない IP PBX ハードウェアはありますが、これらのデバイスと Exchange 2013 との動作はテストされていません。

現在 UCMA 4.0 では IPv6 が有効です。つまり、IPv6 接続を受け入れ可能ですが、デュアル モードでの操作時または送信接続時には、IPv4 も受け入れ可能です。デュアル モードでの実行では、以前のバージョンの Exchange UM に接続する必要がある場合、IPv4 接続が可能です。Lync インストールの場合、この操作は、Active Directory から最新バージョンの Exchange Server のバージョン情報を取得する Lync Server によって実行されます。IP ゲートウェイ、IP PBX、SBC などの従来のテレフォニー デバイスの場合、IPv4 と共に IPv6 接続をサポートするには、両方の種類の接続をリッスンする必要があります。これは、各 SIP ピアが以前のバージョンの Exchange UM との後方互換性のために、両方の種類の接続を受け入れ可能である必要があるためです。これは、両方の種類の接続に対する送信ダイヤルにも必要です。

## IPv6 をサポートするための UM 構成

クライアント アクセス サーバーとメールボックス サーバーをインストールしたら、ユニファイド メッセージングのダイヤル プラン、自動応答、IP ゲートウェイ、およびハント グループを作成する必要があります。UM で IPv6 をサポートできるようにするには、次の手順を実行する必要があります。

  - ネットワーク上の IP ゲートウェイ、IP PBX、または SBC のそれぞれの IPv6 アドレスを使用して、新規の UM IP ゲートウェイを作成するか、既存の UM IP ゲートウェイを構成します。必要な UM IP ゲートウェイを作成および構成している場合、UM IP ゲートウェイの IPv6 アドレスまたは完全修飾ドメイン名 (FQDN) を追加する必要があります。FQDN を UM IP ゲートウェイに追加している場合、UM IP ゲートウェイの FQDN を IPv6 アドレスに解決する適切な DNS レコードを作成しておく必要があります。既存の UM IP ゲートウェイがある場合、**Set-UMIPgateway** コマンドレットを使用すると、IPv6 アドレスまたは FQDN を構成できます。UM IP ゲートウェイを作成または構成したら、**Get-UMIPgateway** コマンドレットを使用して UM IP ゲートウェイのプロパティを表示し、IPv6 設定が正しいことを確認できます。

  - 各 UM IP ゲートウェイで *IPAddressFamily* パラメーターを構成します。IP ゲートウェイで IPv6 パケットを受け入れられるようにするには、UM IP ゲートウェイを IPv4 と IPv6 の両方の接続を受け入れるか、IPv6 接続のみ受け入れるように設定する必要があります。それには、**Set-UMIPgateway** コマンドレットを使用して、*IPAddressFamily* パラメーターを次のいずれかに設定します。
    
      - *IPv4* – これは既定値で、他の値が構成されていない場合に使用されます。
    
      - *IPv6* - これは IPv6 を使用可能にします。ただし、IPv4 は使用されません。
    
      - *Any* – これは IPv6 を使用可能にしますが、デバイスが IPv6 をサポートしていない場合、代わりに IPv4 が使用されます。

  - UM IP ゲートウェイを構成したら、IPv6 をサポートするように、ネットワーク上の IP ゲートウェイ、IP PBX、および SBC も構成する必要があります。詳細については、IPv6 をサポートしているデバイスの一覧およびそれらの正しい構成方法についてハードウェア ベンダーにお問い合わせください。

  - クライアント アクセス サーバーまたはメールボックス サーバーが IPv4 トラフィックの受信のみを行うように設定されている場合、必要に応じて、そのいずれかのサーバーで IPv6 トラフィックを受け入れるように設定しなければならない場合があります。ただし、既定の設定では、Microsoft Exchange ユニファイド メッセージング呼び出しルーター サービスを実行しているクライアント アクセス サーバーと Microsoft Exchange Unified Messaging サービスを実行しているメールボックス サーバーの両方で IPv4 トラフィックと IPv6 トラフィックを受け入れます。クライアント アクセス サーバーとメールボックス サーバーでの IPv6 設定の詳細については、「[Set-UMCallRouterSettings](https://technet.microsoft.com/ja-jp/library/jj215758\(v=exchg.150\))」と「[Set-UMService](https://technet.microsoft.com/ja-jp/library/jj552412\(v=exchg.150\))」を参照してください。
    
    IPv6 をサポートするため、クライアント アクセス サーバーとメールボックス サーバーで *IPAddressFamily* と *IPAddressFamilyConfigurable* の 2 つのパラメーターを構成する必要がある場合があります。クライアント アクセス サーバーとメールボックス サーバーで IPv6 パケットを受け入れることができるようにするには、IPv4 と IPv6 の両方の接続または IPv6 のみの接続を受け入れるようにクライアント アクセス サーバーとメールボックス サーバーを設定する必要があります。*IPAddressFamily* パラメーターを構成するには、*IPAddressFamilyConfigurable* パラメーターを `$true` に設定する必要があります。

## UM IP アドレス指定ロジック

Exchange 2013 での UM の IPv6 サポートのロジックは次のとおりです。

  - デュアル スタックが有効で、クライアント アクセス サーバーとメールボックス サーバーが *IPv6* または *Any* に設定されている場合、クライアント アクセス サーバーとメールボックス サーバーは IPv4 と IPv6 の両方のインターフェイスをリッスンします。それ以外の場合は、IPv4 のみが使用されます。

  - 送信呼び出しの場合、UM IP ゲートウェイ、クライアント アクセス サーバー、およびメールボックス サーバーの *IPAddressFamily* パラメーターが *IPv6* または *Any* が設定されている場合、UM ではデュアル モードが使用されます。それ以外の場合は、IPv4 のみが使用されます。

デュアル モードで送信呼び出しを行うときに *IPAddressFamily* パラメーターが *IPv6* または *Any* に設定されている場合、

  - UCMA は、到達しようとする SIP ピアの FQDN 内のアドレスの一覧を取得します。

  - UCMA はすべての IPv6 アドレス (存在する場合) を試みます。

  - UCMA がアドレスを使用不可と判断すると、そのアドレスを一覧に含め、構成済みの間隔に基づいて再度試行しません。これで UM が既知の不良アドレスを不必要に再試行することを回避できます。

  - 使用可能な IPv6 アドレスがない場合、UCMA は SIP ピアのアドレス一覧の IPv4 アドレスにフォールバックします。

