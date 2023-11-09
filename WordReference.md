# Word 構造体
しりとりバトルにおける、ゲーム中の単語を表します。

## 定義
```csharp
public readonly record struct Word(string Name, WordType Type1, WordType Type2)
 : IComparable<Word>
```
## 引数
- `Name`: 単語の名前を表す文字列です。
- `Type1`: 単語の第一タイプを表す`WordType`列挙型の値です。
- `Type2` 単語の第二タイプを表す`WordType`列挙型の値です。
## プロパティ
