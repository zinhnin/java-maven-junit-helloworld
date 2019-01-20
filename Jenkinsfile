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
            dir '.'
            label env.docker_image_name
        }
    }
    stages {
        stage('maven execution') {
            steps {
                script {
                    dir('.') {
                        sh 'set HTTP_PROXY=$HTTP_PROXY'
                        sh 'set HTTPS_PROXY=$HTTP_PROXY'
                        sh 'mvn clean verify'
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
                        // step([$class: 'CoberturaPublisher', 
                        //   coberturaReportFile: '**/reports/coverage.xml', 
                        //   failUnhealthy: false, 
                        //   failUnstable: false, 
                        //   maxNumberOfBuilds: 0, 
                        //   sourceEncoding: 'UTF_8'
                        // ])
                        // stepcounter settings: [[encoding: 'UTF-8', filePattern: 'web/**/*.py', filePatternExclude: 'web/tests/**/*.py,web/migrations/**/*.py,web/test_*.py', key: 'SourceCode'],[encoding: 'UTF-8', filePattern: 'web/tests/**/*.py,web/test_*.py', key: 'TestCode']]
                        // junit '**/reports/junit.xml'
                    }
                }
            }
        }
    }

}
