﻿---
title: 'Exchange 2007 UM から Exchange 2013 UM へのアップグレード: Exchange 2013 Help'
TOCTitle: Exchange 2007 UM から Exchange 2013 UM へのアップグレード
ms:assetid: 642c922d-7e85-40f0-bb9b-0f20da692be3
ms:mtpsurl: https://technet.microsoft.com/ja-jp/library/Dn169227(v=EXCHG.150)
ms:contentKeyID: 54652970
ms.date: 04/24/2018
mtps_version: v=EXCHG.150
ms.translationtype: HT
---

# Exchange 2007 UM から Exchange 2013 UM へのアップグレード

 

_**適用先:** Exchange Server 2013, Exchange Server 2016_

_**トピックの最終更新日:** 2016-12-09_

ユニファイド メッセージング (UM) を含む Microsoft Exchange 2007 組織を Exchange 2013 ユニファイド メッセージングにアップグレードする場合は、必須の手順の他に、Exchange 2007 UM 展開の一部としてすでに完了している手順もあります。Exchange 2007 でユニファイド メッセージングをサポートするために作成して構成したテレフォニー環境と UM コンポーネントによっては、ボイス オーバー IP (VoIP) ゲートウェイ、IP 構内交換機 (PBX)、従来のまたは SIP 対応 PBX などの追加のテレフォニー環境を展開して、Exchange 2013 UM に必要な追加の UM コンポーネントを構成しなければならない場合があります。

## 始める前に把握しておくべき情報

  - このタスクの予想所要時間:45 ～ 90 分。

  - Exchange 2007 組織と Exchange 2013 組織で必要なすべてのコンポーネントを作成して構成するために必要なアクセス許可が与えられていることを確認します。

  - VoIP ゲートウェイと PBX、IP PBX、セッション開始プロトコル (SIP) 対応 PBX などのテレフォニー コンポーネントが展開され、正しく構成されていることを確認します。

  - Microsoft Exchange ユニファイド メッセージング呼び出しルーター (UM 呼び出しルーター) サービスを実行するクライアント アクセス サーバーと、Microsoft Exchange ユニファイド メッセージング (UM) サービスを実行するメールボックス サーバーが正しく設置され、構成されていることを確認します。UM サービスの詳細については、「[UM サービス](um-services-exchange-2013-help.md)」を参照してください。

  - このトピックの手順で使用可能なキーボード ショートカットについては、「[Exchange 管理センターのキーボード ショートカット](keyboard-shortcuts-in-the-exchange-admin-center-exchange-online-protection-help.md)」を参照してください。


> [!TIP]
> 問題がある場合は、Exchange のフォーラムで質問してください。 次のフォーラムにアクセスしてください。<A href="https://go.microsoft.com/fwlink/p/?linkid=60612">Exchange Server</A>、 <A href="https://go.microsoft.com/fwlink/p/?linkid=267542">Exchange Online</A>、 または <A href="https://go.microsoft.com/fwlink/p/?linkid=285351">Exchange Online Protection</A>。.



## 実行方法

## 手順 1:必要な UM 言語パックをダウンロードしてインストールする

UM 言語パックを使用すると、発信者と Outlook Voice Access ユーザーは複数の言語でボイス メール システムを操作できます。また、追加の言語を Exchange 2013 メールボックス サーバーにインストールすると、発信者と Outlook Voice Access ユーザーはその言語で電子メール メッセージを聞いたり、ボイス メール システムを操作したりすることができます。ただし、すべての着信呼び出しでその言語を使用できるようにするには、すべての Exchange 2013 メールボックス サーバーに必要な UM 言語パックをインストールする必要があります。これは、すべての Exchange 2013 メールボックス サーバーがユニファイド メッセージングの着信呼び出しに応答できるためです。

既定では、Exchange 2013 メールボックス サーバーをインストールすると、U.S. 英語 (en-US) 言語パックがインストールされます。別の UM 言語パックをインストールしない限り、この言語がダイヤル プランで使用できる唯一の言語オプションです。(メールボックス サーバーをコンピューターから削除しない限り、U.S. 英語は削除できません。)UM 言語パックを Exchange 2013 メールボックス サーバーにインストールすると、ダイヤル プランの既定の言語を構成するときに、言語パックに関連付けられた言語が使用可能なオプションとして一覧表示されます。既定では、UM 自動応答が作成と同時に UM ダイヤル プランにリンクされるため、リンク先の UM ダイヤル プランの既定の言語設定が使用されます。ただし、この設定は UM 自動応答が作成された後で変更できます。


> [!NOTE]
> ダイヤル プランで使用する言語が U.S. 英語だけの場合は、この手順を省略して手順 2 に進みます。



UM 言語パックを「[Exchange Server 2013 UM Language Packs](https://go.microsoft.com/fwlink/p/?linkid=266542)」からダウンロードした後で、setup.exe コマンドを使用するか、*\<UMLanguagePack\>*.exe インストール プログラムを実行して、UM 言語パックを追加することができます。詳細については、「[UM 言語パックをインストールする](install-a-um-language-pack-exchange-2013-help.md)」を参照してください。

この例では、setup.exe を使用して日本語 (ja-JP) の UM 言語パックをインストールします。

    setup.exe /AddUmLanguagePack:ja-JP /s:d:\Exchange\UMLanguagePacks /IAcceptExchangeServerLicenseTerms

## 手順 2:Exchange 2007 のカスタム案内応答、アナウンス、メニュー、および音声ガイダンスを Exchange 2013 システム メールボックスに移動する

カスタム案内応答、アナウンス、メニュー、および音声ガイダンスはユニファイド メッセージングのダイヤル プランと自動応答で使用されます。Exchange 2007 または Exchange 2013 をインストールすると、{e0dc1c29-89c3-4034-b678-e6c29d823ed9} という名前のシステム メールボックスが作成され、メッセージ承認や複数メールボックス検索といった機能のサポートに使用されます。このシステム メールボックスは、ダイヤル プランと自動応答のカスタム案内応答、アナウンス、メニュー、および音声ガイダンスを保存するためにも使用されます。システム メールボックスが存在しない場合は、**Setup /PrepareAD** コマンドを使用して作成できます。

既定で、システム メールボックスは Exchange 管理センター (EAC) から確認できません。システム メールボックスの一覧を入手するには、次のいずれかを実行する必要があります。

このコマンドは、すべてのシステム メールボックスの一覧を返します。

    Get-Mailbox -Arbitration

このコマンドは、システム メールボックスとそれぞれのプロパティまたは設定の一覧を返します。

    Get-Mailbox -Arbitration |fl

カスタム案内応答、アナウンス、メニュー、および音声ガイダンスを Exchange 2007 から Exchange 2013 にインポートする場合は、MigrateUMCustomPrompts.ps1 スクリプトを使用する必要があります。EAC を使用して、カスタム案内応答、アナウンス、メニュー、および音声ガイダンスをインポートすることはできません。MigrateUMCustomPrompts.ps1 スクリプトは、Exchange Server 2007 UM のカスタム案内応答、アナウンス、メニュー、および音声ガイダンスのすべてのコピーを Exchange 2013 UM に移行します。既定で、MigrateUMCustomPrompts.ps1 スクリプトは Exchange 2013 メールボックス サーバーの *\<Program Files\>*\\Microsoft\\Exchange Server\\V15\\Scripts フォルダーに配置されており、Exchange 2013 メールボックス サーバーから実行する必要があります。このスクリプトを実行するには、以下の手順を実行します。

1.  **\[スタート\]** \> **\[すべてのプログラム\]** \> **\[Microsoft Exchange Server 2013\]** \> **\[Exchange 管理シェル\]** を順にクリックします。

2.  Exchange 管理シェルのプロンプトで、スクリプトへのパスを入力します。たとえば、「**cd "D:\\Program Files\\Microsoft\\Exchange Server\\V15\\Scripts"**」と入力してから、Enter を押します。

3.  シェル プロンプトで、**".\\MigrateUMCustomPrompts"** と入力してから、Enter を押します。


> [!NOTE]
> <STRONG>Import-UMPrompt</STRONG> コマンドレットを使用して、カスタム音声ガイダンスを個別にインポートすることもできます。Exchange 2007 UM <STRONG>Copy-UMCustomPrompt</STRONG> コマンドレットは、カスタム音声ガイダンスの Exchange 2013 UM へのコピーに対応していません。



Exchange 2013 サーバーから MigrateUMCustomPrompts.ps1 スクリプトを実行した場合は、Active Directory 内でダイヤル プランまたは自動応答の GUID またはオブジェクト識別子参照が実行され、カスタム案内応答、アナウンス、メニュー、または音声ガイダンスの存在が確認されます。存在が確認された場合は、カスタム案内応答、アナウンス、メニュー、および音声ガイダンスが {e0dc1c29-89c3-4034-b678-e6c29d823ed9} という名前のシステム メールボックスにインポートされます。

このシステム メールボックスを使用すると、カスタム案内応答、アナウンス、メニュー、音声ガイダンスをデータベース内の他のメールボックスと併せてバックアップおよび復元できます。これにより必要なリソースの量が削減されます。カスタム案内応答、アナウンス、メニュー、音声ガイダンスをシステム メールボックスに保存することによって、不整合の発生を抑えることができます。メールボックスの移動方法については、「[Exchange 2013 でのメールボックスの移動](mailbox-moves-in-exchange-2013-exchange-2013-help.md)」を参照してください。

## 手順 3:証明書をエクスポートまたはインポートする

Exchange 2007 組織でセキュリティ保護された SIP またはセキュリティ保護されたダイヤル プランを使用する場合は、使用されていた証明書をエクスポートして Exchange 2013 クライアント アクセス サーバーおよびメールボックス サーバーにインポートする必要があります。Exchange 2013 サーバーと、VoIP ゲートウェイ、IP PBX、SIP 対応 PBX との間で送信されるデータは、相互トランスポート層セキュリティ (MTLS) を使用して暗号化されます。証明書は、証明書の所有者の ID を、情報を電子的に暗号化し署名するために使用される一対の電子鍵 (パブリックとプライベート) にバインドします。次の証明書のいずれかを UM サービスと UM 呼び出しルーター サービスに使用できます。

  - 自己署名 (Exchange) 証明書

  - 内部公開キー基盤 (PKI) 証明書

  - 商用サード パーティ証明書

Exchange 2013 をインストールする場合、既定では自己署名証明書が 2 通作成されます:** Microsoft Exchange Server Auth Certificate** および **Microsoft Exchange**です。**Microsoft Exchange** 自己署名証明書は UM によるデータの暗号化に使用できますが、この証明書を UM サービスと UM 呼び出しルーター サービスに割り当てる必要があります。この自己署名証明書は、コピーしてから、VoIP ゲートウェイ、IP PBX、SIP 対応 PBX にインポートできます。ただし、UM と Microsoft Lync Server が統合されている場合は使用できません。

Exchange 2013 サーバーと、VoIP ゲートウェイ、IP PBX、および SIP 対応 PBX との間で送信されるデータを UM で暗号化できるようにするには、次の手順を実行する必要があります。

  - 既存の自己署名 UM 証明書を使用するか、新しい自己署名 Exchange 証明書を作成するか、PKI 証明書の内部証明機関に証明書要求を発行するか、Exchange 2013 のメールボックス サーバーおよびクライアント アクセス サーバーと、VoIP ゲートウェイ、IP PBX、および SIP 対応 PBX との間の相互 TLS に使用可能な商用サードパーティ証明書を購入します。
    
    次のように、EAC を使用して Exchange 自己署名証明書を作成します。
    
    1.  EAC で、**\[サーバー\]** \> **\[証明書\]** に移動してから、**\[追加\]**![\[追加\] アイコン](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "[追加] アイコン") をクリックします。
    
    2.  **\[Exchange 証明書の新規作成\]** ページで、**\[自己署名証明書を作成する\]** を選択してから、**\[次へ\]** を選択します。
    
    3.  証明書のフレンドリ名を入力してから、**\[次へ\]** を選択します。
    
    4.  **\[追加\]** ![\[追加\] アイコン](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "[追加] アイコン") をクリックしてこの証明書を適用する Exchange サーバーを選択してから、**\[次へ\]** を選択します。
    
    5.  証明書に含めるドメインを指定してから、**\[次へ\]** を選択します。サービス用のドメインを追加する場合は、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。
    
    6.  追加したドメインが正しいことを確認してから、**\[完了\]** を選択します。
    

    > [!IMPORTANT]
    > EAC を使用して証明書を作成した場合は、証明書に対してサービスを有効にするように要求されません。証明書を作成してから、EAC を使用してサービスを有効にすることができます。サービスの証明書を有効にする方法については、「<A href="assign-a-certificate-to-the-um-and-um-call-router-services-exchange-2013-help.md">UM および UM 呼び出しルーター サービスへの証明書の割り当て</A>」を参照してください。

    
    シェルで次のコマンドを実行することによって、Exchange 自己署名証明書を作成します。
    
        New-ExchangeCertificate -Services 'UM, UMCallRouter' -DomainName '*.northwindtraders.com' -FriendlyName 'UMSelfSigned' -SubjectName 'C=US,S=WA,L=Redmond,O=Northwindtraders,OU=Servers,CN= Northwindtraders.com' -PrivateKeyExportable $true
    

    > [!TIP]
    > <EM>Services</EM> パラメーターを使用して有効にするサービスを指定した場合は、作成した証明書に対してサービスを有効にするように要求されます。この例では、ユニファイド メッセージング サービスとユニファイド メッセージング呼び出しルーター サービスに対して証明書を有効にするように要求されます。サービスの証明書を有効にする方法については、「<A href="assign-a-certificate-to-the-um-and-um-call-router-services-exchange-2013-help.md">UM および UM 呼び出しルーター サービスへの証明書の割り当て</A>」を参照してください。



  - 組織内のすべての Exchange 2013 クライアント アクセス サーバーおよびメールボックス サーバー上で使用される証明書をインポートします。Exchange 2013 自己署名証明書を使用する場合は、その証明書をコピーしてから、VoIP ゲートウェイ、IP PBX、または SIP 対応 PBX にインポートする必要があります。Exchange 2007 の自己署名証明書を使用する場合は、サブジェクトの別名 (SAN) にすべての Exchange 2013 サーバーのコンピューター名を含める必要があります。組織内に Exchange 2007 ユニファイド メッセージング サーバーが存在する場合は、Exchange 2013 自己署名証明書を使用できますが、Exchange 2007 UM サーバーのコンピューター名を Exchange 2013 証明書内の SAN に追加する必要があります。

  - 組織内のクライアント アクセス サーバーとメールボックス サーバー上の UM サービスと UM 呼び出しルーター サービスに使用する証明書を有効化または指定します。
    
    次のように、EAC を使用して、すべての Exchange 2013 サーバー上の UM サービスと UM 呼び出しルーター サービスで Exchange 自己署名証明が使用できるようにします。
    
    1.  EAC で、**\[サーバー\]** \> **\[証明書\]** に移動して、サービスを有効にする証明書を選択してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。
    
    2.  **\[手順\]** ページで、**\[サービス\]**、**\[ユニファイド メッセージング\]**、**\[ユニファイド メッセージング呼び出しルーター\]** の順に選択します。
    
    シェルで次のコマンドを実行することによって、Exchange 自己署名証明書を有効にします。
    
        Enable-ExchangeCertificate -Thumbprint 5113ae0233a72fccb75b1d0198628675333d010e -Services 'UM, UMCallRouter'

  - 新しいまたは既存の UM ダイヤル プランのすべてを、\[セキュリティで保護された SIP\] または \[セキュリティで保護\] として構成します。

  - 組織内のクライアント アクセス サーバーとメールボックス サーバー上で UM スタートアップ モードを TLS またはデュアルに構成します。

  - 完全修飾ドメイン名 (FQDN) を持つ、新しいまたは既存の UM IP ゲートウェイを作成して構成します。

  - UM IP ゲートウェイのリスニング ポートが TLS ポート 5061 を使用するように構成します。

  - すべての Exchange 2013 クライアント アクセス サーバー上の UM 呼び出しルーター サービスを再起動し、すべての Exchange 2013 メールボックス サーバー上の UM サービスを再起動します。UM サービスの詳細については、「[UM サービス](um-services-exchange-2013-help.md)」を参照してください。

## 手順 4:すべての Exchange 2013 クライアント アクセス サーバー上の UM スタートアップ モードを構成する

セキュリティで保護された SIP ダイヤル プランまたはセキュリティで保護されたダイヤル プランを使用する場合は、Exchange 2013 クライアント アクセス サーバー上の UM スタートアップ モードを構成する必要があります。EAC またはシェルを使用して、Exchange 2013 クライアント アクセス サーバー上の UM 呼び出しルーター サービスの UM スタートアップ モードを指定できます。既定で、Exchange 2013 クライアント アクセス サーバーは TCP モードで起動しますが、トランスポート層セキュリティ (TLS) を使用してボイス オーバー IP (VoIP) トラフィックを暗号化している場合は、TLS モードまたはデュアル モードを使用するように Exchange 2013 クライアント アクセス サーバーを構成する必要があります。UM スタートアップ モードとしてデュアルを使用するようにすべての Exchange 2013 クライアント アクセス サーバーを構成することをお勧めします。これは、すべての Exchange 2013 クライアント アクセス サーバーがすべての UM ダイヤル プランの着信呼び出しに応答でき、これらのダイヤル プランに別々のセキュリティ設定を適用できるためです。UM スタートアップ モードを変更した場合は、UM 呼び出しルーター サービスを再起動して変更を有効にする必要があります。UM サービスの詳細については、「[UM サービス](um-services-exchange-2013-help.md)」を参照してください。

次のように、EAC を使用して Exchange 2013 クライアント アクセス サーバー上の UM スタートアップ モードを構成します。

1.  EAC で **\[サーバー\]** \> **\[サーバー\]** に移動します。

2.  リスト ビューで、変更する Exchange サーバーを選択してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

3.  **\[Exchange Server\]** ページで、**\[ユニファイド メッセージング\]** をクリックします。

4.  **\[UM 呼び出しルーターの設定\]** の **\[UM スタートアップ モード\]** で、ドロップダウン リストから次のいずれかを選択します。
    
      - **\[TCP\]**   mTLS を使用せず、セキュリティで保護されていないダイヤル プランのみを使用する場合は、このオプションを使用します。
    
      - **\[TLS\]**   mTLS を使用して、セキュリティで保護された SIP ダイヤル プランまたはセキュリティで保護されたダイヤル プランのみを使用する場合は、このオプションを使用します。
    
      - **\[デュアル\]**   mTLS を使用して、セキュリティで保護されていないダイヤル プラン、セキュリティで保護された SIP ダイヤル プラン、およびセキュリティで保護されたダイヤル プランを使用する場合は、このオプションを使用します。

5.  UM スタートアップ モードを選択し、**\[保存\]** をクリックします。

シェルで次のコマンドを実行することによって、Exchange 2013 クライアント アクセス サーバー上の UM スタートアップ モードを構成します。

    Set-UMCallRouterSettings -Server MyUMCallRouter.northwindtraders.com -UMStartupMode Dual

## 手順 5:すべての Exchange 2013 メールボックス サーバー上の UM スタートアップ モードを構成する

セキュリティで保護された SIP ダイヤル プランまたはセキュリティで保護されたダイヤル プランを使用する場合は、Exchange 2013 メールボックス サーバー上の UM スタートアップ モードを構成する必要があります。EAC またはシェルを使用して、Exchange 2013 メールボックス サーバー上の UM サービスの UM スタートアップ モードを指定できます。既定で、Exchange 2013 メールボックス サーバーは TCP モードで起動しますが、トランスポート層セキュリティ (TLS) を使用してボイス オーバー IP (VoIP) トラフィックを暗号化する場合は、TLS またはデュアル モードを使用するように Exchange 2013 メールボックス サーバーを構成する必要があります。UM スタートアップ モードとしてデュアルを使用するようにすべての Exchange 2013 メールボックス サーバーを構成することをお勧めします。これは、すべての Exchange 2013 メールボックス サーバーがすべての UM ダイヤル プランの着信呼び出しに応答でき、これらのダイヤル プランに別々のセキュリティ設定を適用できるためです。UM スタートアップ モードを変更した場合は、UM サービスを再起動して変更を有効にする必要があります。UM サービスの詳細については、「[UM サービス](um-services-exchange-2013-help.md)」を参照してください。

次のように、EAC を使用して Exchange 2013 メールボックス サーバー上の UM スタートアップ モードを構成します。

1.  EAC で **\[サーバー\]** \> **\[サーバー\]** に移動します。

2.  リスト ビューで、変更する Exchange サーバーを選択してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

3.  **\[Exchange Server\]** ページで、**\[ユニファイド メッセージング\]** をクリックします。

4.  **\[UM サービス設定\]** の **\[UM スタートアップ モード\]** で、ドロップダウン リストから次のいずれかを選択します。
    
      - **\[TCP\]**   mTLS を使用せず、セキュリティで保護されていないダイヤル プランのみを使用する場合は、このオプションを使用します。
    
      - **\[TLS\]**   mTLS を使用して、セキュリティで保護された SIP ダイヤル プランまたはセキュリティで保護されたダイヤル プランのみを使用する場合は、このオプションを使用します。
    
      - **\[デュアル\]**   mTLS を使用して、セキュリティで保護されていないダイヤル プラン、セキュリティで保護された SIP ダイヤル プラン、およびセキュリティで保護されたダイヤル プランを使用する場合は、このオプションを使用します。

5.  UM スタートアップ モードを選択し、**\[保存\]** をクリックします。

シェルで次のコマンドを実行することによって、Exchange 2013 メールボックス サーバー上の UM スタートアップ モードを構成します。

    Set-UMService -Identity MyUMServer -ExternalHostFqdn host.external.contoso.com -IPAddressFamily Any -UMStartupMode Dual

## 手順 6:UM ダイヤル プランを作成または既存の UM ダイヤル プランを構成する

既存の Exchange 2007 展開に応じて、新しい UM ダイヤル プランを作成するか、既存のダイヤル プランを構成する必要があります。UM ダイヤル プランは、共通のユーザー内線番号を共有する従来の構内交換機 (PBX)、SIP 対応 PBX、IP PBX、または SIP 対応 PBX のセットを表します。ダイヤル プラン内の従来の PBX、SIP 対応 PBX、または IP PBX でホストされているすべてのユーザーの内線番号は同じ桁数で構成されます。ユーザーは、内線番号に特別な番号を付加したり、完全な電話番号をダイヤルしたりしなくても、相互の内線電話番号をダイヤルできます。

UM ダイヤル プランは、ユーザーの内線電話番号が固有になるようにするために、ユニファイド メッセージングで使用されます。テレフォニー ネットワークによっては、IP PBX、従来の PBX、または SIP 対応 PBX が複数存在する場合があります。このようなテレフォニー ネットワークでは、2 人のユーザーが、同じ内線電話番号を持つ場合があります。UM ダイヤル プランがこのような状況を解決します。2 人のユーザーを異なる 2 つの UM ダイヤル プランに入れると、それぞれの内線番号が固有になります。詳細については、「[UM ダイヤル プラン](um-dial-plans-exchange-2013-help.md)」を参照してください。

必要に応じて、EAC を使用して UM ダイヤル プランを作成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM ダイヤル プラン\]** に移動してから、**\[新規作成\]**![\[追加\] アイコン](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "[追加] アイコン") をクリックします。

2.  **\[UM ダイヤル プランの新規作成\]** ページで、以下を入力します。
    
      - **\[名前\]**   ダイヤル プランの名前を入力します。一意な UM ダイヤル プラン名を必ず指定する必要があります。ただし、入力した名前は EAC とシェルでは表示専用として使用されます。UM ダイヤル プラン名の長さは 64 文字以下で、スペースを含めることができます。ただし、次の文字を含めることはできません。" / \\ \[ \] : ; | = , + \* ? \< \>.
        
        ダイヤル プラン名のボックスには 64 文字まで入力できますが、49 文字を超えるダイヤル プラン名は設定できません。49 文字を超えるダイヤル プラン名を作成しようとすると、エラー メッセージが表示されます。このメッセージは、UM ダイヤル プラン名が長すぎるために、UM メールボックス ポリシーを生成できなかったことを通知します。これは、ダイヤル プランを作成すると、"*\<DialPlanName\>* **の既定のポリシー**" という名前の既定の UM メールボックス ポリシーも作成されるためです。既定のポリシー内の 15 文字がダイヤル プラン名に付加されると、総文字数が上限を超えます。UM ダイヤル プランと UM メールボックス ポリシーの *name* パラメーターはどちらも 64 文字にすることができます。ただし、ダイヤル プラン名が 49 文字を超える場合、既定の UM メールボックス ポリシー名が 64 文字よりも長くなります。これはシステムでは許可されません。
    
      - **\[内線番号の長さ (桁数)\]**   ダイヤル プランにおける内線番号の桁数を入力します。内線番号の桁数は、PBX で作成されるテレフォニー ダイヤル プランに基づいています。たとえば、テレフォニー ダイヤル プランに関連付けられているユーザーが、同じテレフォニー ダイヤル プランを使用している別のユーザーに電話をかけるときに 4 桁の内線番号をダイヤルしている場合は、内線番号の桁数として 4 を選択します。
        
        これは 1 ～ 20 の値を持つ必須ボックスです。一般的な内線番号の長さは、3 ～ 7 個の数字です。既存のテレフォニー環境で内線番号が使用されている場合は、それらの内線番号と同じ桁数を指定する必要があります。
        
        内線電話番号ダイヤル プランにユーザーがリンクされている場合は、内線電話番号ダイヤル プランを作成する際に、ユーザーの内線番号を入力する必要があります。UM が有効なユーザーが SIP URI ダイヤル プランまたは E.164 ダイヤル プランにリンクされている場合は、セッション開始プロトコル (SIP) ダイヤル プランまたは E.164 ダイヤル プランにおいても内線番号が必要になります。この内線番号は、Outlook Voice Access ユーザーが Exchange メールボックスにアクセスするときに使用します。
    
      - UNRESOLVED\_TOKENBLOCK\_VAL(UI\_URI 型)
    
      - UNRESOLVED\_TOKENBLOCK\_VAL(UI\_VoIP セキュリティ)
    
      - **\[国/地域コード\]**   発信呼び出しに使用される国/地域コードを入力するには、このボックスを使用します。この番号は、ダイヤルされた電話番号に自動的に付加されます。このボックスには、1 ～ 4 桁の数値を入力できます。たとえば、米国内の国/地域コードは 1、英国内の国/地域コードは 44 です。

3.  **\[保存\]** をクリックします。

必要に応じて、シェルで次のコマンドを実行することによって、UM ダイヤル プランを作成できます。

    New-UMDialplan -Name MyUMDialPlan -URIType E164 -NumberOfDigitsInExtension 5 -VoIPSecurity Secured

必要に応じて、EAC を使用して既存の UM ダイヤル プランを構成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM ダイヤル プラン\]** に移動します。

2.  リスト ビューで、表示または変更する UM ダイヤル プランを選択し、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

3.  **\[UM ダイヤル プラン\]** ページで、**\[構成\]** をクリックします。構成オプションを使用して、特定のダイヤル プラン設定を表示したり、機能を有効または無効にしたりします。

必要に応じて、シェルで次のコマンドを実行することによって、既存の UM ダイヤル プランを構成できます。

    Set-UMDialplan -Identity MyDialPlan -AccessTelephoneNumbers 4255551234 -AudioCodec Wma -CallAnsweringRulesEnabled $false -OutsideLineAccessCode 9 -VoIPSecurity SIPSecured

Exchange 2007 ユニファイド メッセージを展開した場合は、着信呼び出しに応答するためにユニファイド メッセージング サーバーを UM ダイヤル プランに追加する必要がありました。この作業を行う必要はなくなりました。Exchange 2013 では、クライアント アクセス サーバーとメールボックス サーバーを内線電話番号または E.164 ダイヤル プランにリンクすることはできませんが、SIP URI ダイヤル プランにリンクする必要があります。クライアント アクセス サーバーおよびメールボックス サーバーは、すべての種類のダイヤル プランにおけるすべての着信呼び出しに応答します。

## 手順 7:UM IP ゲートウェイを作成または既存の UM IP ゲートウェイを構成する

既存の Exchange 2007 展開によっては、新しい UM IP ゲートウェイを作成するか、既存の UM IP ゲートウェイを構成する必要があります。SIP セキュリティまたはセキュリティで保護されたダイヤル プランを使用している場合は、FQDN を使用して UM IP ゲートウェイを作成し、シェルを利用してポート 5061 をチェックするように構成します。既存の UM IP ゲートウェイの場合は、FQDN を使用していることとポート 5061 をチェックするように構成されていることを確認します。UM IP ゲートウェイが FQDN を使用していない場合は、EAC またはシェルを利用してアドレスを変更します。UM IP ゲートウェイでポート 5061 が使用されていない場合は、シェルを使用してポートを変更します。**Get-UMIPGateway** コマンドレットを使用することによって、UM IP ゲートウェイの設定を確認できます。

UM IP ゲートウェイは、物理的なボイス オーバー IP (VoIP) ゲートウェイ、PBX、または SIP 対応 PBXを表します。VoIP ゲートウェイ、IP PBX、または SIP 対応 PBX を使用してボイス メール ユーザーの着信呼び出しに応答したり、発信呼び出しを送信したりするには、その前に、UM IP ゲートウェイをディレクトリ サービス内に作成しておく必要があります。

UM IP ゲートウェイと UM ハント グループの組み合わせにより、VoIP ゲートウェイ、IP PBX、または SIP 対応 PBX と UM ダイヤル プランの間のリンクが確立されます。複数の UM ハント グループを作成すると、1 つの UM IP ゲートウェイを複数の UM ダイヤル プランに関連付けることができます。詳細については、「[UM IP ゲートウェイ](um-ip-gateways-exchange-2013-help.md)」を参照してください。

必要に応じて、次のように、EAC を使用して UM IP ゲートウェイを作成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM IP ゲートウェイ\]** に移動し、**\[追加\]**![\[追加\] アイコン](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "[追加] アイコン") をクリックします。

2.  **\[UM IP ゲートウェイの新規作成\]** ページで、次の情報を入力します。
    
      - **\[名前\]**   このボックスを使用して、UM IP ゲートウェイの一意の名前を指定します。これは、EAC に表示される表示名です。UM IP ゲートウェイの作成後に表示名を変更する必要がある場合、最初に既存の UM IP ゲートウェイを削除し、次に適切な名前を持つ別の UM IP ゲートウェイを作成します。UM IP ゲートウェイ名は必須ですが、表示にのみ使用されます。組織では複数の UM IP ゲートウェイを使用することがあるため、UM IP ゲートウェイにはわかりやすい名前を使用することをお勧めします。UM IP ゲートウェイ名は最大 64 文字です。スペースも使用できます。ただし、次の文字を含めることはできません。" / \\ \[ \] : ; | = , + \* ? \< \>.
    
      - **\[アドレス\]**   IPv4/IPv6 アドレスまたは FQDN を使用して UM IP ゲートウェイを構成できます。このボックスを使用して、VoIP ゲートウェイ、IP PBX、または SIP 対応 PBX に構成した IP アドレスまたは FQDN を指定します。このボックスには、有効で正しい形式の FQDN のみ指定できます。
        
        英数字を入力できます。IPv4 アドレス、IPv6 アドレス、および FQDN がサポートされています。UM IP ゲートウェイと、セキュリティで保護された SIP モードまたはセキュリティ保護されたモードで運用されているダイヤル プランの間で相互 TLS を使用するには、FQDN を使用して UM IP ゲートウェイを構成する必要があります。また、ポート 5061 で接続要求を待つように構成し、VoIP ゲートウェイまたは IP PBX もすべてポート 5061 で相互 TLS の接続要求を待つように構成されていることを確認する必要があります。UM IP ゲートウェイを構成するには、シェルで次のコマンドを実行します。`Set-UMIPGateway -identity MyUMIPGateway -Port 5061`
        
        FQDN を使用する場合、ホスト名を正しい IP アドレスに解決できるように、VoIP ゲートウェイ、IP PBX、または SIP 対応 PBX に対して DNS ホスト レコードを正しく構成したことも確認する必要もあります。また、IP アドレスの代わりに FQDN を使用していて、かつ UM IP ゲートウェイの DNS 構成が変更される場合、UM IP ゲートウェイをいったん無効化してから有効化し、UM IP ゲートウェイの構成情報が正しく更新されていることを確認する必要があります。
    
      - **\[UM ダイヤル プラン\]** **\[参照\]** をクリックして、UM IP ゲートウェイに関連付ける UM ダイヤル プランを選択します。UM IP ゲートウェイに関連付ける UM ダイヤル プランを選択すると、既定の UM ハント グループも作成され、選択した UM ダイヤル プランに関連付けられます。UM ダイヤル プランを選択しない場合、UM ハント グループを手動で作成し、作成した UM IP ゲートウェイとその UM ハント グループを関連付ける必要があります。

3.  **\[保存\]** をクリックします。

必要に応じて、シェルで次のコマンドを実行することによって、UM IP ゲートウェイを作成できます。

    New-UMIPGateway -Identity MyUMIPGateway -Address "MyUMIPGateway.contoso.com"

必要に応じて、EAC を使用して既存の UM IP ゲートウェイを構成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM IP ゲートウェイ\]** に移動し、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

2.  **\[UM IP ゲートウェイ\]** ページで、**\[構成\]** をクリックします。構成オプションを使用して、特定の UM IP ゲートウェイ設定を表示したり、機能を有効または無効にしたりします。

必要に応じて、シェルで次のコマンドを実行することによって、既存の UM IP ゲートウェイを構成できます。

    Set-UMIPGateway -Identity MyUMIPGateway -Address fe80::39bd:88f7:6969:d223%11 -IPAddressFamily Any -Status Disabled -OutcallsAllowed $false

## 手順 8:UM ハント グループを作成する

既存の Exchange 2007 展開によっては、新しい UM ハント グループの作成を要求される場合があります。テレフォニー ハント グループを使用すると、電話呼び出しを 1 つの番号から複数の内線または電話番号に分散できます。ユニファイド メッセージングでは、UM ハント グループがテレフォニー ハント グループの論理的な表現であり、UM IP ゲートウェイを UM ダイヤル プランにリンクします。

すべての IP PBX または PBX ハント グループに対して少なくとも 1 つの UM ハント グループが必要です。次の手順を完了すると、既定で 1 つの UM ハント グループが作成されます。複数の IP PBX または PBX ハント グループがある場合は、追加の UM ハント グループを作成する必要があります。UM ハント グループの詳細については、「[UM ハント グループ](um-hunt-groups-exchange-2013-help.md)」を参照してください。

必要に応じて、EAC を使用して UM ハント グループを作成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM ダイヤル プラン\]** に移動します。リスト ビューで、変更する UM ダイヤル プランを選択し、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

2.  **\[UM ダイヤル プラン\]** ページの **\[UM ハント グループ\]** で、**\[新規作成\]**![\[追加\] アイコン](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "[追加] アイコン") をクリックします。

3.  **\[UM ハント グループの新規作成\]** ページで、次の情報を入力します。
    
      - **\[名前\]**   UM ハント グループの表示名を作成するには、このボックスを使用します。UM ハント グループ名は必須で、一意である必要があります。ただし、この名前は、EAC とシェルでの表示のためにのみ使用されます。ハント グループを作成した後にその表示名を変更する必要が生じた場合は、まず既存のハント グループを削除し、その後で適切な名前を持つ別のハント グループを作成する必要があります。組織で複数のハント グループを使用する場合は、ハント グループに対してわかりやすい名前を使用することをお勧めします。UM ハント グループ名の長さの上限は 64 文字で、スペースも使用できます。ただし、次の文字を含めることはできません。" / \\ \[ \] : ; | = , + \* ? \< \>.
    
      - **\[UM IP ゲートウェイ\]**   このボックスを使用して UM IP ゲートウェイを選択します。このボックスには、UM ハント グループにリンクされる UM IP ゲートウェイの名前が表示されます。UM IP ゲートウェイを UM ハント グループにリンクするには、**\[参照\]** をクリックします。
    
      - **\[パイロット ID\]**   このボックスを使用して、PBX または IP PBX 上に構成されたパイロット識別子を一意に識別する文字列を指定します。このボックスでは、内線番号または SIP Uniform Resource Identifier (URI) を使用できます。このボックスは英数字の入力に対応しています。従来の PBX では数字のパイロット ID が使用されますが、一部の IP PBX と SIP 対応 PBX では SIP URI を使用できます。

4.  **\[保存\]** をクリックします。

必要に応じて、シェルで次のコマンドを実行することによって、UM ハント グループを作成できます。

    New-UMHuntGroup -Name MyUMHuntGroup -PilotIdentifier 5551234,55555 -UMDialPlan MyUMDialPlan -UMIPGateway MyUMIPGateway


> [!TIP]
> UM ハント グループの設定は構成または変更できません。UM ハント グループの構成設定を変更する場合は、それを削除して、新しい UM ハント グループを正しい設定で追加する必要があります。



## 手順 9:UM 自動応答を作成または構成する

既存の Exchange 2007 展開によっては、新しい UM 自動応答の作成が必要な場合があります。UM 自動応答を使用すると、外部や内部の発信者が UM 自動応答メニュー システムを使用してユーザーを見つけたり、社内ユーザーや組織内の部署に呼び出しを転送したりできるような音声メニュー システムを作成できます。詳細については、「[着信呼び出しへの自動応答とルーティング](automatically-answer-and-route-incoming-calls-exchange-2013-help.md)」を参照してください。

小規模な展開では、発信者がユーザーにボイス メールを残せるように UM を展開するだけで済むかもしれません。そのような展開では、自動応答を作成する必要はありません。しかし、ほとんどの場合、外部から組織に電話を掛ける発信者にとって、自動応答は非常に便利です。

必要に応じて、次のように、EAC を使用して UM 自動応答を作成できます。

1.  
    
    EAC で、**\[ユニファイド メッセージング\]** \> **\[UM ダイヤル プラン\]** に移動します。自動応答を追加する UM ダイヤル プランを選択してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

2.  **\[UM ダイヤル プラン\]** ページの **\[UM 自動応答\]** で、**\[追加\]**![\[追加\] アイコン](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "[追加] アイコン") をクリックします。

3.  **\[UM 自動応答の新規作成\]** ページで、次のボックスに値を入力します。
    
      - **\[名前\]**   このボックスを使用して、UM 自動応答の表示名を作成します。一意な UM 自動応答名を必ず指定する必要があります。ただし、EAC およびシェルでは表示専用として使用されます。自動応答を作成した後にその表示名を変更する必要がある場合は、最初に既存の UM 自動応答を削除してから、適切な名前を持つ別の自動応答を作成する必要があります。組織で複数の UM 自動応答を使用している場合は、UM 自動応答にわかりやすい名前を使用することをお勧めします。UM 自動応答名は最大で 64 文字で、空白を含めることができます。
        
        新しい UM 自動応答の名前に空白を含めることができますが、ユニファイド メッセージングを Lync Server と統合する場合は、自動応答の名前に空白を含めることはできません。したがって、表示名に空白が含まれる自動応答を Lync Server と統合する場合は、まずその自動応答を削除してから、表示名に空白を含まない別の自動応答を作成する必要があります。
    
      - **\[この自動応答を作成して有効にする\]**   このチェック ボックスをオンにすると、UM 自動応答の作成を完了した時点で、自動応答が着信呼び出しに応答するようになります。既定では、新しい自動応答は無効な状態で作成されます。UM 自動応答を無効な状態で作成する場合は、作成後に EAC またはシェルを使用して自動応答を有効にすることができます。
    
      - **\[音声コマンドに応答する自動応答を設定する\]**   このチェック ボックスをオンにすると、UM 自動応答の音声認識が有効になります。自動応答の音声認識が有効な場合、電話をかけてきた人は、音声入力またはタッチトーン入力を使用して、UM 自動応答のシステムまたはカスタム プロンプトに応答できます。既定では、作成直後の自動応答は音声認識に対応しません。発信者に対して、U.S. 英語 (en-US) 以外の言語での音声認識が有効な自動応答を使用するためには、適切な UM 言語パックをインストールし、自動応答でこの言語を使用するようにプロパティを構成する必要があります。Exchange 2013 メールボックス サーバーをインストールすると、既定で、en-US UM 言語パックがインストールされます。
    
      - **\[アクセス番号\]**   発信者が自動応答を呼び出すために使用する内線番号または電話番号を入力します。このボックスに内線番号または電話番号を入力し、**\[追加\]** をクリックしてその番号を一覧に追加します。入力した内線番号または電話番号の桁数と、関連付けられている UM ダイヤル プランで構成されている内線番号の桁数とが一致する必要はありません。これは、UM 自動応答では直通電話が許可されるためです。
        
        入力できる内線番号とアクセス番号の数に制限はありません。ただし、内線番号または電話番号を指定せずに新しい自動応答を作成することもできます。内線番号または電話番号は必須ではありません。既存の内線番号またはパイロット ID を編集または削除できます。既存の内線番号または電話番号を編集するには、**\[編集\]** をクリックします。既存の内線番号または電話番号を一覧から削除するには、**\[削除\]** をクリックします。

4.  **\[保存\]** をクリックします。

必要に応じて、シェルで次のコマンドを実行することによって、UM 自動応答を作成できます。

    New-UMAutoAttendant -Name MyUMAutoAttendant -UMDialPlan MyUMDialPlan -PilotIdentifierList 56000,56100 -SpeechEnabled $true -Status Enabled

必要に応じて、EAC を使用して既存の自動応答を構成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM ダイヤル プラン\]** に移動してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

2.  **\[UM ダイヤル プラン\]** ページの **\[UM 自動応答\]** で、表示または構成する UM 自動応答を選択してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。構成オプションを使用して、特定の自動応答設定を表示したり、機能を有効または無効にしたりします。

必要に応じて、シェルで次のコマンドを実行することによって、既存の自動応答を構成できます。

    Set-UMAutoAttendant -Identity MySpeechEnabledAA -DTMFFallbackAutoAttendant MyDTMFAA -OperatorExtension 50100 -AfterHoursTransferToOperatorEnabled $true -StaroutToDialPlanEnabled $true

## 手順 10:UM メールボックス ポリシーを作成または構成する

既存の Exchange 2007 展開によっては、新しい UM メールボックス ポリシーを作成するか、既存の UM メールボックス ポリシーを構成する必要があります。UM メールボックス ポリシーは、ユーザーに対してユニファイド メッセージングを有効にするときに必要です。UM が有効な各ユーザーのメールボックスは、単一の UM メールボックス ポリシーにリンクされている必要があります。UM メールボックス ポリシーを作成した後で、UM が有効な 1 つ以上のメールボックスをその UM メールボックス ポリシーにリンクします。これにより、その UM メールボックス ポリシーにリンクされた UM が有効なユーザーに対して、PIN の最低桁数やログオンの最大試行回数などの PIN セキュリティ設定を制御できます。詳細については、「[UM メールボックス ポリシー](um-mailbox-policies-exchange-2013-help.md)」を参照してください。

必要に応じて、EAC を使用して UM メールボックス ポリシーを作成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM ダイヤル プラン\]** に移動します。リスト ビューで、変更する UM ダイヤル プランを選択し、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

2.  **\[UM ダイヤル プラン\]** ページの **\[UM メールボックス ポリシー\]** で、**\[追加\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

3.  **\[UM メールボックス ポリシーの新規作成\]** ページで、**\[名前\]** ボックスに新しい UM メールボックス ポリシーの名前を入力します。
    

    > [!NOTE]
    > このボックスを使用して、UM メールボックス ポリシーに一意の名前を指定します。これは、EAC に表示される表示名です。UM メールボックス ポリシーを作成した後にその表示名を変更する必要が生じた場合は、まず既存の UM メールボックス ポリシーを削除し、その後で適切な名前を持つ別の UM メールボックス ポリシーを作成する必要があります。UM が有効なユーザーが関連付けられている UM メールボックス ポリシーは削除できません。UM メールボックス ポリシー名は必須ですが、表示のためにのみ使用されます。組織で複数の UM メールボックス ポリシーを使用する場合もあるため、わかりやすい名前を使用することをお勧めします。UM メールボックス ポリシー名の長さの上限は 64 文字で、スペースも使用できます。ただし、次の文字を含めることはできません。" / \ [ ] : ; | = , + * ? &lt; &gt;.



4.  **\[保存\]** をクリックします。
    

    > [!NOTE]
    > UM メールボックス ポリシーを保存すると、PIN ポリシー、ボイス メール機能、および保護ボイス メールの設定を含むすべての既定の設定が有効になります。先ほど作成した UM メールボックス ポリシーの既定の設定をカスタマイズまたは変更するには、<STRONG>Set-UMMailbox</STRONG> コマンドレットまたは EAC を使用します。



必要に応じて、次のコマンドを実行することによって、シェルで UM メールボックス ポリシーを作成できます。

    New-UMMailboxPolicy -Name MyUMMailboxPolicy -UMDialPlan MyUMDialPlan

必要に応じて、EAC を使用して既存の UM メールボックス ポリシーを構成できます。

1.  EAC で、**\[ユニファイド メッセージング\]** \> **\[UM ダイヤル プラン\]** に移動してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

2.  **\[UM ダイヤル プラン\]** ページの **\[UM メールボックス ポリシー\]** で、表示または構成する UM メールボックス ポリシーを選択してから、**\[編集\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。構成オプションを使用して、特定の UM メールボックス ポリシー設定を表示したり、機能を有効または無効にしたりします。

必要に応じて、シェルで次のコマンドを実行することによって、既存の UM メールボックス ポリシーを構成できます。

    Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 8 -MaxLogonAttempts 12 -MinPINLength 8 -PINHistoryCount 10 -PINLifetime 60 -ResetPINText "The PIN used to allow you access to your mailbox using Outlook Voice Access has been reset."

## 手順 11:既存の UM 対応メールボックスを Exchange 2013 に移動する

Exchange 2007 ユニファイド メッセージングで、組織内のユーザーがボイス メールを使用できるようにすると、UM プロパティの既定のセットがユーザーに適用されるため、ユーザーが UM 機能を利用できるようになります。詳細については、「[ユーザーのボイス メール](voice-mail-for-users-exchange-2013-help.md)」を参照してください。

アップグレード プロセス中に、Exchange 2007 メールボックス サーバーと Exchange 2013 メールボックス サーバーの両方で UM に対応したメールボックスを設定する期間があります。ただし、すべての UM 対応ユーザーを Exchange 2013 メールボックス サーバーに移動する場合は、Exchange 2013 サーバーから EAC またはシェル内の **New-MoveRequest** コマンドレットを使用して、ユーザーの PIN を含むすべてのプロパティと設定を保存する必要があります。

移動要求は、メールボックスをあるメールボックス データベースから別のデータベースに移動する処理です。ローカル移動要求は、単一フォレスト内で発生するメールボックスの移動です。メールボックスの移動方法については、次のトピックを参照してください。

  - [Exchange 2013 でのメールボックスの移動](mailbox-moves-in-exchange-2013-exchange-2013-help.md)

  - [New-MoveRequest](https://technet.microsoft.com/ja-jp/library/dd351123\(v=exchg.150\))

  - [New-MigrationBatch](https://technet.microsoft.com/ja-jp/library/jj219166\(v=exchg.150\))

  - [メールボックスの移動](https://go.microsoft.com/fwlink/p/?linkid=296351)

EAC を使用して Exchange 2007 メールボックスを Exchange 2013 メールボックス サーバーに移動するには:

1.  EAC で、**\[受信者\]** \> **\[移行\]** の順にクリックしてから、**\[追加\]**![編集アイコン](images/Bb124582.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "編集アイコン") をクリックします。

2.  **ローカル メールボックスの移動の新規作成**ウィザードで、移動するユーザーを選択し、**\[OK\]** をクリックしてから、**\[次へ\]** をクリックします。

3.  **\[移動の構成\]** ページで、新しいバッチの名前を指定します。アーカイブ メールボックスとメールボックス データベースの場所に必要なオプションを選択してから、**\[新規\]** をクリックします。

シェルを使用して Exchange 2007 メールボックスを Exchange 2013 メールボックス サーバーに移動するには、次のコマンドを実行します。

    New-MoveRequest -Identity 'tony@alpineskihouse.com' -TargetDatabase "DB01"

## 手順 12:新しいユーザーを UM に対して有効にする、または、既存の UM 対応ユーザーの設定を構成する

ユーザーに対してユニファイド メッセージングを有効にするには、そのユーザーがメールボックスを持っている必要があります。しかし、既定では、メールボックスを持つユーザーでは UM が有効になっていません。ユーザーの UM を有効にすると、ユーザーの UM プロパティおよびボイス メール機能を管理、変更、および構成できるようになります。EAC またはシェルを使用して、ユーザーの UM を有効にすることができます。詳細については、「[ユーザーのボイス メール](voice-mail-for-users-exchange-2013-help.md)」を参照してください。

ユーザーの UM を有効にする場合は、ユーザーのメールボックスにボイス メールが送信されたときに UM で使用される 1 つ以上の内線番号を定義して、ユーザーが Outlook Voice Access を使用できるようにする必要があります。ユーザーの UM を有効にすると、ユーザーのメールボックスに 2 つ目の内線番号を追加することも、ユーザーのメールボックスに Exchange ユニファイド メッセージング (EUM) プロキシ アドレスを構成することで、2 つ目の内線番号を変更または削除することも、さらには EAC でユーザーの追加の内線番号または 2 つ目の内線番号を追加または削除することもできます。内線番号、E.164 番号、または SIP アドレスを追加、変更、または削除するには、「[ボイス メールが有効なユーザーの手順](voice-mail-enabled-user-procedures-exchange-2013-help.md)」を参照してください。

EAC を使用してユーザーをユニファイド メッセージングに対して有効にするには:

1.  EAC で、**\[受信者\]** をクリックします。

2.  リスト ビューで、ユニファイド メッセージングに対して有効にするメールボックスを持つユーザーを選択します。

3.  詳細ウィンドウの **\[電話と音声の機能\]** で **\[有効にする\]** をクリックします。

4.  **\[UM メールボックスを有効にする\]** ページで、**\[UM メールボックス ポリシー\]** の横にある **\[参照\]** ボタンをクリックし、リストからユーザーに割り当てる UM メールボックス ポリシーを見つけて **\[OK\]** をクリックします。

5.  **\[UM メールボックスを有効にする\]** ページで、次のボックスに入力します。
    
      - **\[SIP アドレスまたは E.164 番号\]**   ユーザーの SIP アドレスまたは E.164 番号を入力します。これらのオプションは、ユニファイド メッセージングを有効にしているユーザーが、SIP URI または E.164 ダイヤル プランのいずれかにリンクされた UM メールボックス ポリシーに割り当てられている場合に使用できます。ユーザーが内線電話番号ダイヤル プランに関連付けられている場合は、ユーザーの SIP アドレスまたは E.164 番号を追加することはできません。SIP URI または E.164 ダイヤル プランにリンクされた UM メールボックス ポリシーにユーザーを割り当てる場合でも、ユーザーの内線番号を入力する必要があります。この内線番号は、ユーザーが Outlook Voice Access を使用して自分のメールボックスにアクセスするときに使用されます。このボックスで構成する桁数は、SIP URI または E.164 ダイヤル プランで構成された桁数と一致する必要があります。
    
      - **\[内線番号\]**   UM を有効にするユーザーの内線番号を入力します。
        
        ユーザーの有効な内線番号を入力する必要があります。番号はダイヤル プランで指定された桁数と一致している必要があります。数字または 1 ～ 20 の桁数のみ入力できます。一般的な内線番号の長さは 3 ～ 7 桁です。内線の桁数は、ユーザーに割り当てられた UM メールボックス ポリシーにリンクされたダイヤル プランに設定されています。
    
      - **\[PIN 設定\]** で次のように入力します。
        
          - **\[PIN を自動的に生成する\]**   このボタンをクリックして、UM が有効なユーザーが Outlook Voice Access を通してボイス メール アクセスを使用するための PIN を自動的に生成します。これは既定の設定です。このボタンをクリックすると、ユーザーに割り当てられた UM メールボックス ポリシーで構成された PIN ポリシーに基づいて、自動的に PIN が生成されます。ユーザーの PIN を保護するために、この設定を使用することをお勧めします。PIN は、ユーザーの UM を有効化した後、ユーザーに送信される案内メッセージでユーザーに送られます。既定では、ユーザーは初めてメールボックスにサインインしてボイス メールを取得するときに、この PIN を変更する必要があります。
        
          - **\[PIN の入力\]**   このボタンをクリックして、ユーザーがボイス メール システムにアクセスするときに使用する PIN を手動で指定します。PIN は、この UM を有効化したユーザーと関連付けられた UM メールボックス ポリシーで構成された PIN ポリシー設定に従っている必要があります。たとえば、UM メールボックス ポリシーが 7 桁以上の PIN だけを受け付けるように構成されている場合、このボックスに入力する PIN は 7 桁以上である必要があります。
        
          - **\[初回サインイン時にユーザーに PIN のリセットを要求する\]**   このチェック ボックスを選択して、ユーザーが初めて電話から Outlook Voice Access を使用してボイス メール システムにアクセスするときにボイス メールの PIN を強制的にリセットします。ユーザーは自分がより覚えやすい PIN を入力するよう要求されます。強制的に、UM が有効なユーザーは初回サインイン時に PIN を変更し、データおよび受信トレイへの権限のないアクセスから保護することをセキュリティ上お勧めします。このチェック ボックスは既定でオンになっています。

6.  **\[UM メールボックスの有効化\]** ページで設定を確認します。**\[完了\]** をクリックして、ユーザーのユニファイド メッセージングを有効にします。構成を変更するには、**\[戻る\]** をクリックします。

シェルでユニファイド メッセージングに対してユーザーを有効にし、次のコマンドを実行します。

    Enable-UMMailbox -Identity tonysmith@contoso.com -UMMailboxPolicy MyUMMailboxPolicy -Extensions 51234 -PIN 5643892 -NotifyEmail administrator@contoso.com -PINExpired $true

必要に応じて、EAC を使用して UM に対して有効にしたユーザーを構成できます。

1.  EAC で、**\[受信者\]** \> **\[メールボックス\]**に移動します。

2.  リスト ビューで、UM メールボックス ポリシーを変更するメールボックスを選択します。

3.  \[詳細\] ウィンドウの **\[電話機能と音声機能\]** \> **\[ユニファイド メッセージング\]** で **\[詳細の表示\]** をクリックします。

4.  **\[UM メールボックス\]** ページで、**\[UM メールボックスの設定\]** をクリックして、UM が有効な既存のユーザーの次の UM のプロパティを表示または変更します。
    
      - **\[PIN の状態\]**   この表示専用ボックスには、ユーザーのメールボックスの状態が表示されます。既定では、ユーザーの UM が有効になっている場合、PIN の状態は **\[ロックされていません\]** と表示されます。ただし、ユーザーが正しくない Outlook Voice Access PIN を複数回入力した場合、状態は **\[ロックアウト済み\]** と表示されます。
    
      - **\[UM メールボックス ポリシー\]**   このボックスには、UM が有効なユーザーに関連付けられている UM メールボックス ポリシーの名前が表示されます。**\[参照\]** をクリックすると、この UM メールボックスに関連付ける UM メールボックス ポリシーを見つけて指定することができます。
    
      - **\[オペレーターの個人内線番号\]**   ユーザーに対するオペレーターの個人内線番号を指定するには、このボックスを使用します。既定では、内線電話番号は構成されていません。内線電話番号は 1 ～ 20 文字の長さで指定できます。このオプションを有効にすると、UM が有効になっているユーザーに対する着信呼び出しは、このボックスに指定した内線番号に転送されます。
        
        ダイヤル プランと自動応答には、別の種類のオペレーター内線番号を構成することができます。ただし、それらの内線番号は通常、企業全体にわたる受付またはオペレーター用のものです。オペレーターの個人内線番号設定は、管理者のアシスタントまたは個人秘書が特定のユーザーの着信呼び出しに応答する場合などに使用できます。

5.  **\[UM メールボックス\]** ページの **\[その他の内線番号\]** で、ユーザーの内線番号を追加、変更、および表示できます。
    
      - 内線番号を追加するには、**\[追加\]**![\[追加\] アイコン](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "[追加] アイコン") をクリックします。**\[内線番号の追加\]** ページで、**\[参照\]** を使用して UM ダイヤル プランを選択し、**\[内線番号\]** ボックスに内線番号を入力します。
    
      - 内線番号を削除するには、削除する内線番号を選択し、**\[削除\]**![\[削除\] アイコン](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "[削除] アイコン") をクリックします。

6.  変更を加えた場合は、**\[保存\]** をクリックします。

必要に応じて、シェルで次のコマンドを実行することによって、UM に対して有効にしたユーザーを構成できます。

    Set-UMMailbox -Identity tony@contoso.com -CallAnsweringAudioCodec Wma -CallAnsweringRulesEnabled $false -FaxEnabled $false -UMSMSNotificationOption VoiceMail

## 手順 13:すべての着信呼び出しを Exchange 2013 クライアント アクセス サーバーに送信するように VoIP ゲートウェイ、IP PBX、および SIP 対応 PBX を構成する

Exchange 2013 クライアント アクセス サーバーおよびメールボックス サーバーをインストールすると、両方のサーバーが自動的に有効になり、着信および発信音声呼び出しに応答したり、ボイス メール メッセージを指定された受信者にルーティングしたりできます。Exchange 2013 クライアント アクセス サーバーおよびメールボックス サーバーをインストールして、ユニファイド メッセージングを展開する場合は、Exchange 2013 クライアント アクセス サーバーまたはメールボックス サーバーの UM ダイヤル プランへのリンクや追加は必要ありません。Exchange 2013 クライアント アクセス サーバーおよびメールボックス サーバーはすべての着信呼び出しに応答し、UM ダイヤル プランを使用してユーザーを探します。

Exchange 2013 クライアント アクセス サーバーは、すべての着信呼び出しまたはユニファイド メッセージングに対する Session Initiation Protocol (SIP) 要求のエントリ ポイントです。Exchange 2013 クライアント アクセス サーバー上で SIP 要求を処理するサービスが UM 呼び出しルーター サービスであり、組織内の各 Exchange 2013 クライアント アクセス サーバー上で動作します。

Exchange 2013 UM にアップグレードする場合は、テレフォニー ネットワーク内の PBX に接続する単一または複数の VoIP ゲートウェイを設置して構成するか、セッション開始プロトコル (SIP) 対応 PBX または IP PBX を設置して構成する必要があります。

Exchange 2013 UM にアップグレードするプロセスの最後の手順は、着信呼び出し (ユーザーにボイス メールを残したい発信者、Outlook Voice Access に電話をかけた UM が有効なユーザーからの呼び出し、UM 自動応答に電話をかけた発信者からの呼び出しなど) を Exchange 2013 クライアント アクセス サーバーに送信するように VoIP ゲートウェイ、IP PBX、または SIP 対応 PBX を構成することです。これらすべての呼び出しが最初に VoIP ゲートウェイ、IP PBX、または SIP 対応 PBX で受信されてから、Exchange 2013 組織内の Exchange 2013 クライアント アクセス サーバーに転送されます。詳細については、以下のリソースを参照してください。

  -  
    [UM サービス](um-services-exchange-2013-help.md)

  -  
    [サポートされる VoIP ゲートウェイ、IP Pbx、および PBX の構成に関する注意事項](configuration-notes-for-supported-voip-gateways-ip-pbxs-and-pbxs-exchange-2013-help.md)

  -  
    [Exchange 2013 のテレフォニー アドバイザー](telephony-advisor-for-exchange-2013-exchange-2013-help.md)

## 手順 14:Exchange 2007 ユニファイド メッセージング サーバー上の通話応答を無効にする

Exchange 2007 ユニファイド メッセージング サーバー上で UM を無効にするか、ダイヤル プランから UM サーバーを削除することによって、通話応答を無効にすることができます。UM を無効にした場合は、ユニファイド メッセージング サーバーが着信呼び出しに応答できなくなります。すべての呼び出しを直ちに切断するか、既存の呼び出しが処理されるまで待ってからユニファイド メッセージング サーバーを無効にするかを選択できます。サーバーですべての着信呼び出し処理が完了できるように、通話応答を無効にしてからダイヤル プランからサーバーを削除します。

Exchange 管理コンソールを使用して Exchange 2007 UM サーバー上のユニファイド メッセージングを無効にするには:

1.  EMC のコンソール ツリーで、**\[サーバーの構成\]** \> **\[ユニファイド メッセージング\]** に移動します。

2.  結果ウィンドウで、無効にするユニファイド メッセージング サーバーを選択します。

3.  操作ウィンドウで、次のいずれかをクリックします。
    
    1.  **\[直ちに無効にする\]** オプションを選択すると、ユニファイド メッセージング サーバーは、そのサーバーに接続されているすべての呼び出しを切断します。
    
    2.  **\[呼び出し完了後に無効にする\]** オプションを選択すると、ユニファイド メッセージング サーバーは新しい呼び出しを受け付けなくなりますが、すべての呼び出しが処理されるまで無効にはなりません。

4.  確認ダイアログ ボックスで、**\[はい\]** をクリックして続行します。

シェルを使用して Exchange 2007 UM サーバー上のユニファイド メッセージングを無効にするには、次のコマンドを実行します。

    Disable-UMServer -Identity MyUMServer -Immediate $true


> [!TIP]
> Exchange 2007 UM サーバーから <STRONG>Disable-UMServer</STRONG> コマンドレットを使用するか、Exchange 2013 メールボックス サーバーから <STRONG>Disable-UMService</STRONG> コマンドレットを使用して通話応答を無効にすることができます。



## 手順 15:ダイヤル プランから Exchange 2007 ユニファイド メッセージングを削除する

呼び出しを処理するには、少なくとも 1 つの UM ダイヤル プランに Exchange 2007 ユニファイド メッセージング サーバーが追加されている必要があります。1 つの UM サーバーを複数の UM ダイヤル プランに追加できます。UM ダイヤル プランから Exchange 2007 UM サーバーを削除できます。ダイヤル プランから UM サーバーを削除すると、その UM サーバーは、UM が有効なユーザーのために、呼び出しへの応答や UM 呼び出しの処理を行わなくなります。

Exchange 管理コンソールを使用して、ダイヤル プランから Exchange 2007 UM サーバーを削除するには:

1.  EMC のコンソール ツリーで、**\[サーバーの構成\]** \> **\[ユニファイド メッセージング\]** に移動します。

2.  結果ウィンドウで、ユニファイド メッセージング サーバーを選択します。

3.  操作ウィンドウで **\[プロパティ\]** をクリックします。

4.  **\[UM の設定\]** タブの **\[関連付けられたダイヤル プラン\]** セクションで、**\[削除\]** をクリックします。

5.  確認ダイアログ ボックスで、**\[はい\]** をクリックして、UM ダイヤル プランから Exchange 2007 ユニファイド メッセージング サーバーの削除を確認します。

6.  **\[OK\]** をクリックして、プロパティ ウィンドウを閉じます。

シェルを使用してダイヤル プランから Exchange 2007 UM サーバーを削除するには、次のコマンドを実行します。

    $dp= Get-UMDialPlan "MySIPDialPlan"
    $s=Get-UMServer -id MyUMServer
    $s.dialplans-=$dp.identity
    Set-UMServer -id MyUMServer -dialplans:$s.dialplans

この例では、SIP URI ダイヤル プランが 3 つあります:SipDP1、SipDP2、SipDP3 です。この例では、SipDP3 ダイヤル プランから `MyUMServer` という名前の UM サーバーを削除します。

    Set-UMServer -id MyUMServer -DialPlans SipDP1,SipDP2

この例では、SIP URI ダイヤル プランが 2 つあります:SipDP1 と SipDP2 です。この例では、SipDP2 ダイヤル プランから `MyUMServer` という名前の UM サーバーを削除します。

    Set-UMServer -id MyUMServer -DialPlans SipDP1


> [!TIP]
> Exchange 2007 ユニファイド メッセージング サーバー上のシェルで <STRONG>Set-UMServer</STRONG> コマンドレットを使用するか、Exchange 2013 メールボックス サーバー上で <STRONG>Set-UMService</STRONG> コマンドレットを使用して、単一または複数のダイヤル プランから Exchange 2007 UM サーバーを削除できます。たとえば、すべてのダイヤル プランから UM サーバーを削除するには、次のコマンドを実行します。<CODE>Set-UMServer -identity MyUMServer -DialPlan $null</CODE>



## 正常な動作を確認する方法

ユニファイド メッセージングをセットアップしたら、次の内容を確認して正しい動作を保証します。

  - ボイス メールに対して有効にしたユーザーが Outlook Web App または Outlook にサインインして、ユニファイド メッセージングのウェルカム メッセージを表示できる。

  - UM ユーザーが音声メッセージを受信できる。

  - UM ユーザーが Outlook Voice Access 番号に電話を掛けて、電子メール、予定表アイテム、およびボイス メールを聞くことができる。

  - UM が組織外からの呼び出しをルーティングしており、電話を掛けることができる。
