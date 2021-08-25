# CF CLI Plugin Network Policy

## About
This plugin enables App to App direct communication in CFMR

## Prerequisites
Please make sure `network-policy` service is available in cfmr broker offerings
`cf service-access -b cfmr-broker`

## Installation Steps
Go to `https://github.com/IBM/cf-migration-runtime/releases` and get URL of the plugin version you want to install and then use below command to install the plugin.

- `cf install-plugin -f https://github.com/IBM/cf-migration-runtime/releases/download/v1.0.0/cfmr-cf-cli-plugin-network-policy-linux-amd64`

