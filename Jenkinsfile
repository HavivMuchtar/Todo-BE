pipeline {
    agent any
    stages {
        stage('Build stage') {
            steps {
                echo 'Building Stage'
                sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-containerization -t builderimage -t builderimage:latest --target builder .'
            }
        }
        stage('Delivery Stage') {
            steps {
                echo 'Delivery Stage'
                sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-containerization -t havivmuc/todo-be:jenkins-$BUILD_NUMBER --target delivery .'
            }
        }
        stage('Cleanup Stage') {
            steps {
                echo 'Clean All Other Unnecessary Images'
                sh 'docker system prune -f'
            }
        }
        stage('Push to DockerHub Artifactory') {
            environment {
                mydockerhubpassword = credentials('mydockerhubpass')
            }
            steps {
                    sh 'docker login -u havivmuc -p ${mydockerhubpassword}'
                    sh 'docker push havivmuc/todo-be:jenkins-$BUILD_NUMBER'
        }
    }
}
}
