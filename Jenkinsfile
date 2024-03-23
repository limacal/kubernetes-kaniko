podTemplate(yaml: '''
apiVersion: v1 
kind: Pod metadata:
name: 
kaniko spec:
containers:
- name: kaniko
    image:
    gcr.io/kaniko-project/executor:latest
    args:
    - "--context=git://github.com/scriptcamp/kubernetes-kaniko"
    - "--destination=limacadmin/kaniko-demo-image:1.0" 
    volumeMounts:
    - name: kaniko-secret
    mountPath:
    /kaniko/.docker 
restartPolicy:
Never volumes: 
- name:
    kaniko-secret 
    secret:
        secretName: 
        dockercred items:
            - key: 
                .dockerconfigjson
                path: config.json
''') 
