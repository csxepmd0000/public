# npm run testでエラー

このエラー発生した際に、ローカルでnpm run testを打って詳細を確認していこう。
karmaのjasmineページが表示されると思いますが、そこのエラー文を読解して問題個所を特定する。
<br>

---

## Error: Export of name "CdkTextareaAutosize" Not Found!

結論から言うと、手順は以下ようになる：
1. .tsファイルのexport側に追加したものを宣言する
2. .specファイルにimport追記
3. .specファイルのdeclarationsに加える

<a href="https://stackoverflow.com/questions/54759915/failed-unexpected-directive-contactdetailscomponent-imported-by-the-module-d">stackoverflow-類似問題</a>

---

### CdkTextareaAutosizeとは何

テキストエリアをユーザーの入力を検知し、改行に伴いテキストエリアが自動的に調整してくれるディレクティブ。<a href="https://material.angular.io/cdk/text-field/overview">参考：material/text-field</a><br>
Angular Materialが提供されている<strong>text-field</strong>パッケージに入っている。<br><br>
実装背景：ユーザーが一つのテキストエリアに入力をしたことを例として挙げる。ユーザーの入力と伴い、入力した文字数もどんどん増えていくが、テキストエリアはそのまま固定高さとなっているのはほとんどである。入力している際に、テキストエリアの上の入力した文書を参照したい場合はスクロールバーを利用しなければならない。<br>このディレクティブを活用することで、テキストエリアが自動拡張できるようになる。<br><br>
問題起因：CdkTextareaAutosizeをHTMLとtsファイルに実装したら、その時点でHTMLファイルのcompileは無事に実装され、動作も確認できると思う。しかし、問題点はnpm run testにうまくいかないこと。npm run testを打って、出てきたテストページではエラーが起きていることが確認できる。<br><br>
問題思考：エラーがテスト段階で起きているので、テストファイルが不足している可能性が高い。<br>
specファイルはtsファイルをimportしているので、tsファイル側の宣言が足りないとspecファイルが怒られる。<br><br>
問題解決：specファイルにおいて、.tsファイルでimportしたモジュールをimportしよう。
<br><br>
<p>.spec.ts</p>
<code>
...<br>
<u>import { CdkTextareaAutosize } from '@angular/cdk/text-field';</u>
...<br>
describe('コンポーネント名', () => {
<br>...<br>
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [コンポーネント名, <u>CdkTextareaAutosize</u>],
      <br>...</br>
  <br>}
</code>
以上の追加を行ったら、.tsとHTMLがうまく稼働できなくなるかもしれない。
下の変更もしよう。<br>
<p>.ts</p>
<code>
import { CdkTextareaAutosize } from '@angular/cdk/text-field';
<br>
<br>
export class クラス名 implements OnInit {<br>  
public cdkTextareaAutosize: CdkTextareaAutosize;
  <br>}
</code>
<br><br>
<hr>

### 問題解決途中のもろもろ
jasmineの方でエラー文を読み、提示に従って問題を解決しようとすると、下のようなエラー文が出てくるかもしれない。<br>
<code>Error: Please add an @NgModule annotation</code><br>
.Module.tsファイルに以上のようなimport文を追加したらコンパイラー側は非常に怒られる。結論から言うと、Moduleファイル側の追加は不要。
<br>
npm run testとng s -oは別のもの。ng sがうまく行ってもtestが絶対にいけると限らない。逆に、testがうまく行っても、ng s側はエラー出てくる可能性もある。両方確認してからgit commitしよう。



