## Error: error:0308010C:digital envelope routines::unsupported

<code>
opensslErrorStack:[ 'error:03000086:digital envelope routines::initialization error' ]

...

Node.js v18.12.1

...

</code>
問題背景：FRONT側npm run start実行しようとする際に発生するエラー。

問題思考：現バージョンを17以下に下げる。バージョンを下げたらまだ同じエラー起きているなら、Node.jsの競合が発生していると考えられる。


問題解決：残っている怪しいnode.jsを消す。

node.jsだけでなく、node_modulesフォルダーも削除する。今回はファイルエクスプローラーで怪しいnode_modulesフォルダーを検索するのは切口となっていた。node.js、nvm、npm全部一回削除してから再インストールすることで解決した。

<a href="https://dev.classmethod.jp/articles/completely-uninstall-nodejs-from-windows/">参考：node.jsをきれいに削除するため</a>