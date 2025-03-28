## Overview

This repository is an example of setting up the SonarScanner Analysis with GitHub Actions pipeline for a java/maven project.  
We have multiple CI/CD Pipeline examples, one for connecting to SonarQube Server instance and the other to SonarQube Cloud instance.   

__**PLEASE READ OUR SONARQUBE DOCUMENTATION FOR WORKING WITH GITHUB ACTIONS PIPELINES**__  
[GitHub Actions Workflow with SonarQube Scanner](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/adding-analysis-to-github-actions-workflow/)  

## Important Information in Pipelines
- On triggers are set to only execute on changes to specific branches and a specific directory in the project, this can be modified with whatever trigger you would want to use.
- They have shallow fetch set to 0. this is required for SonarScanner to properly analyze your project.  
- For more information on how to limit your analysis scope and parameters available, please check **SonarScanner Analysis Scope** and **SonarScanner Analysis Parameters** in the Important Links section.
- The action used for SonarScanner Analysis is executed via the Maven command, which applies for both SonarQube Server and SonarQube Cloud. But they require different parameters. Examples for both are provided.
    - SonarQube Cloud Example: [sonarqube-cloud.yml](.github/workflows/sonarqube-cloud.yml)  
    - SonarQube Server Example: [sonarqube-server.yml](.github/workflows/sonarqube-server.yml) 
- For both `sonar.projectKey` and `sonarprojectName`, we are using the following `$(echo ${{ github.repository }} | cut -d'/' -f1)-gh_$(echo ${{ github.repository }}` as naming convention. This results in `OrgName-gh_RepoName`.  
- Please make sure you have set up your `SONAR_TOKEN` and `SONAR_HOST_URL` secrets or variables. In the command used, `SONAR_TOKEN` is set up as a secret and `SONAR_HOST_URL` is set a variable. If set up differently please change the prefix in the respective parameter.
- If your Java project is using a different version than the one executing the analysis. Please check [Project's specific JDK](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/languages/java/#project-specific-jdk).  

## Important Links
[SonarQube Server - GitHub Integration](https://docs.sonarsource.com/sonarqube-server/latest/devops-platform-integration/github-integration/introduction/)  
[SonarQube Cloud - GitHub Integration](https://docs.sonarsource.com/sonarqube-cloud/getting-started/github/)  
[SonarQube Server | Cloud Scan GitHub Action task](https://github.com/marketplace/actions/official-sonarqube-scan)  

[SonarScanner for Maven](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/scanners/sonarscanner-for-maven/)  

[SonarScanner Analysis Scope](https://docs.sonarsource.com/sonarqube-server/latest/project-administration/analysis-scope/)  
[SonarScanner Analysis Parameters](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/analysis-parameters/)   

[Java SonarScanner Analysis Extra Information](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/languages/java/)  
[Java Test Coverage](https://docs.sonarsource.com/sonarqube-server/latest/analyzing-source-code/test-coverage/java-test-coverage/)  

## Example to fail the entire pipeline if Quality Gate fails
In certain situations, you may want to halt/fail the pipeline if the SonarQube Quality Gate fails, preventing subsequent steps from executing.  
This can be achieved by adding `sonar.qualitygate.wait=true` to the parameters section in the **Maven** command.  

Example:
``` sh
    mvn -B verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -X \
          -f sonar-scanner-maven/maven-basic/pom.xml \
          -Dsonar.projectKey=$ORG_NAME-gh_$PROJECT_NAME \
          -Dsonar.projectName=$ORG_NAME-gh_$PROJECT_NAME \
          -Dsonar.projectBaseDir=sonar-scanner-maven/maven-basic/ 
          -Dsonar.qualitygate.wait=true
```

__**For more examples please check:**__
[Onboarding Examples](https://github.com/sonar-solutions/Onboarding-Examples-List)
