Steps: 

Please make sure in all steps you are at `Oregon` region. This is our region as starters they will not walk you alone. Either options Ohio and SF.

1. First create a user <terraform-user> in IAM board. Create a user group <terraform> with full S3, RDS, IAM, Route53 and EC2 piriviliges. Then `aws configure --profile <profileNama>`.  Keeping environment variables in a file can also be benficeial. Also see `~/.aws/credentials`, also `~/.aws/config` when in doubt. 

export AWS_ACCESS_KEY_ID="some access key id"
export AWS_SECRET_ACCESS_KEY="some secret key"
export AWS_REGION="us-west-2"
export AWS_PROFILE="terraform-user"
export AWS_SDK_LOAD_CONFIG=true
export AWS_SHARED_CREDENTIALS_FILE=~/.aws/credentials

Make sure to match the right profile in main.tf/provider.   

2. Pull `git clone github.com/tanerjn/kreuzwerker.git`

3. Open AWS from your browser. Change region to `Oregon`. Go S3, create a bucket at `us-west-2` region. 
   Name bucket `kreuzwerker-state`. Disable ACL.
   Disable Public Access, disable versioning, disable encryption.

4. 3. `cd test/modules/vpc && unzip secrets.zip -d ~/Downloads`

    - Answer question to decrypt the folder. I slither along in a muddy space I like to sit in a nice cool place. What I am?

4. Generate key pair by `ssh-keygen -m PEM`, type `test-kreuzwerker`, add `.pem` extension to the private key. Move both keys to secrets dir.`mv test-kreuzwerker.p*~/Downloads/secrets/`. Open https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#KeyPairs, Menu rigth->Actions->Import Key Pair and paste `test-kreuzwerker.pub` context there. Save it.

5. Go, `cd /test/kreuzwerker-site` directory, do `terraform init` and  `terraform plan` in order.

6. See there is no error and do, `terraform apply` and behold. 

7. Cross fingers. 
    
    If everything is alright, website is hosted at port 8000. Now, also try ssh to the machine `ssh -i ~/Downloads/secrets/test-kreuzwerker.pem ec2-user@ec2-IP>`. 
    Check egress and ingress, i.e `ping de.de`, `curl de.de` and hopefully see the traffic. 

    Also wise to check `terraform state list` once in a while.



