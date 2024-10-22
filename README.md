リポジドリはGithub、Dockerイメージの取得元はECR Public Galleryに設定    
※アプリの業務用端末にECR Public Galleryのアクセス権限を追加必要かも    

初期構築手順    
1.S3バケットを手動で作成    
2.tfbackendファイルのバケット名を作成したバケット名に変更する    
3.以下のコマンドでinit    

dev環境    
terraform init -reconfigure -backend-config=_dev.tfbackend    

prod環境    
terraform init -reconfigure -backend-config=_prod.tfbackend    

4.以下のコマンドでplan、apply    
dev環境    
terraform plan -var-file=_dev.tfvars    

prod環境    
terraform plan -var-file=_prod.tfvars   

※ECSのサービス起動前にECRを作成してDockerイメージをpushする    
※デプロイが動くたびにLBに紐づけているターゲットグループが変更されるのでTerraformで差分として検知する    

ECS周り構築手順 ※ECS、CICD関連をコメントアウトしてapply→ECRにイメージをpush→残りをapplyでも可能    
1.VPC    
2.サブネット、NAT    
3.ルートテーブル、ルートテーブルアタッチ    
4.セキュリティグループ    
5.ロードバランサー、ターゲットグループ     
6.ECR   
7.ECSクラスター    
8.タスク起動用IAMロール      
9.タスク起動用IAMロールへのポリシー割り当て    
10.Cloud9を起動してECRにbuildしたイメージをpushする    
11.タスク定義、Cloudwatch logs    
12.ECSサービス    
13.ALB経由でコンテナに接続できることを確認    

CICD関連構築手順    
0.コンソールからGit hub接続を作成、コードのArnを修正    
1.コードデプロイアプリケーション    
2.コードデプロイ用IAMロールの定義、ポリシーアタッチ    
3.デプロイグループ    
4.Cloud9からgithubにcontainerフォルダをpushする    
※taskdef.jsonとbuildspec.ymlの${ACCOUNT_ID}をAWSアカウントIDに置き換える    
5.CodeBuild、CodeBuild用ロール    
6.CodePipeline    
7.コードを修正してgithubにpushしてCICDが動くことを確認    

