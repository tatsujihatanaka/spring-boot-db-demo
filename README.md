# spring-boot-db-demo
Spring Boot demo for DB read and DataTables.  
Spring Boot を使用した DB 読み取りと DataTables のデモ。

---

## 1. Setup and Execute / 準備と実行

### 1.1. Setup / 準備
#### 1.1.1. DB
以下のファイルでテーブル定義と初期データの投入を行います。
- `src/main/resources/db/schema.sql`
- `src/main/resources/db/data.sql`

#### 1.1.2. DB Connection / DB接続設定
ユーザとパスワードを設定してください。
- `src/main/resources/.env`
```properties
DB_USERNAME=USERNAME
DB_PASSWORD=PASSWORD
```

### 1.2. Execute / 実行
#### 1.2.1. Command / コマンド
ターミナルで以下のコマンドを実行します。
```bash
mvn spring-boot:run
```

---

## 2. Repository Operation Rules / 本リポジトリの運用ルール

### 2.1. Purpose / 目的
- リポジトリ破壊を避ける  
- 初期状態を保持し、ロールバックを可能にする  
- `dev` への直接マージを禁止し、安全な運用を行う  

### 2.2. Branch Strategy / ブランチ戦略
| Branch | Description |
| :--- | :--- |
| **main** | リポジトリ保護のため空のまま運用 |
| **initial** | プロジェクト初期構成を保持するブランチ |
| **dev** | 開発の起点となるブランチ |
| **feature/** | イシュー単位で作成する作業ブランチ |
| **integration/** | feature マージ検証用の一時ブランチ |

---

### 2.3. Repository Creation Flow / リポジトリ作成時

#### **main**
- 何も書かずに初回コミット  
```bash
git commit --allow-empty -m "Initial blank commit"
```

#### **initial**
- `main` から作成し、プロジェクトの初期構成をコミット  
```bash
git checkout main
git checkout -b initial
# .gitignore へ .env / bin/ / out/ を追記後に実行
git add .
git commit -m "Add initial project structure"
```

#### **dev**
- `initial` から作成。開発の基点。
```bash
git checkout initial
git checkout -b dev
```

---

### 2.4. Repository Operation Flow / リポジトリ運用時

#### **feature/(issue-name)** - `dev` から作成し、作業を行う
```bash
git checkout dev
git fetch origin
git pull origin dev
git checkout -b feature/(issue-name)
```

#### **integration/(issue-name)** - マージ検証用の一時ブランチ。テスト後、問題なければ PR を作成して `dev` へ。
```bash
# 検証ブランチの作成とマージ
git checkout dev
git checkout -b integration/(issue-name)
git merge feature/(issue-name)

# 問題がある場合は削除してやり直し
git checkout dev
git branch -D integration/(issue-name)
```