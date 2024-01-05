# Ethereum Beacon APIs

![CI](https://github.com/ethereum/beacon-APIs/workflows/CI/badge.svg)

Collection of RESTful APIs provided by Ethereum Beacon nodes

API browser: [https://ethereum.github.io/beacon-APIs/](https://ethereum.github.io/beacon-APIs/)

## Outline

This document outlines an application programming interface (API) which is exposed by a beacon node implementation of the Ethereum [consensus layer specifications](https://github.com/ethereum/consensus-specs).

The API is a REST interface, accessed via HTTP. The API should not, unless protected by additional security layers, be exposed to the public Internet as the API includes multiple endpoints which could open your node to denial-of-service (DoS) attacks through endpoints triggering heavy processing.
 Currently, the only supported return data type is JSON.

The beacon node (BN) maintains the state of the beacon chain by communicating with other beacon nodes in the Ethereum network.
Conceptually, it does not maintain keypairs that participate with the beacon chain.

The validator client (VC) is a conceptually separate entity which utilizes private keys
to perform validator related tasks, called "duties", on the beacon chain.
 These duties include the production of beacon blocks and signing of attestations.

The goal of this specification is to promote interoperability between various beacon node implementations.

## Render
To render spec in browser you will need any http server to load `index.html` file
in root of the repo.

##### Python
```
python -m http.server 8080
```
And api spec will render on [http://localhost:8080](http://localhost:8080).

##### NodeJs
```
npm install simplehttpserver -g

# OR

yarn global add simplehttpserver

simplehttpserver
```
And api spec will render on [http://localhost:8000](http://localhost:8000).

### Usage

Local changes will be observable if "dev" is selected in the "Select a definition" drop-down in the web UI.

Users may need to tick the "Disable Cache" box in their browser's developer tools to see changes after modifying the source.

## Contributing
Api spec is checked for lint errors before merge.

To run lint locally, install linter with
```
npm install -g @redocly/cli

# OR

yarn global add @redocly/cli
```
and run lint with
```
redocly lint beacon-node-oapi.yaml
```

### Generating spec types

Spec types are generated automatically from the [ethereum/consensus-specs](https://github.com/ethereum/consensus-specs) repo.

```
pip install pyspec2openapi
pyspec2openapi pyspec2openapi.yml types/spec.yaml
```

## Releasing

1. Create and push tag

   - Make sure `info.version` in `beacon-node-oapi.yaml` file is updated before tagging.  This will need to be a PR, and will get the release process started.
   - CD will create github release and upload bundled spec file

2. Create a second PR, containing the updated `index.html`. Also change back the `info.version` in `beacon-node-api.yaml` back to `Dev`

   - The `index.html` file needs a new release entrypoint added to refer to the new release. Find the `urls` field, 
     and add the new release as the first entry in the list.
     Entry should be in following format(replace `<tag>` with real tag name from step 1.):
```
         {url: "./releases/<tag>/beacon-node-oapi.json", name: "<tag>"},
```
