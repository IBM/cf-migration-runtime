### Service Background
Specify a source app, destination app, protocol, and port so that app instances 
can communicate directly without going through the Gorouter, a load balancer, or a firewall. 
Container-to-container networking supports UDP and TCP, and you can configure policies for multiple ports. 
These policies apply immediately without having to restart the app.
Two K8s resources are created for each invocation of this c2c plan.
 1.  A K8s service resource to expose the destination app port for communication.
 2.  A network policy allowing traffic between only the source and destination application
     using a specific pod selector that uses the application GUID value for specificity.

### Service Provisioning Details
- **SOURCEAPP** :  The Cloud Foundry App Name representing the initiator of communication<br/> 
- **DESTAPP** : The Cloud Foundry App Name representing the target of the communication<br/>
- **PORTS** : An array of 1...N JSON stanzas containing a name, port, targetport and protocol.<br/>
  - **name**: Any valid string to be used for the named port<br/>
  - **port**: Any valid integer (>1024 - not privileged ports). Exposes the Kubernetes service on the specified port <br/>
          within the cluster. Other pods within the cluster can communicate with this server <br/>
          on the specified port.<br/>
  - **targetport**: Any valid integer (>1024 - not privileged ports). This will be the same as the port value.  This <br/>
                represents the port on which the service will send requests to, that your <br/>
                pod will be listening on. Your application in the container will need to be <br/>
                listening on this port also. <br/>
  - **protocol**: TCP or UDP (TCP by default)<br/>
  
#### JSON Payload Formatted Sample
```
     {
       "source-guid": "'$(cf app "${SOURCEAPP}" --guid)'",
       "destination-appname": "'"${DESTAPP}"'",
       "destination-guid": "'$(cf app "${DESTAPP}" --guid)'",
       "ports": [
         {
           "name": "cat1",
           "port": 7007,
           "targetport": 7007,
           "protocol": "tcp"
         },
         {
           "name": "cat2",
           "port": 7008,
           "targetport": 7008,
           "protocol": "TCP"
         },
         {
           "name": "cat2a",
           "port": 7009,
           "targetport": 7009,
           "protocol": "TCP"
         },
         {
           "name": "cat3",
           "port": 9001,
           "targetport": 9001,
           "protocol": "udp"
         },
         {
           "name": "cat4",
           "port": 9002,
           "targetport": 9002,
           "protocol": "UDP"
         },
         {
           "name": "cat4a",
           "port": 9003,
           "targetport": 9003,
           "protocol": "UDP"
         }
       ]
     }
```
### Example Single-Line Command
```
SOURCEAPP=frontend;DESTAPP=backend;cf create-service network-policy c2c front2back -c '{"source-guid":"'$(cf app "${SOURCEAPP}" --guid)'","destination-appname":"'"${DESTAPP}"'","destination-guid":"'$(cf app "${DESTAPP}" --guid)'","ports":[{"name":"cat1","port":7007,"targetport":7007,"protocol":"tcp"},{"name":"cat2","port":7008,"targetport":7008,"protocol":"TCP"},{"name":"cat2a","port":7009,"targetport":7009,"protocol":"TCP"},{"name":"cat3","port":9001,"targetport":9001,"protocol":"udp"},{"name":"cat4","port":9002,"targetport":9002,"protocol":"UDP"},{"name":"cat4a","port":9003,"targetport":9003,"protocol":"UDP"}]}'
```

### General Syntax
```
SOURCEAPP=<name_of_CF_App#1>;DESTAPP=<name_of_CF_App#2>;cf create-service network-policy c2c front2back -c <JSON_Payload>
```

>NOTE: It is NOT required to use environment variables.  They are used above to provide maximum flexibility/reuse, however users can also consider using hard-coded strings discovered through individual commands to identify the `guid` values for the source and destination CF applications.
