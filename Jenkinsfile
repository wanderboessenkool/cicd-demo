def templateName = 'openshift/httpd-example'
openshift.withCluster() {
  openshift.withProject() {
    echo "Using project: ${openshift.project()}"
    pipeline {
      agent {
        node {
          label 'nodejs'
        }
      }
      stages {
        stage('create') {
          def scmUrl = scm.getUserRemoteConfigs()[0].getUrl()
          def model = openshift.process(templateName, "-p", "SOURCE_REPOSITORY_URL=$scmUrl", "-", "CONTEXT_DIR=html")
          openshift.newApp(model)
        }
        stage ('build') {
          steps {
            def builds = openshift.selector("bc", templateName).related('builds')
            timeout(5) { 
              builds.untilEach(1) {
                return (it.object().status.phase == "Complete")
              }
            }
          }
        }
        stage('tag') {
          steps {
            openshift.tag("${templateName}:latest", "${templateName}-staging:latest")
          }
        }
      }
    }
  }
}

// vim: ts=2 sts=2 sw=2 et si
