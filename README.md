Steps To Follow
---------------

Install Python and TaskCat
---------------------------
TaskCat uses Python 3 so you will need to install Python, pip (the package installer for Python), and TaskCat via pip in AWS Cloud9.

cd ~/environment
sudo yum -y update
python --version
curl -O https://bootstrap.pypa.io/get-pip.py
python3 get-pip.py --user
sudo pip install --upgrade pip
pip3 install taskcat --user

taskcat --version

Clone Repository Into Your Cloud9 Server
----------------------------------------

cd ~/environment
git clone https://github.com/YOURGITHUBUSERID/taskcat-example.git
cd taskcat-example


Store your GitHub Personal Access Token in AWS Secrets Manager
--------------------------------------------------------------
In order for CodePipeline to use GitHub as a source provider it needs your GitHub personal access token. Since we want to run all changes automatically and we want to be secure, you need to store this secret in an encrypted location. You will do this in AWS Secrets Manager. Here are the steps:


1.Go to the AWS Secrets Manager Console.
2.Click Secrets and click the Store a new secret button.
3.Click on the Other type of secrets radio button.
4.Click on the Plaintext tab and enter the GitHub token value in the text area. You can get this token by going to Personal access tokens and creating one or using an existing token. To create a GitHub token, see the instructions here.
5.Leave the Select the encryption key dropdown with the DefaultEncryptionKey option selected.
6.Click the Next button.
7.Enter github/personal-access-token for the Secret name and description on the Secret name and description page and click Next.
8.On the Configure automatic rotation page, select the Disable automatic rotation radio button.
9.Click the Next button.
10.On the Review page, click the Store button.


Note:
----
There are a few things to note in this CloudFormation template. The default value for the GitHubToken parameter is configured and maintained in pipeline-taskcat.yml as shown below. This assumes that you created the secrets in AWS Secrets Manager and used the name github/personal-access-token. If you have not, you will need to make changes for the CloudFormation template to work.Also change github user name <GitHubUser> in pipeline-taskcat.yml to your account.

Default: '{{resolve:secretsmanager:github/personal-access-token:SecretString}}'


 Launch the Stack
 ----------------
 
aws cloudformation create-stack --stack-name pipeline-taskcat --capabilities CAPABILITY_NAMED_IAM --disable-rollback --template-body file:///home/ec2-user/environment/taskcat-example/pipeline-taskcat.yml --parameters ParameterKey=GitHubUser,ParameterValue=<YOURGITHUBUSERID> ParameterKey=GitHubRepo,ParameterValue=taskcat-example
 
 
 



