CREATING LOCAL GIT REPO AND PUSHING TO REMOTE GIT REPO
git init -b main
git add . (officially there is a branch shown if using git branch -a after this command is executed)
git commit -m "initial commit"
git remote add origin <remote git url> (after creating empty repo in your git remote platform e.g. github; note do not checkboxes for creating README.md or .gitignore to avoid conflicts when pushing from local git repo)
git remote -v (verify there is a remote url mapped to the remote)
git push -u origin main (push your main branch to origin remote while setting the default remote branch i.e. the upstream branch to origin so the next time you push this branch, you can just use git push)

or

git init
git branch -m main
git add .
git commit -m "initial commit"
git remote add origin <remote git url>
git remote -v
git push -u origin main


GIT PULL REQUEST
under settings of the remote git repo, create a branch protection rule under branches in the side menu that requires a pull request in order to merge into the main branch (branch pattern is main)
note: in order for the branch protection rule to kick-in, the repository must be public. If private, the rule will only work if you have a team/enterprise plan
note: also, if pushing directly to the main branch as the repo owner, you will bypass the rule but get a rule violation warning. In order for the rule to work i.e. prevent the merge, create a new local branch, then push to the origin branch, after which, you would see a banner in the remote platform e.g. github, to compare and do a pull request, which you can click to send a pull request for approval - 

git checkout -b my-feature-branch
git add .
git commit -m "commit"
git push -u origin my-feature-branch

then go to the remote platform e.g. github, and click on compare and do a pull request, send the pull request for approval

note: if you are the repo owner, you can't technically approve the pull request but you can check the option to bypass the rule, which will still count as a successful pull request


PROVISIONING EMULATOR RESOURCES
export AWS_ENDPOINT_URL="https://indiscretionary-subaerially-aleena.ngrok-free.dev"
export AWS_ACCESS_KEY_ID="test"
export AWS_SECRET_ACCESS_KEY="test"
export AWS_DEFAULT_REGION="us-east-1"

aws --endpoint-url=$AWS_ENDPOINT_URL ecr create-repository \
  --repository-name my-dotnet-api

aws --endpoint-url=$AWS_ENDPOINT_URL ecs create-cluster \
  --cluster-name production-fargate-cluster
  
aws --endpoint-url=$AWS_ENDPOINT_URL ecs register-task-definition \
  --cli-input-json file:///mnt/c/fargate-task-definition.json

aws --endpoint-url=$AWS_ENDPOINT_URL ecs create-service \
--cluster production-fargate-cluster \
--service-name dotnet-api-fargate-service \
--task-definition dotnet-api-fargate-task \
--desired-count 1 \
--launch-type FARGATE \
--network-configuration "awsvpcConfiguration={subnets=[subnet-12345678],securityGroups=[sg-12345678],assignPublicIp=ENABLED}"