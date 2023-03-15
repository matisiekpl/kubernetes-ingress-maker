<template>
  <div class="hello">
    <input placeholder="Name" v-model="name">
    <input placeholder="Port" v-model="port" type="number">
    <input placeholder="Host" v-model="host">
    <br/>
    <br/>
    <textarea readonly>{{ code }}</textarea>
  </div>
</template>

<script>
var template = `apiVersion: v1
kind: Endpoints
metadata:
  name: {name}
subsets:
  - addresses:
      - ip: 95.111.255.92
    ports:
      - port: {port}
---
apiVersion: v1
kind: Service
metadata:
  name: {name}
spec:
  ports:
    - protocol: TCP
      port: {port}
      targetPort: {port}
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: {name}
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
    - host: {host}
      http:
        paths:
          - path: /
            backend:
              serviceName: {name}
              servicePort: {port}`;


export default {
  data(){
    return {
      name: '',
      port: '',
      host: '',
    }
  },
  computed:{
    code(){
      return template.replace(/{name}/g, this.name).replace(/{port}/g, this.port).replace(/{host}/g, this.host);
    }
  }
}
</script>

<style scoped>
textarea {
  width: 100%;
  height: 800px;
}
</style>