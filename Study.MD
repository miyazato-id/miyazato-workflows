# GitHub Actions Workflow 学習メモ

対象Workflow:

```yaml
name: miyazato-kt1-account1-je-network
```

---

# 1. Workflowの基本構造

GitHub Actionsは大きく以下の構造で作られる。

```text
Workflow
├で実行├─ name

---

## paths

```yaml
paths:
  - cloudformation/kt1/account1-je/network.yml
```

意味

- 対象ファイルが変更された場合のみ実行

例

✅ 実行される

```text
network.yml変更
```

❌ 実行されない

```text
README.mdのみ変更
```

---

# 4. permissions

```yaml
permissions:
  id-token: write
  contents: read
  pull-requests: write
```

## 役割

WorkflowがGitHubに対して持つ権限を定義する。

---

## id-token: write

```yaml
id-token: write
```

### 用途

AWS OIDC認証で使用

Workflow内の

```yaml
aws-actions/configure-aws-credentials
```

で必要。

### イメージ

```text
GitHub
 ↓
OIDC Token発行
 ↓
AWS IAM Role Assume
 ↓
AWS操作
```

---

## contents: read

```yaml
contents: read
```

### 用途

リポジトリを読み取るための権限。

Workflow内では

```yaml
actions/checkout@v4
```

で使用される。

---

## pull-requests: write

```yaml
pull-requests: write
```

### 用途

PRへコメントする権限。

Workflow内では

```bash
gh pr comment
```

で利用される。

---

# 5. jobs

```yaml
jobs:
```

## 役割

実際に実行する処理のまとまり。

---

今回のWorkflow

```yaml
jobs:
  deploy-account1-network:
```

Jobは1つだけ。

---

構造

```text
Workflow
└─ deploy-account1-network
```

---

# 6. Jobの中身

```yaml
deploy-account1-network:
  name: Deploy miyazato-kt1-account1-je-network
  runs-on: ubuntu-latest
```

---

## Job ID

```yaml
deploy-account1-network
```

内部で利用されるJob名。

---

## 表示名

```yaml
name: Deploy miyazato-kt1-account1-je-network
```

Actions画面に表示される名称。

---

## 実行環境

```yaml
runs-on: ubuntu-latest
```

意味

```text
GitHub Hosted Runner(Ubuntu)
```

で実行する。

---

# 7. steps

```yaml
steps:
```

## 役割

Job内で実行する処理。

---

今回のWorkflow

```text
1. Checkout Code
2. Configure AWS Credentials
3. Create ChangeSet
4. Post ChangeSet Summary to PR
5. Deploy to AWS
6. Upload logs to S3
```

---

## 実行順序

上から順番に実行される。

```text
Step1
 ↓
Step2
 ↓
Step3
 ↓
...
```

---

# 8. uses と run の違い

---

## uses

既存Actionを利用する。

例

```yaml
uses: actions/checkout@v4
```

意味

```text
GitHub公式Actionを利用
```

---

例

```yaml
uses: aws-actions/configure-aws-credentials@v4
```

意味

```text
AWS公式Actionを利用
```

---

## run

シェルスクリプト実行。

例

```yaml
run: |
  aws cloudformation deploy
```

意味

```text
Runner上でコマンドを実行
```

---

# 9. PR時の動作

トリガー

```yaml
pull_request
```

---

処理

```text
Checkout
 ↓
AWS認証
 ↓
ChangeSet作成
 ↓
PRコメント
 ↓
ログ保存
```

---

## ChangeSetとは

CloudFormationの差分確認機能。

実際にはデプロイしない。

---

確認できる内容

```text
追加
変更
削除
置換
```

されるAWSリソース。

---

# 10. Push時の動作

トリガー

```yaml
push
```

---

処理

```text
Checkout
 ↓
AWS認証
 ↓
CloudFormation Deploy
 ↓
ログ保存
```

---

# 11. workflow_dispatch時の動作

トリガー

```yaml
workflow_dispatch
```

---

処理

```text
手動実行
 ↓
CloudFormation Deploy
 ↓
ログ保存
```

---

# 12. Jobは複数作成可能

今回

```yaml
jobs:
  deploy-account1-network:
```

のみ。

---

しかし以下のように複数作れる。

```yaml
jobs:
  validate:
  changeset:
  deploy:
```

---

構造

```text
Workflow
├─ validate
├─ changeset
└─ deploy
```

---

# 13. Jobを分けるメリット

## ① 失敗箇所が分かりやすい

```text
✅ validate
✅ changeset
❌ deploy
```

---

## ② 実行順序を制御できる

```yaml
needs: validate
```

例

```text
validate
 ↓
deploy
```

---

## ③ 並列実行できる

```text
lint
test
scan
```

を同時実行可能。

---

## ④ 管理しやすい

処理ごとに責任範囲を分離できる。

---

## ⑤ 再利用しやすい

共通Jobを他Workflowでも利用できる。

---

# 14. 今回のWorkflowの全体像

```text
on:
├─ pull_request
│
│  Checkout
│      ↓
│  AWS認証
│      ↓
│  ChangeSet作成
│      ↓
│  PRへ差分投稿
│      ↓
│  S3へログ保存
│
├─ push(main)
│
│  Checkout
│      ↓
│  AWS認証
│      ↓
│  CloudFormation Deploy
│      ↓
│  S3へログ保存
│
└─ workflow_dispatch
   （手動実行）
       ↓
   CloudFormation Deploy
       ↓
   S3へログ保存
```

---

# 理解したポイント

- `name` は表示名
- `on` は実行トリガー
- `permissions` はWorkflowの権限設定
- `jobs` は処理のまとまり
- `steps` はJob内の処理
- `uses` は既存Action利用
- `run` はシェル実行
- Jobは複数作成可能
- `needs` でJobの順序制御ができる
- 今回のWorkflowはJobが1つ
- PR時はChangeSetのみ
- Push時はCloudFormation Deploy実行
- OIDC認証によりAWSアクセスキー不要
├─ on
├─ permissions
└─ jobs
     └─ job
          └─ steps
```

---

# 2. name

```yaml
name: miyazato-kt1-account1-je-network
```

## 役割

GitHub Actions画面に表示されるWorkflow名。

## 特徴

- 任意の名前を付けられる
- 処理には影響しない
- 見やすさのための名前

例

```yaml
name: Deploy Network
```

```yaml
name: CloudFormation Deployment
```

でも問題ない。

---

# 3. on

```yaml
on:
```

## 役割

Workflowが実行される条件（トリガー）を定義する。

---

## Push時

```yaml
push:
  branches:
    - main
```

意味

- mainブランチへPushされたとき

---

## Pull Request時

```yaml
pull_request:
  branches:
    - main
```

意味

- main向けPR作成・更新時

---

## 手動実行

```yaml
workflow_dispatch:
```

意味

