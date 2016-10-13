vagrant up
vagrant provision

oc login

vpnc

oc create -n openshift -f https://raw.githubusercontent.com/rcernich/application-templates/CLOUD-999/datavirt/jdv63-extensions-support-s2i.json
oc create -n openshift -f https://raw.githubusercontent.com/rareddy/jdv-ose-cicd/master/jdv63-image-stream.json

https://10.1.2.2:8443/console/
https://ce-os-rhel-master.usersys.redhat.com:8443/console

oc login ce-os-rhel-master.usersys.redhat.com:8443
oc project mydemo

oc create -f https://raw.githubusercontent.com/rcernich/application-templates/master/secrets/eap-app-secret.json

oc delete secrets jdv-app-config
oc secrets new jdv-app-config datasources.properties


oc exec -n jdv-demo jdv-app-2-36lvw -- cat /opt/eap/standalone/configuration/standalone-openshift.xml

oadm policy add-role-to-user view system:serviceaccount:jdv-kitchen-sink:default


curl -H "Host: jdv-apr.default.svc.cluster.local" -u "teiidUser:xP,H]\$s'4]" http://10.34.75.115/odata/sqlserver/$metadata


oc delete -n jdv-demo all -l application=jdv-app
