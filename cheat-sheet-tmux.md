tmuxチートシート
##コマンドライン指定
    # 新規セッション開始
    tmux
    
    # 名前をつけて新規セッション開始
    tmux new -s <セッション名>
    
    # セッションの一覧表示
    tmux ls
    
    # 接続クライアントの一覧表示
    tmux lsc
    
    # セッションを再開 ※-t <対象セッション名>でセッション名の指定も可能
    tmux a
    
    # セッションを終了 ※-t <対象セッション名>でセッション名の指定も可能
    tmux kill-session
    
    # tmux全体を終了
    tmux kill-server
    
    # その他コマンドを実行
    tmux [command [flags]]

##キー操作
プレフィックスキー入力後に入力するキー、プレフィックスキーのデフォルトは Ctrl + b

基本

    ?      キーバインド一覧
    :      コマンドプロンプト
            show-options -g や show-window-options -g 入力で設定一覧を表示
            -gはグローバル指定(デフォルト)の意、個別に設定された値は-g無しで確認する


セッション操作

    s      セッションの一覧選択
    d      セッションから離脱(デタッチ)
    $      セッションの名前変更
    ctrl+Z tmuxを一時中断 ※fgで復帰

ウインドウ操作

    c      新規ウインドウ作成
    w      ウインドウの一覧選択
    0-9    指定番号のウインドウへ移動
    &      ウインドウの破棄
    n      次のウインドウへ移動
    p      前のウインドウへ移動
    l      以前のウインドウへ移動
    '      入力番号のウインドウへ移動
    .      入力番号にウインドウ番号を変更
    ,      ウインドウの名前変更
    f      ウインドウの検索

ペイン操作

    %            左右にペイン分割
    "            上下にペイン分割
    q            ペイン番号を表示
    カーソル      指定方向のペインへ移動 ※連続押しでプレフィックス継続
    Ctrl-カーソル ペインのサイズを変更 ※連続押しでプレフィックス継続
    !            ペインを解除してウインドウ化
    x            ペインの破棄
    o            ペインを順に移動
    ;            以前のペインへ移動
    z            現在のペインを最大化/復帰
    スペース      レイアウトを変更
    Alt-1-5      レイウトを変更
    {            ペインの入れ替え(前方向)
    }            ペインの入れ替え(後方向)
    ctrl+o       ペインの入れ替え(全体)
    t            ペインに時計を表示

コピーモード

    [       コピーモードの開始（カーソルキーで自由に移動）
    スペース コピー開始位置決定（viモード）
    エンター コピー終了位置決定（viモード）
    ]       コピーした内容を貼り付け
    
    ※viモードで無い場合は、設定ファイルに set-window-option -g mode-keys vi を追加

# 設定エトセトラ

基本
tmuxの設定は ~/.tmux.conf か /etc/tmux.conf に記述する
set-option, set-window-option, bind-key コマンドを使って設定
set, setw, bind 等は上記コマンドの省略形

マウス操作を有効にする

    #マウス操作を有効にする
    set-option -g mouse on
    
    # スクロールアップするとコピーモードに入る
    bind-key -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
    
    # 最後までスクロールダウンするとコピーモードを抜ける
    bind-key -n WheelDownPane select-pane -t= \; send-keys -M

最初はこれだけ設定しておけば、マウスクリックでペイン選択、ペイン境界線ドラッグでサイズ調整、ホイール操作でバックスクロールが可能
また minttyの場合はShift押下中に左クリック範囲選択でコピー、右クリックでペースト(Options｜Mouseで変更)、Altも同時押下で矩形選択が出来る

プレフィックスキーを Ctrl + b から Ctrl + g に変更

    set-option -g prefix C-g
    unbind-key C-b
    bind-key C-g send-prefix

ターミナル起動時にtmuxを自動実行

```bash:~/.bash_profile
# 初回シェル時のみ tmux実行
if [ $SHLVL = 1 ]; then
  tmux
fi
```

# 参考情報
- [tmuxを使い始めたので基本的な機能の使い方とかを整理してみた](http://kanjuku-tomato.blogspot.jp/2014/02/tmux.html)
- [tmuxの基本的な使い方とコマンドのまとめ](http://www.task-notes.com/entry/20150711/1436583600)
- [tmux 2.1からmouse系のオプションが変更になった。](http://shinespark.hatenablog.com/entry/2015/10/31/155420)
- [tmuxのコピーモードを使わずに任意の範囲選択をする](http://qiita.com/mogulla3/items/ca5bfa585ef19ed1eb9b)
- [tmuxの外観をカスタムして、運用と開発を加速させる](http://blog.glidenote.com/blog/2011/12/16/custom-tmux-visual/)
- [tmuxでXmonadライクで直感的なキーバインドを設定する](http://qiita.com/ssh0/items/b423d65f723eecb81527)
- [達人に学ぶ.tmux.confの基本設定](http://qiita.com/succi0303/items/cb396704493476373edf)



