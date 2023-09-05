
##  🌱Introduction
- Azle is a TypeScript Canister Development Kit (CDK) for the Internet Computer (IC). In other words, it's a TypeScript/JavaScript runtime for building applications (canisters) on the IC.
- Azle is currently developed by Demergent Labs, a for-profit company with a grant from DFINITY.

- Demergent Labs' vision is to accelerate the adoption of Web3, the Internet Computer, and sustainable open source.

- Azle intends to be a full TypeScript/JavaScript environment for the IC (a decentralized cloud platform), with support for all of the TypeScript/JavaScript language and as many relevant host APIs as possible. These host APIs will be similar to those available in the Node.js and web browser environments.

- One of the core benefits of Azle is that it allows web developers to bring their TypeScript/JavaScript skills to the IC. For example, Azle allows the use of various npm packages and VS Code intellisense.

As for the IC, we believe its main benefits can be broken down into the following categories:

* Ownership
* Security
* Developer Experience

- It's important to note that both Azle and the IC are early-stage projects. The IC officially launched in May of 2021, and Azle reached beta in April of 2022.

# Internet Computer Overview
The Internet Computer (IC) is a decentralized cloud platform. Actually, it is better thought of as a progressively decentralizing cloud platform. Its full vision is yet to be fulfilled.

It aims to be owned and operated by many independent entities in many geographies and legal jurisdictions throughout the world. This is in opposition to most traditional cloud platforms today, which are generally owned and operated by one overarching legal entity.

The IC is composed of computer hardware nodes running the IC protocol software. Each running IC protocol software process is known as a replica.

Nodes are assigned into groups known as subnets. Each subnet attempts to maximize its decentralization of nodes according to factors such as data center location and node operator independence.

The subnets vary in size. Generally speaking the larger the size of the subnet the more secure it will be. Subnets currently range in size from 13 to 40 nodes, with most subnets having 13 nodes.

IC applications, known as canisters, are deployed to specific subnets. They are then accessible through Internet Protocol requests such as HTTP. Each subnet replicates all canisters across all of its replicas. A consensus protocol is run by the replicas to ensure Byzantine Fault Tolerance.

View the IC Dashboard to explore all data centers, subnets, node operators, and many other aspects of the IC.

# Canisters Overview
Canisters are Internet Computer (IC) applications. They are the encapsulation of your code and state, and are essentially Wasm modules.

State can be stored on the 4 GiB heap or in a larger 64 GiB location called stable memory. You can store state on the heap using your language's native global variables. You can store state in stable memory using low-level APIs or special stable data structures that behave similarly to native language data structures.

State changes must go through a process called consensus. The consensus process ensures that state changes are Byzantine Fault Tolerant. This process takes a few seconds to complete.

Operations on canister state are exposed to users through canister methods. These methods can be invoked through HTTP requests. Query methods allow state to be read and are low-latency. Update methods allow state to be changed and are higher-latency. Update methods take a few seconds to complete because of the consensus process.




##  ⚠️ Installation Requirements
Follow the instructions exactly as stated below to avoid issues.

You should be using a *nix environment (Linux, Mac OS, WSL if using Windows) with bash and have the following installed on your system:

 * Node.js 18
 * dfx 0.14.2
 * Rust
We highly recommend using nvm to install Node.js (and npm, which is included with Node.js). Run the following commands to install Node.js and npm with nvm:

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


## Hello World

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

* Methodical start

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
 
- Breakdown
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

* tsconfig.json

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

* dfx.json
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

- Local deployment

```
dfx start --background
```
First startup the replica

```
dfx deploy
```

- Interacting with your canister from the command line
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

```
Deployed canisters.
URLs:
  Backend canister via Candid interface:
    azle_hello_world: http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai
```
Open up http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai or the equivalent URL from your terminal to access the web UI and interact with your canister.

- Deployment
There are two main Internet Computer (IC) environments that you will generally interact with: the local replica and mainnet.

When developing on your local machine, our recommended flow is to start up a local replica in your project's root directoy and then deploy to it for local testing.

Start
```
dfx start
```
 or 

 ```
dfx start --background
```

Stop
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


- Interacting with your canister


As a developer you can generally interact with your canister in three ways:

* dfx command line
* dfx web UI
* @dfinity/agent

- dfx command line

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

- dfx web UI

```
Deployed canisters.
URLs:
  Backend canister via Candid interface:
    my_canister: http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai

```
Open up http://127.0.0.1:8000/?canisterId=ryjl3-tyaaa-aaaaa-aaaba-cai&id=rrkah-fqaaa-aaaaa-aaaaq-cai to access the web UI.


- @dfinity/agent

@dfinity/agent is the TypeScript/JavaScript client library for interacting with canisters on the IC. If you are building a client web application, this is probably what you'll want to use.

There are other agents for other languages as well:

* Java
* Python
* Rust


- Deploying to mainnet

Assuming you are setup with cycles, then you are ready to deploy to mainnet.

To deploy all canisters defined in your dfx.json:


```
dfx deploy --network ic

```

To deploy an individual canister:

```
dfx deploy --network ic canister_name

```

:::tip
Congratulations you can now  check out the **[dacade x ICP course and get you some free ICP token reward ](https://dacade.org/communities/icp/courses/typescript-smart-contract-101/learning-modules/b14741ea-ee33-43a4-a742-9cdc0a6f0d1c)**, to build your own Azle Smart Contract.
:::

To learn Deeper about Azle click this link https://demergent-labs.github.io/azle/query_methods.html
