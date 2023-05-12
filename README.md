# Publishing an Operator to OperatorHub.io
## Prerequisites
This assumes an operator has been created with a tool like operator-sdk and tested locally

This also assumes that the operator follows the directory structure of the operator-sdk

Operatorhub.io has Prerequisites in place for the operator to be published
1. The operator must be in a public container registry
2. The operator must be in a public git repository
3. The operator must have a valid semantic version
4. The operator must have a valid operator package manifest
5. The operator must have a valid bundle image


## Steps
1. Create a bundle image
    The bundle directory can be created with the following command

    - in your project directory, use the kustomize command to generate the manifests 

        ```bash
        kustomize build config/manifests | operator-sdk generate bundle --version 0.0.1
        ```

    - Validate the bundle with the following command

        ```bash
        operator-sdk bundle validate ./bundle
        ```

    - NOTE: The generated Makefile has bundling options as well:

        ```bash
        # This generates the kustomize manifests and the bundle directory and validates the bundle.
        make bundle
        ```
    


2. Build and push your bundle image

    ```bash
    docker build -f bundle.Dockerfile -t <image_name>:<tag> .
    ```
 
    ```bash
    docker push quay.io/<namespace>/<operator_name>:<tag>
    ```

3. Create an Operator Package Manifest
    ```bash
    kustomize build config/manifests | operator-sdk generate packagemanifests --version 0.0.1
    ```

4. Submit Operator to OperatorHub.io

The directory should follow the following structure:
```
operator
   ├── 1.0.0
   │   ├── operator.v1.0.0.clusterserviceversion.yaml
   │   ├── operator-crd1.crd.yaml
   │   ├── operator-crd2.crd.yaml
   │   ├── operator-crd3.crd.yaml
   └── operator.package.yaml
```

- Fork the [community-operators](https://github.com/k8s-operatorhub/community-operators) repo 
- Copy the bundle directory to the community-operators repo
- Create a pull request to the community-operators repo


### Additional Resources
- [OperatorHub.io](https://operatorhub.io/)
- [Operator SDK](https://sdk.operatorframework.io/docs/building-operators/golang/tutorial/)
- [community-operators-docks](https://k8s-operatorhub.github.io/community-operators/contributing-prerequisites/)
