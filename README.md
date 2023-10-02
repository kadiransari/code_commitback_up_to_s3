# code_commitback_up_to_s3
version: 0.2
phases:
  install:
    commands:
      - pip install git-remote-codecommit
  build:
    commands:
       - env
       #- yum install git
       - aws configure set aws_access_key_id <aws_access_key>
       - aws configure set aws_secret_access_key <aws_secret_access_key>
       - aws configure set region ap-south-1
       - git config --global credential.helper '!aws codecommit credential-helper $@'
       - git config --global credential.UseHttpPath true
       - git clone https://git-codecommit.ap-south-1.amazonaws.com/v1/repos/argocd-project
       - dt=$(date '+%d-%m-%Y-%H:%M:%S');
       - echo "$dt" 
       - zip -yr $dt-$REPOSITORY_NAME-codecommitbackup.zip ./
       - aws s3 cp $dt-$REPOSITORY_NAME-codecommitbackup.zip s3://test-bucket72081118  # Replace 'test-bucket72081118' with your bucket name

artifacts:
  files: '**/*'
