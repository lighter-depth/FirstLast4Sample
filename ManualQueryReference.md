# SBFirstLast4 手動クエリ

## 基本的な構文
次のコマンドは、有効なクエリの一例です。
```csharp
@TW.Where(x => x.\c($C) && x.Start == 'あ').Select(x => x.Name + " Length: " + x.Name.Length.ToString())
```
出力:
```
[あむろなみえ Length: 6, あか Length: 2, あんてぃーく Length: 6, あれんじ Length: 4, あーつ Length: 3, あいのて Length: 4, あーてぃすと Length: 6, あーと Length: 3, あると Length: 3, あば Length: 2, あんり Length: 3, あーがいる Length: 5, あんさんぶる Length: 6, あいいろ Length: 4]
```
--------------------
有効なコマンドは、必ず以下の二要素を含まなければなりません。
- 辞書セレクタ
- メソッド チェーン

**辞書セレクタ**を用いてクエリの対象となる辞書を指定し、**メソッド チェーン**を用いて具体的な操作を指定します。  
```csharp
// コマンドの例
@NN.Where(x => x.FirstChar == 'あ').Select(x => x.LastChar)

// 辞書セレクタ部
@NN

// メソッド チェーン部
.Where(x => x.FirstChar == 'あ').Select(x => x.LastChar)
```
<br>

## 辞書セレクタ
**辞書セレクタ**は、クエリのソースとなるオブジェクトを示す特殊なリテラルです。  
以下の文法により表現されます。
```antlr
dictionary_selector
 : selector_symbol dictionary_code
 ;
selector_symbol
 : '@'
 ;
dictionary_code
 : 'NN' | 'NW' | 'TN' | 'TW' | 'PN' | 'PW' | '8X' | '6X' | 'D4' | '4X' | 'SO' 
 ;
```
**辞書コード**は、ソースの識別に用いられる一意なコードであり、二文字のアルファベットにより示されます。  
辞書コードの完全な一覧は以下の通りです。
|  辞書コード名  |  型  |     備考     |
| -- | ------------- | ------- |
| NN | `List<string>`        | 無属性単語を扱う標準の辞書です。|
| NW | `Word[]`              | 無属性単語を扱う`Word`型辞書です。|
| TN | `string[]`            | 有属性単語の`Name`プロパティを扱う`string`型辞書です。|
| TW | `List<Word>`          | 有属性単語を扱う標準の辞書です。|
| PN | `string[]`            | すべての単語の`Name`プロパティを含む統合辞書です。|
| PW | `Word[]`              | すべての単語を扱う標準の統合辞書です。|
| 8X | `string[]`            | ８倍弱点の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| 6X | `string[]`            | ６倍弱点の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| D4 | `string[]`            | ４注単語の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| 4X | `string[]`            | ４倍弱点の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| SO | `IEnumerable<object>` | オブジェクト辞書のシングルトンです。|
<br>

> [!WARNING]
> `string`型の要素を持つ辞書は、タイプに関する情報(`Word.Type1`, `Word.Type2` およびすべての関連プロパティ)を完全に消失します。  
> **タイプを伴った操作を行う場合には、必ず`List<Word>`型あるいは`Word[]`型の辞書を用いてください。**
<br>

辞書セレクタの指定を行わないコマンドや、無効な辞書セレクタを含むコマンドは、自動的に`SO`辞書上のクエリへと変換されます。
<br>

## メソッド チェーン
**メソッド チェーン**は、ソースに対する具体的なクエリ操作を表す、特殊な構文です。   
以下の文法により表現されます。
```antlr
method_chain
 : dictionary_selector method_call+
 ;
method_call
 : '.' method_name '(' args? ')'
 ;
args
 : expr (',' expr)*
 ;
```
<br>

メソッド チェーンの構文中で使用可能なメソッドを**クエリ演算子**と呼びます。  
クエリ演算子の一覧は次の通りです。  
<br>
<details>
 <summary>使用可能なメソッド一覧（折り畳み）</summary>
 <div>
  
- [All<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.all?view=net-7.0#system-linq-enumerable-all-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [Any<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.any?view=net-7.0#system-linq-enumerable-any-1(system-collections-generic-ienumerable((-0))))
- [Any<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.any?view=net-7.0#system-linq-enumerable-any-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [Average<TSource>(IEnumerable<TSource>, Func<TSource,Decimal>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.average?view=net-7.0#system-linq-enumerable-average-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-decimal))))
- [Average<TSource>(IEnumerable<TSource>, Func<TSource,Double>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.average?view=net-7.0#system-linq-enumerable-average-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-double))))
- [Average<TSource>(IEnumerable<TSource>, Func<TSource,Int32>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.average?view=net-7.0#system-linq-enumerable-average-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int32))))
- [Average<TSource>(IEnumerable<TSource>, Func<TSource,Int64>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.average?view=net-7.0#system-linq-enumerable-average-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int64))))
- [Average<TSource>(IEnumerable<TSource>, Func<TSource,Single>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.average?view=net-7.0#system-linq-enumerable-average-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-single))))
- [Concat<TSource>(IEnumerable<TSource>, IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.concat?view=net-7.0#system-linq-enumerable-concat-1(system-collections-generic-ienumerable((-0))-system-collections-generic-ienumerable((-0))))
- [Contains<TSource>(IEnumerable<TSource>, TSource)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.contains?view=net-7.0#system-linq-enumerable-contains-1(system-collections-generic-ienumerable((-0))-0))
- [Count<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.count?view=net-7.0#system-linq-enumerable-count-1(system-collections-generic-ienumerable((-0))))
- [Count<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.count?view=net-7.0#system-linq-enumerable-count-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [DefaultIfEmpty<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.defaultifempty?view=net-7.0#system-linq-enumerable-defaultifempty-1(system-collections-generic-ienumerable((-0))))
- [DefaultIfEmpty<TSource>(IEnumerable<TSource>, TSource)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.defaultifempty?view=net-7.0#system-linq-enumerable-defaultifempty-1(system-collections-generic-ienumerable((-0))-0))
- [Distinct<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.distinct?view=net-7.0#system-linq-enumerable-distinct-1(system-collections-generic-ienumerable((-0))))
- [Except<TSource>(IEnumerable<TSource>, IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.except?view=net-7.0#system-linq-enumerable-except-1(system-collections-generic-ienumerable((-0))-system-collections-generic-ienumerable((-0))))
- [First<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.first?view=net-7.0#system-linq-enumerable-first-1(system-collections-generic-ienumerable((-0))))
- [First<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.first?view=net-7.0#system-linq-enumerable-first-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [FirstOrDefault<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.firstordefault?view=net-7.0#system-linq-enumerable-firstordefault-1(system-collections-generic-ienumerable((-0))))
- [FirstOrDefault<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.firstordefault?view=net-7.0#system-linq-enumerable-firstordefault-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [GroupBy<TSource,TKey,TElement>(IEnumerable<TSource>, Func<TSource,TKey>, Func<TSource,TElement>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.groupby?view=net-7.0#system-linq-enumerable-groupby-3(system-collections-generic-ienumerable((-0))-system-func((-0-1))-system-func((-0-2))))
- [GroupBy<TSource,TKey>(IEnumerable<TSource>, Func<TSource,TKey>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.groupby?view=net-7.0#system-linq-enumerable-groupby-2(system-collections-generic-ienumerable((-0))-system-func((-0-1))))
- [Intersect<TSource>(IEnumerable<TSource>, IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.intersect?view=net-7.0#system-linq-enumerable-intersect-1(system-collections-generic-ienumerable((-0))-system-collections-generic-ienumerable((-0))))
- [Last<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.last?view=net-7.0#system-linq-enumerable-last-1(system-collections-generic-ienumerable((-0))))
- [Last<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.last?view=net-7.0#system-linq-enumerable-last-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [LastOrDefault<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.lastordefault?view=net-7.0#system-linq-enumerable-lastordefault-1(system-collections-generic-ienumerable((-0))))
- [LastOrDefault<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.lastordefault?view=net-7.0#system-linq-enumerable-lastordefault-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [LongCount<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.longcount?view=net-7.0#system-linq-enumerable-longcount-1(system-collections-generic-ienumerable((-0))))
- [LongCount<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.longcount?view=net-7.0#system-linq-enumerable-longcount-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [Max<TSource,TResult>(IEnumerable<TSource>, Func<TSource,TResult>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.max?view=net-7.0#system-linq-enumerable-max-2(system-collections-generic-ienumerable((-0))-system-func((-0-1))))
- [Max<TSource>(IEnumerable<TSource>, Func<TSource,Decimal>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.max?view=net-7.0#system-linq-enumerable-max-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-decimal))))
- [Max<TSource>(IEnumerable<TSource>, Func<TSource,Double>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.max?view=net-7.0#system-linq-enumerable-max-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-double))))
- [Max<TSource>(IEnumerable<TSource>, Func<TSource,Int32>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.max?view=net-7.0#system-linq-enumerable-max-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int32))))
- [Max<TSource>(IEnumerable<TSource>, Func<TSource,Int64>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.max?view=net-7.0#system-linq-enumerable-max-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int64))))
- [Max<TSource>(IEnumerable<TSource>, Func<TSource,Single>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.max?view=net-7.0#system-linq-enumerable-max-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-single))))
- [Min<TSource,TResult>(IEnumerable<TSource>, Func<TSource,TResult>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.min?view=net-7.0#system-linq-enumerable-min-2(system-collections-generic-ienumerable((-0))-system-func((-0-1))))
- [Min<TSource>(IEnumerable<TSource>, Func<TSource,Decimal>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.min?view=net-7.0#system-linq-enumerable-min-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-decimal))))
- [Min<TSource>(IEnumerable<TSource>, Func<TSource,Double>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.min?view=net-7.0#system-linq-enumerable-min-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-double))))
- [Min<TSource>(IEnumerable<TSource>, Func<TSource,Int32>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.min?view=net-7.0#system-linq-enumerable-min-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int32))))
- [Min<TSource>(IEnumerable<TSource>, Func<TSource,Int64>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.min?view=net-7.0#system-linq-enumerable-min-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int64))))
- [Min<TSource>(IEnumerable<TSource>, Func<TSource,Single>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.min?view=net-7.0#system-linq-enumerable-min-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-single))))
- [OrderBy<TSource,TKey>(IEnumerable<TSource>, Func<TSource,TKey>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.orderby?view=net-7.0#system-linq-enumerable-orderby-2(system-collections-generic-ienumerable((-0))-system-func((-0-1))))
- [OrderByDescending<TSource,TKey>(IEnumerable<TSource>, Func<TSource,TKey>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.orderbydescending?view=net-7.0#system-linq-enumerable-orderbydescending-2(system-collections-generic-ienumerable((-0))-system-func((-0-1))))
- [Select<TSource,TResult>(IEnumerable<TSource>, Func<TSource,TResult>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.select?view=net-7.0#system-linq-enumerable-select-2(system-collections-generic-ienumerable((-0))-system-func((-0-1))))
- [SelectMany<TSource,TResult>(IEnumerable<TSource>, Func<TSource,IEnumerable<TResult>>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.selectmany?view=net-7.0#system-linq-enumerable-selectmany-2(system-collections-generic-ienumerable((-0))-system-func((-0-system-collections-generic-ienumerable((-1))))))
- [Single<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.single?view=net-7.0#system-linq-enumerable-single-1(system-collections-generic-ienumerable((-0))))
- [Single<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.single?view=net-7.0#system-linq-enumerable-single-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [SingleOrDefault<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.singleordefault?view=net-7.0#system-linq-enumerable-singleordefault-1(system-collections-generic-ienumerable((-0))))
- [SingleOrDefault<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.singleordefault?view=net-7.0#system-linq-enumerable-singleordefault-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [Skip<TSource>(IEnumerable<TSource>, Int32)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.skip?view=net-7.0#system-linq-enumerable-skip-1(system-collections-generic-ienumerable((-0))-system-int32))
- [SkipWhile<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.skipwhile?view=net-7.0#system-linq-enumerable-skipwhile-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [Sum<TSource>(IEnumerable<TSource>, Func<TSource,Decimal>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.sum?view=net-7.0#system-linq-enumerable-sum-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-decimal))))
- [Sum<TSource>(IEnumerable<TSource>, Func<TSource,Double>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.sum?view=net-7.0#system-linq-enumerable-sum-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-double))))
- [Sum<TSource>(IEnumerable<TSource>, Func<TSource,Int32>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.sum?view=net-7.0#system-linq-enumerable-sum-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int32))))
- [Sum<TSource>(IEnumerable<TSource>, Func<TSource,Int64>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.sum?view=net-7.0#system-linq-enumerable-sum-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-int64))))
- [Sum<TSource>(IEnumerable<TSource>, Func<TSource,Single>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.sum?view=net-7.0#system-linq-enumerable-sum-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-single))))
- [Take<TSource>(IEnumerable<TSource>, Int32)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.take?view=net-7.0#system-linq-enumerable-take-1(system-collections-generic-ienumerable((-0))-system-int32))
- [TakeWhile<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.takewhile?view=net-7.0#system-linq-enumerable-takewhile-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
- [ThenBy<TSource,TKey>(IOrderedEnumerable<TSource>, Func<TSource,TKey>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.thenby?view=net-7.0#system-linq-enumerable-thenby-2(system-linq-iorderedenumerable((-0))-system-func((-0-1))))
- [ThenByDescending<TSource,TKey>(IOrderedEnumerable<TSource>, Func<TSource,TKey>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.thenbydescending?view=net-7.0#system-linq-enumerable-thenbydescending-2(system-linq-iorderedenumerable((-0))-system-func((-0-1))))
- [ToArray<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.toarray?view=net-7.0#system-linq-enumerable-toarray-1(system-collections-generic-ienumerable((-0))))
- [ToList<TSource>(IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.tolist?view=net-7.0#system-linq-enumerable-tolist-1(system-collections-generic-ienumerable((-0))))
- [Union<TSource>(IEnumerable<TSource>, IEnumerable<TSource>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.union?view=net-7.0#system-linq-enumerable-union-1(system-collections-generic-ienumerable((-0))-system-collections-generic-ienumerable((-0))))
- [Where<TSource>(IEnumerable<TSource>, Func<TSource,Boolean>)](https://learn.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.where?view=net-7.0#system-linq-enumerable-where-1(system-collections-generic-ienumerable((-0))-system-func((-0-system-boolean))))
 
</div>
</details>
<br>
<br>

### クエリ内部
クエリ内部では、簡易的な文法を用いた、引数の指定を行うことが可能です。  
使用可能な文法は以下の通りです。
- リテラル
- `@item` キーワード
- メンバーアクセス（`x.member`, `x.method(args)`）
- インデクサー（`x[index]`）
- クラス メンバーアクセス（`T.member`, `T.method(args)`）
- 算術否定演算子（`-`）
- 算術二項演算子（`+`, `-`, `*`, `/`, `%`）
- 論理演算子（`!`, `&&`, `||`）
- 比較演算子（`==`, `!=`, `>`, `<`, `>=`, `<=`）
- 三項演算子（`?:`）
- ヌル合体演算子（`??`）
- ラムダ演算子（`=>`）

#### 集合クエリ内部
以下のクエリ演算子内部では、通常のクエリ内部の機能に加えて、クエリ演算子の使用も可能です。

- Concat(IEnumerable, IEnumerable)
- Except(IEnumerable, IEnumerable)
- Intersect(IEnumerable, IEnumerable)
- Union(IEnumerable, IEnumerable)

### クエリ外部
メソッド チェーンの外部(トップ レベル)では、クエリ内部に加えて以下の機能を利用することができます。

- クエリ演算子
- 辞書セレクタ
- プリプロセッサ ディレクティブ

ただし、以下の機能は利用できない場合があります。
- メンバーアクセス（`x.member`, `x.method(args)`）
- インデクサー（`x[index]`）
- クラス メンバーアクセス（`T.member`, `T.method(args)`）

## コメント
コマンド中、プリプロセッサ ディレクティブ中、およびモジュール定義中において、`//`から始まる文字列は**コメント**とみなされ、翻訳結果から除外されます。

```csharp
@D4W.Except(@4XW) // コメントの例

// コメントの範囲は行末まで @NN.Select(@item.Deduce())

"文字列中の//記号は無効です"
```

## プリプロセッサ ディレクティブ
あるコマンドにおいて、任意個の空白を除外した最初のトークンが`#`である場合、そのコマンドは**プリプロセッサ ディレクティブ**として扱われます。

### 条件付き翻訳
`#ifdef`, `#ifndef`, `#elifdef`, `#elifndef`, `#else` および `#endif`ディレクティブを使用することで、定義されているシンボルに応じた翻訳の条件付けを行うことが可能です。

```c
#ifdef FOO
// シンボル FOO が定義済みの場合にのみ実行
#elifdef BAR
// シンボル FOO が未定義で、シンボル BARが定義済みの場合にのみ実行
#else
// シンボル FOO およびシンボル BAR がともに未定義の場合にのみ実行
#endif
```

これらのディレクティブは、モジュール定義の文脈下でのみ有効です。
### 定義
### 定義の削除
### 表示
### モジュールの管理

## リテラル
手動クエリは、以下のリテラルをサポートします。

### 論理値リテラル
`true`または`false`の値が使用可能です。これらのリテラルは`bool`型として扱われます。

### 整数リテラル
整数リテラルは、サフィックスにより`int`、`long`、`uint`あるいは`ulong`のいずれかの型を示します。すべての整数リテラルは、`0x`プレフィックスを用いることで**16進数リテラル**として扱うことができます。また、`int`型リテラルに限り、`0b`プレフィックスを用いることで**2進数リテラル**として解釈されます。

|型名|サフィックス|
| -- | -- |
|`int`|なし|
|`long`|`L`|
|`uint`|`U`|
|`ulong`|`UL`|

### 実数リテラル
実数リテラルは、サフィックスにより`float`、`double`あるいは`decimal`のいずれかの型を示します。また、`float`および`double`リテラルに限り、`e`記法を用いた科学表記が可能です。
|型名|サフィックス|
| -- | -- |
|`double`|なし|
|`float`|`f`|
|`double`|`d`|
|`decimal`|`m`|

### 文字リテラル
`'a'`のように、引用符で囲まれた文字は**文字リテラル**として解釈されます。  
[単純なエスケープ シーケンス](https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/language-specification/lexical-structure#character-literals)、
および[Unicode文字エスケープ シーケンス](https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/language-specification/lexical-structure#unicode-character-escape-sequences)の利用が可能です。
<br>
<br>
文字リテラルは`char`型として扱われます。

### 文字列リテラル
`"Hello"`のように、二重引用符で囲まれた0個以上の文字は**文字列リテラル**として解釈されます。  
文字列リテラル中には単純なエスケープ シーケンスおよびUnicode文字エスケープ シーケンスを含めることができます。
<br>
<br>
文字列リテラルは`string`型として扱われます。
> [!NOTE]
> FirstLast4は挿入文字列リテラル(`$"`)をサポートしません。代替として[string.Format](https://learn.microsoft.com/ja-jp/dotnet/api/system.string.format?view=net-7.0#system-string-format(system-string-system-object()))メソッドを用いることができます。

### 正規表現リテラル
`` `^[あ-う]{2,5}[がぐ]ー*$` ``のように、バッククオート(`` ` ``)で囲まれた文字は**正規表現リテラル**として解釈されます。  
正規表現リテラルは[`Regex`型](https://learn.microsoft.com/ja-jp/dotnet/api/system.text.regularexpressions.regex?view=net-7.0)のオブジェクトに変換されるため、`Regex`型のメソッド(`IsMatch`など)を用いたマッチングが可能です。

```javascript
// [あ, い, う, え, お] のいずれかで終わる単語にマッチ
@TW.Where(w => `[あいうえお]ー*$`.IsMatch(w.Name))
```

### コレクションリテラル
角かっこ(`[]`)を用いて複数の値を囲むことにより、**配列の初期化**を行うことができます。
```csharp
// 配列を初期化
[1, 2, 3, 4, 5].Where(i => i % 2 == 0)
```
配列の型は要素の型から推論されます。したがって、共通の型を推論できない場合には配列を作成することはできません。
```javascript
// 配列の型を推論できないのでエラー
[1, 2.0, 'a', /りんご YF/w]
```
ただし、共通の型への明示的なキャストを行うことにより、動的な型が異なる場合でも配列の作成が可能です。
```cpp
// キャストを行えば作成可能
[static_cast<object>(3), static_cast<object>(2.5), static_cast<object>("hello")]
```
### 辞書リテラル
[辞書セレクタ](#辞書セレクタ)と同じ構文を式中で用いることで、対応する辞書を参照することができます。
```csharp
@TN.Except(@4X).Concat(@D4)
```
無効な辞書セレクタを使用した場合や、型が一致しない場合にはエラーが発生します。

```csharp
@TW.Except(@AA) // エラー: 無効な辞書セレクタ

@TW.Concat(@D4) // エラー: 無効な型変換
```
### `WordType`リテラル
`$`マークに続けて既定の文字列を入力することにより、**`WordType`型のリテラル**として解釈される場合があります。  
既定の文字列には、`WordType`列挙型の各値に対応する**タイプシンボル名**および**タイプ名コード**が含まれます。  
具体的な一覧は次の通りです。
|タイプ名|タイプシンボル名|タイプ名コード|
|--|--|--|
|ノーマル|`Normal`|`N`|
|動物|`Animal`|`A`|
|植物|`Plant`|`Y`|
|地名|`Place`|`G`|
|感情|`Emote`|`E`|
|芸術|`Art`|`C`|
|食べ物|`Food`|`F`|
|暴力|`Violence`|`V`|
|医療|`Health`|`H`|
|人体|`Body`|`B`|
|機械|`Mech`|`M`|
|理科|`Science`|`Q`|
|時間|`Time`|`T`|
|人物|`Person`|`P`|
|工作|`Work`|`K`|
|服飾|`Cloth`|`L`|
|社会|`Society`|`S`|
|遊び|`Play`|`J`|
|虫|`Bug`|`D`|
|数学|`Math`|`X`|
|暴言|`Insult`|`Z`|
|宗教|`Religion`|`R`|
|スポーツ|`Sports`|`U`|
|天気|`Weather`|`W`|
|物語|`Tale`|`O`|
|無属性|`Empty`|`I`|
> [!NOTE]
> タイプ名コードはCase-Insensitiveです。また、タイプシンボル名は`WordType`列挙型のメンバー名と完全に同一です。  
> すなわち、以下の4つのコードは全て同一の意味となります。
> 
> ```
> // タイプシンボル名
> @TW.Where(x => x.\c($Sports))
> 
> // タイプ名コード(大文字)
> @TW.Where(x => x.\c($U))
> 
> // タイプ名コード(小文字)
> @TW.Where(x => x.\c($u))
> 
> // 完全修飾名
> @TW.Where(x => x.\c(WordType.Sports))
> ```

### `Word`リテラル
スラッシュ(`/`)で囲まれた文字列は、`Word`構造体のリテラルとして解釈される場合があります。  
FirstLast4は、**明示的な`Word`リテラル**および**Deduceリテラル**の2種類のリテラルをサポートします。
#### 明示的な`Word`リテラル
以下の文法により表現されるリテラルは、**明示的な`Word`リテラル**として解釈されます。
```antlr
explicit-word-literal
 : '/' literal_body '/w'
 ;

literal-body
 : word-name (' ' type-code type-code?)?
 ;

word-name
 : ('ぁ'..'ゟ' | 'ー')*
 ;

type-code
 : 'A'..'Z' | 'a'..'z'
 ;
```
明示的な`Word`リテラルは、明示的なタイプ指定を行いつつ`Word`構造体を生成する場合に適しています。

```javascript
/あいいろ C/w // あいいろ(芸術)

/くり FY/w // くり(食べ物/植物)

/ぬれぎぬ/w // ぬれぎぬ(分類なし)
```

> [!IMPORTANT]
> リテラルの仕様上、不正なタイプを持つ単語を生成することも可能です。
> ```
> /るーじゅばっく B/w
> 
> /ぬりかべ VV/w
> ```
> 不正なタイプが許容されない場合には、Deduceリテラルを使用してください。
#### Deduce リテラル
以下の文法により表現されるリテラルは、**Deduceリテラル**として解釈されます。
```antlr
deduce-literal
 : '/' word-name '/d'
 ;

word-name
 : ('ぁ'..'ゟ' | 'ー')*
 ;
```
