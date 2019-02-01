pipeline {
    options{
        timeout(time:1,unit:'HOURS')
    }
    environment {
        docker_image_name = "java8-maven3-junit5"
    }

    agent {
        dockerfile {
            filename 'Dockerfile'
            args '--build-arg NODE_ENV=123'
            dir '.'
            label env.docker_image_name
        }
    }
    stages {
        stage('maven execution') {
            steps {
                script {
                    dir('.') {
                        // sh 'set HTTP_PROXY=$HTTP_PROXY'
                        // sh 'set HTTPS_PROXY=$HTTP_PROXY'
                        sh 'mvn clean compile site package'
                    }
                }
            }
        }
        stage('Analysis') {
            steps {
                script {
                    dir('.') {
                        step([$class: 'JacocoPublisher', 
                              execPattern: 'target/*.exec',
                              classPattern: 'target/classes',
                              sourcePattern: 'src/main/java',
                              exclusionPattern: 'src/test*'
                        ])
                        stepcounter settings: [
                            [encoding: 'UTF-8', filePattern: '**/**/*.java', filePatternExclude: '**/tests/**/*.java', key: 'SourceCode'],
                            [encoding: 'UTF-8', filePattern: '**/tests/**/*.java', key: 'TestCode']
                        ]
                    }
                }
            }
        }
    }
}
