podTemplate(
  yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
      - name: calculator
        image: dlambrig/week8:1.1
        env:
        - name: CALCIP
        value: 10.1.3.213
        command:
        - sleep
        args: -99d
      restartPolicy: Never
    '''
) {
  node(POD_LABEL) {
    stage('k8s') {
      git ''
      container('calculator') {
        stage('start calculator') {
          sh '''
            cd Chapter09/sample3
            curl -k  -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/default/deployments -XPOST -H "Content-type: application/yaml" --data-binary @hazelcast.yaml
            curl -k  -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://$KUBERNETES_SERVICE_HOST:$KUBERNETE_SERVICE_PORT/apis/apps/v1/namespaces/default/deployments -XPOST -H "Content-type: application/yaml" --data-binary @calculator.yam
            '''
        }
      }
    }
  }
}