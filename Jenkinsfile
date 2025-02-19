pipeline{

    agent any

    environment{
        currJobName = jobName()
        imageName = "shahuljava/${currJobName}:${env.BUILD_NUMBER}"
        dockerCredentialsId = 'dockerhub'
        dockerImage = ''
    }

    tools{
        gradle 'gradle_8.11'
    }

    stages{

        stage('Gradle Build') {
            steps {
                sh "gradle clean build"
            }
        }

        stage('Building image'){
            steps{
                script{
                    dockerImage = docker.build imageName
                }
            }
        }

        stage('Deploy Image'){
            steps{
                script{
                    docker.withRegistry('', dockerCredentialsId){
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}

def String jobName() {
    def jobNameParts = env.JOB_NAME.tokenize('/') as String[]
    return jobNameParts.length < 2 ? env.JOB_NAME : jobNameParts[jobNameParts.length - 2]
}