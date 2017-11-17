Example pipeline + code for Jenkins in OpenShift
================================================

``` bash
oc new-project cicd-demo
oc new-app jenkins-persistent
oc new-project development
oc new-app templates/httpd-dev.json
oc new-project testing
oc new-app templates/httpd-test.json
oc policy add-role-to-user edit system:serviceaccount:cicd-demo:jenkins -n development
oc policy add-role-to-user edit system:serviceaccount:cicd-demo:jenkins -n testing
```
