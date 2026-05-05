---
title: "AEO対策の具体的な方法 — Google AI OverviewとPerplexityに選ばれるサイトの作り方"
author: "田中 克彦"
organization: "合同会社Amplest"
canonical_url: "https://amplest.cloud/blog/aeo-strategy-guide"
published_at: "2026-04-16"
language: "ja"
---

# AEO対策の具体的な方法 — Google AI OverviewとPerplexityに選ばれるサイトの作り方

## 1. 背景・文脈

Google AI Overview、Perplexity、Bing Copilotといった回答エンジンの普及により、検索結果の形態が大きく変わりつつあります。かつての「10個の青いリンク」から、AIが生成した回答が検索結果の最上部に表示される時代へ移行しています。

AEO（Answer Engine Optimization）とは、こうしたAIが生成する回答のソースとして自社サイトが選ばれるよう最適化する施策です。対象となる主な回答エンジンは以下の通りです。

- **Google AI Overview** — Google検索の最上部に表示されるAI生成の要約
- **Perplexity** — ソース付きで回答する検索特化AI
- **Bing Copilot** — Microsoftの検索結果にAIが回答を付加

## 2. 解決すべき課題

従来のSEO対策では、検索順位を上げることに注力していましたが、回答エンジン時代ではAIに「信頼できるソース」として認識・引用されることが重要です。以下の課題に対応する必要があります。

- AIが自社サイトのコンテンツを正確に「理解」できていない
- 質問に対する明確な回答構造がサイトに不足している
- 著者情報や会社信頼性についての情報が不充分である
- AIクローラーがサイトへのアクセスを効率的に行えていない
- 効果測定の仕組みが存在しない

## 3. 解決策・アプローチ

### 柱1: 構造化データ（JSON-LD）の完全実装

構造化データはAIがサイトの内容を「理解」するための言語です。ページの種類に応じて、以下のスキーマを実装します。

**必須スキーマ一覧**

- トップページ → Organization + WebSite
- サービスページ → Service + FAQPage + BreadcrumbList
- 料金ページ → Product + FAQPage
- ブログ記事 → Article + FAQPage + BreadcrumbList
- 会社概要 → Organization + Person

**実装例: FAQPage スキーマ**

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "AEO対策とは何ですか？",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "AEO対策とは、AIが生成する回答のソースとして自社サイトが選ばれるよう最適化する施策です。"
      }
    }
  ]
}
```

FAQPageスキーマを実装したページは、AI引用率が未実装ページの2.5倍になるというデータがあります。

**チェックポイント**

- 全ページにBreadcrumbList JSON-LDがある
- サービスページにFAQPage JSON-LDがある（最低5問）
- ブログ記事にArticle JSON-LDがある
- トップページにOrganization JSON-LDがある
- Google Rich Results Testで全ページ検証済み

### 柱2: 質問応答構造のコンテンツ設計

AIは「質問に対する回答」を探しています。コンテンツをその構造に合わせることが重要です。

**H2/H3を質問文にする**

悪い例：
```
## 構造化データについて
```

良い例：
```
## 構造化データとは何ですか？
```

AIは質問文の見出しを検出し、直後の段落を「回答」として引用する傾向があります。

**回答の「ゴールデンパラグラフ」**

質問見出しの直後に、2〜3文で完結する回答段落を配置します。

```
## AEO対策にはどれくらいの費用がかかりますか？

AEO対策の費用は、構造化データの実装（10〜30万円）と月次運用（3〜15万円/月）に分かれます。
無料ツールを活用すれば、基本的な構造化データは自社で実装することも可能です。
以下、費用の内訳を詳しく解説します。

（詳細な説明が続く...）
```

このゴールデンパラグラフがAIに引用されるスニペットになります。

**リストと表の活用**

AIは情報が整理されたコンテンツを好みます。

- 手順は番号付きリスト（ol）で表示
- 比較は表（table）で表示
- 要素の列挙は箇条書き（ul）で表示

### 柱3: E-E-A-T（経験・専門性・権威性・信頼性）の強化

GoogleのAI Overviewは、E-E-A-Tスコアが高いサイトを優先的に引用します。

**著者情報の明示**

すべてのブログ記事に著者情報ボックスを設置し、以下の情報を含めます。

- 著者名（実名）
- 肩書き・専門分野
- 経歴・実績の要約
- Person スキーマでの構造化

**会社情報の充実**

About ページに以下を明記します。

- 会社名、所在地、設立年
- 事業内容と実績
- 代表者のプロフィール
- 取引実績・メディア掲載

**一次情報の定期公開**

AIが最も信頼するのは一次情報です。

- 自社の調査データ・統計
- ケーススタディ・事例紹介
- 業界レポート・ホワイトペーパー

月に1本以上、一次データを含む記事を公開することを推奨します。

### 柱4: テクニカルAEO

サイトの技術的な側面も回答エンジンの評価に影響します。

**robots.txt でAIクローラーを許可**

```
User-agent: OAI-SearchBot
Allow: /

User-agent: PerplexityBot
Allow: /

User-agent: ClaudeBot
Allow: /

User-agent: Googlebot-Extended
Allow: /
```

**llms.txt の設置**

サイトのルートに `llms.txt` を配置し、AIクローラー向けのサイト概要を提供します。これは新しい標準として注目されています。

**Core Web Vitals の最適化**

ページ読み込み速度が遅いサイトは、AIクローラーにとっても取得コストが高く、引用対象から外れやすくなります。

- LCP（Largest Contentful Paint）: 2.5秒以内
- FID（First Input Delay）: 100ms以内
- CLS（Cumulative Layout Shift）: 0.1以内