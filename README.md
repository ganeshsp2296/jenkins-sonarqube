# jenkins-sonarqube
# Jenkins + SonarQube Integration Pipeline

This project demonstrates a complete CI/CD pipeline using **Jenkins**, **SonarQube**, and **GitHub Webhooks** for automated code quality checks and branch-based builds.

---

## 🔧 Technologies Used

- **Jenkins** (Pipeline, Git, SonarQube Scanner)
- **SonarQube** (Self-hosted with PostgreSQL)
- **GitHub** (Source control + webhook trigger)
- **Ubuntu 24.04 EC2 instance (t2.medium)**
- **Java 17, Maven, Git, sonar-scanner CLI**

---

## 🚀 Features Implemented

### ✅ 1. Jenkins + SonarQube Integration

- SonarQube installed on same EC2 instance (`http://<public-ip>:9000`)
- Jenkins connected to SonarQube via **Manage Jenkins → Configure System**
- SonarQube token securely added to **Jenkins credentials**
- Code analysis is done automatically via `sonar-scanner` CLI

### ✅ 2. Parameterized Jenkins Pipeline

- Pipeline accepts a `BRANCH_NAME` as input
- Enables building any feature or release branch directly
- Code is cloned from GitHub and analyzed per-branch

### ✅ 3. GitHub Webhook Integration

- GitHub webhook added under **Settings → Webhooks**
- Push events trigger Jenkins job automatically
- Webhook URL format:  
  `http://<jenkins-ip>:8080/job/jenkins-sonarqube-job/build?delay=0sec`

---

## 📄 Jenkinsfile (Pipeline Script)

```groovy
pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/ganeshsp2296/jenkins-sonarqube.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonar') {
                    sh 'sonar-scanner'
                }
            }
        }
    }
}
