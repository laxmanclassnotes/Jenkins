### SpringPetClinic deployment using Jar file into S3-bucket & GitHub Pull Request
-----------------------------------------------------------------------------------
* Springpetcinic  github `fork` into our repository.
[Refer Here](https://github.com/spring-projects/spring-petclinic)
* Create a `Jenkinsfile` into our repository. 
* Create a two ec2 instances `master` and `node` 
      * Master: Java-17, jenkins install on ubuntu.
      * Node:  Java-8-17, Maven install.
* Take master <public-ip> and connect Jenkins.
* In these In these `manage jenkins` add credentials, jdk-paths in global-tool configuration and nodes are created using prive ip of (node) ec2 machine. 
![preview](./images/jenkins18.png)
* Crate one job using pipeline in these git credentials are added.
* In Jenkinsfile
[Refer Here](https://github.com/laxman1849/spring-petclinicpr12.git)
* Build the maven package
* After we take jar file move to `S3-bucket` following these steps:
*  Navigate to IAM and create one group 
![Preview](./images/jenkins19.png)
* And give permissions to 
![Preview](./images.jenkins20.png)
![preview](./images/jenkins21.png)
![preview](./images/jenkins22.png)
 * And generate Accesskey and Secreatkey  these keys are copy and paste it notepad ++
    * We install aws-cli on ubuntu in `node`
    * Use these below steps:
```
  sudo apt update
  sudo apt install awscli -y
  aws --version
  aws config
```
    * In these we add aws accesskey and Secreatkey
![preview](./images/jenkins26.png)
 * Navigste to S3 and crate one new bucket and give permissions and  `ACL` enabled
![Preview](./images/jenkins23.png)
* And added below thse below lines for to upload to S3 bucket 
``` yaml
stage('craeting folder') {
            steps {
                sh "mkdir -p /tmp/${JOB_NAME}/${BUILD_ID}"
                sh "cp -r **/spring-petclinic-*.jar /tmp/${JOB_NAME}/${BUILD_ID}"
                sh "aws s3 sync /tmp/${JOB_NAME}/${BUILD_ID} s3://<s3 bucket name> --acl public-read-write"
            }
        }
```
* Then go to jenkins and bild the pipeline then build Success we see the Jar file into our S3 bucket.
![preview](./images/jenkins24.png)
![preview](./images/jenkins25.png)
* Successfully uploaded
* Next `deploy` the above format using `ansible-playbook` 
* Requirements:
    * Install ansible on ubuntu 
```
 sudo apt-add-repository ppa:ansible/ansible
 sudo apt update
 sudo apt install ansible -y
```
* and follow below these commands
![preview](./images/jenkins27.png)
  `sudo vi hosts <localhost>`
![preview](./images/jenkins28.png)
[Refer Here](https://github.com/laxman1849/spring-petclinicpr12/blob/develop/Jenkinsfile)in develop branch jenkins file.
[Refer Here](httphttps://github.com/laxman1849/spring-petclinicpr12/blob/release/Jenkinsfile)for ansible-playbook steps
[Refer Here](https://github.com/laxman1849/spring-petclinicpr12/blob/release/springpetclinic.yaml) springpetclinic.yaml file for (S3 Jar file)
[Refer Here](https://github.com/laxman1849/spring-petclinicpr12/blob/release/springpet.service) for service file.
[Refer Here](https://github.com/laxman1849/spring-petclinicpr12/blob/release/hosts)host file

[Refer Here](https://github.com/laxman1849/spring-petclinicpr12/tree/release)for change branch content.
* Navigate to pipeline and create one pipeline build for release branch then the build was Succesfull we see the results as below
![preview](./images/jenkins30.png)
![preview](./images/jenkins29.png)
![preview](./images/jenkins31.png)

### GitHub Pull Request Builder
-------------------------------

* Open Jenkins Navigete to manage jenkins ==> Manage plugins => install `GitHub Pull Request Builder` 
[Refer Here](https://plugins.jenkins.io/ghprb/) dollow these steps.
* Go to Github account > settings > Developer settings > Personal AccessToken select`classic` > generate token and give permissions 
* Once generate the token copy the notepad 
![Preview](./images/jenkins32.png)
* Navigate to manage jenkins go to `credentials` and new token add with username and passwd
![Preview](./images/jenkins33.png)
* Navigate to configure system goto GitHub Pull Request Builder 
![Preview](./images/image34.png)
* Create one Free style project (select gihub project)
[Refer Here](https://github.com/laxman1849/spring-petclinicpr12/tree/develop)
   * In these we selecting `Jenkinsfile` (we take release branch details)
     * given steps follow
![Preview](./images/jenkins35.png)
![Preview](./images/jenkins36.png)
![Preview](./images/jenkins37.png)
![Preview](./images/jenkins38.png)   
![Preview](./images/jenkins39.png)
* In these above Images we Take only the original `GitHub Repositry` Url's
  * Navigate to GitHub Account 
![Preview](./images/jenkins40.png)
![Preview](./images/jenkins41.png)
![Preview](./images/jenkins42.png)
![Preview](./images/jenkins43.png)
* Then Fork the Content into Another account 
![Preview](./images/jenkins44.png)
![Preview](./images/jenkins45.png)
![Preview](./images/jenkins46.png) 
 * In these `dummy repository`add some changes and commit it
![Preview](./images/jenkins47.png)  
 * Navigate to pull request
![Preview](./images/jenkins48.png)
![Preview](./images/jenkins49.png)
![Preview](./images/jenkins50.png)
* select pullrequest then atomatically triggered in our project
![Preview](./images/jenkins51.png)
![Preview](./images/jenkins52.png)
* optional ( Then you can use this changes content merge you use ).   