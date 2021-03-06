# プロセスを再起動と複製をDocker オーケストレーターに任せる

<br/><br/>

### 一段落説明

Kubernetes のような Docker ランタイムオーケストレータは、コンテナの健全性と配置の決定を行うのが非常に得意です。コンテナの数を最大化し、ゾーン間でバランスをとり、多くのクラスタ要因を考慮しながら、これらの決定を行います。言葉がなくても、彼らは失敗したプロセス(つまりコンテナ)を特定し、適切な場所で再起動します。にもかかわらず、CPU 利用率のためにノードプロセスを複製したり、障害時にプロセスを再起動したりするために、カスタムコードやツールを使いたくなる人もいるかもしれません(例えば、クラスタモジュール、PM2)。これらのローカルツールは、クラスタレベルで利用可能な視点やデータを持っていません。例えば、インスタンスリソースが3つのコンテナをホストでき、2つのリージョンやゾーンが与えられている場合、Kubernetes はゾーン間でコンテナを分散させるように注意します。このようにして、ゾーンやリージョナルの障害が発生してもアプリは生き続けます。逆にローカルツールを使ってプロセスを再起動する場合、Docker オーケストレータはエラーに気づかず、コンテナを新しいインスタンスやゾーンに再配置するような思慮深い決定を下すことができません。

<br/><br/>

### コード例 – 中間ツールを使わずに直接 Node.js を呼び出す

<details>

<summary><strong>Dockerfile</strong></summary>

```

FROM node:12-slim

# ビルドロジックはこちら

CMD ["node", "index.js"]
```

</details>

<br/><br/>

### アンチパターン コード例 – プロセスマネージャを使用する
<details>

<summary><strong>Dockerfile</strong></summary>

```
FROM node:12-slim

# ビルドロジックはこちら

CMD ["pm2-runtime", "indes.js"]
```

</details>
