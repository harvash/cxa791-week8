podTemplate(
  yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: centos
        image: centos
        command:
        - sleep
        args:
        - 99d
      restartPolicy: Never
    '''
) {
  node(POD_LABEL) {
    stage('k8s') {
      git 'https://github.com/harvash/cxa791-week8.git'
      container('centos') {
        stage('start calc') {        
          sh '''
            ls -l
            cd Chapter09/sample3
            curl -k  -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/default/deployments -XPOST -H "Content-type: application/yaml" --data-binary @hazelcast.yaml
            curl -k  -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/apis/apps/v1/namespaces/default/deployments -XPOST -H "Content-type: application/yaml" --data-binary @calculator.yaml
            curl -ik -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETES_SERVICE_PORT/api/v1/namespaces/default/pods
            curl http://10.102.107.249:8080/sum?a=3'&'b=3
            curl http://10.102.107.249:8080/div?a=3'&'b=3
           '''
        }
      }
    }
  }
}