pipeline{
    agent any
    
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'github-creds', url: 'https://github.com/muraliraws2022/javawebapplication'
            }
        }

stage("Maven Build"){
            steps{
                script{
                // Get Home Path of Maven 
                def mvnHome = tool name: 'maven-3', type: 'maven'
                sh "${mvnHome}/bin/mvn package"
                }
            }
        }

 stage("Deploy to Tomcat Server"){
            steps{
                sshagent(['tomcat-keypair']) {
                sh """
		    echo $WORKSPACE
		    mv target/*.war target/javawebapplication.war
                    scp -o StrictHostKeyChecking=no target/javawebapplication.war  ec2-user@172.31.90.125:/opt/tomcat8/webapps/
                    ssh ec2-user@172.31.90.125 /opt/tomcat8/bin/shutdown.sh
                    ssh ec2-user@172.31.90.125 /opt/tomcat8/bin/startup.sh
                
                """
                }
            
            }
        }
    } 
