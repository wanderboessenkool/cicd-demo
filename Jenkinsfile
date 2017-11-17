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
          sh env
          def model = openshift.process(templateName, "-p", "SOURCE_REPOSITORY_URL=$GIT_URL")
          openshift.newApp(templateName)
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
