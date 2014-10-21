## Nilモデル

利便性を高めるために、例えば ```page._info``` を実行した時には NilModel を返すようになっています。(このとき、. _info はまだ設定されていないものとします。)NilModel は将来的に利用するモデルのプレースホルダーです。NilModel があることによって、わざわざ中間の値を初期化することなく、深くネストされた値のバインドをすることが可能になっています。

```ruby
page._info
# => <Volt::Model:70260787225140 nil>

page._info._name
# => <Volt::Model:70260795424200 nil>

page._info._name = 'Ryan'
# => <Volt::Model:70161625994820 {:info => <Volt::Model:70161633901800 {:name => "Ryan"}>}>
```

ただし、1つ注意しておくべきことは、NilModel は「真」の値であるということです。(Ruby では nil と false のみが false として扱われます。)分かりやすくするために、NilModel に対して ```.nil?``` を実行すると true を返すようになっています。

真偽チェックを利用するよくあるケースとして、|| (論理和) を使ってデフォルト値を設定するものがあります。Volt はこれと同様の働きをするメソッド #or を提供します。これは NilModel に対しても有効です。

このように書くのではなく、

```ruby
    a || b
```

このように書きます:

```ruby
    a.or(b)
```

`#and` は && と同様の働きをします。#and と #or によって、NilModel も含めて、初期値の扱いを簡単にすることができます。

-- TODO: Document .true?/ .false?