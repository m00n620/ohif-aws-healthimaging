# OHIF healthlake adapter

# Setting up

## Prerequisites
* Node.js +14
* OHIF follow the [Getting started guide if needed](https://v3-docs.ohif.org/development/getting-started/)
* Install health lake package `yarn add  ohif-healthlake`
* Create access key in the AWS portal
* Follow AWS documentation on how to create a medical imaging datasource
* Start the HealthLake proxy to secure your access keys
```bash
docker run -p 8089:8089 -e AWS_ACCESS_KEY_ID='YOUR_KEY' -e AWS_SECRET_ACCESS_KEY='YOUR_SECRET' -e AWS_REGIOS='YOUR_REGION' mateusfreira/ohif-healthlake-proxy
```
* Add healthlake adapter as a OHIF plugin `platform/viewer/pluginConfig.json`
```json
  "extensions": [
    //....
    {
      "packageName": "ohif-healthlake",
      "version": "0.0.8"
    }
  ],

```
* Configure the data source to access healthlake via the proxy
```js
window.config = {
  friendlyName: 'HealthLake Data',
  namespace: 'ohif-healthlake.dataSourcesModule.healthlake',
  sourceName: 'healthlake',
  configuration: {
    name: 'healthlake',
    healthlake: {
      datastoreID: $DATASTORE_NAME,
      endpoint: 'http://localhost:8089',// Add here the address to you proxy
    },
    imageRendering: 'healthlake',
    thumbnailRendering: 'wadors',
    enableStudyLazyLoad: true,
    supportsFuzzyMatching: false,
    supportsWildcard: true,
    staticWado: true,
    singlepart: 'bulkdata,video,pdf,image/jphc',
  }
};
```
# How to contribute

```bash
git clone git@github.com:RadicalImaging/OhiF-healthlake.git
cd OhiF-healthlake
yarn install
# start coding
yarn test # to run unit tests
```

## Description 
Support metadata and imaging data loading from healthlake

## FAQ
## Why do we need the proxy server?
* You should never expose your AWS keys in the client, we created this tiny proxy with the only propose to hide the AWS keys in the backend.
* The Proxy server available here is meant to be for development only, in real use cases we encoraje you to implement authentication on top of the proxy so you secure the access to your data.


## Authors 
Bill Wallace, Mateus Freira, Radical Imaging 

## License 
MIT