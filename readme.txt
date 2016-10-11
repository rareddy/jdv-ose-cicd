vagrant up
vagrant provision

oc login

vpnc

# creating base image
git clone https://github.com/rareddy/jdv-ose-cicd.git

# creating the base image for dv (once after final this step will not be required)
oc -n openshift create -f jdv-ose-cicd/jdv63-image-stream.json
oc -n openshift import-image jboss-datavirt63-openshift --insecure=true

#delete image stream (if needs to recreate)
oc -n openshift delete is jboss-datavirt63-openshift

#tasks to be done by the DBA
git clone https://github.com/rareddy/jdv-ose-dba.git

# mount the file system for the JDV, or create any mount to any NFS
oc secrets new jdv-app-data jdv-ose-dba/mount-directory/
oc volume dc/jdv-app --add --name=data --mount-path=/jdv-mount --type=secret --secret-name=jdv-app-data

# create secrets file for creating data sources.
oc secrets new jdv-app-config datasources.properties
	
# create injected image (NOTE: jdv-ose-cicd/jdv63-extension-s2i.json needs to modified with 
# respective GIT locations, for jdv-ose-dba(extension), jdv-ose-vdb (source))
oc -n openshift create -f jdv-ose-cicd/jdv63-extension-s2i.json

# import injected image
oc -n openshift import-image jdv-app --insecure=true

#delete previous images if you are re-doing it.
oc -n sample-project delete build,route,pod,service,deploymentconfig,buildconfig,replicationcontroller,template,imagestream,pv,persistentvolumeclaim --all

# new image process
oc process -f jdv-ose-cicd/jdv63-extension-s2i.json | oc create -f -

# watch the image building
oc  build-logs jdv-app-1

# pod list, make sure all are running and ready
oc get pods -o wide

# to see the log output of the EAP instance
oc logs {pod-name}

# if you need to log into the pod
oc -it exec {pod-name} /bin/sh

#useful commands
oc get builds
oc projects
ov get services
sudo journalctl -u atomic-openshift-master -f &

