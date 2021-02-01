# Section-12-Deploying-Sample-App-With-CRUD-Functionality-on-AWS-With-CodePipeline-


# Deploy CRUD App with CodePipeline (For Single ec2) - Lab

**Step 1.Goto AWS Management Console>Services>Developers Tools>CodePipeline>Create Pipeline**

**Step 2.In Pipeline Settings give the following details:**
- Pipeline name - cp-crud
- Service role - Existing service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step 3.Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - crud-app
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 4.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - crud-cb
- Build type - Single build

Click on Next

**Step 5.Deploy-optional in Add deploy stage**
- Deploy provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Application name - crud-app-cd
- Deployment group - crud-app-cd-dg 

Click on Next

**Step 6.Review all stages**
- Click on Create pipeline

**Step 7. The pipeline has been created**
- See the progress of pipeline Source>Build>Deploy


**Step 8.Goto Ec2>crud-server Instance>Security Groups>sg-xxxx>Edit inbound rules**
- Protocol:Port = Tcp:8080 from Source(0.0.0.0/0)

Click on Save
**Step 9.After the successful deployment Copy the Public ip address and paste in browser to see that Application running**
 
**Step 10.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following commands
```sh
$ git status
$ git add .
$ git commit -m "changes index color for cp "
$ git push
```

**Step 11.Go back to see the change in Pipeline**
- Pipeline is activated Just now
- See the latest commit - "changes index color for cp "
- Monitor all stages Build>Source>Deploy

**Step 12.Open Postman Tool>Environment>Manage Environments**
- Select ec2 environment and update the url current Value as follows:
- e.g.13.222.125.52:8080

Click on Update and exit

**Step 13.Now select ec2 as Environment**

**Step 14.Goto Pipeline Deployment is successful now**

**Step 15.Now select {{url}}/create table and Click on Send**
- Table created successfully

**Step 16.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
  - Table is created

**Step 17.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/readData
- Click on Send

**Step 18.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/insertData
- Click on Send

**Step 19.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 20.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 21.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 22.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 23.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data deleted

**Step 24.Goto Postman Tool and select ec2 as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 25.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted

### End of Lab

# Deploy CRUD App with CodePipeline (ASG + ALB) - Lab

**Step 1.Goto Developers Tools>CodeDeploy>Applications>crud-app-cd>crud-app-cd-asg-dg**
- Click on Edit
- In deployment type select In-place
- Click on Triggers>Create Trigger>Create Deployment Trigger
  - Trigger name - dep-success
  - Amazon SNS Topics - Select your Topic"MyFirstTopic"
  - Click on Create Trigger

Click on Save changes

**Step 2.Goto AWS Management Console>Services>Developers Tools>CodePipeline>Create Pipeline**

**Step 3.In Pipeline Settings give the following details:**
- Pipeline name - cp-crud-asg-alb
- Service role - Existing service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step 4.Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - crud-app
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 5.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - crud-cb
- Build type - Single build

Click on Next

**Step 6.Deploy-optional in Add deploy stage**
- Deploy provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Application name - crud-app-cd
- Deployment group - crud-app-cd-asg-dg 

Click on Next

**Step 7. Review all stages**
- Click on Create pipeline

**Step 8. Monitor lifecycle events**

**Step 9. Goto your Email-inbox for Trigger notification**
- e.g.SUCCEEDED:AWS CodeDeploy notification setup for trigger MyFirstTopic

**Step 10. Goto Visual Studio Code**
- Edit index.ejs and about.ejs file for the new color
- Run the following commands
```sh
$ git status
$ git add .
$ git commit -m "changed color of index and about cp"
$ git push
```

**Step 11. Pipeline is activated for the changes**

**Step 12. Wait for pipeline to be completed**

**Step 13.Refresh the ALB to see the changes**

**Step 14. Open the Postman Tool**

**Step 15.Goto Environment>Manage Environments**
- Select ALB environment 
- Change the url current Value DNS name of ALB as follows:
- http://alb-crud-asg-xxxxx.ap-south-1-1.elb.amazonaws.com

Click on Update and exit

**Step 16.Now select ALB as Environment**

**Step 17.select {{url}}/create table and Click on Send**
- Table created successfully

**Step 18.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
- Table is created

**Step 19.Goto Postman Tool and select ALB as Environment**
- Put the following value - http://{{url}}/insertData
- Click on Send

**Step 20.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 21.Goto Postman Tool and Now select ALB as Environment**
- Put the following value - http://{{url}}/readData
- Click on Send

**Step 22.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 23.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 24.Goto Postman Tool and Now select ALB as Environment**

- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 25.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**

Check for the data deleted
**Step 26.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 27.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted

**Step 28.Goto Artifacts store to validate**
- Amazon S3>codepipelie-ap-south-1-852554154xx>cp-crud-asg-alb>SourceArti/
- Amazon S3>codepipelie-ap-south-1-852554154xx>cp-crud-asg-alb>Buildartif/

### End of Lab


# Deploy CRUD App with CodePipeline (Elastic Beanstalk) - Lab

**Step 1.AWS Console>Services>Elastic Beanstalk>Create Application**
- Create a web app
  - Application Name- crud-app-ebs
  - Platform 
    - Platform 
      - Node.js
   - Platform branch
      - Node.js 12 running on 64bit Amazon Llinux2
   - Platform version
      - 5.2.3(Recommended)
   - Application Code - Select Sample application

Click on Create Application

**Step 2. Creating environment started**
- Elastic BeanStalk launches an environment named crudappebs-env
- Click on crudappebs-env to see it running

**Step 3.AWS Console>Developers Tools>Pipeline>Pipelines>Create Pipeline**

In Pipeline Settings give following details:
- Pipeline name - cp-ebs
- Service role - Select Existing service role
- Select Allow AWS CodePipeline to create a service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step 4.Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - crud-App
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 5.Build - optional in Add build stage**
- Build provider - AWS code build
- Region - Asia Pacific(Mumbai)
- Project name - crud-cb
- Build type - Single build

Click on Next

**Step 6.Deploy-optional in Add deploy stage**
- Deploy provider - AWS Elastic BeanStalk
- Region - Asia Pacific(Mumbai)
- Application name - crud-app-ebs
- Deployment group - crudappebs-env

Click on Next

**Step 7.Review all stages**
- Click on Create pipeline

**Step 8. The pipeline cp-ebs has been created**
- Monitor the progress of all the stages Build>Source>Deploy

**Step 9 Deployment has been completed** 
- Refresh the crudappebs-env link 
- See that it is running

**Step 10.Open Postman Tool>Environment>Manage Environments**

**Step 11.Now select ebs as Environment and change its url value to "crudappebs-env link" and click on Update**

**Step 12.Select {{url}}/create table and Click on Send**
- Table created successfully

**Step 13.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
- Table is created

**Step 14.Goto Postman Tool and select ALB as Environment**
- Put the following value - http://{{url}}/insertData
- Click on Send

**Step 15.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 16.Goto Postman Tool and Now select ALB as Environment**
- Put the following value - http://{{url}}/readData
- Click on Send

**Step 17.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 18.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 19.Goto Postman Tool and Now select ALB as Environment**

- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 20.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**

Check for the data deleted
**Step 21.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 22.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted

**Step 23.Open Visual Studio Code and goto pages>index.ejs**
- Edit the file for new color
- save it
- Run the following command
```sh
$ git status
$ git add .
$ git commit -m "changed index color for cp-ebs "
$ git push
```

**Step 24.Go back to Pipeline and see that it is started for change**
- See the latest commit - "changed index color for cp-ebs "
- Monitor all stages Build>Source>Deploy

**Step 25.Deployment has been completed** 
- Refresh the crudappebs-env link
- See that color is changed

### End of Lab



# Deploy CRUD App with CodePipeline (Staging +Pre-prod) - Lab

**Step 1.Goto Pipeline>pipeline-cp-ebs>edit**
- Click on Add Stage
  - Stage name - Pre-prod
  - Click on add stage

**Step 2.Pre-prod>Add action group**
- Action name - Approval-1
- Action provider - Manual approval
- SNS topic ARN - arn:aws:sns:ap-south-1xxxxx-Topic
- URL for review - Copy Sample-node-env URL

Click on Done

**Step 3.Pre-prod>+ Add action group**
- Action name - single-ec2-deployment
- Action provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Input artifacts - BuildArtifact
- Application name - crud-app-cd
- Deployment group - cd-app-cd-dg

- Click on Done

Click on save to save pipeline changes

**Step 4.Open Visual Studio Code and goto pages>about.ejs**
- Edit the file for new color
- save it
- Run the following commands
```sh
$ git status
$ git add .
$ git commit -m "changed color of about for manual approval-1 "
$ git push
```
**Step 5. Pipeline has been activated for the changes**

**Step 6. Goto your Email-Inbox to approve the Pre-prod stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-1

Click on link to Approve

**Step 7. Go back to Pipeline and click on Review in Pre-prod stage**
- Give approval comment 

Click on Approve

**Step 8. Pre-prod "single-ec2-deployment" started after approval**
- Deployment has been completed

**Step 9. Open Postman Tool>Environment>Manage Environments**

**Step 10. Now select ec2 as Environment and change its url value to ec2 instance crud-server's IP**

**Step 11. Select {{url}}/create table and Click on Send**
- Table created successfully

**Step 12. Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
- Table is created

**Step 13.Goto Postman Tool and select ec2 as Environment**
- Put the following value - http://{{url}}/insertData
- Click on Send

**Step 14.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 15.Goto Postman Tool and Now select ec2 as Environment**
- Put the following value - http://{{url}}/readData
- Click on Send

**Step 16.Goto Postman Tool and Now select ec2 as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 17.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 18.Goto Postman Tool and Now select ec2 as Environment**

- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 19.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**

Check for the data deleted
**Step 20.Goto Postman Tool and Now select ec2 as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 21.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted

### End of Lab




# Deploy CRUD App with CodePipeline (Staging +Pre-prod + Prod) - Lab

**Step 1.Goto Developers tools>CodePipeline>Pipelines>cp-ebs>edit**
- Click on Add Stage 
  - Stage name - Production
  - Click on add stage

**Step 2.Production>Add action group**
- Action name - Approval-2
- Action provider - Manual approval
- SNS topic ARN - arn:aws:sns:ap-south-1xxxxx-Topic

Click on Done

**Step 3.Production>+ Add action group**
- Action name - fleet-of-ec2-instances-deployment
- Action provider - AWS CodeDeploy
- Region - Asia Pacific(Mumbai)
- Input artifacts - BuildArtifact
- Application name - crud-app-cd
- Deployment group - crud-app-cd-asg-dg

- Click on Done

Click on save to Save pipeline changes

**Step 4.Open Visual Studio Code and goto pages**
- Edit about.ejs and index.ejs file for new color
- save it
- Run the following commands
```sh
$ git status
$ git add .
$ git commit -m "changed index and about color for cp-production-deployment "
$ git push
```
**Step 5.Go back to Pipeline and see that it is re-started for changes**
- See the latest commit - "changed index and about color for cp-production-deployment "
- Monitor all stages Build>Source>Deploy

**Step 6.Deployment has been completed** 
- Refresh the crudappebs-env link
- See that color is changed for both the pages

**Step 7.Goto your Email-Inbox to approve the Pre-prod stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-1

Click on link to Approve

**Step 8.Go back to Pipeline and click on Approval-1>Review in Pre-prod stage**
- Give approval comment 

Click on Approve

**Step 9.Pre-prod "single-ec2-deployment" started after approval**
- Production stage is pending for approval

**Step 10.Goto your Email-Inbox again to approve the Production stage**
- Search for email with Subject:APPROVAL NEEDED: AWS CodePipeline-cp-ebs for action Approval-2

Click on link to Approve

**Step 11.Go back to Pipeline and click on Approval-2>Review in Production stage**
- Give approval comment 

Click on Approve

**Step 12.Production "fleet-of-ec2-instances-deployment" started after approval**

**Step 13 Monitor the progress of the Deployment**

**Step 14.After the deployment is completed run APIs**

**Step 15.Open Postman Tool>Environment>Manage Environments**

**Step 16.Now select ALB as Environment and change its url value to ALB DNS name**

**Step 17.select {{url}}/create table and Click on Send**
- Table created successfully

**Step 18.Check the Table created in DynamoDB**
- Goto AWS Console>DynamoDB>Tables
- Table is created

**Step 19.Goto Postman Tool and select ALB as Environment**
- Put the following value - http://{{url}}/insertData
- Click on Send

**Step 20.Now Goto AWS Console>DynamoDB>Tables>Items>info**
- New Item added successfully

**Step 21.Goto Postman Tool and Now select ALB as Environment**
- Put the following value - http://{{url}}/readData
- Click on Send

**Step 22.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/updateData
- Click on Send

**Step 23.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**
- Check for the data updated

**Step 24.Goto Postman Tool and Now select ALB as Environment**

- Put the value - http://{{url}}/deleteData
- Click on Send

**Step 25.Now Goto AWS Console>DynamoDB>Tables>Items>info>actors**

Check for the data deleted
**Step 26.Goto Postman Tool and Now select ALB as Environment**
- Put the value - http://{{url}}/deleteTable
- Click on Send

**Step 27.Now Goto AWS Console>DynamoDB>Tables**
- Refresh and see that Table is deleted


### End of Lab

# Section Clean-up - Lab

**Step 1.Goto Ec2 Dashboard>AutoScalingGroup>Auto Scaling groups**
- Select CodeDeploy_crud ASG to Delete>Confirm delete

**Step 2.Goto Elastic BeanStalk>Applications>crud-app-ebs**
- Click on Actions>Delete Application>Confirm deletion

**Step 3.Goto Ec2Dashboard>Load Balancing>Load Balancers**
- Select alb-cd>Actions>Delete the load balancer>Confirm Yes,delete

**Step 4.Goto Ec2Dashboard>Load Balancing>Target Groups**
- Select Target Group tg-cd>Actions>Delete>Delete target group>Confirm>Yes,delete

**Step 5.Goto Ec2 Dashboard and Terminate crud-server single Instance**

### End of Lab








