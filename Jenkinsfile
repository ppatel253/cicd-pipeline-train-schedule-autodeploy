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
                withKubeConfig(caCertificate: 'LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFb3dJQkFBS0NBUUVBdU5wWDVCS0UrU0Rha3loeUxEWTY3elhzUUpEVXN0VTBCMG53dm1mTjdmUHZBMkUrCjd0d3VKUThUZnRsVklVNzE5L3BLT2tvTUJBRThIaUxxRmNDOUU3M1J4UUJaT1ovYXA3Wk52Vy84Qk96S3RiQWYKZ2krZkJYVVk5MW56cU9lelNnbWE2MDVQSFptaE5ZK3E5L1N2ajFyTlZ2SVpPN2JON2FuSXFoUGcxWVVCeVJsNwplTlZvV2FTVXlMV2p3SHN4YTV4TDNFN1NKSnVJMUNYYURTdGNzWk9zT3V4Yk9ZZzlXQmRKTHFhTWJxUkcvdTIzCnZXTzhzSWVsTHJHd3ZsekRGZU9aUW81cGtqZ3VaUGZVcjQyZ0NuMU9yc21Qcmtkd2VKUmhPVEdBeE1zWVBaM24KWWxYNzArOEg0T2pseCtiWi9saElQWlF4TThLYlZZSUdHdHdueFFJREFRQUJBb0lCQVFDd29ZSG1Ib1FOQVFXYQpJOFdrMVZUUENqbHJJSGo5UUpmc2R3eWhBOU5VRWdoS3RIdE1CZnZaMFdRUmkxMjY3QlpBRTFzbUEyY2oxSUk3ClVhVlhqOG1idVg5ZHZJMkZjR2VnWmFRMjVYQnN6RTZOK1hMQ0ZQcmdYaG16RUxLd3JHVTIwNkxDUVJ0Nzd6YUoKVlhSS1pmWHpCeEs2aDY3d3ppWkxlRGFUdlZiUzdobmR4NmVvMGlvaDlsYXQwL0ZwVEdKK3Z3czMrT0J5elgwZQo1NGhFd0RSL1kyYXVIblhLUUczZjUxbkJGQ1lCdmtneHhaZmU3QW94M2pFZFY1R1dKTjlwVk1Rb0czQklUcmFqClcxbVp6NnhmMlllUVdVOVZJZzE1M2g3aEg1RVdJVWN3MEJaSlhGWmMxUnd2T2FodEFpd1QrYzEwWE5GK090WDcKc2JxWGFpbUJBb0dCQU1rMFJETGY2L214cVJBQ0wzaEYvZnNLZStZb2Q2cW1qMTFoZEx6cDhBUlNTSStqTXRHbwpRZ2t5N2Y2V1hXcEFTQmo5elRVUkZPaldVR1BFSkFodEcxdHdGcHRCVmJyRlJ6RVdiOHZMYXlDaUtXMDRiQXM5CjRMY3RhZkltVW5CRDNBcHU3VktYdUFvVDdtbFJJU3N1dEFrM3Z5ZW5WZWxQUUdHNmh2Ym81aG9SQW9HQkFPc3kKRmJxN25xN1lnM2VjZzFnUzNlY3lzRU5Wbm83WTFhVGVhSlJSa0VHbkZtN2xDR0VaOGF2TTFyVGp5clAxMkpwYQpZcnppWnFYaEFCaVAvUmZXK3g0RmhtMUpDSUlMd3hHb0daYS90cVdUUXF3cjR0eVhZUU95Sng2Wm5GU3RQUCs0ClBjYmxnV0wxZDd0aGZQUEhhRE5jb3h6UktybHQwdTRLVG8xS0lGNTFBb0dBRWkvclNqMzdjcUtnaVpYby9KSTMKRnc0bmpQSnpmclYzbUJWTEhCaDVYYXFpYkhsd0NvdVBESTNSL1lWU0JGeXpKNjhwY1hhTjBVNHVEaGFZdXpwQgprczVhL01XM0hoN2ZCSXptcGlGSkdiWU8wSlNkRDVjVVRQcUo3UjJScmh3ME02MDdQcEFBVHNqbWNCYXBUT0gzCjNDMXgxZi9HUUVTdHFTZlhNMUp5R2tFQ2dZQVF0MlNYK3hIU040MVFpUTFFeTBKK1ZqbjA3emJ2ekRXVEhFT3UKbHdWN3hSWnhGMUk3Skk3cXVRcGhuUGNoUjAzMzJvaStDQkZucE5CTzdwejhmc2ttWHhvbzFsSUdmRE9ISTcxOQoyMjV0NGtPUTNTV29yQkczSTRob1FsNjhIZndCNk9ScktKRERxZEt5dk1FV3lvdDdheEFrMGpFSk1PY1hDQ0NCCkprMmFxUUtCZ0VHeURUQ0o5Vkd1Ulp6TGtQekdLM2VoSVRLY0p0RHdocTdGdmpHTXQ2UG1lSWlNY1lCUWlSRVkKajNLdXJoOTFVVjZrWllIdHU1T0Z1Z0FqK3BDVmVKdzc3clRXTThaY2JFbS9zNzRsS2xlcWhWampjTElaOGtpTgpRdkxMV2hDVXJmVys3RklORXZmT0IvQUNkaGlEdFJPMklFQkZIKzNaNzE3YXF3NUpjdXZwCi0tLS0tRU5EIFJTQSBQUklWQVRFIEtFWS0tLS0tCg==', clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'kubernetesHub', namespace: 'default', serverUrl: 'https://172.31.81.175:6443') {
    sh 'kubectl apply -f train-schedule-kube.yml'
}
//                 withKubeConfig(caCertificate: '''-----BEGIN CERTIFICATE-----
//                     MIIFVjCCAz6gAwIBAgIUQ+NxE9izWRRdt86M/TX9b7wFjUUwDQYJKoZIhvcNAQEL
//                     BQAwQzELMAkGA1UEBhMCQ04xHDAaBgNVBAoTE2lUcnVzQ2hpbmEgQ28uLEx0ZC4x
//                     FjAUBgNVBAMTDXZUcnVzIFJvb3QgQ0EwHhcNMTgwNzMxMDcyNDA1WhcNNDMwNzMx
//                     MDcyNDA1WjBDMQswCQYDVQQGEwJDTjEcMBoGA1UEChMTaVRydXNDaGluYSBDby4s
//                     THRkLjEWMBQGA1UEAxMNdlRydXMgUm9vdCBDQTCCAiIwDQYJKoZIhvcNAQEBBQAD
//                     ggIPADCCAgoCggIBAL1VfGHTuB0EYgWgrmy3cLRB6ksDXhA/kFocizuwZotsSKYc
//                     IrrVQJLuM7IjWcmOvFjai57QGfIvWcaMY1q6n6MLsLOaXLoRuBLpDLvPbmyAhykU
//                     AyyNJJrIZIO1aqwTLDPxn9wsYTwaP3BVm60AUn/PBLn+NvqcwBauYv6WTEN+VRS+
//                     GrPSbcKvdmaVayqwlHeFXgQPYh1jdfdr58tbmnDsPmcF8P4HCIDPKNsFxhQnL4Z9
//                     8Cfe/+Z+M0jnCx5Y0ScrUw5XSmXX+6KAYPxMvDVTAWqXcoKv8R1w6Jz1717CbMdH
//                     flqUhSZNO7rrTOiwCcJlwp2dCZtOtZcFrPUGoPc2BX70kLJrxLT5ZOrpGgrIDajt
//                     J8nU57O5q4IikCc9Kuh8kO+8T/3iCiSn3mUkpF3qwHYw03dQ+A0Em5Q2AXPKBlim
//                     0zvc+gRGE1WKyURHuFE5Gi7oNOJ5y1lKCn+8pu8fA2dqWSslYpPZUxlmPCdiKYZN
//                     pGvu/9ROutW04o5IWgAZCfEF2c6Rsffr6TlP9m8EQ5pV9T4FFL2/s1m02I4zhKOQ
//                     UqqzApVg+QxMaPnu1RcN+HFXtSXkKe5lXa/R7jwXC1pDxaWG6iSe4gUH3DRCEpHW
//                     OXSuTEGC2/KmSNGzm/MzqvOmwMVO9fSddmPmAsYiS8GVP1BkLFTltvA8Kc9XAgMB
//                     AAGjQjBAMB0GA1UdDgQWBBRUYnBj8XWEQ1iO0RYgscasGrz2iTAPBgNVHRMBAf8E
//                     BTADAQH/MA4GA1UdDwEB/wQEAwIBBjANBgkqhkiG9w0BAQsFAAOCAgEAKbqSSaet
//                     8PFww+SX8J+pJdVrnjT+5hpk9jprUrIQeBqfTNqK2uwcN1LgQkv7bHbKJAs5EhWd
//                     nxEt/Hlk3ODg9d3gV8mlsnZwUKT+twpw1aA08XXXTUm6EdGz2OyC/+sOxL9kLX1j
//                     bhd47F18iMjrjld22VkE+rxSH0Ws8HqA7Oxvdq6R2xCOBNyS36D25q5J08FsEhvM
//                     Kar5CKXiNxTKsbhm7xqC5PD48acWabfbqWE8n/Uxy+QARsIvdLGx14HuqCaVvIiv
//                     TDUHKgLKeBRtRytAVunLKmChZwOgzoy8sHJnxDHO2zTlJQNgJXtxmOTAGytfdELS
//                     S8VZCAeHvsXDf+eW2eHcKJfWjwXj9ZtOyh1QRwVTsMo554WgicEFOwE30z9J4nfr
//                     I8iIZjs9OXYhRvHsXyO466JmdXTBQPfYaJqT4i2pLr0cox7IdMakLXogqzu4sEb9
//                     b91fUlV1YvCXoHzXOP0l382gmxDPi7g4Xl7FtKYCNqEeXxzP4padKar9mK5S4fNB
//                     UvupLnKWnyfjqnN9+BojZns7q2WwMgFLFT49ok8MKzWixtlnEjUwzXYuFrOZnk1P
//                     Ti07NEPhmg4NpGaXutIcSkwsKouLgU9xGqndXHt7CMUADTdA43x7VF8vhV929ven
//                     sBxXVsFy6K2ir40zSbofitzmdHxghm+Hl3s=
//                     -----END CERTIFICATE-----''', clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'kubernetesHub', namespace: 'default', serverUrl: 'https://172.31.81.175:6443') {
//                         sh 'kubectl apply -f train-schedule-kube.yml'
//                     }
//             }
//         }
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
