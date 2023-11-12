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
- `Type2`: 単語の第二タイプを表す`WordType`列挙型の値です。
## プロパティ
- `Default`: 既定の`Word`型インスタンスを表します。
- `End`: 単語の「最後の文字」を表す`char`型のプロパティです。このプロパティは`Name.LastChar`と同じ値を示します。
- `IsCritable`: 単語が自然急所を実行可能かどうかを表す`bool`型のプロパティです。人体タイプ、あるいは暴言タイプを含む単語に対し、無条件で`true`となります。
- `IsDefault`: 単語が既定状態であるかどうかを表す`bool`型のプロパティです。
- `IsDoubleType`: 単語が複合タイプであるかどうかを表す`bool`型のプロパティです。
- `IsEmpty`: 単語が無属性であるかどうかを表す`bool`型のプロパティです。このプロパティは、第二タイプに関わらず、第一タイプに基づいてタイプの有無を判断します。
- `IsHeal`: 単語が「回復単語」であるかどうかを表す`bool`型のプロパティです。医療タイプ、あるいは食べ物タイプを含む単語に対し、無条件で`true`となります。
- `IsSingleType`: 単語が単タイプであるかどうかを表す`bool`型のプロパティです。このプロパティは、通常の単語生成規則を満たすインスタンス(第一タイプを主要タイプとする規則)上に用いられることを想定しています。
- `IsViolence`: 単語が「暴力単語」であるかどうかを表す`bool`型のプロパティです。暴力タイプを含み、なおかつ回復単語でない単語に対してのみ`true`となります。
- `Start`: 単語の「最初の文字」を表す`char`型のプロパティです。このプロパティは`Name.FirstChar`と同じ値を示します。
## メソッド
### CalcEffectiveDmg(Word)
このインスタンスを攻撃側、`other`を防御側の単語とした際の、タイプ相性によるダメージ倍率を計算します。  
メソッドのエイリアスは`\cl`です。
```csharp
public double CalcEffectiveDmg(Word other);
```
### CalcEffectiveDmg(WordType, WordType)
`t1`を攻撃側、`t2`を防御側とした時の、タイプ相性によるダメージ倍率を計算します。
メソッドのエイリアスは`\cl`です。
```csharp
public static double CalcEffectiveDmg(WordType t1, WordType t2);
```
### CompareTo(Word)
このインスタンスと指定した`Word`オブジェクトとを`Name`プロパティに基づいて比較し、並べ替え順序において、このインスタンスの位置が指定した文字列の前、後ろ、または同じのいずれであるかを示します。
```csharp
public int CompareTo(Word other);
```
### FromString(String)
文字列値から単語のタイプを推論し、`Word`構造体のインスタンスを生成します。
> [!NOTE]
> タイプの推論に失敗した場合は、必ず無属性のインスタンスが生成されます。  
> 単語が無属性辞書に含まれるかどうかを加味したい場合は、`@TN`辞書セレクタを用いてください。
```csharp
public static Word FromString(string name);
```
### IsSuitable(Word)
このインスタンスが、指定した`Word`オブジェクトに対し、しりとりのルール上適切であるかどうかを示します。
```csharp
public int IsSuitable(Word prev);
```
#### 戻り値
**Int32**  
しりとりのルール上の適切性を表すフラグです。
|値| 意味 |
| -- | -- |
| 0 | しりとりのルール上正常です。 |
| 1 | 開始文字が一致していません。 |
| -1 | 「ん」で終わっています。 |

### ToString()
このインスタンスの値を、それと等価な文字列形式に変換します。
```csharp
public override string ToString();
```
