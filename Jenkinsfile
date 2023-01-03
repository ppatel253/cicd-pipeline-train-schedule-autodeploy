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
                
                withKubeConfig(caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUMvakNDQWVhZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRJeU1USXhOREU1TkRBME4xb1hEVE15TVRJeE1URTVOREEwTjFvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTFNICnB4NmNVWWo2bmZncTNleGE2ZUhxb1F3a0p4Qi9Db3FRRnM2SmNHbnd5N1pIOGZzeG1OOFhPdEJjcFRxZGdleXIKZHNDVDNyakFyMmVWQm1PSlhNcGZ4NlFiZ0pxYmttWnZ6U0k2TysvWlVPMnRpemprRUdvbDJIVnNxRDMxalM4WQo5Z1ZlZHRscnFuSUJEczRWbFhMOU9paXNmTFhLVGNNcCtwRHFsWDhHYjQ3V3NtMTBUZVpWRm03ZVJ1LzEyak11Ckl6WnloQytKcUYxa3ppZHVTV21pQ1d0QWFiUXB6THY3eXVQL0F6R2wrMHVpbWFxbC8xczIvMkFlLzlQMlNmUzcKOXVpR1J4dXN6TGZTaExrb0JvZlk5ZmZsQ2xhOFdMZGRlSTdCVmFHUnViNnd6dEpyb0R2bVpkU2daMzFNRVlERQpYZ3NqTTRQbHdRMjNmMnZJc2xFQ0F3RUFBYU5aTUZjd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0hRWURWUjBPQkJZRUZPRWVkdHlHVTkrYzZJWVRzakRmYS9HeTNvZGxNQlVHQTFVZEVRUU8KTUF5Q0NtdDFZbVZ5Ym1WMFpYTXdEUVlKS29aSWh2Y05BUUVMQlFBRGdnRUJBRFlKMUExSjdKNStHWmxZV2VzWApjYjhqOEdISU45TmUyYWhubG1NZ3BLZTJwdUdCeVhudTBEUE9OdXlyQUpVMngwMEI5eVRQcnZRd2pnMnJXWWUvCnZndG1DUW1QMFk1NDRjQ0s4eWlmQ3BySHBzUkxBRHJJQ0N5UGYza1Q0dVMxbmdqRTVyWHo1WFNEbURrbXEzdGcKb2hneUZubm9OeG8wM2J5WE9kelB4c0lHU3hTdDVndDJGd2RobXQ0N1dycE9sUGNwUUp0Q3dpT0N4N3FlTHBVRQp3NGlnZWZOcXJGNWQwZnJveGMwbHBGdXpobTU4YlBDMWoxOUZkdTBHY2g4L0ZBU0c3SDQyNUMrVHRicy9CSlU2CjlqQXcvNzBXOWExNmQxaGhVd3ZUUnA1NzljM3dxUVRROEZ1dGYxNGdKVms4TVRqT3g1aVBuMWg4d2xhT3MyTlEKQW93PQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==', clusterName: 'kubernetes', contextName: 'kubernetes-admin@kubernetes', credentialsId: 'kubernetes', namespace: 'default', serverUrl: 'https://172.31.81.175:6443') {
                    sh 'kubectl apply -f train-schedule-kube.yml'
                }
            }
        }
    }
}
