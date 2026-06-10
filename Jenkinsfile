pipeline {
  agent any

  tools {
    nodejs 'Node20' 
  }

  environment {
    DOCKERHUB_REPO_BACKEND = 'JulianaFranco140/lymon-backend'
    DOCKERHUB_REPO_FRONTEND = 'JulianaFranco140/lymon-frontend'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: scm.branches,
          userRemoteConfigs: scm.userRemoteConfigs,
          doGenerateSubmoduleConfigurations: false,
          extensions: [
            [$class: 'SubmoduleOption', recursiveSubmodules: true, parentCredentials: true, trackingSubmodules: false, shallow: false]
          ]
        ])
      }
    }

    stage('Backend Build & Test') {
      steps {
        dir('lymon-backend') {
          powershell '''
            corepack enable
            corepack prepare pnpm@10.33.0 --activate
            pnpm install --frozen-lockfile
            pnpm run test:cov
            pnpm run build
          '''
        }
      }
    }

    stage('Frontend Build & Test') {
      steps {
        dir('lymon-frontend') {
          powershell '''
            corepack enable
            corepack prepare pnpm@10.0.0 --activate
            pnpm install --frozen-lockfile
            pnpm run test:cov:scope
            pnpm run build
          '''
        }
      }
    }

    stage('Docker build') {
      steps {
        powershell '''
          docker build -t $env:DOCKERHUB_REPO_BACKEND:latest lymon-backend
          docker build -t $env:DOCKERHUB_REPO_FRONTEND:latest lymon-frontend
        '''
      }
    }

    stage('Docker push') {
      when {
        branch 'main'
      }
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          powershell '''
            $env:DOCKER_PASS | docker login -u $env:DOCKER_USER --password-stdin
            docker push $env:DOCKERHUB_REPO_BACKEND:latest
            docker push $env:DOCKERHUB_REPO_FRONTEND:latest
          '''
        }
      }
    }
  }
  
  post {
    always {
      cleanWs()
    }
  }
}