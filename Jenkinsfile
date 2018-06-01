pipeline {
    agent any
    triggers {
        // rebuild this plugin if gate-top gets rebuilt so we can check we haven't introduced a breaking change
        upstream(upstreamProjects: "gate-top", threshold: hudson.model.Result.SUCCESS)
    }
    options {
        disableConcurrentBuilds()
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
                expression { currentBuild.result != "FAILED" && currentBuild.changeSets != null && currentBuild.changeSets.size() > 0 }
            }
            steps {
                withAnt(installation: 'ANT-1.9.7', jdk: 'JDK1.8') {
                    sh 'ant test javadoc'
                }
            }
            post {
                always {
                }
                success {
                    step([$class: 'JavadocArchiver', javadocDir: 'doc/javadoc', keepAll: false])
                }
            }
       }
                expression { currentBuild.result != "FAILED" && currentBuild.changeSets != null && currentBuild.changeSets.size() > 0 }
            }
            steps {
                withAnt(installation: 'ANT-1.9.7', jdk: 'JDK1.8') {
                    sh 'ant distro'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts:'gateplugin-Ontology-*.zip'
                }
            }
       }
    }
            }
