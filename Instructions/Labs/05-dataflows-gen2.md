---
lab:
  title: Microsoft Fabric でデータフロー (Gen2) を作成して使用する
  module: Ingest Data with Dataflows Gen2 in Microsoft Fabric
---

# Microsoft Fabric でデータフロー (Gen2) を作成する

Microsoft Fabric では、データフロー (Gen2) でさまざまなデータ ソースに接続し、Power Query Online で変換を実行します。 その後はデータ パイプラインで使用して、レイクハウスやその他の分析ストアにデータを取り込んだり、Power BI レポートのデータセットを定義したりできます。

このラボの目的は、データフロー (Gen2) のさまざまな要素を紹介することであり、エンタープライズに存在する可能性のある複雑なソリューションを作成することではありません。 このラボの所要時間は**約 30 分**です。

> **注**: この演習を完了するには、Microsoft Fabric ライセンスが必要です。 無料の Fabric 使用版ライセンスを有効にする方法の詳細については、[Fabric の開始](https://learn.microsoft.com/fabric/get-started/fabric-trial)に関するページを参照してください。 これを行うには、Microsoft の "学校" または "職場" アカウントが必要です。** ** お持ちでない場合は、[Microsoft Office 365 E3 以降の試用版にサインアップ](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans)できます。

## ワークスペースの作成

Fabric でデータを操作する前に、Fabric 試用版を有効にしてワークスペースを作成します。

1. `https://app.fabric.microsoft.com` で [Microsoft Fabric](https://app.fabric.microsoft.com) にサインインし、 **[Power BI]** を選択します。
2. 左側のメニュー バーで、 **[ワークスペース]** を選択します (アイコンは &#128455; のようになります)。
3. 任意の名前で新しいワークスペースを作成し、Fabric 容量を含むライセンス モード ("試用版"、*Premium*、または *Fabric*) を選択します。**
4. 新しいワークスペースを開くと、次に示すように空です。

    ![Power BI の空のワークスペースを示すスクリーンショット。](./Images/new-workspace.png)

## レイクハウスを作成する

ワークスペースが作成されたので、次にポータルで **Data Engineering** エクスペリエンスに切り替えて、データの取り込み先となるデータ レイクハウスを作成します。

1. Power BI ポータルの左下で、 **[Power BI]** アイコンを選択し、**Data Engineering** エクスペリエンスに切り替えます。

2. **[Data Engineering]** ホーム ページで、任意の名前で新しい**レイクハウス**を作成します。

    1 分ほどすると、新しい空のレイクハウスが作成されます。

 ![新しいレイクハウス。](./Images/new-lakehouse.png)

## データフロー (Gen2) を作成してデータを取り込む

レイクハウスが作成されたので、それにデータを取り込む必要があります。 これを行う 1 つの方法は、"抽出、変換、読み込み" (ETL) プロセスをカプセル化するデータフローを定義することです。**

1. ワークスペースのホーム ページで、 **[新しいデータフロー Gen2]** を選択します。 数秒後、次に示すように、新しいデータフローの Power Query エディターが開きます。

 ![新しいデータフロー。](./Images/new-dataflow.png)

2. **[Text ファイルまたは CSV ファイルからイポート]** を選択し、次の設定を使用して新しいデータ ソースを作成します。
 - **ファイルへのリンク**: 選択**
 - **ファイル パスまたは URL**: `https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/orders.csv`
 - **接続**: 新しい接続の作成
 - **データ ゲートウェイ**: (なし)
 - **認証の種類**: 匿名

3. **[次へ]** を選択してファイル データをプレビューし、データ ソースを**作成**します。 Power Query エディターには、次に示すように、データ ソースと、データを書式設定するためのクエリ ステップの初期セットが表示されます。

 ![Power Query エディターでのクエリ。](./Images/power-query.png)

4. ツール バーのリボンで、 **[列の追加]** タブを選択します。次に、 **[カスタム列]** を選択し、次に示すように、数式 `Date.Month([OrderDate])` に基づいた数値を含む **MonthNo** という名前の新しい列を作成します。

 ![Power Query エディターでのカスタム列。](./Images/custom-column.png)

 カスタム列を追加するステップがクエリに追加され、その結果としての列がデータ ペインに表示されます。

 ![カスタム列ステップでのクエリ。](./Images/custom-column-added.png)

> **ヒント:** 右側の [クエリの設定] ペインで、 **[適用したステップ]** に各変換ステップが含まれていることがわかります。 下部にある **[ダイアグラム フロー]** ボタンを切り替えて、ステップの視覚的な図を有効にすることもできます。
>
> ステップを上下に移動することや、歯車アイコンを選択して編集することができます。また、各ステップを選択してプレビュー ウィンドウに変換が適用されることを確認できます。

## データフローのデータ同期先を追加する

1. ツール バーのリボンで、 **[ホーム]** タブを選択します。次に、 **[データ同期先の追加]** ドロップダウン メニューで **[Lakehouse]** を選択します。

   > **メモ:** このオプションが淡色表示されている場合は、既にデータ同期先が設定されている可能性があります。 Power Query エディターの右側にある [クエリの設定] ペインの下部でデータ同期先を確認します。 同期先が既に設定されている場合は、歯車を使用して変更できます。

2. **[データ変換先に接続]** ダイアログ ボックスで、接続を編集し、Power BI 組織アカウントを使用してサインインし、データフローからレイクハウスへのアクセスに使用する ID を設定します。

 ![データ同期先の構成ページ。](./Images/dataflow-connection.png)

3. **[次へ]** を選択し、使用可能なワークスペースの一覧でご自分のワークスペースを見つけて、この演習の開始時に作成したレイクハウスを選択します。 次に、**orders** という名前の新しいテーブルを指定します。

   ![データ同期先の構成ページ。](./Images/data-destination-target.png)

   > **メモ:** **[デスティネーション設定]** ページの [列マッピング] で OrderDate と MonthNo が選択されておらず、"日付/時刻に変更" という情報メッセージが表示されていることに注意してください。**

   ![データ同期先の設定ページ。](./Images/destination-settings.png)

1. このアクションを取り消し、Power Query Online で OrderDate 列と MonthNo 列戻ります。 列ヘッダーを右クリックし、 **[型の変更]** を選択します。

    - OrderDate = 日付/時刻
    - MonthNo = 整数

1. ここで、前に説明したプロセスを繰り返して、レイクハウスの同期先を追加します。

8. **[デスティネーション設定]** ページで、 **[追加]** を選択し、設定を保存します。  同期先である**レイクハウス**は、Power Query エディターのクエリにアイコンとして示されます。

   ![レイクハウスを同期先に持つクエリ。](./Images/lakehouse-destination.png)

9. **[発行]** を選択してデータフローを発行します。 次に、ワークスペースにデータフロー **Dataflow 1** が作成されるまで待ちます。

1. 発行されたら、ワークスペース内のデータフローを右クリックし、 **[プロパティ]** を選択して、データフローの名前を変更できます。

## パイプラインにデータフローを追加する

データフローをアクティビティとしてパイプラインに含めることができます。 パイプラインを使用してデータ インジェストと処理アクティビティを調整し、1 つのスケジュールされたプロセスでデータフローを他の種類の操作と組み合わせることができます。 パイプラインは、Data Factory エクスペリエンスなど、いくつかの異なるエクスペリエンスで作成できます。

1. Fabric 対応ワークスペースで、引き続き **Data Engineering** エクスペリエンスを使用していることを確認します。 **[新規]** 、 **[データ パイプライン]** の順に選択し、メッセージが表示されたら、**Load data** という名前の新しいパイプラインを作成します。

   パイプライン エディターが開きます。

   ![空のデータ パイプライン。](./Images/new-pipeline.png)

   > **ヒント**: データのコピー ウィザードが自動的に開いた場合は、閉じます。

2. **[パイプライン アクティビティの追加]** を選択し、パイプラインに **データフロー** アクティビティを追加します。

3. 新しい **Dataflow1** アクティビティを選択した状態で、 **[設定]** タブの **[データフロー]** ドロップダウン リストで、"**Dataflow 1**" (先ほど作成したデータフロー) を選択します

   ![データフロー アクティビティを含むパイプライン。](./Images/dataflow-activity.png)

4. **[ホーム]** タブで、 **&#128427;** ( *[保存]* ) アイコンを使用してパイプラインを保存します。
5. **[&#9655; 実行]** ボタンを使用してパイプラインを実行し、完了するまで待ちます。 これには数分かかることがあります。

   ![データフローが正常に完了したパイプライン。](./Images/dataflow-pipeline-succeeded.png)

6. 左端のメニュー バーで、レイクハウスを選択します。
7. **[テーブル]** の **[...]** メニューで、 **[更新]** を選択します。 次に、 **[テーブル]** を展開し、データフローによって作成された **orders** テーブルを選択します。

   ![データフローによって読み込まれたテーブル。](./Images/loaded-table.png)

> **ヒント**: Power BI Desktop の "Dataflows コネクタ" を使用して、データフローで行われたデータ変換に直接接続します。**
>
> また、追加の変換を行い、新しいデータセットとして公開し、特殊なデータセットの対象ユーザーを想定して配布することもできます。
>
>![Power BI データ ソース コネクタ](Images/pbid-dataflow-connectors.png)

## リソースをクリーンアップする

Microsoft Fabric でのデータフローの調査が完了したら、この演習用に作成したワークスペースを削除できます。

1. ブラウザーで Microsoft Fabric に移動します。
1. 左側のバーで、ワークスペースのアイコンを選択して、それに含まれるすべての項目を表示します。
1. ツール バーの **[...]** メニューで、 **[ワークスペースの設定]** を選択します。
1. **[その他]** セクションで、 **[このワークスペースの削除]** を選択します。
1. 変更を Power BI Desktop に保存しないでください。既に保存されている場合は、.pbix ファイルを削除してください。