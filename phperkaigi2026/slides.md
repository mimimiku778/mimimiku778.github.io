---
marp: true
theme: default
paginate: true
style: |
  section {
    font-family: 'Noto Sans JP', 'Hiragino Kaku Gothic ProN', sans-serif;
    font-size: 32px;
  }
  section.title {
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }
  section.big {
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 2.2em;
  }
  section.big h2 {
    font-size: 1.4em;
  }
  section.big h3 {
    font-size: 0.8em;
  }
  section.mid {
    font-size: 40px;
  }
  section.profile {
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    gap: 40px;
    padding: 60px 60px;
  }
  section.profile .left {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-width: 260px;
  }
  section.profile .left img.avatar {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    object-fit: cover;
  }
  section.profile .left .name {
    font-size: 28px;
    font-weight: bold;
    margin-top: 16px;
    text-align: center;
  }
  section.profile .left .subname {
    font-size: 20px;
    color: #666;
    text-align: center;
  }
  section.profile .right {
    flex: 1;
  }
  section.profile .right img.logo {
    height: 200px;
    margin-bottom: 8px;
  }
  section.profile .right img.product {
    max-width: 100%;
    max-height: 280px;
    border-radius: 8px;
    margin-bottom: 8px;
  }
  section.profile-product {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 32px;
    padding: 40px 48px;
  }
  section.profile-product .product-area {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 32px;
    width: 100%;
  }
  section.profile-product .product-area img.product-img {
    max-height: 560px;
    max-width: 55%;
    border-radius: 8px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.15);
  }
  section.profile-product .product-area .product-info {
    flex: 1;
  }
---

<!-- HTML生成: npx @marp-team/marp-cli --no-stdin phperkaigi2026/slides.md --html -o phperkaigi2026/slides.html -->

<!-- _class: title -->

# いらないものを消したら<br>フレームワークができた話

<br>

## pika

<!--
「pikaです。独学でPHPフレームワークを自作した話をします」

繰り返しフレーズ（ぼそっと→はっきり）
「問題が、仕組みを教えてくれた」
「問題が、設計を導いた」
-->

---

<!-- _class: profile -->

<div class="left">
<img class="avatar" src="./images/profile.jpg">
<div class="name">pika | keita nemoto</div>
</div>

<div class="right">

## 所属

<img class="logo" src="./images/digital-circus-logo.png">

デジタルサーカス株式会社
エンジニア歴 1年半
プログラミング歴 3年半

</div>

<!--
「pikaです。デジタルサーカスでエンジニアをしています。プログラミング歴は3年半です」
-->

---

<!-- _class: big -->

## オープンチャットの評判を
## ユーザーが自由に投稿できるサービスを作りたかった

<!--
「LINEのオープンチャットの評判を、ユーザーが自由に投稿できるサービスを作りたかったんです」
-->

---

<!-- _class: big -->

## WordPressでやってみた

### でも思いどおりのカスタマイズができなかった

<!--
「最初はWordPressとプラグインを組み合わせてやってみたんですが、思いどおりのカスタマイズができなかったんです」
-->

---

<!-- _class: big -->

## 自分で作るしかない

### Progateでプログラミングを始めた

<!--
「自分で作るしかないと思って、Progateでプログラミングを始めました」
-->

---

## 一応動くものは作れた

```php
class PostReview extends ApiAbstract
{
    public function __construct()
    {
        $this->init($this->validation(), ...);
        $insert_db = new InsertDB($this->sql);
        $result = $this->add($insert_db, $post_value);
        httpResponse(200, 'OK');
    }
}
```

でもコンストラクタに全部詰め込んでいた

<!--
「一応動くものは作れたんですが、コンストラクタに処理を全部詰め込んでいて、機能を足すたびにコードが絡まっていきました」
一拍
-->

---

<!-- _class: big -->

## どこに何を書けばいいか分からない

### 何度も最初からやり直した

<!--
「どこに何を書けばいいか、分からなかったんです」
黙る。読ませる。
「何度も最初からやり直しました」
★急がない
-->

---

<!-- _class: big -->

## フレームワークを調べ始めた

<!--
「この地獄を抜け出すためにフレームワークを調べ始めました」
-->

---

<!-- _class: big -->

## 既存のフレームワークやライブラリを使いたくない

### 謎の厨二病にかかっていた

<!--
「でも既存のフレームワークやライブラリを使いたくない、謎の厨二病にかかっていたんです」
-->

---

<!-- _class: big -->

## Laravelの書き方だけ真似して
## 裏側は全部自分で作ることにした

<!--
「だからLaravelの書き方だけ真似して、裏側は全部自分で作ることにしました」
-->

---

## まず困っていたのは「どこに何を書くか」

```
app/Controllers/Pages/
    ImagePageController.php   ← /image でアクセスされる
    IndexPageController.php   ← / でアクセスされる
```

URLとファイル名を対応させた。ファイルを置くだけ。設定ファイルは0行

<!--
「まず一番困っていたのは、どこに何を書くかでした。最初は普通にURLごとに.phpファイルを置いていたんですが、そこから.htaccessでindex.phpにアクセスを集約させて、条件分岐で読み込むファイルを決める形にしました。それをさらに発展させて、URLとクラス名を対応させて、ファイルを置くだけでアクセスできるようにしました。設定ファイルは0行です」
-->

---

## 次に、Laravelのコントローラを見て思った

```php
// 自分のコード: 使うクラスを毎回自分で生成して渡していた
$db = new Database();
$store = new ImageStore($db);
$controller = new ImageController($store);
```
```php
// Laravelのコード: 引数に型を書くだけで使える
public function index(ImageStore $store) { ... }
```

これは便利だ。自分でも作りたい

<!--
「次にLaravelのコントローラを見たら、引数に型を書くだけでクラスが使えるようになっていたんです。自分のコードでは毎回new Databaseとかnew ImageStoreとか書いて渡していたので、これは便利だなと」
-->

---

## 裏側で何をしているか

```php
function resolve(string $className): object {
    $ref = new ReflectionClass($className);
    $params = $ref->getConstructor()->getParameters();

    $args = [];
    foreach ($params as $param) {
        $typeName = $param->getType()->getName();
        $args[] = $this->resolve($typeName); // ★ 再帰
    }

    return $ref->newInstanceArgs($args);
}
// ImageStore → Database → Config … 全部自動で生成される
```

<!--
「ChatGPTにリフレクションというものを教えてもらって、引数の型を読み取れるようになりました。最初は1つのクラスを渡すだけだったんですが、クラスが大きくなって分割したいと思ったとき、再帰にすればいいだけだと気づきました」
「1行変えただけで、芋づる式に全部自動で生成されるようになりました」←力入れる
ぼそっと「問題が、仕組みを教えてくれた」★1回目
-->

---

<!-- _class: title -->

# できた

URL対応 / 自動クラス生成 / バリデーション
CSRF対策 / 自動XSSエスケープ / ヘルパー関数

外部ライブラリ依存ゼロ

<!--
「で、できました」
止まる。読ませる。「外部依存ゼロ」は口で言わない。
-->

---

### こう定義すると

```php
Route::path('image/store@post')            // POST /image/store
    ->matchFile('file', ['image/jpeg'])    // 画像だけ許可
    ->matchNum('imageSize', max: 1000);    // 数値の範囲チェック
```

### このコントローラが呼ばれる

```php
class ImageApiController
{
    public function store(
        GdImageFactoryInterface $image,  // 自動で生成される
        ImageStoreInterface $store,      // 自動で生成される
        array $file,                     // バリデーション済み
        int $imageSize                   // バリデーション済み
    ) {
        // やりたいことだけ書く
```

<!--
「こう定義すると、このコントローラが呼ばれます。クラスは自動でインスタンス化されて、値はバリデーション済み。やりたいことだけ書けばいい」
一拍
-->

---

<!-- _class: big -->

## 後から知ったんですが

<!--
「……後から知ったんですが」
黙る。★急がない
-->

---

<!-- _class: big -->

## 「ルーティング」と呼ばれていた

### 「index.phpから条件分岐で読み込む」の答え

<!--
「自分が作ったもの、全部名前がついていました。index.phpから条件分岐で読み込む仕組みは、ルーティングと呼ばれていました」
-->

---

<!-- _class: big -->

## 「設定より規約」と呼ばれていた

### Convention over Configuration
### 「どこに何を書くか」の答え

<!--
「どこに何を書くかの答えは、設定より規約と呼ばれていました」
-->

---

<!-- _class: big -->

## 「依存性の注入」と呼ばれていた

### Dependency Injection
### newを手で書いて繋いでいた問題の答え

<!--
「newを手で繋いでいた問題の答えは、依存性の注入と呼ばれていました」
淡々と
-->

---

<!-- _class: big -->

# 問題が、設計を導いた

問題を持っていたから、設計に辿り着いた

<!--
「問題を持っていたから、設計に辿り着けたんです」
一拍。「問題が、設計を導いた」はっきり。★2回目
-->

---

<!-- _class: profile-product -->

<div class="product-area">
<img class="product-img" src="./images/openchat-graph.png">
<div class="product-info">

## このフレームワークで
## オプチャグラフを作って公開した

LINEオープンチャットの
グラフ可視化ツール

月間アクティブユーザー 約2万人
月間PV 約10万

コントローラ 41個 / モデル 145個
コミット 3,300+ / PR 233本

</div>
</div>

<!--
「このフレームワークでオプチャグラフを作って公開しました。今も動いています」
-->

---

<!-- _class: big -->

## 問題を持っていれば、答えに辿り着ける

### 皆さんもぜひ、自分の手で作ってみてください

ありがとうございました

<!--
「問題を持っていれば、答えに辿り着けます。皆さんもぜひ自分の手で作ってみてください。ありがとうございました」
-->
