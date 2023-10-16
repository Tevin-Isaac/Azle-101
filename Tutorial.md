
# Internet Computer Overview

![slide___1](https://github.com/Tevin-Isaac/Azle-101/assets/81568615/4f59fe17-45a0-4182-a75a-d870290d3f44)

The Internet Computer is a blockchain that runs canister smart contracts, which are code units bundling together WebAssembly bytecode and the memory pages the bytecode runs in. 

The Internet Computer is composed of individual subnet blockchains running in parallel and connected together by the use of chain key cryptography. 
This means that canisters running on a subnet can seamlessly call canisters hosted in any other subnet of the Internet Computer blockchain. Moreover, the governance system of the Internet Computer can dynamically increase the capacity of the Internet Computer by adding new subnets, allowing dapps to scale out.

Developers can thus build new dapps consisting of multiple canisters running in parallel on different subnets and possibly integrate them with existing canisters already running on the Internet Computer.

The Internet Computer (IC) is a decentralized cloud platform. Actually, it is better thought of as a progressively decentralizing cloud platform. Its full vision is yet to be fulfilled.

# Canisters Overview

Canisters can be developed in a variety of languages, such as Rust, JavaScript, Python, and TypeScript, though the primary language for canister development is Motoko, since Motoko has been developed specifically for canister development on the IC.

We'll dive further into Motoko and other languages in the next sections.

To create canister smart contracts it is common practice to use an SDK. The IC SDK is a common entry point. The IC SDK supports a few programming languages out of the box.

Because the Internet Computer blockchain supports dapps compiled to standard WebAssembly modules, one can use many different programming languages to create IC canister smart contracts. 
To build a canister with a particular programming language, one needs a canister development kit (CDK) for their particular language. A CDK is an adapter used by the IC SDK that provides a programming language with the features necessary to create and manage canisters. 

To make starting easier, the IC SDK already comes with CDK for multiple languages.

In theory, any language that can be compiled into a WebAssembly module, can produce modules tailored for the IC deployable as an ICP smart contract.


As a developer you can generally interact with your canister in three ways:

* dfx command line
* dfx web UI
* @dfinity/agent

# canister processs for a developer

![process](https://github.com/Tevin-Isaac/Azle-101/assets/81568615/626296bb-2aa1-4c75-a47c-e1248f3cad9e)

## IC SDK vs CDK vs dfx
There are a few components worth expanding on:

IC SDK: this is the SDK created by DFINITY for creating and managing canisters. This is just one SDK in the ICP community and a common entry point for developers.

Canister Development Kit (CDK): a CDK is an adapter used by the IC SDK that provides a programming language with the features necessary to create and manage canisters. The IC SDK comes with a few CDKs already installed for you so you can use them in the language of your choice. For example, the IC SDK comes with the Rust CDK already installed. CDKs that are not installed but available for developers include the:

Python CDK.
TypeScript CDK.
C++ CDK.
Since CDKs are components used by the SDK, some developers choose to use the CDK directly (without the IC SDK), but typically are used as part of the whole IC SDK.

dfx: dfx is the command-line interface for the IC SDK. This is why many commands for the IC SDK start with the command "dfx .." such as dfx new or dfx stop.


Deployed canisters.
```
URLs:
  Backend canister via Candid interface:
    azle_hello_world: http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai
```
```
Open up http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai or the equivalent URL from your terminal to access the web UI and interact with your canister.
```

##  🌱Introduction to Azle

- Azle is a TypeScript Canister Development Kit (CDK) for the Internet Computer (IC). In other words, it's a TypeScript/JavaScript runtime for building applications (canisters) on the IC.
- Azle is currently developed by Demergent Labs, a for-profit company with a grant from DFINITY.

- Demergent Labs' vision is to accelerate the adoption of Web3, the Internet Computer, and sustainable open source.

- Azle intends to be a full TypeScript/JavaScript environment for the IC (a decentralized cloud platform), with support for all of the TypeScript/JavaScript language and as many relevant host APIs as possible. These host APIs will be similar to those available in the Node.js and web browser environments.

- It's important to note that both Azle and the IC are early-stage projects. The IC officially launched in May of 2021, and Azle reached beta in April of 2022.

## 🗈Prerequisites​

To follow  you will need to have the following:
* A connection to the internet.
* A command line interface.
* Visual Studio (VS) Code

##  ⚠️ Installation Requirements

Follow the instructions exactly as stated below to avoid issues.

You should be using a *nix environment (Linux, Mac OS, WSL if using Windows) with bash and have the following installed on your system:

 * Node.js 18
 * dfx 0.14.2
 * Rust

- We highly recommend using nvm to install Node.js (and npm, which is included with Node.js). Run the following commands to install Node.js and npm with nvm:

```bash 
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash```

Now restart your terminal and run the following command:

nvm install 18
dfx
Run the following command to install dfx 0.14.2:

DFX_VERSION=0.14.2 sh -ci "$(curl -fsSL https://sdk.dfinity.org/install.sh)"
If after trying to run dfx commands you encounter an error such as dfx: command not found, you might need to add $HOME/bin to your path. Here's an example of doing this in your .bashrc:

```bash 

echo 'export PATH="$PATH:$HOME/bin"' >> "$HOME/.bashrc" 

```


## Hello World Canister Smart contract

* Method 1(Quick Start)

```bash 
npx azle new hello_world
cd hello_world

npm install
npm run dfx_install
npm run replica_start
npm run canister_deploy_local

npm run canister_call_set_message
npm run canister_call_get_message

```

* Method 2

The project directory and file structure
Assuming you're starting completely from scratch, run these commands to setup your project's directory and file structure:

```bash 
mkdir azle_hello_world
cd azle_hello_world

mkdir src

touch src/index.ts
touch tsconfig.json
touch dfx.json

```
Now install Azle, which will create your package.json and package-lock.json files:


```bash 
npm install azle

```

Open up azle_hello_world in your text editor (we recommend VS Code).

# index.ts

```
import { $query, $update } from 'azle';

// This is a global variable that is stored on the heap
let message: string = '';

// Query calls complete quickly because they do not go through consensus
$query;
export function getMessage(): string {
    return message;
}

// Update calls take a few seconds to complete
// This is because they persist state changes and go through consensus
$update;
export function setMessage(newMessage: string): void {
    message = newMessage; // This change will be persisted
}

```
 
- **`Breakdown`**

```
import { $query, $update } from 'azle'; 

```
The code starts off by importing the $query and $update annotations from azle. The azle module provides most of the Internet Computer (IC) APIs for your canister.

```
// This is a global variable that is stored on the heap
let message: string = '';
```
We have created a global variable to store the state of our application. This variable is in scope to all of the functions defined in this module. We have annotated it with a type and set it equal to an empty string.

```
// Query calls complete quickly because they do not go through consensus
$query;
export function getMessage(): string {
    return message;
}
```
We are exposing a canister query method here. When query methods are called they execute quickly because they do not have to go through consensus. This method simply returns our global message variable.

```
// Update calls take a few seconds to complete
// This is because they persist state changes and go through consensus
$update;
export function setMessage(newMessage: string): void {
    message = newMessage; // This change will be persisted
}
```
We are exposing an update method here. When update methods are called they take a few seconds to complete. This is because they persist changes and go through consensus. A majority of nodes in a subnet must agree on all state changes introduced in calls to update methods. This method accepts a string from the caller and will store it in our global message variable.

That's it! We've created a very simple getter/setter Hello World application. But no Hello World project is complete without actually yelling Hello world!


To do that, we'll need to setup the rest of our project.

- **`tsconfig.json`** 

```
{
    "compilerOptions": {
        "strict": true,
        "target": "ES2020",
        "experimentalDecorators": true,
        "strictPropertyInitialization": false,
        "moduleResolution": "node",
        "allowJs": true,
        "outDir": "HACK_BECAUSE_OF_ALLOW_JS"
    }
}
```

- **`dfx.json`**
```
{
    "canisters": {
        "azle_hello_world": {
            "type": "custom",
            "build": "npx azle azle_hello_world",
            "root": "src",
            "ts": "src/index.ts",
            "candid": "src/index.did",
            "wasm": ".azle/azle_hello_world/azle_hello_world.wasm.gz"
        }
    }
}

```


##  Deployment

There are two main Internet Computer (IC) environments that you will generally interact with: the local replica and mainnet.

When developing on your local machine, our recommended flow is to start up a local replica in your project's root directoy and then deploy to it for local testing.

- Start
```
dfx start
```
 or 

 ```
dfx start --background
```

- Stop
```
dfx stop
```


If your replica starts behaving strangely, we recommend starting the replica clean, which will clean the dfx state of your project:

```
dfx start --clean
```

- Deploying to the local replica
```
dfx deploy
```

To deploy an individual canister:

```
dfx deploy canister_name
```


## Candid file

A Candid file is an interface description language (IDL) file used in the Internet Computer blockchain project.
It provides a language-independent description of a canister’s interfaces, including the names, parameters, result formats, and data types for each function a canister defines. This allows for interoperability between frontends and services written in different languages, such as Motoko, Rust, or JavaScript.
Candid files are typically denoted with a *.did* extension and they play a crucial role in ensuring that data is properly converted from its definition in a programming language (like Rust) to run safely on the Internet Computer blockchain.
Candid has a variety of features that make it a particularly good choice for developing dapps on the Internet Computer. 
These features include:

* Candid's implementations map the Candid value directly to the values and types of the host language, meaning developers do not construct or deconstruct some abstract Candid value.
* Candid defines rules for how series and their associated interface can be upgraded in a simple manner.
* Candid is a higher-order language, meaning it can receive more than plain data such as references to methods and services.
* Candid has native support for specific IC features, such as query annotation.



A typical interface description in Candid might look like this:
```
service counter : {
  add : (nat) -> ();
  subtract : (nat) -> ();
  get : () -> (int) query;
  subscribe : (func (int) -> ()) -> ();
}
```
In this example, the described service counter consists of the following public methods:

* The add and subtract methods change the value of a counter.

* The get method reads the current value of a counter.

* The subscribe method can be used to invoke another function, for example, to invoke a notification callback method each time the counter value changes

:::tip
 To learn more about candid click this link :https://internetcomputer.org/docs/current/developer-docs/backend/candid/candid-concepts
:::





## Interacting with your hello world canister from the command line
```
dfx canister call azle_hello_world getMessage
```
We should see ("") representing an empty message.


Now let's yell Hello World!:

```
dfx canister call azle_hello_world setMessage '("Hello World!")'
```

Retrieve the message:
```
dfx canister call azle_hello_world getMessage
```
We should see ("Hello World!")

- Interacting with your canister from the web UI





- Interacting with your canister


As a developer you can generally interact with your canister in three ways:

* dfx command line
* dfx web UI
* @dfinity/agent

# dfx command line

```
# assume a canister named my_canister

# builds and deploys all canisters specified in dfx.json
dfx deploy

# builds all canisters specified in dfx.json
dfx build

# builds and deploys my_canister
dfx deploy my_canister

# builds my_canister
dfx build my_canister

# removes the Wasm binary and state of my_canister
dfx uninstall-code my_canister

# calls the methodName method on my_canister with a string argument
dfx canister call my_canister methodName '("This is a Candid string argument")'

```

# dfx web UI

```
Deployed canisters.
URLs:
  Backend canister via Candid interface:
    my_canister: http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai

```
Open up http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai to access the web UI.

![upload_81a8c18388a8534ed8002edccc979f13](https://github.com/Tevin-Isaac/Azle-101/assets/81568615/9bac0318-67ec-4b96-8473-6a28351a5ccf)

# @dfinity/agent

@dfinity/agent is the TypeScript/JavaScript client library for interacting with canisters on the IC. If you are building a client web application, this is probably what you'll want to use.
There are other agents for other languages as well:

* Java
* Python
* Rust


# Local deployment

```
dfx start --background
```
First startup the replica

```
dfx deploy
```
Then run the second command once dfx is running in the background.


## Deploying to mainnet

Assuming you are setup with cycles, then you are ready to deploy to mainnet.

To deploy all canisters defined in your dfx.json:


```
dfx deploy --network ic

```

To deploy an individual canister:

```
dfx deploy --network ic canister_name

```

# Common deployment issues
If you run into an error during deployment, try the following:

* Ensure that you have followed the instructions correctly in the installation chapter, especially noting the build dependencies
* Start the whole deployment process from scratch by running the following commands: dfx stop or simply terminate dfx in your terminal, dfx start --clean, npx azle clean, dfx deploy
* Look for more error output by adding the --verbose flag to the build command in your dfx.json file like so: "build": "npx azle build hello_world --verbose
* Look for errors in each of the files in ~/.config/azle/rust/[rust_version]/logs
Reach out to our Telegram or whatsapp group.


Congratulations🎉 you can now  check out the **[dacade x ICP course and get you some free ICP token reward ](https://dacade.org/communities/icp/courses/typescript-smart-contract-101/learning-modules/b14741ea-ee33-43a4-a742-9cdc0a6f0d1c)**, to build your own Azle Smart Contract.


To learn Deeper about Azle click refer to this link https://demergent-labs.github.io/azle/query_methods.html
