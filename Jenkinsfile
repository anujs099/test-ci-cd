pipeline{
    agent any
    tools{
        node 'node24.3.0'
    }  
    parameters{
        gitParameter(name:"branch_name",type:"PT_BRANCH",defaultValue:"develop",description:"select the branch to build")
    } 
    
    environment{
        BUILD_TIMESTAMP = sh(script: "date +%Y%m%d_%H%M%S", returnStdout: true).trim()
        CI = 'true'
    }

    stages{
        stage("checkout"){
            steps{
                git branch:"${params.branch_name}",
                credentialsId:"70c3ba1e-fed0-4e14-9682-50049bb7faaa",
                url:"git@github.com:anujs099/test-ci-cd.git"
            }
        }


        stage('install dependencies'){
            steps{
                script{
                    echo "installing dependencies"
                    sh "npm ci --include=dev"   
                }              
            }
        }

        stage('create build'){
            steps{
                script{
                    sh "npm run build"
                }
            }
        }

        stage("deploy"){
            steps:{
                script{
                    echo "deployment pending"
                }
            }
        }

    }

    post {
        always {
            echo "done !"
            cleanWs()
        }
        failure{
            echo "build failed"
        }
        success{
            script {
                echo "no errors, Build Successful, creating artifect of ${params.branch_name}"
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }
}