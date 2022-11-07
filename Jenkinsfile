pipeline {
    agent {
        label 'ubuntu-aws'
    }
    environment {
        REGISTRY_NAME               = credentials('REGISTRY_NAME')
        DOCKER_REGISTRY_USERNAME    = credentials('DOCKER_REGISTRY_USERNAME')
        DOCKER_REGISTRY_PASSWORD    = credentials('DOCKER_REGISTRY_PASSWORD')
    }

    stages {
        stage('git-checkout') {
            steps {
                // dir('wordpress') {
                git url: 'https://github.com/tindd1810/wordpress.git'
                // }
            }
        }
        stage('build wordpress/push-registry') {
            steps {
                sh '''#!/usr/bin/env bash
                cd wordpress
                docker login -u ${DOCKER_REGISTRY_USERNAME} -p ${DOCKER_REGISTRY_PASSWORD}
                docker build --tag "${REGISTRY_NAME}/wordpress-tindd:${BUILD_NUMBER}" .
                docker push "${REGISTRY_NAME}/wordpress-tindd:${BUILD_NUMBER}"
                '''
            }
        }
        stage('build db/push-registry') {
            steps {
                sh '''#!/usr/bin/env bash
                cd mysql
                docker login -u ${DOCKER_REGISTRY_USERNAME} -p ${DOCKER_REGISTRY_PASSWORD}
                docker build --tag "${REGISTRY_NAME}/mysql-tindd:${BUILD_NUMBER}" .
                docker push "${REGISTRY_NAME}/mysql-tindd:${BUILD_NUMBER}"
                '''
            }
        }
    }
}