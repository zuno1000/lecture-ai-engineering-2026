# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：glossary_search_glossary
- STEP 2 の [tool call]：glossary_search_glossary({'query': '3レベル'})
- STEP 3 の [tool call]：fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
- STEP 4 の [tool call]：
  - aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング 概要'})
  - aws_aws___search_documentation({'search_phrase': '"Versioning" Amazon S3 overview what is'})
- 気づき・疑問：
  - 設定ファイルに数行書き足すだけで、エージェントのコードを変えずにツールが1個から7個に増えた。commandを書くと自分のCodespaces内で起動し、urlを書くと外部のサーバーに接続するという違いも[server]の行で確認できた。
  - STEP4では、1回目の検索で目的の内容が得られず、モデルが英語の検索語に変えてもう一度検索していた。そのため、結果としてモデル応答は3回になったと考えられる。

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎
- やったこと：
  - app/mcp_server.pyのTODOの行の下に`server.tool()(tools.get_lecture_summary)`を1行書き足した。
  - `uv run python -m app.mcp_agent --list-tools`でツールの一覧を確認した。
  - `uv run python -m app.mcp_agent --trace "第2回では何を学びますか？"`を実行した。
  - `uv run pytest -q`を実行した。
- 結果：
  - ツールの一覧が7個から8個に増え、glossary_get_lecture_summaryが加わった。
  - 質問に対して`[tool call] glossary_get_lecture_summary({'lecture': 2})`が呼ばれ、第2回の要点をもとにした回答が返った。なお、モデル応答回数は2回だった。
  - テストは21 passedですべて通った。
- 気づき・疑問：
  - エージェント側を変えていないのに、足したツールが使えたことが気になった点として挙げられる。これは、エージェントが起動するたびに公開されているツールの一覧をサーバーに問い合わせているためであると推察される。
  - 疑問として、存在しない回について聞いた場合、ツールが何を返しモデルがそれをどう回答に反映するのかが挙げられる。