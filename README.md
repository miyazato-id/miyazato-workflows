# cicd-test-infra

vesta-aws-infra/
├── .github/
│   ├── pull_request_template.md  # Pull Request テンプレート
│   └── workflows/                # GitHub Actionsの設定
│       ├── deploy-dv.yml         # 開発環境デプロイ自動化スクリプト
│       ├── deploy-st.yml         # 検証環境デプロイ自動化スクリプト
│       └── deploy-pr.yml         # 本番環境デプロイ自動化スクリプト
├── cloudformation/               # AWSリソースを定義するIaCコード
│   ├── dv/                       # 開発環境
│   │   ├── account1-je/          # アカウント1 (東京リージョン): バッチ処理機能、オンライン処理機能
│   │   │   ├── network.yml       # VPC, Route53
│   │   │   ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│   │   │   ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│   │   │   ├── cost.yml          # Budgets, Cost Explorer
│   │   │   └── app.yml           # API Gateway, ECS, CodeDeploy, Step Functions, EventBridge
│   │   ├── account2-je/          # アカウント2 (東京リージョン): 帯域制御機能
│   │   │   ├── network.yml       # VPC
│   │   │   ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│   │   │   ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│   │   │   ├── cost.yml          # Budgets, Cost Explorer
│   │   │   └── bandwidth.yml     # EC2(VyOS), EC2(踏み台サーバ)
│   │   └── account3-jw/          # アカウント3 (大阪リージョン): バックアップ機能
│   │       ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│   │       ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│   │       └── cost.yml          # Budgets, Cost Explorer
│   ├── st/                       # 検証環境
│   │   ├── account1-je/          # アカウント1 (東京リージョン): バッチ処理機能、オンライン処理機能
│   │   │   ├── network.yml       # VPC, Route53
│   │   │   ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│   │   │   ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│   │   │   ├── cost.yml          # Budgets, Cost Explorer
│   │   │   └── app.yml           # API Gateway, ECS, CodeDeploy, Step Functions, EventBridge
│   │   ├── account2-je/          # アカウント2 (東京リージョン): 帯域制御機能
│   │   │   ├── network.yml       # VPC
│   │   │   ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│   │   │   ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│   │   │   ├── cost.yml          # Budgets, Cost Explorer
│   │   │   └── bandwidth.yml     # EC2(VyOS), EC2(踏み台サーバ)
│   │   └── account3-jw/          # アカウント3 (大阪リージョン): バックアップ機能
│   │       ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│   │       ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│   │       └── cost.yml          # Budgets, Cost Explorer
│   └── pr/                       # 本番環境
│       ├── account1-je/          # アカウント1 (東京リージョン): バッチ処理機能、オンライン処理機能
│       │   ├── network.yml       # VPC, Route53
│       │   ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│       │   ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│       │   ├── cost.yml          # Budgets, Cost Explorer
│       │   └── app.yml           # API Gateway, ECS, CodeDeploy, Step Functions, EventBridge
│       ├── account2-je/          # アカウント2 (東京リージョン): 帯域制御機能
│       │   ├── network.yml       # VPC
│       │   ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│       │   ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│       │   ├── cost.yml          # Budgets, Cost Explorer
│       │   └── bandwidth.yml     # EC2(VyOS), EC2(踏み台サーバ)
│       └── account3-jw/          # アカウント3 (大阪リージョン): バックアップ機能
│           ├── security.yml      # IAM, KMS, SecurityHub, GuardDuty, Inspector, Detective
│           ├── log.yml           # CloudTrail, CloudWatch, Config, Data Firehose, Athena, Health, SNS
│           └── cost.yml          # Budgets, Cost Explorer
└── README.md                     # デプロイ手順