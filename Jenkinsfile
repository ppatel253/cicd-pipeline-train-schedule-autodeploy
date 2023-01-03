pipeline {
    agent any
    environment {
        //be sure to replace "bhavukm" with your own Docker Hub username
        DOCKER_IMAGE_NAME = "ppatel253/train-schedule"
        def dockerhubaccountid = "ppatel253"
        
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry([ credentialsId: "dockerHub", url: "" ]) {
                        app.push()
                        app.push("latest")
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                withKubeConfig(caCertificate: '''-----BEGIN CERTIFICATE-----
                    MIIFVjCCAz6gAwIBAgIUQ+NxE9izWRRdt86M/TX9b7wFjUUwDQYJKoZIhvcNAQEL
                    BQAwQzELMAkGA1UEBhMCQ04xHDAaBgNVBAoTE2lUcnVzQ2hpbmEgQ28uLEx0ZC4x
                    FjAUBgNVBAMTDXZUcnVzIFJvb3QgQ0EwHhcNMTgwNzMxMDcyNDA1WhcNNDMwNzMx
                    MDcyNDA1WjBDMQswCQYDVQQGEwJDTjEcMBoGA1UEChMTaVRydXNDaGluYSBDby4s
                    THRkLjEWMBQGA1UEAxMNdlRydXMgUm9vdCBDQTCCAiIwDQYJKoZIhvcNAQEBBQAD
                    ggIPADCCAgoCggIBAL1VfGHTuB0EYgWgrmy3cLRB6ksDXhA/kFocizuwZotsSKYc
                    IrrVQJLuM7IjWcmOvFjai57QGfIvWcaMY1q6n6MLsLOaXLoRuBLpDLvPbmyAhykU
                    AyyNJJrIZIO1aqwTLDPxn9wsYTwaP3BVm60AUn/PBLn+NvqcwBauYv6WTEN+VRS+
                    GrPSbcKvdmaVayqwlHeFXgQPYh1jdfdr58tbmnDsPmcF8P4HCIDPKNsFxhQnL4Z9
                    8Cfe/+Z+M0jnCx5Y0ScrUw5XSmXX+6KAYPxMvDVTAWqXcoKv8R1w6Jz1717CbMdH
                    flqUhSZNO7rrTOiwCcJlwp2dCZtOtZcFrPUGoPc2BX70kLJrxLT5ZOrpGgrIDajt
                    J8nU57O5q4IikCc9Kuh8kO+8T/3iCiSn3mUkpF3qwHYw03dQ+A0Em5Q2AXPKBlim
                    0zvc+gRGE1WKyURHuFE5Gi7oNOJ5y1lKCn+8pu8fA2dqWSslYpPZUxlmPCdiKYZN
                    pGvu/9ROutW04o5IWgAZCfEF2c6Rsffr6TlP9m8EQ5pV9T4FFL2/s1m02I4zhKOQ
                    UqqzApVg+QxMaPnu1RcN+HFXtSXkKe5lXa/R7jwXC1pDxaWG6iSe4gUH3DRCEpHW
                    OXSuTEGC2/KmSNGzm/MzqvOmwMVO9fSddmPmAsYiS8GVP1BkLFTltvA8Kc9XAgMB
                    AAGjQjBAMB0GA1UdDgQWBBRUYnBj8XWEQ1iO0RYgscasGrz2iTAPBgNVHRMBAf8E
                    BTADAQH/MA4GA1UdDwEB/wQEAwIBBjANBgkqhkiG9w0BAQsFAAOCAgEAKbqSSaet
                    8PFww+SX8J+pJdVrnjT+5hpk9jprUrIQeBqfTNqK2uwcN1LgQkv7bHbKJAs5EhWd
                    nxEt/Hlk3ODg9d3gV8mlsnZwUKT+twpw1aA08XXXTUm6EdGz2OyC/+sOxL9kLX1j
                    bhd47F18iMjrjld22VkE+rxSH0Ws8HqA7Oxvdq6R2xCOBNyS36D25q5J08FsEhvM
                    Kar5CKXiNxTKsbhm7xqC5PD48acWabfbqWE8n/Uxy+QARsIvdLGx14HuqCaVvIiv
                    TDUHKgLKeBRtRytAVunLKmChZwOgzoy8sHJnxDHO2zTlJQNgJXtxmOTAGytfdELS
                    S8VZCAeHvsXDf+eW2eHcKJfWjwXj9ZtOyh1QRwVTsMo554WgicEFOwE30z9J4nfr
                    I8iIZjs9OXYhRvHsXyO466JmdXTBQPfYaJqT4i2pLr0cox7IdMakLXogqzu4sEb9
                    b91fUlV1YvCXoHzXOP0l382gmxDPi7g4Xl7FtKYCNqEeXxzP4padKar9mK5S4fNB
                    UvupLnKWnyfjqnN9+BojZns7q2WwMgFLFT49ok8MKzWixtlnEjUwzXYuFrOZnk1P
                    Ti07NEPhmg4NpGaXutIcSkwsKouLgU9xGqndXHt7CMUADTdA43x7VF8vhV929ven
                    sBxXVsFy6K2ir40zSbofitzmdHxghm+Hl3s=
                    -----END CERTIFICATE-----''', clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'kubernetesHub', namespace: 'default', serverUrl: 'https://172.31.81.175:6443') {
                        sh 'kubectl apply -f train-schedule-kube.yml'
                    }
            }
        }
//         stage('CanaryDeploy') {
//             when {
//                 branch 'master'
//             }
//             environment { 
//                 CANARY_REPLICAS = 1
//             }
//             steps {
//                 kubernetesDeploy(
//                     kubeconfigId: 'kubeconfig',
//                     configs: 'train-schedule-kube-canary.yml',
//                     enableConfigSubstitution: true
//                 )
//             }
//         }
//         stage('DeployToProduction') {
//             when {
//                 branch 'master'
//             }
//             environment { 
//                 CANARY_REPLICAS = 0
//             }
//             steps {
//                 input 'Deploy to Production?'
//                 milestone(1)
//                 kubernetesDeploy(
//                     kubeconfigId: 'kubeconfig',
//                     configs: 'train-schedule-kube-canary.yml',
//                     enableConfigSubstitution: true
//                 )
//                 kubernetesDeploy(
//                     kubeconfigId: 'kubeconfig',
//                     configs: 'train-schedule-kube.yml',
//                     enableConfigSubstitution: true
//                 )
//             }
//         }
    }
}
