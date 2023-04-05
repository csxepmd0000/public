# APIにおけるテストコード作成

環境：Visual Studio 2022

前もって考えるべきこと：
1. どの部分をテストする
1. テストパターン
1. 実際値と予想結果（実際値どうやって取得するか、予想結果網羅したか等考える必要もある）

今回はAPIの処理の部分にテストコードを書いていく。なのでxxServiceTest.csのようなファイルを作成する。

namespaceとclassはさておき、class内に、まずテスト主体となるxxServiceを宣言し、初期化を行う。

（Service側の初期化はDIでやっているけど、こっちは何故newでやっているかというと、シンプルだから？）

初期化：

<code>private FileService _service;
private readonlMock<IBlobRepository_blobRepository = new();
private readonlMock<IFileRepository_fileRepository = new();
[SetUp]
public void Setup() =_service = new FileServi(_blobRepository.Object_fileRepository.Object);
</code>

実際のテストケース：

<code>

 [TestCase(TestName = "テストケース1")]

</code>

テストメソッド：
テストファイル自体生きているか確認（絶対通るケース）

<code>

public void TestTrue()

{
    Assert.Pass();
}
</code>

単純テスト：
とある変数の値が予想通りか

<code>
[TestCase(TestName = "TestString")]

public void TestString()

{
    var actual = _service.テストしたいメソッド;
    Assert.That(actual, Is.EqualTo(予想結果));
}

</code>

分岐テスト：
とある処理には複数パータンがあって、すべての分岐を通して確認する必要性が出てくる。

<code>
[TestCase(TestName = "pattern1")]

public void Pattern1()

{
    var actual = _service.テストしたいメソッド(Pattern1の場合の引数);
    Assert.That(actual, Is.EqualTo(予想結果Pattern1));
}

[TestCase(TestName = "pattern2")]

public void Pattern2()

{
    var actual = _service.テストしたいメソッド(Pattern2の場合の引数);
    Assert.That(actual, Is.EqualTo(予想結果Pattern2));
}

</code>

注意：TestCaseのTestName文字列には"."などファイル管理システムに戸惑わせる符号を使うと、テスト結果がうまく取得されない可能性がある。

テストの実行：
VSのメニューバーには「テスト」というタブがあり、そのなかの「すべてのテストを実行」を押す。

あるいは同タブの「テストエクスプローラー」を呼び出してそこでテストを実行する。（この場合では、単一テストケースをテストすることができるので、かなり時間節約できる。）

