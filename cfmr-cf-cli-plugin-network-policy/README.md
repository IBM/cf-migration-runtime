# CF CLI Plugin Network Policy

## About

This plugin enables App to App direct communication in CFMR

## Prerequisites

Please make sure `network-policy` service is available in cfmr broker offerings

```bash
cf service-access -b cfmr-broker
```

![image](https://user-images.githubusercontent.com/84785003/130772976-1b0246a0-44fd-4f17-afdb-1fff84f1cab5.png)

## Installation Steps

1. Go to `https://github.com/IBM/cf-migration-runtime/releases` and get URL of the plugin version you want to install

2. Use below command to install the plugin

    ```bash
    cf install-plugin -f https://github.com/IBM/cf-migration-runtime/releases/download/v1.0.0/cfmr-cf-cli-plugin-network-policy-linux-amd64
    ```

3. Verify if plugin has been installed

    ```bash
    cf plugins
    ```

4. To see plugin help

    ```bash
    cf add-cfmr-network-policy --help
    ```

    > **Example:**
    cf add-cfmr-network-policy frontend --destination-app backend --port 7007,7008,9003,9004 --protocol tcp,tcp,udp,udp

5. Verify if service instance has been created

   ```bash  
   cf service <SOURCE-APP>-<DESTINATION-APP>
   ```
