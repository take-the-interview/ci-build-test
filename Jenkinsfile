#!groovy

project_name="ci-build-test"

import java.util.regex.*

/* Only keep the 10 most recent builds. */
def projectProperties = [
    [$class: 'BuildDiscarderProperty',strategy: [$class: 'LogRotator', numToKeepStr: '10']],
]

library 'global-libs@v1.2.1'

try {
    def gitCommit
    node("Solaris") {
        /* Only keep the 10 most recent builds. */
        properties([
              [
                $class: 'jenkins.model.BuildDiscarderProperty',
                strategy: [
                  $class: 'LogRotator',
                  numToKeepStr: '10'
                  ]
              ]
        ])

        stage('Clean workspace') {
            deleteDir()
            sh 'ls -laht'
        }

        stage('Checkout source') {
            checkout scm
            // checkout(
            //     [
            //             $class: 'GitSCM',
            //             branches: [[name: '*/$BRANCH_NAME']],
            //             userRemoteConfigs: [[credentialsId: 'ttiops', url: "https://github.com/take-the-interview/${project_name}.git"]],
            //             extensions: [
            //                     [$class: 'CloneOption', depth: 2, noTags: true, shallow: true]
            //             ]
            //     ]
            // )
            gitCommit = sh(returnStdout: true, script: 'git rev-parse HEAD').trim()
        }

        stage("Deploy") {
            sh """
            env
            """
        }
    }
}

catch (exc) {
    echo "Caught: ${exc}"

    currentBuild.result = 'FAILURE'
}

