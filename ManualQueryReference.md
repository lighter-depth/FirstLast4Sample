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


## 辞書セレクタ
**辞書セレクタ**は、クエリのソースとなるオブジェクトを示す特殊なリテラルです。  
以下の構文により表現されます。
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
| NN | `List<string>` | 無属性単語を扱う標準の辞書です。|
| NW | `List<Word>`   | 無属性単語を扱う`Word`型辞書です。|
| TN | `List<string>` | 有属性単語の`Name`プロパティを扱う`string`型辞書です。|
| TW | `List<Word>`   | 有属性単語を扱う標準の辞書です。|
| PN | `List<string>` | すべての単語の`Name`プロパティを含む統合辞書です。|
| PW | `List<Word>`   | すべての単語を扱う標準の統合辞書です。|
| 8X | `List<string>` | ８倍弱点の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| 6X | `List<string>` | ６倍弱点の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| D4 | `List<string>` | ４注単語の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| 4X | `List<string>` | ４倍弱点の存在する単語の`Name`プロパティを扱う`string`型辞書です。|
| SO | `object[]` | オブジェクト辞書のシングルトンです。|

> [!WARNING]
> `List<string>`型の辞書は、タイプに関する情報(`Word.Type1`, `Word.Type2` およびすべての関連プロパティ)を完全に消失します。  
> **タイプを伴った操作を行う場合には、必ず`List<Word>`型の辞書を用いてください。**

