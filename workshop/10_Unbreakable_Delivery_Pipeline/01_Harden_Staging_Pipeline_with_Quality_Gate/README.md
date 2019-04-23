# Harden Staging Pipeline with Quality Gate

In this lab you'll add an additional quality gate to your CI pipeline. In other words, an end-to-end check will verify the functionality of the sockshop application in the staging environment.

## Step 1: Add e2e Test to Staging Pipeline
1. Uncomment the following snippet in the Jenkins pipeline of `k8s-deploy-staging`.
    ```
    stage('Run production ready e2e check in staging') {
      steps {
        echo "Waiting for the service to start..."
        sleep 150
        recordDynatraceSession (
          envId: 'Dynatrace Tenant',
          testCase: 'loadtest',
          tagMatchRules: tagMatchRules
        ) 
        {
          container('jmeter') {
            script {
              def status = executeJMeter ( 
                scriptName: "jmeter/front-end_e2e_load.jmx",
                resultsDir: "e2eCheck_${env.APP_NAME}",
                serverUrl: "front-end.staging", 
                serverPort: 8080,
                checkPath: '/health',
                vuCount: 10,
                loopCount: 5,
                LTN: "e2eCheck_${BUILD_NUMBER}",
                funcValidation: false,
                avgRtValidation: 4000
              )
              if (status != 0) {
                currentBuild.result = 'FAILED'
                error "Production ready e2e check in staging failed."
              }
            }
          }
        }
        perfSigDynatraceReports(
          envId: 'Dynatrace Tenant', 
          nonFunctionalFailure: 1, 
          specFile: "monspec/e2e_perfsig.json"
        )
      }
    }
    ```

## Step 2: Set the Upper and Lower Limit in the Performance Signature
1. Open the file `monspec\e2e_perfsig.json`.
1. Set the upper and lower limit for the response time as follows:
    ```
    "upperLimit" : 800,
    "lowerLimit" : 600
    ```
1. Commit/Push the changes to your GitHub repository *k8s-deploy-staging*.

:arrow_forward: [Next Step: Simulate Early Pipeline Break](../02_Simulate_Early_Pipeline_Break)
