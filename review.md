# PHP App ① レビュー

## 全般

### 以下のaタグのリンクを押下した際にedit.phpの$_GETにどんな値が格納されるか説明してください。

```html
<a href="edit.php?todo_id=123&todo_content=焼肉">更新</a>
```

- `todo_id=123&todo_content=焼肉`という値が格納される

### 以下のフォームの送信ボタンを押下した際にstore.phpの$_POSTにどんな値が格納されるか説明してください。

```html
<form action="store.php" method="post">
    <input type="text" name="id" value="123">
    <textarea name="content">焼肉</textarea>
    <button type="submit">送信</button>
</form>
```

- `id=123`、`content=焼肉`という値が格納される

### `require_once()` は何のために記述しているか説明してください。

- 引数に渡したファイルで定義されている関数などを使用できるようにするため

### `savePostedData($post)`は何をしているか説明してください。

- `getRefererPath()`でリクエスト元のURLを文字列で取得し、そのリクエスト元のページのパスが`/new.php`であれば`createTodoData()`を実行、`/edit.php`であれば`updateTodoData()`を実行、`/index.php`であれば`deleteTodoData()`を実行といった具合に処理を振り分けている

### `header('location: ./index.php')`は何をしているか説明してください。

- `savePostedData()`を実行後、`index.php`へリダイレクトしている

### `getRefererPath()`は何をしているか説明してください。

- リクエスト元のURLを文字列で取得しそのパスを返している

### `connectPdo()` の返り値は何か、またこの記述は何をするための記述か説明してください。

- 返り値：PDOオブジェクト
- 記述の目的：データベースと接続するため

### `try catch`とは何か説明してください。

- `try`は通常実行する処理であり、`catch`は特定の条件下でのみ発生するエラー（例外）が生じた場合に実行する処理

### Pdoクラスをインスタンス化する際に`try catch`が必要な理由を説明してください。

- 例外が生じた場合も正常に動くよう対処する必要があるため

## 新規作成

### `createTodoData($post)`は何をしているか説明してください。

- `connectPdo()`の返り値であるPDOオブジェクトを`$dbh`へ、実行したいSQL文を`$sql`へそれぞれ格納し、`$dbh`のqueryメソッドの引数として`$sql`を渡すことでINSERT文を実行している

## 一覧

### `getTodoList()`の返り値について説明してください。

- deleted_atカラムが`NULL`と一致する全てのデータが格納されているgetAllRecordsオブジェクト

### `<?= ?>`は何の省略形か説明してください。

- `<?php echo ?>`の省略形

## 更新

### `getSelectedTodo($_GET['id'])`の返り値は何か、またなぜ`$_GET['id']` を引数に渡すのか説明してください。

- 返り値：getTodoTextByIdオブジェクト
- 引数に渡す目的：URLクエリパラメータ（index.phpでURLのパラメータとして渡したid）を取得し、それをそのままfunctions.phpのgetSelectedTodo関数に渡すため

### `updateTodoData($post)`は何をしているか説明してください。

- `connectPdo()`の返り値であるPDOオブジェクトを`$dbh`へ、実行したいSQL文を`$sql`へそれぞれ格納し、`$dbh`のqueryメソッドの引数として`$sql`を渡すことでUPDATE文を実行している

## 削除

### `deleteTodoData($id)`は何をしているか説明してください。

- 指定したidのレコードのdeleted_atカラムを現在の時間に更新している

### `deleted_at`を現在時刻で更新すると一覧画面からToDoが非表示になる理由を説明してください。

- connection.phpの`getAllRecords()`内に`WHERE deleted_at IS NULL`と記述することで、deleted_atカラムがNULLと一致するデータのみをtodosテーブルから取得し、functions.phpの`getTodoList()`で画面に表示しているため

### 今回のように実際のデータを削除せずに非表示にすることで削除されたように扱うことを〇〇削除というか。

- 論理削除

### 実際にデータを削除することを〇〇削除というか。

- 物理削除

### 前問のそれぞれの削除のメリット・デメリットについて説明してください。

- 論理削除のメリット：データを削除しないため、必要に応じて過去のデータを復元や参照することができる
- 論理削除のデメリット：データを削除しないため、過去のデータが蓄積されローディングなどの挙動に悪影響を及ぼすことがある
- 物理削除のメリット：データを完全に削除するため、不要なデータが蓄積されずローディングなどの挙動に悪影響を及ぼす可能性が低い
- 物理削除のデメリット：データを完全に削除するため、過去のデータを復元や参照することができない