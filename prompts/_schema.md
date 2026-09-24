# プロンプト記録スキーマ

`prompts/music.yaml` と `prompts/image.yaml` の各エントリはこの形式に従う。
1プロンプト = 1レコード。当たりも却下も同じファイルに入れ、`status` で区別する。

## フィールド定義

| フィールド | 必須 | 説明 |
|---|---|---|
| `id` | ○ | 一意なID。`YYYY-MM-DD-連番`(例: `2026-06-14-001`)。 |
| `parent` | △ | 改善の起点となったエントリの `id`。派生でなければ省略可。 |
| `changed` | △ | 親から**何を変えたか**を1語で(例: `tempo 70->65` / `instrument -drums`)。派生時のみ。 |
| `tool` | ○ | 使用ツール。`lyria3` / `midjourney` / その他。 |
| `status` | ○ | `winner`(採用) / `testing`(検証中) / `rejected`(却下)。 |
| `rating` | ○ | 5段階(1=外し, 5=理想)。 |
| `genre` | ○ | ジャンル/世界観タグ。`lofi` / `ambient` / `piano` / `dnb` / `minimal` など。語彙を揃える。 |
| `prompt` | ○ | プロンプト全文。 |
| `negative` | △ | ネガティブプロンプト(あれば)。 |
| `params` | △ | seed等のパラメータ。`{ seed: 12345 }` の形。 |
| `note` | ○ | **「何が効いたか / 何が惜しかったか」を一言。** 最重要項目。 |
| `date` | ○ | 生成日(`YYYY-MM-DD`)。 |

## status の運用

- `testing`: 生成したが評価が定まっていない、または派生検証中。
- `winner`: チャンネルに使える/使った。
- `rejected`: 使わない。**削除せず残す。** `note` に却下理由を必ず書く。

## parent / changed の使い方(系譜管理)

プロンプト改善で派生を作ったら、`parent` に起点の `id`、`changed` に変更点を1語で残す。
これにより「どの変更が効いた/外したか」を後から辿れる。

- `changed` は1変数のみ(改善ルールに従い同時に複数変えない)。
- 例: `tempo 70->65` / `instrument +felt-piano` / `negative +no-reverb` / `texture +tape-hiss`
- 後で「BPM変更系の派生だけ集めて傾向を見る」といった逆算がしやすくなる。

## 評価(rating)の目安

- 5: そのまま使いたい / 4: 微調整で使える / 3: 方向は合うが詰めが必要 / 2: 致命的にズレ / 1: 完全に外れ

## エントリ例(音楽・起点)

```yaml
- id: 2026-06-14-001
  tool: lyria3
  status: winner
  rating: 5
  genre: lofi
  prompt: >
    Warm lo-fi hip-hop for deep focus, around 70 BPM, mellow and steady.
    Dusty Rhodes piano chords, muted boom-bap drums, vinyl crackle.
    Instrumental, no vocals.
  negative: "no vocals, no tempo changes, no harsh transitions"
  params: { seed: null }
  note: "Rhodesの温かさが満足度の核?要・派生検証"
  date: 2026-06-14
```

## エントリ例(音楽・派生)

```yaml
- id: 2026-06-15-003
  parent: 2026-06-14-001
  changed: "tempo 70->65"
  tool: lyria3
  status: testing
  rating: 4
  genre: lofi
  prompt: >
    ...(BPMのみ65に変更、他は親と同じ)...
  note: "より沈み込む。集中には合うが眠くなる手前。Rhodes要因の検証はまだ"
  date: 2026-06-15
```
