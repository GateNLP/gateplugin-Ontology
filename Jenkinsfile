pipeline {
    agent any
    triggers {
        // rebuild this plugin if gate-top gets rebuilt so we can check we haven't introduced a breaking change
        upstream(upstreamProjects: "gate-top", threshold: hudson.model.Result.SUCCESS)
    }
    stages {
       stage('Build') {
            steps {
                withAnt(installation: 'ANT-1.9.7', jdk: 'JDK1.8') {
                    sh 'ant clean build'
                }
            }
       }
       stage('Document') {
            when{
                expression { currentBuild.result != "FAILED" }
            }
            steps {
                withAnt(installation: 'ANT-1.9.7', jdk: 'JDK1.8') {
                    sh 'ant test javadoc'
                }
            }
            post {
                always {
                    warnings canRunOnFailed: true, consoleParsers: [[parserName: 'Java Compiler (javac)']], defaultEncoding: 'UTF-8', excludePattern: "**/test/**", failedNewAll: '0', unstableNewAll: '0', useStableBuildAsReference: true
                }
                success {
                    step([$class: 'JavadocArchiver', javadocDir: 'doc/javadoc', keepAll: false])
                }
            }
       }
    }
}