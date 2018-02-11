# Rubyアルゴリズム・データ構造3

## 今回のテーマ

```
線形探索法（リニアサーチ）、二分探索法（バイナリサーチ）のアルゴリズムを理解し、実装できるようになる。
```

## 線形探索法（リニアサーチ）とは

検索対象のデータを先頭から順に調べていく検索アルゴリズムの１つです。

### 線形探索法アルゴリズムのイメージ

線形検索法の考え方はいたってシンプルです。下記の図のような配列があるとします。

<a href="https://diveintocode.gyazo.com/2e32e72d36ffbedc3fea5985c2d39175"><img src="https://t.gyazo.com/teams/diveintocode/2e32e72d36ffbedc3fea5985c2d39175.png" alt="https://diveintocode.gyazo.com/2e32e72d36ffbedc3fea5985c2d39175" width="309"/></a>

このデータ`5`がどこにあるか検索したいときの流れは

1. 最初のデータである1を見ます。

<a href="https://diveintocode.gyazo.com/03b22ba806d6f25d6af0b71c236048ad"><img src="https://t.gyazo.com/teams/diveintocode/03b22ba806d6f25d6af0b71c236048ad.png" alt="https://diveintocode.gyazo.com/03b22ba806d6f25d6af0b71c236048ad" width="309"/></a>

2. 1ではないので、次のデータ3を見ます。

<a href="https://diveintocode.gyazo.com/8067e882d32958251f6d7984b162edcd"><img src="https://t.gyazo.com/teams/diveintocode/8067e882d32958251f6d7984b162edcd.png" alt="https://diveintocode.gyazo.com/8067e882d32958251f6d7984b162edcd" width="309"/></a>

3. 3ではないので、次のデータ5を見ます。5を見つけたので処理を終了させます。

<a href="https://diveintocode.gyazo.com/729db5a89bf7d1f67308d670157b590e"><img src="https://t.gyazo.com/teams/diveintocode/729db5a89bf7d1f67308d670157b590e.png" alt="https://diveintocode.gyazo.com/729db5a89bf7d1f67308d670157b590e" width="309"/></a>

つまり配列の最初から順番に探していくのです。

### 線形探索法のメリット
```
・アルゴリズムが簡単
・データがソートされていなくても検索可能
```

### 線形探索法のデメリット
```
・要素数と同じ回数（全データ）の検索が必要な場合あり
```

### 線形探索法アルゴリズムのイメージ
線形探索法の処理をアルゴリズムとして表すと、次のようになります。
![線形探索法](/線形探索法.png)

## 二分探索法（バイナリサーチ）とは

整列済みのデータに対して、検索対象データを半分に分割しながら検索するアルゴリズムです。

### 二分探索法アルゴリズムのイメージ
まずは、イラストを用いて二分探索法のイメージを掴みましょう。

[![https://diveintocode.gyazo.com/3ae90b2e79618a18353b498aad5807e5](https://t.gyazo.com/teams/diveintocode/3ae90b2e79618a18353b498aad5807e5.png)](https://diveintocode.gyazo.com/3ae90b2e79618a18353b498aad5807e5)

今回は「5」という数字を検索する場合を想定してみましょう。

まず、最初に配列の中央の値に注目します。

この場合、配列の中央の値は「4」になります。

[![https://diveintocode.gyazo.com/9c605638a6f4e70c29cbe17d196ccd51](https://t.gyazo.com/teams/diveintocode/9c605638a6f4e70c29cbe17d196ccd51.png)](https://diveintocode.gyazo.com/9c605638a6f4e70c29cbe17d196ccd51)

中央の値である「4」と今回検索対象としている「5」を比較すると「5」の方が大きいです。

このことから「5」は「4」の右側にあることがわかります。

[![https://diveintocode.gyazo.com/5bf14c9936e96227523e2a865f7ee122](https://t.gyazo.com/teams/diveintocode/5bf14c9936e96227523e2a865f7ee122.png)](https://diveintocode.gyazo.com/5bf14c9936e96227523e2a865f7ee122)

再び、配列の中央の値に注目します。

この場合、配列の中央の値は「6」になります。

[![https://diveintocode.gyazo.com/5fff9011b259acd8c8c24a877b69f6f0](https://t.gyazo.com/teams/diveintocode/5fff9011b259acd8c8c24a877b69f6f0.png)](https://diveintocode.gyazo.com/5fff9011b259acd8c8c24a877b69f6f0)

中央の値である「6」と今回検索対象としている「5」を比較すると「6」の方が大きいです。

このことから「5」は「6」の左側にあることがわかります。

このように、検索する範囲を半分に絞込みながら検索していくのが「二分探索法」です。

### 二分探索法の処理

二分探索法は、大きく分けて以下の3つの処理から構成されています。

- 中央の要素を調べる
- 中央の要素と、検索対象の要素を比較する
- 検索の範囲を半分に絞込む（中央の要素が調べたい値より大きければ中央より後半部分を、調べたい値より小さければ中央より前半部分を調べる。）

**中央の要素を調べる**

先ほどのイラストに`添え字`を追加すると次のようになります。

[![https://diveintocode.gyazo.com/267607b42f8f8852f924c64bad78764d](https://t.gyazo.com/teams/diveintocode/267607b42f8f8852f924c64bad78764d.png)](https://diveintocode.gyazo.com/267607b42f8f8852f924c64bad78764d)

配列の中央の数字を計算で出す場合は、
`( 配列の先頭の添字 + 配列の末尾の添字 ) / 2`という計算式で導くことが出来ます。
今回の場合、`( 0 + 6 ) / 2 = 3`となり、中央の値の添字は[3]となります。

**配列の要素の数が偶数の場合**

要素の数が5個や7個の場合は2で割り切ることが出来るのですが、

例えば要素の数が6個の場合、配列の中央を求める計算式は、`0+5/2=2.5`となります。

添字[2.5]という要素は存在しないため、小数を整数化する必要があります。

小数を整数化する方法としては、「四捨五入」、「切り上げ」、「切り捨て」といった方法が考えられます。

**中央の要素と、検索対象の要素を比較する**

中央のデータを決定したら、次はそのデータと、検索対象のデータが一致しているかどうか比較します。

中央のデータが検索対象のデータと一致した場合、検索対象のデータが見つかったことになり、その時点で探索は終了になります。

中央のデータが検索対象のデータと一致しない場合、
中央のデータが検索対象のデータよりも、大きいか、小さいかの2パターンが考えられます。

**検索の範囲を半分に絞込む**

先程の比較結果をもとに、探索範囲を絞って行きます。

|比較結果|絞り込み方|
|:---------------|:----------|
|検索対象のデータ<中央のデータ|前半分に絞る|
|中央のデータ＜検索対象のデータ|後ろ半分に絞る|

二分探索法では、上記の処理を繰り返し行うことで対象のデータを検索します。

**検索対象のデータが存在しない場合**

先頭の要素の値が末尾の値よりも大きくなった場合、検索対象のデータが存在しないということが言えます。

検索対象のデータが存在しない場合に、無限ループに入ってしまわないよう、

中央のデータを選ぶ処理を実行する前に、先頭の要素の値が、末尾の値以下であることを確認する条件式が必要です。

### 二分探索法アルゴリズムのイメージ

二分探索法の処理をアルゴリズムとして表すと、次のようになります。

変数はそれぞれ次のものを表しています。

|変数   |対象         |
|:-----|:------------|
|lead  |先頭の要素の添字|
|last  |末尾の要素の添字|
|mid   |中央の要素の添字|

![二分探索法](/二分探索法.png)

### 小課題

ここからは、小課題に取り組んでいただきます。
小課題は課題として提出する必要はありませんが、必ず一度手を動かして取り組んでみてください！

小課題の解答例は、このテキストの一番最後に記載しています。

#### 課題の内容

**小課題１**

線形探索法のアルゴリズムをRubyを使って実装してください。

実装要件
```
1. 用意する配列は次のとおりとし、配列を一度画面に表示させること
   numbers = [1,3,5,11,12,13,17,22,25,28]
2. 探したい数字が入力できること
3. 探したい数字が見つかれば、その数字のインデックス（配列の位置）を表示させること
4. もし探したい数字がなければ`None`と表示させること
```

例

```
$ ruby classic_algorithm.rb
>> [1,3,5,11,12,13,17,22,25,28]
>> 探したい数字を入力してください。
>> 1
>>0

$ ruby classic_algorithm.rb
>> [1,3,5,11,12,13,17,22,25,28]
>> 探したい数字を入力してください。
>> 13
>> 5

$ ruby classic_algorithm.rb
>> [1,3,5,11,12,13,17,22,25,28]
>> 探したい数字を入力してください。
>> 2
>> None
```

**小課題２**

二分探索法のアルゴリズムをRubyを使って実装してみましょう。

※実装要件は、小課題１と同様です。

#### 質問について

質問はこちらのURLにお願いいたします。

https://diveintocode.jp/diver/questions/new

## テキストフィードバックについて

今後のDIVE INTO CODEの発展のために、お時間があれば
フィードバックのご協力をお願い致します。
以下を参考に、DIVERのコメント欄にご投稿して頂けると有難いです。

・テキストの難易度は適切であったか

・テキストの分量は適切であったか

・説明していない用語はなかったか

・テキストに間違いはなかったか

・テキストのゴールに対して適切な内容であったか

・他に説明を加えてほしいことは無かったか

・やってて楽しいテキストであったか

・有益なテキストであったか

**その他なんでもご意見をご投稿ください**

**DIVER質問機能のコメント欄に記述をお願い致します。**

#### 小課題１の解答例

```
def linear_search(numbers,value)
  i = 0
  while i <= numbers.length-1
    if numbers[i] == value
      return i
    end
    i+=1
  end
  return "None"
end

numbers = [1,3,5,11,12,13,17,22,25,28]
p(numbers)

puts('探したい数字を入力してください')
target_number = gets.to_i

message = linear_search(numbers,target_number)

puts(message)
```

#### 小課題２の解答例

```
def binary_search(numbers,value)
  lead = 0
  last = numbers.length-1

  while lead <= last do
    mid = ((lead + last) / 2).round
    if (numbers[mid] == value)
      return mid
    elsif (numbers[mid] > value)
      last = mid - 1
    else
      lead = mid + 1
    end
  end
  return "None"
end

numbers = [1,3,5,11,12,13,17,22,25,28]
p(numbers)

puts('探したい数字を入力してください')
target_number = gets.to_i

message = binary_search(numbers,target_number)

puts(message)
```
