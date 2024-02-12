pipeline {
    environment { // Environment variables defined for all steps
        DOCKER_IMAGE = "registry.demo.local:5000/juice-shop"

    }

    agent any

    stages {
        stage("Build image") {
            steps {
                script {
                    // Use commit tag if it has been tagged
                    tag = sh(returnStdout: true, script: "git tag --contains").trim()
                    if ("$tag" == "") {
                        if ("${BRANCH_NAME}" == "master") {
                            tag = "latest"
                        } else {
                            tag = "${BRANCH_NAME}"
                        }
                    }
                    image = docker.build("$DOCKER_IMAGE:$tag")
                }
            }
        }

        stage("Push to registry") {
            steps {
                script {
                    sh label: "Push to registry", script: "docker push ${DOCKER_IMAGE}:$tag"
                }
            }
        }
}
}
