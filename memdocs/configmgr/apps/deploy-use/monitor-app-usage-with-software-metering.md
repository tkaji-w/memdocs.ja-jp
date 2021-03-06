---
title: ソフトウェア使用状況測定でアプリの使用状況を監視する
titleSuffix: Configuration Manager
description: Configuration Manager のソフトウェア使用状況測定で使用できる操作について説明します。
ms.date: 09/20/2017
ms.prod: configuration-manager
ms.technology: configmgr-app
ms.topic: conceptual
ms.assetid: b1fdaee2-2816-4447-94cd-609f6948f215
author: mestew
ms.author: mstewart
manager: dougeby
ms.openlocfilehash: 13d8cce1811c4a27f6a3e7485eea4d65132c2888
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81689250"
---
# <a name="software-metering-in-configuration-manager"></a>Configuration Manager のソフトウェア メータリング

*適用対象:Configuration Manager (Current Branch)*

このトピックには、Configuration Manager のソフトウェア使用状況測定を使用するときに実行する可能性がある、すべての操作の参照が含まれています。

> [!IMPORTANT]
>  ソフトウェア使用状況測定は、ファイル名の末尾が **.exe**の Windows PC デスクトップ アプリを監視するときに使用されます。 Windows モダン アプリ (Windows 8 で使用されるアプリなど) は監視されません。

##  <a name="prerequisites-for-software-metering"></a>ソフトウェア使用状況測定の前提条件
ソフトウェア使用状況測定には外部依存関係はありません。依存関係は製品内にのみ存在します。

|依存関係|説明|
|----------------|----------------------|
|ソフトウェア使用状況測定のクライアント設定|ソフトウェア使用状況測定を使用するには、クライアント設定 **[クライアントのソフトウェア使用状況の測定を有効にする]** が有効で、コンピューターに展開されている必要があります。 ソフトウェア使用状況測定の設定を階層内のすべてのコンピューターに展開したり、カスタム設定をコンピューター グループに展開したりできます。 このトピックの「**ソフトウェア使用状況測定の構成**」を参照してください。|
|レポート サービス ポイント|ソフトウェア使用状況の測定レポートを表示するには、事前にレポート サービス ポイントを構成する必要があります。 詳細については、「[レポートの概要](../../core/servers/manage/introduction-to-reporting.md)」をご覧ください。|

##  <a name="configure-software-metering"></a>ソフトウェア使用状況測定の構成
 この手順に従って、ソフトウェア使用状況の測定に既定のクライアント設定を構成し、階層内のすべてのコンピューターに適用します。 これらの設定を一部のコンピューターのみに適用する場合は、カスタム クライアント デバイス設定を作成して、ソフトウェア使用状況測定を使用するコンピューターを含むコレクションに展開します。 カスタムのデバイス設定の作成方法について詳しくは、「[Configure client settings](../../core/clients/deploy/configure-client-settings.md)」 (クライアント設定の構成) を参照してください。

1. Configuration Manager コンソールで、 **[管理]**  >  **[クライアント設定]**  >  **[既定のクライアント設定]** の順にクリックします。

2. **[ホーム]** タブの **[プロパティ]** グループで、 **[プロパティ]** をクリックします。

3. **既定の設定の** ダイアログ ボックスで、をクリックして **ソフトウェア使用状況測定**です。

4. **[デバイスの設定]** 一覧で、次を構成します。

   -   **クライアントのソフトウェア使用状況の測定を有効にする**:ソフトウェア使用状況の測定を有効にするには **[True]** を選択します。

   -   **[データ コレクションのスケジュール]** :ソフトウェア使用状況測定データをクライアント コンピューターから収集する頻度を構成します。 既定値を使用してすべて **7 日間**  をクリックしてまたは **スケジュール** カスタム スケジュールを指定します。

5. **[OK]** をクリックして **[既定の設定]** ダイアログ ボックスを閉じます。

   クライアント コンピューターは、次にクライアント ポリシーをダウンロードするときに、これらの設定で構成されます。 1 つのクライアントのポリシーの取得を開始する場合は、「[Manage clients](../../core/clients/manage/manage-clients.md)」 (クライアントの管理) を参照してください。

##  <a name="create-software-metering-rules"></a>ソフトウェア使用状況測定規則の作成
 ソフトウェア使用状況測定規則の作成ウィザードを使って Configuration Manager サイトの新しいソフトウェア使用状況測定規則を作成します。

1.  Configuration Manager コンソールで、 **[資産とコンプライアンス]**  >  **[ソフトウェア使用状況の測定]** の順にクリックします。

3.  **ホーム** ] タブで、 **作成** グループで、[ **ソフトウェア使用状況測定規則の作成**です。

4.  ソフトウェア使用状況測定規則の作成ウィザードの **[全般]** ページで、次の情報を指定します。

    -   **名前** - ソフトウェア使用状況測定規則の名前。 一意でわかりやすい名前にする必要があります。

        > [!NOTE]
        >  ソフトウェア メータリングの規則は、規則に含まれるファイル名が異なるのであれば同じ名前を使用できます。

    -   **ファイル名** - 使用状況を測定するプログラム ファイルの名前。 クリックして **参照** を表示する、 **開く**  ダイアログ ボックスを使用するプログラム ファイルを選択できます。

        > [!NOTE]
        >  **[ファイル名]** フィールドに実行可能なファイル名を入力しても、そのファイルが存在するかどうか、または必要なヘッダー情報がそのファイルに含まれているかどうかはチェックされません。 なるべく、 **[参照]** ボタンを使用して使用状況を測定する実行可能ファイルを指定するようにしてください。
        >
        >  ファイル名にはワイルドカード文字は使用できません。
        >
        >  **[元のファイル名]** に値を指定している場合、このボックスは任意指定です。

    -   **元のファイル名** - 使用状況を測定する実行可能なファイルの名前。 この名前は、ファイル名そのものではなくファイルのヘッダーに含まれる情報と一致するので、名前を変更した実行可能ファイルを元の名前で使用状況測定する場合に役立ちます。

        > [!NOTE]
        >  元のファイル名にはワイルドカード文字は使用できません。
        >
        >  **[ファイル名]** に値を指定している場合、このボックスは任意指定です。

    -   **バージョン** -使用状況を測定する実行可能ファイルのバージョン。 ワイルドカード文字 ( &#42; ) を任意の文字列の代わりに使用できます。または、ワイルドカード文字 (? ) を任意の 1 文字の代わりに使用できます。 すべてのバージョンの実行可能ファイルの使用状況を測定するには、既定値 ( &#42; ) を使用します。

    -   **言語** - 使用状況を測定する実行可能ファイルの言語。 既定値は使用しているオペレーティング システムの現在の設定言語です。 使用状況を測定する実行可能ファイルを **[参照]** ボタンをクリックして選択する場合、そのファイルのヘッダーに言語情報が存在すれば、このボックスは自動的に入力されます。 ファイルのすべての言語バージョンの使用状況を測定するには、ドロップダウン リストで **[任意]** を選択します。

    -   **説明** - ソフトウェア使用状況測定規則の説明 (任意指定)。

    -   **このソフトウェアの使用状況測定規則を次のクライアントに適用** – 使用状況測定規則で指定されたサイトに割り当てられているクライアントを階層内のすべてのクライアントにソフトウェアを適用するかどうかを **サイト**  ボックスの一覧です。

5.  続行するには、 **[次へ]** をクリックします。

6.  設定を確認して確定し、ウィザードを完了してソフトウェア使用状況測定規則を作成します。 新しいソフトウェア メータリングの規則が表示される、 **ソフトウェア使用状況測定** 内のノード、 **資産とコンプライアンス** ワークスペース。

##  <a name="configure-automatic-software-metering-rules"></a>自動ソフトウェア使用状況測定規則の構成
 Configuration Manager では、ソフトウェア使用状況の測定を構成して、サイト データベースに保持されている最新の利用状況インベントリ データから無効になったソフトウェア使用状況測定規則を自動的に生成できます。 このインベントリ データを構成して、指定した割合のコンピューターで使用されるアプリケーションのみに測定規則が作成されるように構成できます。 サイトで許可する自動的に生成されるソフトウェア メータリング規則の最大数も指定できます。

> [!NOTE]
>  既定では、自動的に作成されたソフトウェア使用状況測定規則は、無効になっています。 これらの規則から使用状況データを収集するには、規則を有効にする必要があります。

1.  Configuration Manager コンソールで、 **[資産とアプライアンス]**  >  **[ソフトウェア使用状況の測定]** の順にクリックしてから、 **[ホーム]** タブの **[設定]** グループで、 **[ソフトウェア使用状況測定規則のプロパティ]** をクリックします。

3.  **[ソフトウェア使用状況測定規則のプロパティ]** をクリックして、次を構成します。

    -   **[データの保管日数]** - ソフトウェア使用状況測定規則で生成されたデータがサイト データベースに保持される期間を指定します。 既定値は **90** 日です。

    -   **[最新の使用状況インベントリ データから、無効にした使用状況測定規則を自動的に作成する]** を有効にします。

    -   **[階層内でプログラムを使用する必要のあるコンピューターの割合 (%) が次の値に達すると、ソフトウェア使用状況測定規則が自動的に作成されるようにする]** - 既定値は **[10]** パーセントです。

    -   **[階層内にあるソフトウェア使用状況測定規則が次の数に達すると、規則の自動作成を無効にする]** - 既定値は **[100]** 規則です。

4.  **[OK]** をクリックして **[既定の設定]** ダイアログ ボックスを閉じます。

##  <a name="manage-software-metering-rules"></a>ソフトウェア使用状況測定規則の管理
 **資産とコンプライアンス** ワークスペースで、 **ソフトウェア使用状況測定**, 使用状況測定規則を管理するソフトウェアを選択し、管理タスクを選択しています。

 次の表で、管理タスクに関する詳細と、各タスクを選択する前に必要となる追加情報について説明します。

|管理タスク|詳細|
|---------------------|-------------|
|**有効化**<br /><br /> **無効化**|ソフトウェア使用状況測定規則の有効化または無効化 この設定はに従ってクライアント コンピュータにダウンロード、 **クライアント ポリシーのポーリング間隔** で、 **クライアント ポリシー** (既定では 60 分ごと) のクライアント設定のセクションです。<br /><br /> 「[Configure client settings](../../core/clients/deploy/configure-client-settings.md)」 (クライアント設定の構成) を参照してください。|

##  <a name="monitor-software-metering"></a>ソフトウェア使用状況測定の監視
 Configuration Manager のソフトウェア使用状況の測定機能には、ソフトウェア使用状況測定操作に関する情報を監視できる複数の組み込みレポートが含まれています。 これらのレポートは、 **ソフトウェア使用状況測定**に分類されています。

 Configuration Manager でのレポートの構成方法について詳しくは、「[レポートの概要](../../core/servers/manage/introduction-to-reporting.md)」をご覧ください。

 また、ソフトウェア使用状況の測定機能によって Configuration Manager データベースに格納されたデータを使用して、クエリとコレクションを作成することもできます。

 Configuration Manager のコレクションについて詳しくは、「[コレクションの概要](../../core/clients/manage/collections/introduction-to-collections.md)」を参照してください。

 Configuration Manager のクエリについて詳しくは、「[クエリの概要](../../core/servers/manage/introduction-to-queries.md)」を参照してください。

##  <a name="security-and-privacy-for-software-metering"></a>ソフトウェア使用状況測定のセキュリティとプライバシー

### <a name="security-issues-for-software-metering"></a>ソフトウェア使用状況の測定に関するセキュリティの問題
 攻撃者は、ソフトウェア使用状況測定のクライアント設定が無効化されていても管理ポイントが受け付けてしまう、無効なソフトウェア使用状況測定情報を Configuration Managerに送ってくる可能性があります。 これにより多くの使用状況測定規則が階層内全体でレプリケートされ、ネットワークおよび Configuration Manager サイト サーバーでサービス拒否攻撃を引き起こす可能性があります。

 攻撃者が無効なソフトウェア使用状況測定データを作成できるため、ソフトウェア使用状況測定情報は必ず正しいと考えないでください。

 ソフトウェア使用状況の測定は、クライアント設定として既定で有効になっています。

###  <a name="privacy-information-for-software-metering"></a>ソフトウェア使用状況の測定に関するプライバシー情報
 ソフトウェア メータリングにより、クライアント コンピューターでのアプリケーションの使用状況が監視されます。 既定では、ソフトウェア使用状況の測定は有効になっています。 どのアプリケーションの使用状況を監視するかを構成する必要があります。 使用状況測定情報は Configuration Manager データベースに格納されます。 情報は管理ポイントへの転送中に暗号化されますが、暗号化された形式で Configuration Manager データベースに格納されるわけではありません。

 この情報は、サイト メンテナンス タスクによって削除されるまで、データベースに保存されます。 **期限切れのソフトウェア使用状況測定データの削除** (5 日ごと) および **期限切れのソフトウェア メータリング概要データの削除** (270 日ごと)。 削除間隔は構成できます。 使用状況測定情報がマイクロソフトに送信されることはありません。

 ソフトウェア使用状況の測定を構成する前に、プライバシー要件について検討してください。

## <a name="example-scenario-for-using-software-metering"></a>ソフトウェア使用状況測定の使用のシナリオ例
 このセクションでは、次のビジネス要件の解決に役立つソフトウェア使用状況測定規則の例を作成します。

- 社内にある特定のアプリのコピーの数を調べる

- アプリケーションの未使用のコピーを検出する

- 特定のアプリケーションを定期的に使用しているユーザーを調べる

  Woodgrove Bank では、標準のオフィス生産性スイートとして Microsoft Office 2010 を導入しました。 しかし、レガシ アプリケーションをサポートするために、一部のコンピューターでは引き続き Microsoft Office Word 2003 を実行する必要があります。 IT 部門は、レガシ アプリケーションが使用されなくなった場合に、これらの Word 2003 を削除することによって、サポートおよびライセンス管理のコストを削減したいと思っています。 また、ヘルプ デスクも、レガシ アプリケーションを使用しているユーザーを特定したいと思っています。

  Woodgrove Bank の IT システム マネージャーは、Configuration Manager のソフトウェア使用状況測定を使って、これらのビジネス目標を達成します。 管理者は次のアクションを実行します。

- ソフトウェア使用状況測定の前提条件をチェックし、レポート サービス ポイントがインストールされて機能していることを確認します。
- ソフトウェア使用状況測定のための既定のクライアント設定を構成します。<br>管理者はソフトウェア使用状況測定を有効にし、7 日ごとに 1 回という既定のデータ収集スケジュールを使用します。<br>管理者は、ソフトウェア インベントリのクライアント設定 **[これらのファイルの種類をインベントリ対象とする]** を構成することで、拡張子 .exe を持つファイルに対してインベントリを実行するようにソフトウェア インベントリを構成します。<br>管理者は、**woodgrove.exe** という名前の新しいソフトウェア使用状況測定の規則を追加して、レガシ アプリケーションを監視します。
- 7 日間待機します。その後、クライアント コンピューターによって **woodgrove.exe** 実行可能ファイルの使用状況データのレポートが開始されます。
- 管理者は、Configuration Manager レポートの **[メータリングされたソフトウェア プログラムすべてのインストール ベース]** を使用して、アプリケーション **woodgrove.exe** がどのコンピューターに読み込まれたのかを確認します。
- 6 か月後、管理者は、ソフトウェア使用状況測定の規則と過去 6 か月以内の日付を指定して、 **[指定した日以降、インストール済みのメータリングされたプログラムが実行されていないコンピューター]** レポートを実行します。 このレポートでは、6 か月以内にこのプログラムを実行しなかった 120 台のコンピューターが特定されます。
- 管理者は、特定したコンピューター上でレガシ アプリケーションが不要であることを確認するために、さらにいくつかのチェックを行います。 その後、管理者はレガシ アプリケーションと Word 2003 をこれらのコンピューターからアンインストールします。<br>管理者は、レポート **[特定のメータリングされたソフトウェア プログラムを実行したユーザー]** を実行し、レガシ アプリケーションを使用し続けているユーザーのリストをヘルプ デスクに提供します。
- 管理者は、ソフトウェア使用状況測定レポートを週次で確認し続け、必要に応じて改善策を実行します。

  この一連のアクションの結果として、不要になったアプリケーションを削除することで、IT サポートとライセンスのコストが削減されます。 さらに、ヘルプ デスクは、必要としていた、レガシ アプリケーションを実行するユーザーの一覧を入手することができました。
