こういうのあんまりアウトプットしてないな(っていうか情報共有したらいいんじゃないの)と思ったので、各言語でちょこちょこと書き進めてみる。リファクタの本質的なとこだと思う。
自分や人のレビューを見て思ったことをアウトプットすることで自分なりに消化したり、似たようなことをこの言語でああいうのどうするんだっけみたいなメモも兼ねてる。

# typescript

交差型とユニオン型

例えば、あんまりいい例じゃない気がするがAPIのレスポンスをインターフェースに落とし込むことにする。

レスポンスとしてステータスコード・レスポンスデータ・エラー内容が考えられる。

```typescript
interface Response{
  status: number
  data: object | null
  error: object | null
}
```

例えばこんな感じかな。何も考えなければこういうインターフェースになるが、もう一歩進んだ型定義としてこのようなものが考えられる。

```typescript
type Response = {
  status: Status
  data: object
} | {
  status: Status
  error: object
}
```

こうすれば、dataとerrorが両立しえないことを型が教えてくれる。
恩恵としては、余計なnull判定が不要になることが挙げられる。

ユニオン型以外にも交差型を使ってみると

```typescript
type Response = {
  status: Status
} & ({
  data: object
} | {
  error: object
})
```

若干読みづらい感はありますが、このように型で論理構造を限定していくと、いわゆるコードが教えてくれるという状態になっていく。

statusについても制限を加えたければ、

```typescript
type Response = {
  status: SuccessStatus
  data: object
} | {
  status: ErrorStatus
  error: object
}
```

こんな風に定義できると、不適な組み合わせをそもそも排除できるので、コーダーは余計な事を考えなくて済む。
ということを考えながらインターフェースを定義できると、コードのノイズが減るかなと思います。



まあ、必ずしも型をガチガチに固める必要があるのかどうかはケースバイケースですが、このような思考をワンクッション挟む癖がついてると、コードの品質が上がっていくんじゃないかなと考えています。この例も採用するかどうかはプロジェクトによると思いますけど、考え方としては大事だと思ってます。

受け取る側にとって最も都合のいい形で定義する

例えば、下記のようなレスポンスに応じてダイアログを出しわけるコンポーネントを考える。

```typescript
interface Response{
  a: Code
  b: Code
}

type Code= "1" | "2" | "3"
```

AとBというデータがあり、1,2,3というフラグが同時に渡される。1は問題なし、2は警告を出す、3はエラー処理のイメージ。

この1,2,3というのに別名をつけたい人は定義したものとして読み替えてください。



悪いコード例

```typescript
type Key = keyof Response; // "a" | "b"
interface DialogState{
  open: bool //ダイアログの開閉状態
  warn: Key [] | null
  error: Key [] | null
}

export const SamplePage = () => {
  // ダイアログ表示制御関連
  const [dialogState, setDialogState] = useState<DialogState | null>(null);
  
  const onDuplicateCheck = async (value:FormValue) => {
    const response = await api(value);
    if (
      response.a=== "1" &&
      response.b=== "1"
    ) {
      //重複なしの場合はそのまま遷移
      next()
      return;
    }
    //エラーが含まれる場合はエラーダイアログを表示する
    if (
      response.a=== "3" ||
      response.b=== "3"
    ) {
      setDialogState({
        open: true,
        warn: null, // エラーが優先されるので、警告はnullでOK
        error: generateErrorArray(response.a, response.b)//いい感じに配列化する関数があるとする
      });
      return;
    }
    // 警告の場合
    setDialogState({
      open: true,
      warn: generateWarnArray(response.a, response.b)
      error: null,
    });
  };

  return (
    <>
      { dialogState.warn !== null && (
        <Dialog
          open={dialogState.open}
          meg={hogehoge}
          onClose={closeDialog}
        />
      )}
      { dialogState.error !== null && (
        <Dialog
          open={dialogState.open}
          msg={fugafuga}
          onClose={closeDialog}
        />
      )}
    </>
  );
};
```

愚直に書くとこのようなコードになるが、このコードの問題点を考えてみます。



まず、コンポーネント内のロジックがなーーんか妙に長い気がする、というコードのにおいを感じ取れるか、です。



大事な考え方として、フロントのデータ構造はバックエンドに準拠する必要は一切ない、ということです。
この場合、バックエンドからのレスポンスをそのまま受け取っていますが、この時点でロジックが短くなるようにデータ構造を整理しておけないか？という思考のプロセスを挟んでみます。

```typescript
const response = await api(value);
```

つまり、この時点でもっと都合のいい形でデータくれよ、っていうのをやっておけば良いんじゃないか？ということで、ここを関数化してこのあたりのロジックはまとめられそうではあります。
ただ、結局この問題の本質はフロントにとって都合のいいインターフェース(論理構造を具体化した型)が存在しないことにあるのを突き止めなければなりません。

都合のいいインターフェースを定義する

では具体的に落とし込んでいくと、上記コンポーネントにとってダイアログ表示ロジックにとって都合のいいインターフェースを考える。

```typescript
interface Output{
  result: "success" | "warning" | "error"
  subject: ("a" | "b")[] // (keyof Response)[]
}
```

まず、ダイアログを表示するのに本来必要なインターフェースってこんな感じだと思うんですよね。
出し分けに必要な「成功/警告/エラー」がわかればいいし、それはバックエンドのものをそのまま使う必要もない。で、aとbという項目のどっち、もしくは両方で起きてるかを配列で受け取ればいい。もちろんレスポンスとしては{warn:[a], error[b]}という形で返ってくる可能性はあるが、両方を表示する要件がなければコンポーネント目線では両方は要らないのです。
openという開閉状態も、実は別に持っておく必要がなく、状態がなければ表示しない、状態によって出すべきダイアログは排他的なので中を見て決めればOK、よって開閉状態は別に保つ必要ないこともわかります。

で、更に一歩進んで、警告・エラーがない場合、つまり”success”を受け取っている場合、subjectに値が入るワケがないので、これもインターフェースに落とし込みたい。

```typescript
type Output={
  result: "success"
} | {
  result: "warning" | "error"
  subject: ("a" | "b")[]
}
```

こっちが正しい論理構造であるといえる。

この状態で、レスポンスを関数なりスキーマなり何らかの手段でこのOutput型で受け取れるようにすると…

```typescript
type DialogState = Output;

export const SamplePage = () => {
  // ダイアログ表示制御関連
  const [dialogState, setDialogState] = useState<DialogState | null>(null);
  
  const onDuplicateCheck = async (value:FormValue) => {
    const output: Output= convert(await api(value));
    if (output.result === "sucess") {
      //重複なしの場合はそのまま遷移
      next()
      return;
    }
    setDialogState(output);
  };

  return (
    <>
      <Dialog
        open={dialogState.result == "warning"}
        meg={hogehoge}
        onClose={closeDialog}
      />
      <Dialog
        open={dialogState.result == "success"}
        msg={fugafuga}
        onClose={closeDialog}
      />
    </>
  );
};
```

めっちゃシンプルになりましたね。(笑)

これで終わってもいいのですが、もう一段階リファクタが可能なポイントがあります。どこだか分かりますかね？



```typescript
type DialogState = Output;
```

これ、何のためにわざとらしく書いてたのかというと、実際の論理構造の理想は実は下記になります。



```typescript
type DialogState = Exclude<Output,{result: "success"}>;
```

ダイアログの表示にsuccess状態ってこの例だと関係ないですよね。なので、Excludeで除外可能です。例がシンプルなのでここで恩恵があるかっていうと特にないんですが、このように正確な型が定義できるようになると、よりコードの品質が上がり、シンプルでメンテしやすいコードが生み出せるようになっていくんじゃないかなと思います。
