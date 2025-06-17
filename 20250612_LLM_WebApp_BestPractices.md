---
marp: true
theme: default
paginate: true
class: lead
style: |
  .highlight {
    background-color: #fff3cd;
    border-left: 4px solid #ffc107;
    padding: 10px;
  }
  .tech-stack {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 20px;
    border-radius: 10px;
  }
  .best-practice {
    background: #e8f5e8;
    border: 2px solid #28a745;
    padding: 15px;
    border-radius: 8px;
  }
  details {
    margin: 10px 0;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  summary {
    font-weight: bold;
    cursor: pointer;
    color: #007bff;
  }
  .code-highlight {
    background: #f8f9fa;
    padding: 15px;
    border-radius: 5px;
    border-left: 4px solid #007bff;
  }
---

# 🚀 LLM Webアプリ開発
## Claude Code × o3-pro ベストプラクティス

**TypeScript + Next.js で実現する AI駆動開発**

---

## 🎯 目次

<div class="tech-stack">

### 🔥 主要ユースケース
- 💬 **チャットボット** - 対話型AI体験の構築
- 🔍 **RAG** - 知識拡張型生成システム
- 🤖 **エージェント** - 自動化ワークフロー
- 🛠️ **ツール連携** - 外部システム統合

</div>

---

## 💬 チャットボット開発

### 🎪 システム設計の核心

<div class="best-practice">

**🏆 ベストプラクティス**
- システムプロンプトで役割明示
- コンテキスト管理の最適化
- ストリーミング応答でUX向上

</div>

<details>
<summary>🔧 詳細な実装ガイド</summary>

#### 🎭 システムプロンプトと役割定義
- 初期メッセージでシステム役割を設定
- ボットの人格やルールを明示
- 明確な指示で応答品質向上

#### 🧠 コンテキスト管理
- 過去の対話履歴を保持
- セッションIDでユーザごとに管理
- 履歴が長い場合は要約戦略

</details>

---

## 🚀 o3-pro の活用ポイント

<div class="highlight">

### 💡 大規模コンテキストの威力
**最大約20万トークン**のコンテキスト長を活用

</div>

<details>
<summary>⚡ 実装のコツ</summary>

<div class="code-highlight">

```typescript
// ストリーミング応答の実装例
const response = await openai.chat.completions.create({
  model: 'o3-pro',
  messages: conversationHistory,
  stream: true,
  max_tokens: 4000
});
```

</div>

- 複雑な対話や長文コンテンツに対応
- コスト増大に注意、重要な情報に絞る
- Server-Sent Eventsでリアルタイム表示

</details>

---

## 🛡️ セキュリティ & 型安全性

### 🔒 型定義とバリデーション

<div class="code-highlight">

```typescript
// チャットメッセージの型定義
type ChatMessage = {
  role: 'user' | 'assistant' | 'system';
  content: string;
};

// Zodでバリデーション
const messageSchema = z.object({
  prompt: z.string().min(1).max(4000)
});
```

</div>

<details>
<summary>🛡️ プロンプトインジェクション対策</summary>

- システムメッセージで指示制限
- OpenAIの内容フィルタAPI活用
- Anthropicの安全設定利用

</details>

---

## 🔍 RAG（知識拡張生成）

### 📚 外部知識の統合戦略

<div class="tech-stack">

**🎯 RAGの核心技術**
- ベクター検索で関連文書取得
- 埋め込みベクトルで類似度計算
- プロンプト構築で幻覚を削減

</div>

<details>
<summary>🔬 技術的詳細</summary>

#### 🧮 ベクター検索と埋め込み

<div class="code-highlight">

```typescript
// 文書の埋め込み取得
const res = await openai.embeddings.create({
  model: 'text-embedding-ada-002',
  input: queryText
});
const embedding: number[] = res.data[0].embedding;
```

</div>

#### 📝 プロンプト構築
- コンテキストとユーザ質問を明確に区別
- 「以下は参考情報です：...」で区別
- 提供情報を元にした回答を促進

</details>

---

## 🤖 エージェントワークフロー

### 🧠 マルチステップタスクの設計

<div class="best-practice">

**🎪 エージェント設計原則**
- 思考プロセスの可視化
- ツール組み合わせの最適化
- 安全性とループ制御

</div>

<details>
<summary>⚙️ 実装アーキテクチャ</summary>

#### 🎯 マルチステップタスクの計画
- Chain-of-Thought促進のプロンプト
- o3シリーズの推論能力活用
- 逐次的な思考プロセス

#### 🔧 ツールの組み合わせ
- システムメッセージでツール一覧定義
- LangChain.jsなどのライブラリ活用
- MCP（Model Context Protocol）準拠

</details>

---

## 🛠️ ツール呼び出し管理

### 🎮 関数コーリングの最適化

<div class="highlight">

**🎯 重要ポイント**
- 関数スキーマの明確な定義
- 入力検証の徹底
- 権限制御とエラーハンドリング

</div>

<details>
<summary>🔧 実装ガイドライン</summary>

#### 📋 関数定義のベストプラクティス
- シンプルで明確な責務
- 引数は必要最低限
- わかりやすい名前と説明

#### 🛡️ セキュリティ対策
- 受け取った引数の型・範囲チェック
- 危険な操作はデフォルト無効
- 人間の承認フローを組み込み

</details>

---

## 🔗 Claude Code統合

### 🎨 統合時の重要な考慮事項

<div class="tech-stack">

**🚀 統合のポイント**
- AI生成コードの必須レビュー
- プロジェクト規約との整合性
- モデル間の出力フォーマット調整

</div>

<details>
<summary>💡 実装のベストプラクティス</summary>

#### 🔍 AI生成コードのレビュー
- 型チェック（`npm run typecheck`）実行
- ユニットテストでロジック検証
- セキュリティ問題の確認

#### 📝 プロジェクト規約との整合
- `CLAUDE.md`にスタイルガイド記載
- コーディング規約やアーキテクチャ遵守
- 不適切な変更の追記修正

</details>

---

## 🎯 まとめ：成功への道筋

<div class="best-practice">

### 🏆 AI駆動開発の勝利パターン

1. **🔒 セキュリティファースト** - 型安全性と入力検証
2. **🎪 UX重視** - ストリーミングとリアルタイム応答
3. **🧠 知的統合** - RAGとエージェントの組み合わせ
4. **🛡️ 人間中心** - レビューと承認フロー
5. **⚡ パフォーマンス** - コスト最適化とキャッシュ戦略

</div>

---

## 🚀 Next Steps

### 💎 さらなる進化のために

- **🔬 実験的機能** - 新しいモデルの継続的評価
- **📊 メトリクス** - 応答品質とコストの監視
- **🌐 スケール** - 本番運用での最適化
- **🤝 コミュニティ** - ベストプラクティスの共有

<div class="highlight">

**🎉 AI駆動開発で未来を創ろう！**

</div>