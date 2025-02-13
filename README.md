# Hats Modules Registry

## Overview

A Hats Module is any contract that serves as an [Eligibility](https://docs.hatsprotocol.xyz/for-developers/hats-protocol-overview/eligibility-modules), [Toggle](https://docs.hatsprotocol.xyz/for-developers/hats-protocol-overview/toggle-modules) and/or a [Hatter](https://docs.hatsprotocol.xyz/for-developers/hats-protocol-overview/hat-admins-and-hatter-contracts#hatter-contracts) module.
Modules customize, automate and extend the behavior of Hats Protocol, and can also serve as adapters or integration points with other protocols and applications.

Hats Protocol is open and permission-less, and so anyone is free to use any compatible modules. But, only modules that are included in the registry are supported in the [Modules SDK](https://github.com/Hats-Protocol/modules-sdk) and get a native support in the [Hats App](https://app.hatsprotocol.xyz/).

The registry is designed to support any existing module that inherits the [HatsModule contract](https://github.com/Hats-Protocol/hats-module/blob/main/src/HatsModule.sol).
For full documentation on how to build new Hats Modules, click [here](https://docs.hatsprotocol.xyz/for-developers/building-hats-modules).

## How To Add A New Module

### Step 1 - Add your module's JSON file to the modules directory.

For each module, the registry contains a JSON file with the following structure:

```json
{
  "name": "<module's name (string)>",
  "details": ["<first paragraph describing the module (string)>", "<second paragraph describing the module (string)>"],
  "links": [
  {
    "label": "<link's name/description>",
    "link": "<link's URL>"
  }
  ],
  "parameters": [
  {
    "label": "<parameter's name/description>",
    "functionName": "<parameter's function name to retrieve the parameter from its instance>",
    "displayType": "<parameter's type of content, for display purpose>"
  },
  ]
  "type": {
    "eligibility": "<whether the module is an eligiblity module (boolean)>",
    "toggle": "<whether the module is a toggle module (boolean)>",
    "hatter": "<whether the module is a hatter module (boolean)>"
  },
  "implementationAddress": "<module's deployed implemenataion address (string)>",
  "deployments": [
    {
      "chainId": "<chain ID (string)>",
      "block": "<block number of the deployment transaction (string)>"
    }
  ],
  "creationArgs": {
    "useHatId": "<True if the hatId property should be set with the ID of the hat for which the module is deployed, otherwise false>"
    "immutable": [
      {
        "name": "<argument's name (string)>",
        "description": "<short description (string)>",
        "type": "<argument's solidity type, e.g. 'uint256' (string)>",
        "example": "<example argument which will be used for automatic testing>",
        "displayType": "<parameter's type of content, for display purpose>"
      }
    ],
    "mutable": [
      {
        "name": "<argument's name (string)>",
        "description": "<short description (string)>",
        "type": "<argument's solidity type, e.g. 'uint256' (string)>",
        "example": "<example argument which will be used for automatic testing>",
        "displayType": "<parameter's type of content, for display purpose>"
      }
    ]
  },
  "abi": "<module's contract ABI>"
}
```

(Note that arrays in the object above contain one example entry).

Here are some notes on its expected structure:

- The "name" property represents the name of the module.
- The "details" property is structured as an array of strings. Each array entry represents a paragraph (which will be rendered in a UI).
- The "links" property should have at least one link to the module's source code.
- The "parameters" property is used in order to dynamically fetch and display data from module instances.
  - The "label" property is used to display the name/description of each parameter.
  - The "functionName" property represents the name of the function from which the parameter should be retrieved. The function should be a view/pure function with no input and one output (not a tuple, but can be an array).
  - The "displayType" property is used in order to display a proper UI component for the parameter. For example, displaying a date for a parameter representing a timestamp. The supported types are currently:
    - "default" - Infers automatically the UI component for the value
    - "timestamp" - Unix timestamp, will be presented as a Date component
    - "hat" - hat component
    - "token" - Token component
    - "seconds" - Amount of time denominated in seconds
    - "amountWithDecimals" - For token amounts, takes into account the token's decimals
- There should be at least one field on the "type" property which is set to "true". A module might serve as more than one type, in which case there will be more than one field set to "true".
- For each chain provided in "deployments", there should be a deployed implementation contract with an address matching the provided "implementationAddress" property and with an ABI
  matching the "abi" property.
- For both the immutable and mutable arguments, their order should match the order of the arguments as provided to the module.
- The "example" fields will be used to automatically test the module's deployment.

### Step 2 - Bundle

The modules in the registry are bundled into one file, which is then consumed by its users.

Run:

```bash
yarn bundle
```

### Step 3 - Test

Run:

```bash
yarn test
```

### Step 4 - Format

Run:

```bash
yarn prettier
```

י
