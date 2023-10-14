---
lab:
  title: Fabric で Data Activator を使う
  module: Get started with Data Activator in Microsoft Fabric
---

# Fabric で Data Activator を使う

Microsoft Fabric の Data Activator は、データで発生している内容に基づいてアクションを実行します。 Data Activator を使用すると、データを監視し、データの変更に対応するトリガーを作成できます。

このラボの所要時間は約 **30** 分です。

> **注**: この演習を完了するには、Microsoft Fabric ライセンスが必要です。 無料の Fabric 試用版ライセンスを有効にする方法の詳細については、[Fabric の概要](https://learn.microsoft.com/fabric/get-started/fabric-trial)に関するページを参照してください。 これを行うには、Microsoft の "*学校*" または "*職場*" アカウントが必要です。 お持ちでない場合は、[Microsoft Office 365 E3 以降の試用版にサインアップ](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans)できます。

## ワークスペースの作成

Fabric でデータを操作する前に、Fabric 試用版を有効にしてワークスペースを作成してください。

1. `https://app.fabric.microsoft.com` で [Microsoft Fabric](https://app.fabric.microsoft.com) にサインインし、 **[Power BI]** を選択してください。
2. 左側のメニュー バーで、 **[ワークスペース]** を選択します (アイコンは &#128455; に似ています)。
3. 任意の名前で新しいワークスペースを作成し、Fabric 容量を含むライセンス モード ("試用版"、*Premium*、または *Fabric*) を選択してください。**
4. 新しいワークスペースを開くと次に示すように空のはずです。

    ![Power BI の空のワークスペースのスクリーンショット。](./Images/new-workspace.png)

このラボでは、Fabric の Data Activator を使用して *Reflex* を作成します。 Data Activator には、Data Activator の機能を探索するために使用できる便利なサンプル データ セットが用意されています。 このサンプル データを使用して、いくつかのリアルタイム データを分析する *Reflex* を作成し、条件が満たされたときに電子メールを送信するトリガーを作成します。

> **注**: Data Activator のサンプル プロセスにより、バックグラウンドでランダム データが生成されています。 条件とフィルターの複雑さが増すほど、それらのトリガーにかかる時間が長くなります。 グラフにデータが表示されない場合は、数分待ってからページを最新の情報に更新してください。 ラボを続行するために、グラフにデータが表示されるのを待つ必要はありません。

## シナリオ

このシナリオでは、あなたはさまざまな製品を販売および出荷する会社のデータ アナリストです。  あなたは、Redmond 市へのすべての出荷と販売のデータに対して責任を負います。 あなたは、配達中の荷物を監視する Reflex を作成したいと考えています。 出荷する製品のカテゴリの 1 つは、輸送中に特定の温度で冷蔵する必要がある医療処方薬です。 あなたは、処方薬が入った荷物の温度が特定のしきい値より高いか低い場合に、出荷部門に電子メールを送信する Reflex を作成したいと考えています。 理想的な温度は 33 度から 41 度の間です。 Reflex イベントには既に同様のトリガーが含まれているため、Redmond 市に発送される荷物専用にトリガーを作成します。 それでは作業を始めましょう。

## Reflex を作成する

1. **Microsoft Fabric** エクスペリエンス ポータルで、**Data Activator** エクスペリエンスを選択します。これには、まず、画面の左下隅にある現在の Fabric エクスペリエンス アイコンを選択し、メニューから **[Data Activator]** を選択します。 たとえば、次のスクリーンショットでは、現在の Fabric エクスペリエンスは **Power BI** です。

    ![Data Activator エクスペリエンスを選択しているスクリーンショット。](./Images/data-activator-select-experience.png)

1. これで、Data Activator のホーム画面が表示されます。 右下の Fabric エクスペリエンス アイコンも、Data Activator のアイコンに変わりました。 **[Reflex (プレビュー)]** ボタンを選択して、新しい Reflex を作成しましょう。

    ![Data Activator のホーム画面のスクリーンショット。](./Images/data-activator-home-screen.png)

1. 実際の運用環境では、独自のデータを使用します。 しかし、このラボでは、Data Activator によって提供されるサンプル データを使用します。 **[サンプル データの使用]** ボタンを選択して、Reflex の作成を完了します。

    ![Data Activator のデータの取得画面のスクリーンショット。](./Images/data-activator-get-started.png)

1. 既定では、Data Activator によって *Reflex YYYY-MM-DD hh:mm:ss* という名前の Reflex が作成されます。 ワークスペースには複数の Reflex が存在する可能性があるため、既定の Reflex の名前をよりわかりやすいものに変更する必要があります。 この例では、左上隅にある現在の Reflex 名の横にあるプルダウンを選び、名前を ***Contoso Shipping Reflex*** に変更します。

    ![Data Activator Reflex のホーム画面のスクリーンショット。](./Images/data-activator-reflex-home-screen.png)

これで Reflex が作成されたので、トリガーとアクションの追加を開始できます。

## Reflex のホーム画面の理解を深める

Reflex のホーム画面は、"デザイン" モードと "データ "モードの 2 つのセクションに分かれています。** ** モードを選択するには、画面の左下にあるそれぞれのタブを選択します。  [デザイン] モード タブでは、トリガー、プロパティ、イベントを使用してオブジェクトを定義します。** [データ] モード タブでは、データ ソースを追加し、Reflex で処理するデータを表示できます。** [デザイン] モード タブを見てみましょう。このタブは、Reflex の作成時に既定で開かれます。**

### デザイン モード

現在 "デザイン" モードではない場合は、画面左下の **[デザイン]** タブを選択します。**

![Data Activator Reflex デザイン モードのスクリーンショット。](./Images/data-activator-design-tab.png)

"デザイン" モードに慣れるため、画面のさまざまなセクション、トリガー、プロパティ、イベントを選択します。** 各セクションの詳細については、以降のセクションで説明します。

### データ モード

現在 "データ" モードではない場合は、画面左下の **[データ]** タブを選択します。** 実際の例では、EventStreams と Power BI ビジュアルから独自のデータ ソースをここに追加します。 このラボでは、Data Activator によって提供されるサンプル データを使用しています。 このサンプルは、パッケージの配信状態を監視する 3 つの EventStream が既に設定されています。

![Data Activator Reflex データ モードのスクリーンショット。](./Images/data-activator-data-tab.png)

それぞれのイベントを選択し、ストリームで使用されているデータを確認してください。

![Data Activator Reflex データ モード イベントのスクリーンショット。](./Images/data-activator-get-data-tab-event-2.png)

Reflex にトリガーを追加しますが、その前に、新しいオブジェクトを作成しましょう。

## オブジェクトの作成

実際のシナリオでは、Data Activator サンプルに *Package* というオブジェクトが既に含まれているため、この Reflex 用に新しいオブジェクトを作成する必要がない場合があります。 しかし、このラボでは、作成方法を示すために新しいオブジェクトを作成します。 *Redmond Packages* という名前の新しいオブジェクトを作成しましょう。

1. 現在 "データ" モードではない場合は、画面左下の **[データ]** タブを選択します。**

1. ***Package In Transit*** (輸送中の荷物) イベントを選択します。 *PackageId*、*Temperature*、*ColdChainType*、*City*、*SpecialCare* の各列の値に細心の注意を払います。 これらの列を使用してトリガーを作成します。

1. 右側に [データの割り当て] ダイアログがまだ開いていない場合は、画面の右側にある **[データの割り当て]** ボタンを選びます。**

    ![Data Activator Reflex データ モードの [データの割り当て] ボタンのスクリーンショット。](./Images/data-activator-data-tab-assign-data-button.png)

1. [データの割り当て] ダイアログで、***[新しいオブジェクトに割り当てる]*** タブを選び、次の値を入力します。**

    - **[オブジェクト名]** : *Redmond Packages*
    - **[キー列の割り当て]** : *PackageId*
    - **プロパティの割り当て**: *City、ColdChainType、SpecialCare、Temperature*

    ![Data Activator Reflex データ モードの [データの割り当て] ダイアログのスクリーンショット。](./Images/data-activator-data-tab-assign-data.png)

1. **[保存]** 、 **[保存してデザイン モードに切り替える]** の順に選択します。

1. これで "デザイン" モードに戻ります。** ***Redmond Packages*** という名前の新しいオブジェクトが追加されています。 この新しいオブジェクトを選択し、その *Events* を展開し、**Package in Transit** イベントを選択します。

    ![新しいオブジェクトを含む Data Activator Reflex デザイン モードのスクリーンショット。](./Images/data-activator-design-tab-new-object.png)

次はトリガーを作成します。

## トリガーを作成する

トリガーで実行する操作を確認してみましょう。"あなたは、処方薬が入った荷物の温度が特定のしきい値より高いか低い場合に、出荷部門に電子メールを送信する Reflex を作成したいと考えています。理想的な温度は 33 度から 41 度の間です。Reflex イベントには既に同様のトリガーが含まれているため、Redmond 市に発送される荷物専用にトリガーを作成します。"**

1. **Redmond Packages** オブジェクトの *Package In Transit* イベント内で、上部メニューの **[新しいトリガー]** ボタンを選択してください。 新しいトリガーは、既定の名前の *Untitled* で作成されます。トリガーをより適切に定義するために、名前を ***Medicine temp out of range*** (薬の温度が範囲外) に変更します。

    ![Data Activator Reflex デザインで新しいトリガーを作成するスクリーンショット。](./Images/data-activator-trigger-new.png)

1. 次は、Reflex をトリガーするプロパティまたはイベント列を選択します。 オブジェクトの作成時にいくつかのプロパティを作成したため、 **[既存のプロパティ]** ボタンを選び、***Temperature*** プロパティを選択します。 

    ![Data Activator Reflex デザインでプロパティを選択するスクリーンショット。](./Images/data-activator-trigger-select-property.png)

    このプロパティを選択すると、過去の温度値のサンプルを含むグラフが返されます。

    ![Data Activator プロパティの履歴値のグラフのスクリーンショット。](./Images/data-activator-trigger-property-sample-graph.png)

1. 次は、このプロパティからトリガーする条件の種類を決定する必要があります。 この場合、温度が 41 度を超えるか 33 度を下回ると Reflex をトリガーします。 数値範囲を探しているので、 **[数値]** ボタンを選択し、 **[終了範囲]** 条件を選択します。

    ![Data Activator Reflex デザインで条件の種類を選択するスクリーンショット。](./Images/data-activator-trigger-select-condition-type.png)

1. 次に、条件の値を入力する必要があります。 範囲値として「***33***」と「***44***」を入力します。 "数値範囲の終了条件" を選択しているため、温度が *33* 度を下回るか *44* 度を超えたときにトリガーを起動する必要があります。**

    ![Data Activator Reflex デザインで条件値を入力するスクリーンショット。](./Images/data-activator-trigger-select-condition-define.png)

1. ここまでは、トリガーを起動するプロパティと条件を定義しましたが、必要なすべてのパラメーターが含まれているわけではありません。 *city* が **Redmond** の場合と、*special care* の種類の **Medicine** に対してのみトリガーが起動することを引き続き確認する必要があります。 次は、これらの条件に対していくつかのフィルターを追加してみましょう。  **[フィルターの追加]** ボタンを選択し、プロパティを ***City*** に設定し、リレーションシップを ***[等しい]*** に設定し、値として「***Redmond***」と入力してください。 次に、***SpecialCare*** プロパティを持つ新しいフィルターを追加し、これを ***[等しい]*** に設定し、値として「***Medicine***」と入力してください。

    ![Data Activator Reflex デザインでフィルターを追加するスクリーンショット。](./Images/data-activator-trigger-select-condition-add-filter.png)

1. 薬が冷蔵されていることを確認するために、フィルターをもう 1 つ追加しましょう。 **[フィルターの追加]** ボタンを選択し、***ColdChainType*** プロパティを設定し、これを ***[等しい]*** に設定し、値として「***Refrigerated***」と入力してください。

    ![Data Activator Reflex デザインでフィルターを追加するスクリーンショット。](./Images/data-activator-trigger-select-condition-add-filter-additional.png)

1. もう少しです。 トリガーの起動時に実行するアクションを定義するだけです。 この場合は、出荷部門にメールを送信します。 **[Email]** ボタンを選択します。

    ![Data Activator でアクションを追加するスクリーンショット。](./Images/data-activator-trigger-select-action.png)

1. メール アクションに次の値を入力します。

    - **[送信先]** : 現在のユーザー アカウントが既定で選択されているはずです。このラボではこれで問題ありません。
    - **[件名]** : "Redmond Medical Package が許容温度範囲外です"**
    - **見出し**: "温度が高すぎるか低すぎます"**
    - **[追加情報]** : チェックボックスの一覧から *Temperature* プロパティを選択します。

    ![[データ アクティベーターの定義] アクションのスクリーンショット。](./Images/data-activator-trigger-define-action.png)

1. **[保存]** を選び、上部のメニューから **[開始]** を選択します。

これで、Data Activator でトリガーを作成して開始しました。

## トリガーを更新して停止する

このトリガーの唯一の問題は、トリガーによって温度を含むメールが送信されたが、荷物の *PackageId* が送信されなかったことです。 次に進み、*PackageId* を含めるようにトリガーを更新しましょう。

1. **Redmond Packages** オブジェクトから **Packages in Transit** イベントを選択し、上部のメニューから **[新しいプロパティ]** を選択します。

    ![Data Activator でオブジェクトからイベントを選択するスクリーンショット。](./Images/data-activator-trigger-select-event.png)

1. *Packages in Transit* イベントから列を選択して、**PackageId** プロパティを追加しましょう。 プロパティ名を *Untitled* から *PackageId* に忘れずに変更してください。

    ![Data Activator でプロパティを作成するスクリーンショット。](./Images/data-activator-trigger-create-new-property.png)

1. トリガー アクションを更新しましょう。 **Medical temp out of range** トリガーを選択し、下部の **[実行]** セクションまでスクロールし、 **[追加情報]** を選択して、**PackageId** プロパティを追加します。 **[保存]** ボタンはまだ選択しないでください。

    ![Data Activator でトリガーするプロパティを追加するスクリーンショット。](./Images/data-activator-trigger-add-property-existing-trigger.png)

1. トリガーを更新したので、トリガーを更新して保存しないのが正しいアクションですが、このラボでは逆の操作を行い、 **[更新]** ボタンの代わりに **[保存]** ボタンを選択して、何が起こるかを確認します。 [更新] ボタンを選択する必要がある理由は、トリガーの [更新] を選択すると、トリガーが保存され、現在実行中のトリガーが新しい条件で更新されるためです。** ** [保存] ボタンを選択しただけの場合、トリガーの更新を選択するまで、現在実行中のトリガーが新しい条件で更新されることはありません。** 次に進み、 **[保存]** ボタンを選択します。

1. [更新] の代わりに [保存] を選択したため、画面の上部に "プロパティの更新が利用可能です。トリガーに最新の変更が適用されるように、今すぐ更新してください。" というメッセージが表示されていることがわかります。** ** ** そのメッセージにはさらに [更新] ボタンも含まれています。** 次に進み、 **[保存]** ボタンを選択します。

    ![Data Activator でトリガーを更新するスクリーンショット。](./Images/data-activator-trigger-updated.png)

1. 上部のメニューから **[停止]** ボタンを選択して、トリガーを停止してください。

## リソースをクリーンアップする

この演習では、Data Activator でトリガーを含む Reflex を作成しました。 これで、Data Activator インターフェイスと、Reflex とそのオブジェクト、トリガー、プロパティの作成方法について理解できたはずです。

Data Activator Reflex の探索が完了したら、この演習用に作成したワークスペースを削除できます。

1. 左側のバーで、ワークスペースのアイコンを選択して、それに含まれるすべての項目を表示します。
2. ツール バーの **[...]** メニューで、 **[ワークスペースの設定]** を選択します。
3. **[その他]** セクションで、 **[このワークスペースの削除]** を選択します。