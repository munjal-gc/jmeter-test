node {
   def antCommand = '/web/ant/bin/ant'
   stage('Preparation') { // for display purposes
      //clean up existing build
      sh "rm -rf *"
   
      // Get some code from a GitHub repository
      sh "mkdir CaseyCloud DeploymentArtifacts middleware-services"
      sh 'pwd'
      sh 'ls'
      dir('CaseyCloud') {
            // some block
            git branch: 'master', credentialsId: 'munjal-github-ssh-Global', url: 'git@github.com:TheCaseyGroup/CaseyCloud.git'
      }

      dir('DeploymentArtifacts') {
            // some block
            git branch: 'master', credentialsId: 'munjal-github-ssh-Global', url: 'git@github.com:TheCaseyGroup/DeploymentArtifacts.git'
      }

      dir('middleware-services') {
            // some block
            git branch: 'jenkins', credentialsId: 'munjal-github-ssh-Global', url: 'git@github.com:munjal-gc/middleware-services.git'
      }
   }
   stage('Build') {
       dir('middleware-services') {
            // some block
            sh "'${antCommand}' -version"
            sh "pwd"
            sh "ls"
            echo "building Projects..."

            sh "'${antCommand}' -buildfile Core/ant/build.xml -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
            sh "'${antCommand}' -buildfile CommonServices/ant/build.xml -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
            sh "'${antCommand}' -buildfile MeteredBilling/ant/build.xml -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
            sh "'${antCommand}' -buildfile SalesNavigatorGateway/ant/build.xml -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
            sh "'${antCommand}' -buildfile GeoCoder/ant/build.xml -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
            sh "'${antCommand}' -buildfile FacadeServices/ant/build.xml  jar-client -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
            sh "'${antCommand}' -buildfile ScheduleOptimizer/ant/build.xml -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
            sh "'${antCommand}' -buildfile FacadeServices/ant/build.xml -Djboss.home.dir=/web/jboss -Ddeployment.dir=/web/jboss/standalone/deployments"
       }    
    }
    stage('integrationTestRun') {
      bzt "Hello_API_Test_Plan.jmx"
    }
   stage('Results') {
      // junit '**/target/surefire-reports/TEST-*.xml'
      // archive 'target/*.jar'
   }
}