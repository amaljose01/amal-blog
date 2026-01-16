---
layout: post
title: "Jenkins Full Setup Guide: CI/CD Pipeline Complete"
date: 2018-08-21 10:00:00 +0000
categories: [jenkins, ci-cd, devops, automation]
author: Amal
image: /assets/images/posts/2018-08-15-jenkins-full-setup-guide.svg
---



# Jenkins Full Setup Guide: CI/CD Pipeline Complete

## Introduction

Jenkins is an open-source automation server widely used for continuous integration and continuous deployment (CI/CD). This guide covers complete setup and configuration.

## Part 1: Jenkins Installation

### Prerequisites

```bash
# Java is required
java -version

# Install Jenkins on Ubuntu
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins

# Start Jenkins
sudo systemctl start jenkins
sudo systemctl enable jenkins

# Access Jenkins
# http://localhost:8080
```

### Initial Configuration

```
1. Get initial admin password
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword

2. Install suggested plugins
3. Create first admin user
4. Configure Jenkins URL
```

## Part 2: Jenkins Configuration

### Global Configuration

```
Manage Jenkins → Configure System

- Jenkins URL
- System Admin Email
- Number of Executors
- Labels
```

### Plugin Management

```
Essential Plugins:
- Git Plugin
- GitHub Plugin
- Pipeline
- Docker
- Email Extension
- Slack Notification
- Maven Plugin
```

## Part 3: Creating Pipelines

### Scripted Pipeline

```groovy
node {
    stage('Checkout') {
        checkout scm
    }
    
    stage('Build') {
        sh 'mvn clean build'
    }
    
    stage('Test') {
        sh 'mvn test'
    }
    
    stage('Deploy') {
        sh './deploy.sh'
    }
}
```

### Declarative Pipeline

```groovy
pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh './deploy.sh'
            }
        }
    }
    
    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

## Part 4: Advanced Features

### Webhook Integration

```
GitHub → Settings → Webhooks
Payload URL: http://your-jenkins:8080/github-webhook/
Content Type: application/json
Events: Push events
```

### Email Notifications

```groovy
post {
    failure {
        emailext(
            subject: "Build Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
            body: "Build failed. Check console at ${env.BUILD_URL}",
            to: "${env.CHANGE_AUTHOR_EMAIL}"
        )
    }
}
```

### Slack Integration

```groovy
post {
    always {
        slackSend(
            channel: '#builds',
            message: "Build ${env.BUILD_NUMBER}: ${currentBuild.result}",
            color: currentBuild.result == 'SUCCESS' ? 'good' : 'danger'
        )
    }
}
```

## Part 5: Jenkins Best Practices

### Security

```
- Enable authentication
- Use API tokens
- Secure credentials
- Run Jenkins in Docker with limited permissions
- Regular updates
```

### Performance Tuning

```
- Increase Java heap
- Disable unnecessary plugins
- Distribute load with agents/slaves
- Archive old builds
- Use proper Git configuration
```

## Summary

Jenkins enables:
- Automated builds
- Continuous testing
- Continuous deployment
- Team collaboration
- Quality assurance

Master Jenkins for enterprise CI/CD!
