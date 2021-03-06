﻿---
title: 'デジタル証明書と SSL: Exchange 2013 Help'
TOCTitle: デジタル証明書と SSL
ms:assetid: a9e2e08c-d46a-4135-a387-eb653212b676
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/Dd351044(v=EXCHG.150)
ms:contentKeyID: 48269896
ms.date: 05/23/2018
mtps_version: v=EXCHG.150
ms.translationtype: MT
---

# デジタル証明書と SSL

 

_**適用先:**Exchange Server 2013_

_**トピックの最終更新日:**2013-08-26_

SSL (Secure Sockets Layer) は、クライアントとサーバーの間の通信をセキュリティで保護するための方法です。Exchange Server 2013 では、SSL を使用して、サーバーとクライアント間の通信をセキュリティで保護します。クライアントには、携帯電話、組織のネットワーク内のコンピューター、および組織のネットワーク外のコンピューターがあります。

既定では、Exchange 2013 をインストールした場合、Outlook Web App、Exchange ActiveSync、および Outlook Anywhere を使用する際に、SSL を使用してクライアント通信が暗号化されます。

SSL では、デジタル証明書を使用する必要があります。ここでは、さまざまな種類のデジタル証明書の概要と、Exchange 2013 でこれらの種類のデジタル証明書を使用するように構成する方法について説明します。

**目次**

Overview of digital certificates

Digital certificates and proxying

Digital certificates best practices

## デジタル証明書の概要

デジタル証明書は、ユーザーやコンピューターの身元を確認するオンライン パスワードのような働きをする電子ファイルです。クライアント通信に使用する SSL 暗号化チャネルの作成に使用されます。証明書は、証明機関 (CA) によって発行されるデジタル ステートメントです。証明機関は証明書の保有者の身元を保証し、当事者が暗号化を使用して安全な方法で通信できるようにします。

デジタル証明書は次の 2 つの働きをします。

  - 証明書の保有者 (ユーザー、Web サイト、さらにルーターなどのネットワーク リソースまで) の身元が正しいことを証明します。

  - オンラインでやり取りされるデータの盗用や改ざんを防止します。

デジタル証明書は、信頼されているサード パーティ CA または Windows 公開キー基盤 (PKI) によって証明書サービスを使用して発行してもらうことも、自己署名入りの証明書を作成することもできます。証明書の種類によって、それぞれ長所と短所があります。どの種類のデジタル証明書にも改ざん防止策が施されており、偽造することはできません。

証明書はさまざまな目的で使用するために発行できます。これには、Web ユーザー認証、Web サーバー認証、S/MIME (Secure/Multipurpose Internet Mail Extensions)、インターネット プロトコル セキュリティ (IPsec)、トランスポート層セキュリティ (TLS)、およびコード署名などがあります。

証明書には公開キーが含まれ、その公開キーを対応する秘密キーを保持するユーザー、コンピューター、またはサービスの ID に結び付けます。公開キーと秘密キーは、送信される前のデータを暗号化するために、クライアントとサーバーで使用されます。Windows ベースのユーザー、コンピューター、およびサービスでは、信頼されたルート証明書ストアにルート証明書のコピーがあり、証明書に有効な証明のパスが含まれている場合に、CA に対する信頼が確立されます。証明書が有効であるためには、証明書が取り消されたり、有効期限が切れたりしていないことが必要です。

## 証明書の種類

主に、デジタル証明書には次の 3 種類があります。自己署名入り証明書、Windows PKI で生成された証明書、およびサード パーティ証明書です。

## 自己署名入りの証明書

Exchange 2013 をインストールすると、メールボックス サーバーで自己署名入りの証明書が自動的に構成されます。自己署名入りの証明書は、証明書を作成したアプリケーションによって署名されます。証明書のサブジェクトと名前が一致します。発行者とサブジェクトが証明書で定義されています。この自己署名入りの証明書は、クライアント アクセス サーバーとメールボックス サーバー間の通信を暗号化するために使用されます。クライアント アクセス サーバーはメールボックス サーバーの自己署名入りの証明書を自動的に信頼するため、メールボックス サーバーにサード パーティ証明書は不要です。Exchange 2013 をインストールすると、クライアント アクセス サーバーにも自己署名入りの証明書が作成されます。この自己署名入りの証明書によって、クライアント プロトコルで通信に SSL を使用できます。Exchange ActiveSync および Outlook Web App では、自己署名入りの証明書を使用して SSL 接続を確立できます。Outlook Anywhere では、クライアント アクセス サーバーの自己署名入りの証明書を処理できません。自己署名入りの証明書は、クライアント コンピューターまたはモバイル デバイス上の信頼されたルート証明書ストアに手動でコピーする必要があります。クライアントが SSL 経由でサーバーに接続し、サーバーが自己署名入りの証明書を提示する場合、クライアントはその証明書が信頼された機関によって発行されていることを確認するよう求められます。クライアントは、明示的に発行元の機関を信頼する必要があります。クライアントが信頼関係を確認すると、SSL 通信を続行できます。


> [!NOTE]
> 既定では、メールボックス サーバーにインストールされるデジタル証明書は、自己署名入りの証明書です。組織内のメールボックス サーバーの自己署名入りの証明書を、信頼されたサード パーティ証明書と置き換える必要はありません。クライアント アクセス サーバーはメールボックス サーバーの自己署名入りの証明書を自動的に信頼するため、メールボックス サーバーの証明書をさらに構成する必要はありません。



小規模な組織では、サードパーティの証明書を使用しなかったり、自分で証明書を発行するための独自の PKI をインストールしなかったりする場合がよくあります。これは、このようなソリューションにコストがかかり過ぎるか、管理者に独自の証明書階層を作成するための経験や知識が不足しているか、またはその両方の理由によるものです。自己署名入りの証明書を使用すると、コストは最小限で済み、セットアップも簡単です。その一方で、自己署名入りの証明書を使用する場合、証明書のライフサイクル管理、更新、信頼管理、および失効のインフラストラクチャの確立がはるかに難しくなります。

## Windows 公開キー基盤証明書

2 つめの種類の証明書は、Windows PKI によって生成された証明書です。PKI は、公開キー暗号を使用して、電子トランザクションにかかわる各当事者の正当性を確認および認証するデジタル証明書、証明機関 (CA)、および登録機関 (RA) のシステムです。Active Directory を使用する組織で PKI を実装すると、証明書のライフサイクル管理、更新、信頼管理、および失効のインフラストラクチャが実現します。ただし、Windows PKI によって生成された証明書を作成および管理するには、サーバーとインフラストラクチャの展開にある程度の追加コストがかかります。

Windows PKI を展開するには証明書サービスが必要であり、コントロール パネルの **\[プログラムの追加と削除\]** を使用してインストールできます。証明書サービスは、ドメイン内の任意のサーバーにインストールできます。

ドメインに参加している Windows CA から証明書を取得する場合は、CA を使用して証明書の要求または署名を行い、ネットワーク上のサーバーやコンピューターに発行することができます。これにより、サード パーティ証明書ベンダーと同じような PKI を、低コストで使用できます。このような PKI 証明書は、他の種類の証明書のように公に展開することはできません。ただし、PKI の CA が秘密キーを使用してリクエスターの証明書に署名すると、リクエスターが検証されます。この CA の公開キーは、証明書の一部になっています。信頼されたルート証明書ストアにこの証明書を持つサーバーは、その公開キーを使用して要求者の証明書を解読し、要求者を認証することができます。

PKI で生成された証明書を展開する手順は、自己署名入りの証明書の展開に必要な手順に似ています。Microsoft Exchange への SSL 接続を確立できるようにするコンピューターまたはモバイル デバイスの信頼されたルート証明書ストアに、信頼されたルート証明書のコピーを PKI からインストールする必要があります。

Windows PKI によって、組織では独自の証明書を発行できます。クライアントは、内部ネットワークの Windows PKI に証明書を要求して受け取ることができます。Windows PKI では、証明書を更新したり、取り消したりすることができます。

## 信頼されたサード パーティ証明書

サード パーティ (商用) 証明書は、サード パーティ (商用) CA によって生成される証明書で、購入してネットワーク サーバーで使用することができます。自己署名入りの証明書および PKI ベースの証明書の 1 つの問題は、証明書がクライアント コンピューターやモバイル デバイスによって自動的に信頼されないために、クライアント コンピューターやデバイスの信頼されたルート証明書ストアに証明書をインポートする必要があることです。サード パーティ (商用) 証明書には、このような問題はありません。ほとんどの商用 CA 証明書は、信頼されたルート証明書ストアに既に存在しているため、信頼された状態になっています。発行者が信頼されているので、証明書も信頼されます。サード パーティ証明書を使用すると、展開が大幅に簡略化されます。

大規模な組織や、証明書を公に展開する必要がある組織では、証明書に付随するコストこそ発生するものの、サード パーティ (商用) 証明書を使用するのが最良の方法です。小規模および中規模な組織では、商用証明書が最良の方法ではない場合があるため、それ以外のいずれかの証明書オプションを選択することもあります。

ページのトップへ

## 証明書の種類の選択

インストールする証明書の種類を選択するときには、いくつかの点について検討する必要があります。有効な証明書は署名されている必要があります。自己署名されている場合と、CA によって署名されている場合があります。自己署名入りの証明書には制限事項があります。たとえば、すべてのモバイル デバイスで、信頼されたルート証明書ストアにデジタル証明書をインストールできるわけではありません。モバイル デバイスに証明書をインストールできるかどうかは、モバイル デバイスの製造元およびモバイル サービス プロバイダーによって異なります。製造元やモバイル サービス プロバイダーによっては、信頼されたルート証明書ストアへのアクセスを無効にしている場合があります。この場合、自己署名入り証明書も Windows PKI CA からの証明書もモバイル デバイスにインストールできません。

## 既定の Exchange 証明書

既定では、Exchange がクライアント アクセス サーバーとメールボックス サーバーの両方に自己署名入りの証明書をインストールし、すべてのネットワーク通信が暗号化されます。すべてのネットワーク通信が暗号化されると、すべての Exchange サーバーに使用可能な X.509 証明書があることが必要となります。クライアント アクセス サーバーのこの自己署名入り証明書を、クライアントが自動的に信頼する証明書と置き換える必要があります。

「自己署名入り」とは、証明書が Exchange サーバー自体によってのみ作成および署名されたことを意味します。既定の自己署名入り証明書は、一般的に信頼されている CA によって作成および署名されていなかったため、同じ組織内の他の Exchange サーバー以外のソフトウェアからは信頼されません。既定の証明書は、すべての Exchange サービスで有効になっています。これには、インストール先の Exchange サーバーのサーバー名に対応するサブジェクトの別名 (SAN) が含まれます。また、サーバー名およびサーバーの完全修飾ドメイン名 (FQDN) の両方が含まれる SAN の一覧も含まれます。

Exchange 組織の他の Exchange サーバーはこの証明書を自動的に信頼しますが、Web ブラウザー、Outlook クライアント、携帯電話、およびその他の電子メール クライアントなどのクライアントと外部電子メール サーバーがこの証明書を自動的に信頼することはありません。そのため、この証明書を Exchange クライアント アクセス サーバーの信頼されているサード パーティ証明書と置き換えることを検討してください。自分の内部 PKI を持っており、すべてのクライアントがそのエンティティを信頼している場合は、自分で発行する証明書も使用できます。

## サービスごとの証明書要件

Exchange では複数の用途で証明書が使用されます。また、ほとんどのお客様が複数の Exchange サーバーで証明書を使用しています。一般に、所有する証明書が少ないほうが、証明書の管理は容易になります。

## IIS

以下のすべての Exchange サービスは、指定された Exchange クライアント アクセス サーバーで同じ証明書を使用します。

  - Outlook Web App

  - Exchange 管理センター (EAC)

  - Exchange Web サービス

  - Exchange ActiveSync

  - Outlook Anywhere

  - 自動検出

  - Outlook アドレス帳の配布

Web サイトと関連付けできるのは 1 つの証明書のみであり、既定ではこれらのサービスがすべて 1 つの Web サイトで提供されるため、これらのサービスのクライアントが使用するすべての名前が証明書内にあるか、証明書内のワイルドカード名に該当する必要があります。

## POP/IMAP

POP または IMAP で使用する証明書は、IIS で使用する証明書とは別に指定できます。ただし管理を簡単にするため、POP や IMAP のサービス名を IIS 証明書に追加し、これらのすべてのサービスで 1 つの証明書を使用することをお勧めします。

## SMTP

個々の証明書を使用して受信コネクタを個別に構成することもできます。証明書には、SMTP クライアント (または他の SMTP サーバー) がそのコネクタに到達するために使用する名前が含まれている必要があります。証明書の管理を簡単にするため、TLS トラフィックのサポートを必要とするすべての名前を 1 つの証明書に含めることを検討してください。

## デジタル証明書とプロキシ

プロキシ処理は、1 台のサーバーが他のサーバーにクライアント接続を送信する方法です。Exchange 2013 の場合、クライアント アクセス サーバーがそのクライアントのメールボックスのアクティブ コピーを含むメールボックス サーバーに対する受信クライアント要求をプロキシ処理する際に、プロキシ処理が発生します。

クライアント アクセス サーバーが要求をプロキシ処理する場合、SSL が暗号化に使用されますが、認証には使用されません。メールボックス サーバーの自己署名入りの証明書は、クライアント アクセス サーバーとメールボックス サーバー間のトラフィックを暗号化します。

## リバース プロキシと証明書

多くの Exchange 展開では、インターネット上に Exchange サービスを公開するのにリバース プロキシを使用します。リバース プロキシを構成して、SSL 暗号化を停止し、サーバー上で暗号化されていないトラフィックを検証し、リバース プロキシ サーバーから Exchange サーバーに対して、背後で新しい SSL 暗号化チャネルを開くことができます。これを SSL ブリッジと呼びます。リバース プロキシ サーバーを構成するもう 1 つの方法としては、リバース プロキシ サーバーの背後で Exchange サーバーに直接 SSL 接続するという方法があります。いずれかの展開モデルを使用することで、インターネット上のクライアントは、mail.contoso.com などの Exchange アクセス用のホスト名を使用してリバース プロキシ サーバーに接続します。次にリバース プロキシ サーバーが Exchange クライアント アクセス サーバーのコンピューター名など、別のホスト名を使用して Exchange に接続します。Exchange クライアント アクセス サーバーのコンピューター名を証明書に含める必要はありません。これはほとんどの一般的なリバース プロキシ サーバーが、クライアントの使用する元のホスト名を Exchange クライアント アクセス サーバーの内部ホスト名と照合できるためです。

## SSL およびスプリット DNS

スプリット DNS は、DNS 要求の発信場所によって、同じホスト名に対して異なる IP アドレスを構成することのできるテクノロジです。スプリット ホライズン DNS、スプリット ビュー DNS、またはスプリット ブレイン DNS とも呼ばれます。スプリット DNS を使用すると、クライアントの接続経路 (インターネットまたはイントラネット) に関わらず同じホスト名を使用して Exchange への接続を許可することにより、Exchange で管理する必要のあるホスト名の数を減らすことができます。スプリット DNS を使用すると、イントラネットから発信される要求が、インターネットから発信される要求とは異なる IP アドレスを受け取ることができます。

スプリット DNS は通常、小規模な Exchange 展開では必要ありません。これは、ユーザーが接続経路 (イントラネットまたはインターネット) に関わらず同じ DNS エンドポイントにアクセスできるためです。ただし大規模な展開の場合、この構成では送信インターネット プロキシ サーバーおよびリバース プロキシ サーバーに過大な負荷がかかります。大規模な展開では、スプリット DNS を構成して、たとえば外部ユーザーは mail.contoso.com に、内部ユーザーは internal.contoso.com にアクセスするようにします。この構成でスプリット DNS を使用することで、ユーザーが場所によって異なるホスト名を使用することを覚えておく必要がなくなります。

## リモート Windows PowerShell

Kerberos 認証および Kerberos 暗号化は、Exchange 管理センター (EAC) および Exchange 管理シェルの両方から、リモート Windows PowerShell アクセスで使用します。したがって、SSL 証明書をリモート Windows PowerShell での使用向けに構成する必要はありません。

ページのトップへ

## デジタル証明書のベスト プラクティス

組織のデジタル証明書の構成は、組織の特定の要件によって異なりますが、ベスト プラクティスに関する情報が含まれており、要件に合うデジタル証明書の構成を選択できます。

## ベスト プラクティス:信頼されたサード パーティ証明書を使用する

信頼されていない証明書に関するエラーをクライアントが受信することのないよう、Exchange サーバーが使用する証明書は、クライアントが信頼する対象によって発行されたものである必要があります。ほとんどのクライアントを、すべての証明書または証明書発行者を信頼するように構成できますが、信頼されているサードパーティの証明書を Exchange サーバーで使用する方がより簡単です。これは、ほとんどのクライアントが既にそのルート証明書を信頼しているためです。Exchange のために特別に構成された証明書を提供するサード パーティの証明書発行者が複数あります。EAC を使用して、大部分の証明書発行元で機能する証明書要求を生成できます。

## 証明機関の選択方法

証明機関 (CA) とは、証明書を発行しその正当性を保証する企業です。クライアント ソフトウェア (たとえば MicrosoftInternet Explorer などのブラウザー、Windows や Mac OS などのオペレーティング システム) には、そのクライアント ソフトウェアが信頼する組み込みの CA 一覧があります。通常、この一覧は CA を追加および削除することで構成できますが、構成作業が煩雑な場合があります。証明書を購入する CA を選択する場合は、以下の条件を使用してください。

  - Exchange サーバーと接続する予定のクライアント ソフトウェア (オペレーティング システム、ブラウザー、および携帯電話) によって、CA が信頼されていることを確認します。

  - 「統合コミュニケーション証明書」のサポートを表明している CA を、Exchange サーバーでの使用に選択します。

  - 使用予定の証明書の種類を、CA がサポートしていることを確認します。サブジェクトの別名 (SAN) 証明書を使用することを検討してください。すべての CA が SAN 証明書をサポートしているわけではなく、CA が必要な数のホスト名をサポートしていない場合もあります。

  - 購入した証明書のライセンスで、使用予定のサーバー数に対してその証明書を使用できることを確認してください。1 台のサーバーでしか証明書を使用できない CA もあります。

  - CA 間の証明書の価格を比較します。

## ベスト プラクティス:SAN 証明書を使用する

Exchange の展開の際のサービス名の構成方法によっては、複数のドメイン名を示すことのできる証明書が Exchange サーバーで必要となる場合があります。この問題はワイルドカード証明書 (\*.contoso.com 向けなど) によって解決できますが、どのサブドメインにも使用できる証明書を保持することでセキュリティに影響が及ぶ状態は、多くのユーザーにとって望ましくありません。より安全な方法は、必要な各ドメインを SAN として証明書で一覧にすることです。既定では、証明書要求が Exchange によって生成される場合に、この方法が使用されます。

## ベスト プラクティス:Exchange 証明書ウィザードを使用して証明書を要求する

Exchange には、証明書を使用するサービスが多数あります。証明書を要求する際の一般的なエラーとして、正しいサービス名のセットを含めずに要求を作成してしまうことがあります。Exchange 管理センターの証明書ウィザードを使用することで、証明書要求に正しい名前の一覧を含めることができます。このウィザードによって、証明書と組み合わせるサービスを指定し、選択したサービスに基づいて証明書に必要な名前を追加して、サービスと組み合わせて使用できるようにします。最初に Exchange 2013 サーバーのセットを展開し、個別のサービス向けに展開で使用するホスト名を決定した際に、証明書ウィザードを実行します。Active Directory を展開する各 Exchange サイトで必要な証明書ウィザードの実行回数は、1 回だけであることが理想的です。

証明機関を使用すると、購入した証明書の SAN 一覧にあるホスト名を忘れる心配がありません。証明機関は、無料の猶予期間 (証明書を返却し、同じ証明書をホスト名を追加して要求できる期間) 付きで使用できます。

## ベスト プラクティス:最少限のホスト名を使用する

使用する証明書を最少限にするほか、使用するホスト名も最少限にする必要があります。この方法でコストを節約できます。多くの証明書プロバイダーでは、証明書に追加するホスト名の数に基づいて課金されます。

必要なホスト名の数 (およびそれに伴う証明書管理の煩雑さ) を減らすための最も重要な手順は、証明書のサブジェクトの別名に個々のサーバーのホスト名を含めないことです。

Exchange 証明書に含める必要のあるホスト名は、クライアント アプリケーションが Exchange と接続する際に使用するホスト名です。以下に、Contoso という名前の会社で必要な、一般的なホスト名の一覧を示します。

  - **Mail.contoso.com**   このホスト名によって、MicrosoftOutlook、Outlook Web App、Outlook Anywhere、オフライン アドレス帳、Exchange Web サービス、POP3、IMAP4、SMTP、Exchange コントロール パネル、および ActiveSync など、Exchange との大部分の接続がカバーされます。

  - **Autodiscover.contoso.com**   このホスト名は、MicrosoftOffice Outlook 2007 以降のバージョン、Exchange ActiveSync、および Exchange Web サービス クライアントなどの、自動検出をサポートするクライアントによって使用されます。

  - **Legacy.contoso.com** Exchange 2007 および Exchange 2013 との共存のシナリオではこのホスト名が必要です。Exchange 2007 と Exchange 2013 にメールボックスを持つクライアントがいる場合、レガシ ホスト名を構成することで、ユーザーがアップグレード プロセス中に 2 番目の URL を覚えておく必要がなくなります。

## ワイルドカード証明書について

ワイルドカード証明書は、1 つのドメインと複数のサブドメインをサポートするよう設計されています。たとえば、\*.contoso.com でワイルドカード証明書を構成すると、その証明書は mail.contoso.com、web.contoso.com、および autodiscover.contoso.com で使用できます。

ページのトップへ

