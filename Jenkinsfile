pipeline{
   agent any
   stages{
      stage('login server'){
         steps{
            sshagent(credentials:['Prod-Machine']){
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@65.0.72.243 ls -a'
            }
         echo "Suceess Login" 
         }
      }
      stage('Clean Old Build'){
         steps{
            sshagent(credentials:['Prod-Machine']){
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@65.0.72.243 sudo rm -rf /home/ubuntu/react-test1/build'
            }
         echo "Build Directory Clean Successfull"
         }
      }
      stage('Build'){
         steps{
            sshagent(credentials:['Prod-Machine']){
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@65.0.72.243 ./npm-build-script.sh'
            }
         echo "Build Artifcats Successfull"
         }
      }
      stage('Copy the Artifcats to Apache2 Document Root'){
         steps{
            sshagent(credentials:['Prod-Machine']){
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@65.0.72.243 sudo cp -r /home/ubuntu/react-test1/build /var/www'
            }
         echo "Artifcats stored in apache folder Successfull"
         }
      }
      stage('Restart the Apache2 Service'){
         steps{
            sshagent(credentials:['Prod-Machine']){
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@65.0.72.243 sudo a2ensite 001-reactjs-prod.conf'
               sh 'ssh -o StrictHostKeyChecking=no ubuntu@65.0.72.243 sudo systemctl restart apache2'
            }
         echo "Apache2 restarted successfully"
         }
      }
   }
}
