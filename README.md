# digi-jenkins

Repository containing multiple Jenkins pipeline configurations for CI/CD experimentation.

## Overview

This repository contains various Jenkins pipeline configurations designed to demonstrate different approaches to continuous integration and deployment using Jenkins. The configurations showcase different agent setups and pipeline structures.

## Jenkins Pipeline Files

This repository contains multiple Jenkins pipeline configuration files:

- `Jenkinsfile` - Main pipeline configuration using a node-agent
- `Jenkinsfile2` - Alternative pipeline configuration
- `Jenkinsfile3` - Additional pipeline variant
- `Jenkinsfile4` - Fourth pipeline configuration variant

Each file represents a different approach or setup for running CI/CD processes.

## Main Jenkins Pipeline (Jenkinsfile)

The primary pipeline configuration (`Jenkinsfile`) includes the following stages:

```groovy
pipeline {
  agent { label 'node-agent' }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/AbdelrhmanEzzat/digi-jenkins.git'
      }
    }

    stage('Install') {
      steps {
        sh '''
          export NVM_DIR="$HOME/.nvm"
          [ - s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
          npm install
        '''
      }
    }

    stage('Test') {
      steps {
        sh '''
          export NVM_DIR="$HOME/.nvm"
          [ - s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
          npm test
        '''
      }
    }
  }

  post {
    success { echo '✅ All tests passed on EC2!' }
    failure { echo '❌ Build failed!' }
    always { cleanWs() }
  }
}
```

## Pipeline Stages Explained

1. **Checkout**: Clones the repository from GitHub
2. **Install**: Sets up the Node.js environment using NVM and installs dependencies
3. **Test**: Runs the test suite using npm test
4. **Post-build**: Provides feedback on build success/failure and cleans workspace

## Requirements

- Jenkins server with properly configured agents
- Node.js environment on the build agents
- Git access to the repository
- NVM (Node Version Manager) installed on agents

## Using These Pipelines

To use any of these pipeline configurations:

1. Create a new Jenkins job (Pipeline type)
2. Point the job to this repository
3. Jenkins will automatically detect and use the Jenkinsfile in the root
4. For alternative configurations, specify the path to the specific Jenkinsfile in the job configuration

## Agent Configurations

Different Jenkinsfiles may specify different agent labels:
- `node-agent`: Standard Node.js execution environment
- `ec2`: Amazon EC2 instances (mentioned in one version)

## Post-Build Actions

All pipelines include post-build actions that:
- Report success/failure status
- Clean the workspace after each build