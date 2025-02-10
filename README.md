# Authentication and Order Processing
 - Using Ruby for authentication tokens
 - Using GoLang order service:
     - calls the authentication API to request a token
     - Parses the token and extract user details
     - Returns a JSON response confirming an order placement

## 🛠 File Explanation
  - ***ConfigMap/golang-order-configmap.yaml***
      - This is the configuration of the ConfigMap for storing data in a key value form.
      - We use it to store two **Environment Variables** that are used in the GoLang service.
      - Following commands are used for implementing these changes:
        - ```bash
          Kubectl apply -f ConfigMap/golang-order-configmap.yaml
          ```
      - If the ConfigMap is changed, then to implement the changes in the Deployment 
        - ```bash
          kubectl rollout restart deployment golang-order-deployment
          ```
  - ***deployments/***:
      - ***/golang-order-deployment***:
          - We set the replicas to 1
          - Specified the template for the pods:
            - Specified the image for the service:
              ```bash
              pamitedu/golang-order-service:latest
              ```
            - Specified the port number for the container:
              ```bash
              ports:
               - containerPort: 8080
              ```
            - Added the envFrom block for accessing the ConfigMap and using the data stored there:
              ```bash
              envFrom:
               - configMapRef:
                    name: golang-order-configmap
              ```
      - ***/ruby-auth-deployment***
          - We set the replicas to 1
          - Specified the template for the pods:
            - Specified the image for the service:
              ```bash
              pamitedu/ruby-authentication-service:latest
              ```
            - Specified the port number for the container:
              ```bash
              ports:
               - containerPort: 4567
              ```
  - ***services/***
      - ***/golang-order-service***:
          - Specifying the selector:
            ```bash
            selector:
              app: golang-order
            ```
          - Specifying the port configuration:
            ```bash
             ports:
              - protocol: TCP
                port: 8080
                targetPort: 8080
             type: NodePort
            ```
      - ***/ruby-auth-service***:
          - Specifying the selector:
            ```bash
            selector:
              app: ruby-auth
            ```
          - Specifying the port configuration:
            ```bash
             ports:
              - protocol: TCP
                port: 4567
                targetPort: 4567
             type: NodePort
            ```
         
        
