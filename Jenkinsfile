@Library('jenkins-shared-library') _

def configMap = [
    project: "roboshop",
    component: "catalogue"
]

if (! env.BRANCH_NAME.equalsIgnoreCase('main') ) {
    nodeJSEKSPipeline(configMap)
} 
else {
    echo "Please follow the CR process"
}