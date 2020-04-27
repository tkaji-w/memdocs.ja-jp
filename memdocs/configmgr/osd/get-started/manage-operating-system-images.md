---
title: OS イメージの管理
titleSuffix: Configuration Manager
description: Windows イメージ (WIM) ファイルに格納されている OS イメージを管理する方法について説明します。
ms.date: 07/26/2019
ms.prod: configuration-manager
ms.technology: configmgr-osd
ms.topic: conceptual
ms.assetid: fab13949-371c-4a4c-978e-471db1e54966
author: aczechowski
ms.author: aaroncz
manager: dougeby
ms.openlocfilehash: 190a203494cecfd28c198197f3a582adff745265
ms.sourcegitcommit: bbf820c35414bf2cba356f30fe047c1a34c5384d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/21/2020
ms.locfileid: "81708890"
---
# <a name="manage-os-images-with-configuration-manager"></a>Configuration Manager で OS イメージを管理する

*適用対象:Configuration Manager (Current Branch)*

Configuration Manager の OS イメージは、Windows イメージ (WIM) ファイル形式で格納されます。 これらのイメージは、新しい OS をコンピューターにインストールして構成するために使用する参照ファイルとフォルダーのコレクションを圧縮したものです。 OS イメージは、多くの OS 展開シナリオで必要です。


## <a name="os-image-types"></a>OS イメージの種類

[既定の OS イメージ](#default-image)を使用するか、構成する[参照コンピューター](#bkmk_capture)から OS イメージをビルドすることができます。 参照コンピューターを構築するときに、OS ファイル、ドライバー、サポート ファイル、ソフトウェア更新プログラム、ツール、およびアプリケーションを OS に追加します。 それをキャプチャしてイメージ ファイルを作成します。

### <a name="default-image"></a>既定の画像

Windows のインストール ファイルには、既定の OS イメージが含まれています。 このイメージは、ドライバーの標準セットを含む基本的な OS イメージです。 既定の OS イメージを使用する場合は、タスク シーケンス ステップを使用してアプリをインストールし、デバイスに OS をインストールした後に、その他の構成を行います。 Windows ソース ファイル (`\Sources\install.wim`) で、既定の OS イメージを見つけます。  

#### <a name="default-image-advantages"></a>既定のイメージの長所

- イメージのサイズは、キャプチャしたイメージのサイズよりも小さいです。  

- タスク シーケンス ステップを使用したアプリのインストールと構成は、より動的です。 たとえば、デバイスを再イメージ化しなくても、タスク シーケンスで構成とインストールするアプリを変更できます。  

#### <a name="default-image-disadvantages"></a>既定のイメージの短所

- OS のインストールで、より時間がかかることがあります。 アプリケーションのインストールとその他の構成は、OS のインストールの完了後に行われます。  


### <a name="captured-image-from-a-reference-computer"></a><a name="bkmk_capture"></a> 参照コンピューターからキャプチャされたイメージ

カスタマイズされた OS イメージを作成するには、目的の OS で参照コンピューターを構築します。 次に、アプリケーションをインストールし、設定を構成します。 参照コンピューターから OS イメージをキャプチャして、WIM ファイルを作成します。 参照コンピューターを手動で構築するか、タスク シーケンスを使用して構築ステップの一部またはすべてを自動化します。 詳細については、[OS イメージのカスタマイズ](customize-operating-system-images.md)に関するページを参照してください。  

#### <a name="captured-image-advantages"></a>キャプチャしたイメージの長所

- インストール時間は、既定のイメージを使用する場合より短縮されます。 たとえば、キャプチャした OS イメージを使用して、アプリケーションをプレインストールできます。 こうすると、タスク シーケンス ステップを使用して、後から同じアプリケーションをインストールする必要がなくなります。  

#### <a name="captured-image-disadvantages"></a>キャプチャしたイメージの短所

- イメージのサイズは、既定のイメージのサイズよりも大きくなる可能性があります。  

- アプリケーションとツールの更新プログラムが必要な場合は、新しいイメージを作成する必要があります。  


## <a name="add-an-os-image"></a><a name="BKMK_AddOSImages"></a> OS イメージを追加する  

OS イメージを使用するには、最初にご使用の Configuration Manager サイトにイメージを追加します。

1. Configuration Manager コンソールで、 **[ソフトウェア ライブラリ]** ワークスペースに移動し、 **[オペレーティング システム]** を展開して、 **[オペレーティング システム イメージ]** ノードを選択します。  

2. リボンの **[ホーム]** タブの **[作成]** グループで、 **[オペレーティング システム イメージの追加]** を選択します。 このアクションにより、オペレーティング システム イメージの追加ウィザードが開始されます。  

3. **[データ ソース]** ページで、次の情報を指定します。

    - OS イメージ ファイルへのネットワーク **パス**。 たとえば、`\\server\share\path\image.wim` となります。

    - **指定された WIM ファイルから特定のイメージ インデックスを抽出し**、一覧からイメージ インデックスを選択します。<!--3719699--> バージョン 1902 以降では、このオプションにより、ファイル内のすべてのイメージ インデックスではなく、1 つのインデックスが自動的にインポートされます。 このオプションを使用すると、イメージ ファイルが小さくなり、オフライン サービスが高速になります。 さらに、ソフトウェア更新プログラムの適用後に小さくなったイメージ ファイルに対して、[イメージ サービスを最適化する](#bkmk_resetbase)プロセスもサポートされます。  

        > [!Note]  
        > Configuration Manager では、ソース イメージ ファイルは変更されません。 同じソース ディレクトリに新しいイメージ ファイルが作成されます。
        >
        > この抽出プロセスは、60 GB を超えるなど、非常に大きなイメージ ファイルの場合は失敗することがあります。 DISM エラーは `Not enough storage is available to process this command.` です。Configuration Manager によって使用されるコマンド ラインは、smsprov.log および dism.log にあります。 手動で同じコマンドを実行して、イメージをインポートします。<!-- SCCMDocs-pr issue 3502 -->  

    - バージョン 1906 以降では、クライアントでコンテンツを事前キャッシュする場合は、イメージの**アーキテクチャ**と**言語**を指定します。 詳細については、「[コンテンツの事前キャッシュを構成する](../deploy-use/configure-precache-content.md)」を参照してください。<!--4224642-->  

4. **[全般]** ページで、以下の情報を指定します。 この情報は、複数の OS イメージがあるときに区別するのに役立ちます。  

    - **名前**:イメージの一意の名前。 既定では、名前は WIM ファイル名に由来します。  

    - **バージョン**:省略可能なバージョン識別子。 このプロパティは、イメージの OS バージョンにする必要はありません。 多くの場合は、パッケージの組織のバージョンです。  

    - **コメント**:省略可能な簡単な説明。  

5. ウィザードを完了します。  

このコンソール ウィザードに相当する PowerShell コマンドレットについては、「[New-CMOperatingSystemImage (New-CMOperatingSystemImage)](https://docs.microsoft.com/powershell/module/configurationmanager/new-cmoperatingsystemimage?view=sccm-ps)」をご覧ください。

次に、OS イメージを配布ポイントに配布します。  


## <a name="distribute-content-to-distribution-points"></a><a name="BKMK_DistributeBootImages"></a> コンテンツを配布ポイントに配布する  

他のコンテンツと同じように、配布ポイントに OS イメージを配布します。 タスク シーケンスを展開する前に、少なくとも 1 つの配布ポイントに OS イメージを配布します。 詳細については、「[コンテンツの配布](../../core/servers/deploy/configure/deploy-and-manage-content.md#bkmk_distribute)」をご覧ください。  


[!INCLUDE [Apply software updates to an image](includes/wim-apply-updates.md)]


## <a name="prepare-the-os-image-for-multicast-deployments"></a><a name="BKMK_OSImageMulticast"></a> マルチキャスト展開用に OS イメージを準備する  

マルチキャスト展開を使用して、複数のコンピューターで同時に OS イメージをダウンロードできるようにします。 イメージは、各クライアントが個別の接続を経由して配布ポイントからイメージのコピーをダウンロードするのではなく、配布ポイントによりクライアントにマルチキャストされます。 OS の展開方法に [[マルチキャストを使用した、ネットワーク経由での Windows の展開]](../deploy-use/use-multicast-to-deploy-windows-over-the-network.md) を選択した場合は、マルチキャストをサポートするように OS イメージを構成します。 次に、イメージをマルチキャスト対応の配布ポイントに配布します。

1. Configuration Manager コンソールで、 **[ソフトウェア ライブラリ]** ワークスペースに移動し、 **[オペレーティング システム]** を展開して、 **[オペレーティング システム イメージ]** ノードを選択します。  

2. マルチキャスト対応の配布ポイントに配布する OS イメージを選択します。  

3. リボンの **[ホーム]** タブの **[プロパティ]** グループで、 **[プロパティ]** を選択します。  

4. **[配布の設定]** タブに切り替えて、次のオプションを構成します。  

    - **このパッケージのマルチキャストでの送信を許可する (WinPE のみ)** :マルチキャストを使用して OS イメージを同時に展開するには、Configuration Manager にこのオプションを選択します。  

    - **マルチキャスト パッケージを暗号化する**:サイトでイメージを配布ポイントに送信する前に暗号化するかどうかを指定します。 イメージに秘匿性の高い情報が含まれている場合は、このオプションを使用してください。 イメージが暗号化されていないと、その内容がネットワーク上にクリア テキストで表示されます。 未承認ユーザーにイメージの内容がインターセプトされて表示される可能性があります。  

    - **このパッケージをマルチキャストでのみ送信する**:マルチキャスト セッションの間だけ配布ポイントでイメージを展開するかどうかを指定します。  

         **[このパッケージをマルチキャストでのみ送信する]** を選択する場合は、タスク シーケンスの展開オプションを **[実行中のタスク シーケンスでコンテンツが必要になったときにローカルにダウンロードする]** に指定する必要もあります。 詳細については、「 [Deploy a task sequence](../deploy-use/deploy-a-task-sequence.md)」をご覧ください。  

5. **[OK]** を選択して設定を保存し、イメージのプロパティを閉じます。  