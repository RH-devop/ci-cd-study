# CI/CD Study

Next.js アプリケーションを題材に、GitHub Actions と Vercel を使用した
CI/CD パイプラインを構築した学習プロジェクトです。

## 概要

GitHub の `main` ブランチへの push をトリガーとして GitHub Actions
を起動し、コードの検証から Vercel
への本番デプロイまでを自動化しています。

``` text
Local
  ↓ git push
GitHub / main
  ↓
GitHub Actions
  ├─ npm ci
  ├─ ESLint
  ├─ TypeScript Type Check
  └─ Next.js Build
  ↓
All checks passed
  ↓
Vercel Production Deploy
```

## CI/CD Pipeline

GitHub Actions では以下の処理を順番に実行します。

1.  Repository Checkout
2.  Node.js セットアップ
3.  依存関係のインストール（`npm ci`）
4.  ESLint による静的解析
5.  TypeScript の型チェック（`tsc --noEmit`）
6.  Next.js の Production Build
7.  Vercel CLI のセットアップ
8.  Vercel Production への自動デプロイ

Lint・Type Check・Build
のいずれかが失敗した場合は、後続のデプロイ処理を実行しない構成にしています。

## Workflow

GitHub Actions の Workflow は以下に定義しています。

`.github/workflows/ci.yml`

`main` ブランチへの push をトリガーに実行されます。

## Secrets

Vercel へのデプロイに必要な認証情報はコード内に直接記述せず、GitHub
Actions の Repository Secrets で管理しています。

-   `VERCEL_TOKEN`
-   `VERCEL_ORG_ID`
-   `VERCEL_PROJECT_ID`

Workflow から `${{ secrets.* }}`
を利用して参照することで、認証情報をリポジトリ上に公開しない構成にしています。

## 実際に確認したこと

CI/CD 構築時には TypeScript の型エラーにより Type Check が失敗し、Build
以降の処理が停止することを確認しました。

エラー修正後に再度 push し、

`Lint → Type Check → Build → Deploy`

の全工程が正常終了し、Vercel Production
への自動デプロイが実行されることを確認しています。

## 使用技術

-   Next.js
-   React
-   TypeScript
-   Node.js
-   npm
-   ESLint
-   Git / GitHub
-   GitHub Actions
-   Vercel
-   Vercel CLI

## 学習目的

CI/CD の概念を理解するだけでなく、実際にパイプラインを構築し、

-   Git の push を起点とした自動処理
-   CI によるコード品質チェック
-   エラー発生時のパイプライン停止
-   Secrets を利用した認証情報管理
-   CI 成功後の Production 自動デプロイ

までを一連の流れとして実装・確認することを目的としています。
