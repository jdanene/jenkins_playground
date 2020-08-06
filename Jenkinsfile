
class Globals {
  static int generalBuildTime = 30;
  static int testRolesBuildTimeout = 30;
  static int recoveryStageAndStashTime = 10;
  static int recoveryTimeout = 20;
  static int pilTimeout = 30;

  static int NormalTimeout() {
    return generalBuildTime + testRolesBuildTimeout;
  }

  static int ExtendedTimeout() {
    return generalBuildTime + testRolesBuildTimeout + recoveryStageAndStashTime + recoveryTimeout + pilTimeout;
  }
}


node {
    try {
        stage('Test') {
            println(Globals.NormalTimeout());
            println(currentBuild.result);

            sh 'echo "Fail!"; exit 0'
        }
        echo 'This will run only if successful'
    } catch (e) {
        echo 'This will run only if failed'

        // Since we're catching the exception in order to report on it,
        // we need to re-throw it, to ensure that the build is marked as failed
        throw e
    } finally {
        println(currentBuild.result );

        def currentResult = currentBuild.result ?: 'SUCCESS'
        if (currentResult == 'UNSTABLE') {
            echo 'This will run only if the run was marked as unstable'
        }

        def previousResult = currentBuild.previousBuild?.result
        if (previousResult != null && previousResult != currentResult) {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }

        echo 'This will always run'
    }
}