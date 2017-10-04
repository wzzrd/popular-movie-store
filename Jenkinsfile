node('maven') {
             // define commands
             def ocCmd = "oc --token=`cat /var/run/secrets/kubernetes.io/serviceaccount/token` --server=https://openshift.default.svc.cluster.local --certificate-authority=/run/secrets/kubernetes.io/serviceaccount/ca.crt"
             def mvnCmd = "mvn" // -s configuration/cicd-settings.xml

             stage 'Build'
             git branch: 'demotest1', url: 'https://github.com/wzzrd/popular-movie-store.git'
             def v = version()
             sh "${mvnCmd} clean install -DskipTests=true"

             stage 'Test and Analysis'
             parallel (
                 'Test': {
                     sh "${mvnCmd} test"
                     step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
                 },
                 'Static Analysis': {
                     // missing Sonar, skipping this
                 }
             )

             stage 'Push to Nexus'
             // missing Nexus, skipping this

             stage 'Deploy Locally'
  
             stage 'Deploy Azure'
             input message: "Promote to Azure?", ok: "Promote"
             // tag for stage
             sh "${ocCmd} tag dev/tasks:latest stage/tasks:${v}"
          }
