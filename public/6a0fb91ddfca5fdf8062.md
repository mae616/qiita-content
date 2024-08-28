---
title: 'Uno CSS: Warning: Received `true` for a non-boolean attribute `flex`.'
tags:
  - TypeScript
  - React
  - エラー対処
  - unocss
private: false
updated_at: '2024-08-28T17:57:40+09:00'
id: 6a0fb91ddfca5fdf8062
organization_url_name: null
slide: false
ignorePublish: false
---
UnoCSSの適用で詰まったとこのエラー対処のメモです。
あとで見たら思いっきり公式ドキュメントに書いてありましたが。

# 環境
Vite
React
TypeScript


# 経緯
これ見てUnoCSSてなんかいいなと思って軽い気持ちで触ってみようとインストールしました。

https://www.youtube.com/watch?v=ymEF5u5m4Hw



UnoCSSをインストール 
参考: https://unocss.dev/integrations/vite
```zsh
yarn add -D unocss
```
設定
```diff_typescript: vite.config.ts
+ import UnoCSS from 'unocss/vite'
+ import React from '@vitejs/plugin-react'
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [
+    UnoCSS(),
+    React()
  ],
})

```

```diff_typescript: uno.config.ts(新規作成)
+ import { defineConfig } from 'unocss'
+ 
+ export default defineConfig({
+   // ...UnoCSS options
+ })
```

main.tsxに下記を追加
```
import 'virtual:uno.css'
```


動画見てカッコ良さそうだったから
@unocss/preset-attributify　を追加した。

```zsh
yarn add -D @unocss/preset-uno
yarn add -D @unocss/preset-attributify
```


参考: https://unocss.dev/presets/attributify
```diff_typescript: uno.config.ts
import {
  defineConfig,
+  presetAttributify,
+  presetUno,
} from "unocss";

export default defineConfig({
  presets: [
+    presetAttributify(),
+    presetUno()
  ],
})
```

TypeScriptで属性が解釈できなくてエラー
> Typescript errors: Property xxx does not exist on type
的なの

参考: https://github.com/unocss/unocss/issues/742
を見て、

```diff_typescript: shims.d.ts (新規作成)
+ import type { AttributifyAttributes } from '@unocss/preset-attributify'
+ 
+ declare module 'react' {
+   interface HTMLAttributes<T> extends AttributifyAttributes {}
+ }
```

```diff_json: tsconfig.json

    (略)
    "include": [
             "src", 
             "images.d.ts", 
+            "shims.d.ts"
         ],
```
公式にも書いてあった: https://unocss.dev/presets/attributify#react



# で、エラー
JSXにflexを追加したら
> Warning: Received `true` for a non-boolean attribute `flex`.


ちゃんとvite.configで`React()`より`UnoCSS()`を前に書いてるけどな...🤔
参考: https://unocss.dev/integrations/vite#frameworks


# エラー対処
https://unocss.dev/transformers/attributify-jsx

```diff_typescript: uno.config.ts
import {
  defineConfig,
  presetAttributify,
  presetUno,
+  transformerAttributifyJsx,
} from "unocss";

export default defineConfig({
  presets: [presetAttributify(), presetUno()],
+  transformers: [transformerAttributifyJsx()],
});
```

したら治った。

思いっきり　https://unocss.dev/presets/attributify#valueless-attributify に書いてあった。
