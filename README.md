# bgm-channel

作業用BGM(インスト)YouTubeチャンネル制作のためのナレッジ管理リポジトリ。

自分が作業中に聞きたい音を、AIを使って制作・公開するプロジェクト。
このリポジトリ自体は**音や動画を作る場所ではなく**、当たったプロンプトを
構造化して記録・改善・蓄積するための「頭脳」にあたる。

## 現在のフェーズ

**手動制作 + プロンプトのナレッジ蓄積 + プロンプト改善。**
音楽生成・画像生成・動画作成・編集はすべて手動。自動化はフォーマットが
固まってから段階的に足す。詳細な方針は `docs/production-report.md` を参照。

## ディレクトリ構成

```
bgm-channel/                 ← このリポジトリ(Git管理する)
├── README.md                この入口
├── CLAUDE.md                プロジェクトの憲法。Claude Codeが毎回読む
├── docs/
│   ├── production-report.md 決定事項の詳細レポート
│   └── releases.md          プロンプト→リリース→YouTube の対応表
├── prompts/
│   ├── _schema.md           記録フォーマットの定義
│   ├── music.yaml           Lyria 3 用プロンプト記録
│   └── image.yaml           Midjourney 用プロンプト記録
├── knowledge/
│   └── insights.md          「何が効いたか」の逆算メモ
└── .claude/
    └── skills/              将来の Skill 用(現在は空)

bgm-assets/                  ← リポジトリの外。音源・画像・動画を置く(Git管理しない)
```

## Claude Code での使い方

このリポジトリを Claude Code で開くと、`CLAUDE.md` の指示に従って
次の3つの役割で動く:

1. **記録** — 渡したプロンプトと評価を `_schema.md` の形式で
   `prompts/*.yaml` に追記する。当たり(winner)も却下(rejected)も残す。
2. **改善** — 起点の `id` を指定すると、1変数だけ変えた改善案を2〜3個出す。
   生成と合否判定はユーザーが手動で行う(Claude Codeは案出しのみ)。
3. **言語化** — 蓄積から見えた傾向を `knowledge/insights.md` に反映する。

### 典型的なフロー

1. Lyria 3 / Midjourney で手動生成する
2. 良かった/惜しかったプロンプトを評価とともに Claude Code に渡す
3. Claude Code が `prompts/*.yaml` に記録する
4. 「この id を改善して」と頼むと派生案が出る → また手で生成して評価
5. 公開したら `docs/releases.md` に動画とプロンプトの対応を記録する

## ルール(要約)

- 音は必ず `instrumental, no vocals`
- AIツールは有料ティアのみ。公開前に全曲を YouTube Copyright checker に通す
- AI開示トグルをオンにする
- 量産パターン禁止。世界観の一貫性(人間のキュレーション)が差別化の核
- メディア完成品はこのリポジトリに入れない(`bgm-assets/` か GCS / Drive へ)

詳細は `CLAUDE.md` と `docs/production-report.md` を参照。
