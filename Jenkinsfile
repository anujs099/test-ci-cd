pipeline{
    agent any
    tools{
        nodejs 'node24.3.0'
    }  
    options { skipDefaultCheckout() }
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
                script{
                    def cleanBranch = params.branch_name.replace('origin/', '')
                
                    git branch:"${cleanBranch}",
                    credentialsId:"70c3ba1e-fed0-4e14-9682-50049bb7faaa",
                    url:"git@github.com:anujs099/test-ci-cd.git"
                }
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
            steps{
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
       success {

            echo "No errors, Build Successful. Archiving artifacts for branch: ${params.branch_name}"
            

            sh """
                mkdir -p build_backup
                cp -r dist/* build_backup/ || true
            """
        
            archiveArtifacts artifacts: 'dist/**', fingerprint: true, allowEmptyArchive: false
        }
    }
}