# Generated from . Do not edit directly.

library identifier: 'RHTAP_Jenkins@main', retriever: modernSCM(
  [$class: 'GitSCMSource',
   remote: 'https://github.com/redhat-appstudio/tssc-sample-jenkins.git'])

pipeline {
  agent {
        kubernetes {
            label 'jenkins-agent'
            cloud 'openshift'
            serviceAccount 'jenkins'
            podRetention onFailure()
            idleMinutes '30'
            containerTemplate {
                name 'jnlp'
                image 'image-registry.openshift-image-registry.svc:5000/jenkins/jenkins-agent-base:latest'
                ttyEnabled true
                args '${computer.jnlpmac} ${computer.name}'
            }
        }
    }    
  environment {
        ROX_CENTRAL_ENDPOINT = "${ env.ROX_CENTRAL_ENDPOINT }"
        /* GITOPS_AUTH_USERNAME = "${ env.GITOPS_AUTH_USERNAME }" */
        /* Set this to the user for your specific registry */
        /* IMAGE_REGISTRY_USER = "${ env.IMAGE_REGISTRY_USER }" */
        /* Set this only when using an external Rekor instance */
        /* REKOR_HOST = "${ env.REKOR_HOST }" */
        /* Set this only when using an external TUF instance */
        /* TUF_MIRROR = "${ env.TUF_MIRROR }" */
        /* QUAY_IO_CREDS_USR = "${ env.QUAY_IO_CREDS_USR }" */
        /* ARTIFACTORY_IO_CREDS_USR = "${ env.ARTIFACTORY_IO_CREDS_USR }" */
        /* NEXUS_IO_CREDS_USR = "${ env.NEXUS_IO_CREDS_USR }" */
        COSIGN_PUBLIC_KEY = "${ env.COSIGN_PUBLIC_KEY }"
        ROX_API_TOKEN = credentials('ROX_API_TOKEN')
        GITOPS_AUTH_PASSWORD = credentials('GITOPS_AUTH_PASSWORD')
        /* Set this password for your specific registry */
        /* IMAGE_REGISTRY_PASSWORD = credentials('IMAGE_REGISTRY_PASSWORD') */
        /* QUAY_IO_CREDS_PSW = credentials('QUAY_IO_CREDS_PSW') */
        /* ARTIFACTORY_IO_CREDS_PSW = credentials('ARTIFACTORY_IO_CREDS_PSW') */
        /* NEXUS_IO_CREDS_PSW = credentials('NEXUS_IO_CREDS_PSW') */
        COSIGN_SECRET_PASSWORD = credentials('COSIGN_SECRET_PASSWORD')
        COSIGN_SECRET_KEY = credentials('COSIGN_SECRET_KEY')}
    stages {
        stage('init') {
            steps {
                script {
                    rhtap.info('init')
                    rhtap.init()
                }
            }
        }

        stage('build') {
            steps {
                script {
                    rhtap.info('buildah_rhtap')
                    rhtap.buildah_rhtap()
                    rhtap.info('cosign_sign_attest')
                    rhtap.cosign_sign_attest()
                }
            }
        }

        stage('scan') {
            steps {
                script {
                    rhtap.info('acs_deploy_check')
                    rhtap.acs_deploy_check()
                    rhtap.info('acs_image_check')
                    rhtap.acs_image_check()
                    rhtap.info('acs_image_scan')
                    rhtap.acs_image_scan()
                }
            }
        }

        stage('deploy') {
            steps {
                script {
                    rhtap.info('update_deployment')
                    rhtap.update_deployment()
                }
            }
        }

        stage('summary') {
            steps {
                script {
                    rhtap.info('show_sbom_rhdh')
                    rhtap.show_sbom_rhdh()
                    rhtap.info('summary')
                    rhtap.summary()
                }
            }
        }

    }
}
