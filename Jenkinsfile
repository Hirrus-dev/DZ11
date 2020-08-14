pipeline {
    agent any
    stages{
        stage ('git'){
            steps{
                git 'https://github.com/Hirrus-dev/boxfuse.git'
            }
        }
        stage ('build'){
            steps{
                withMaven(maven:'m-3'){
                    sh "mvn clean verify"
                }
                
            }
        }
        stage ('public'){
            steps{
                script{
                    docker.withRegistry('http://localhost:5000') {
                        def customImage = docker.build("my-image")
                        /* Push the container to the custom Registry */
                        customImage.push()
                    }
                }
            }
        }
        stage ('Delete Images') {
            steps{
                sh 'docker images -q -f dangling=true | xargs --no-run-if-empty docker rmi && docker rmi localhost:5000/my-image my-image'
            }
        }
        stage ('Deploy Image'){
            steps{
                script{
                    docker.withRegistry('http://localhost:5000') {
                        def customImage = docker.image("localhost:5000/my-image")
                        customImage.run('-p 8000:8080')
                    }
                }
            }
        }
    }
}