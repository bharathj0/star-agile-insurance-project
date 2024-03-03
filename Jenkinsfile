node{
    stage('Git Code Cheeckout'){
        git 'https://github.com/bharathj0/star-agile-insurance-project.git'
    }
    stage('Code Building'){
        sh 'mvn clean package'
    }
    stage('Publishing HTML Reports'){
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Project/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    }
    stage('Building Image'){
        sh 'docker build -t bharathj0/insurance:1.0 .'
    }
    stage('Docker login and push'){
        withCredentials([string(credentialsId: 'docpass', variable: 'docpass')]) {
            
        sh "docker login -u bharathj0 -p ${docpass}"
        sh "docker push bharathj0/insurance:1.0"
    }
    }
    stage('deployment using ansible'){
        ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
    }
}
