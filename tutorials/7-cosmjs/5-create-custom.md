---
title: "Create Custom CosmJS Interfaces"
order: 6
description: Work with your blockchain
tags:
  - tutorial
  - cosm-js
  - dev-ops
---

# Create Custom CosmJS Interfaces

<HighlightBox type="learning">

In this section, you will:

* Create custom CosmJS interfaces to connect to custom Cosmos SDK modules.
* Define custom interfaces with Protobuf.
* Define custom types and messages.
* Integrate with Ignite - previously known as Starport.

</HighlightBox>

CosmJS comes out of the box with interfaces that connect with the standard Cosmos SDK modules such as `bank` and `gov` and understand the way their messages are serialized. Since your own blockchain's modules are unique, they need custom CosmJS interfaces. That process consists of several steps:

1. Creating the Protobuf objects and clients in TypeScript.
2. Creating extensions that facilitate the use of the above clients.
3. Any further level of abstraction that you deem useful for integration.

This section assumes that you have a working Cosmos blockchain with its own modules. It is based on CosmJS version [`v0.28.3`](https://github.com/cosmos/cosmjs/tree/v0.28.3).

## Compiling the Protobuf objects and clients

You can choose which library you use to compile your Protobuf objects into TypeScript or JavaScript. Reproducing what [cosmjs-types](https://github.com/confio/cosmjs-types/blob/main/scripts/codegen.js) or [Stargate](https://github.com/cosmos/cosmjs/blob/main/packages/stargate/CUSTOM_PROTOBUF_CODECS.md) do is a good choice.

### Preparation

This exercise assumes that:

1. The library you're creating is in 'myLib'.
2. Your Protobuf definition files are in `./proto/`.
3. You want to transpile them into TypeScript in `./src/codegen/`.

Install `telescope`, in your package or in Docker:

<CodeGroup>

<CodeGroupItem title="Local">

```sh
$ npm install --save-dev --save-exact @cosmology/telescope@1.0.5
```

</CodeGroupItem>

<CodeGroupItem title="Docker">

```sh
$ docker run --rm -it \
    -v $(pwd):/myLib \
    -w /myLib \
    ts-telescope \
    npm install --save-dev --save-exact @cosmology/telescope@1.0.5
```

</CodeGroupItem>

<CodeGroupItem title="Docker Image">

```Dockerfile
FROM --platform=linux node:lts-slim as base

ENV TELESCOPE_VERSION=1.0.5

ENV PATH /usr/src/app/node_modules/.bin:$PATH

RUN npm install --global @cosmology/telescope@${TELESCOPE_VERSION}

ENTRYPOINT ["telescope"]
```

Then build the image:

```sh
$ docker build . -t ts-telescope
```

You can replace `ts-telescope` with your preferred name.

</CodeGroupItem>

</CodeGroup>

---

You can confirm the version you received by running:

<CodeGroup>

<CodeGroupItem title="Local">

```sh
$ npx telescope --version
```

</CodeGroupItem>

<CodeGroupItem title="Docker">

```sh
$ docker run --rm -it \
    ts-telescope --version
```

</CodeGroupItem>

</CodeGroup>

This returns something like:

```txt
1.0.5
```

The compiler tools are ready. Time to use them.

### Getting third party files

You need to get the imports that appear in your `.proto` files. Usually you can find the following in [`query.proto`](https://github.com/cosmos/cosmos-sdk/blob/d98503b/proto/cosmos/bank/v1beta1/query.proto#L4-L6):

```proto
import "cosmos/base/query/v1beta1/pagination.proto";
import "gogoproto/gogo.proto";
import "google/api/annotations.proto";
```

You need local copies of the right file versions in the right locations. Pay particular attention to Cosmos SDK's version of your project. You can check by running:

```sh
$ grep cosmos-sdk go.mod
```

This returns something like:

```txt
github.com/cosmos/cosmos-sdk v0.45.4
```

Use this version as a tag on Github. One way to retrieve the [pagination file](https://github.com/cosmos/cosmos-sdk/blob/v0.45.4/proto/cosmos/base/query/v1beta1/pagination.proto) is:

```sh
$ mkdir -p ./proto/cosmos/base/query/v1beta1/
$ curl https://raw.githubusercontent.com/cosmos/cosmos-sdk/v0.45.4/proto/cosmos/base/query/v1beta1/pagination.proto -o ./proto/cosmos/base/query/v1beta1/pagination.proto
```

You can do the same for the others, found in the [`third_party` folder](https://github.com/cosmos/cosmos-sdk/tree/v0.45.4/third_party/proto) under the same version:

```sh
$ mkdir -p ./proto/google/api
$ curl https://raw.githubusercontent.com/cosmos/cosmos-sdk/v0.45.4/third_party/proto/google/api/annotations.proto -o ./proto/google/api/annotations.proto
$ curl https://raw.githubusercontent.com/cosmos/cosmos-sdk/v0.45.4/third_party/proto/google/api/http.proto -o ./proto/google/api/http.proto
$ mkdir -p ./proto/gogoproto
$ curl https://raw.githubusercontent.com/cosmos/cosmos-sdk/v0.45.4/third_party/proto/gogoproto/gogo.proto -o ./proto/gogoproto/gogo.proto
```

Or you can add the latest whole `third_party` folders by running `npx telescope install` under `myLib` folder:

<CodeGroup>
<CodeGroupItem title="Interactive">

```sh
$ npx telescope install
```

then select packages you want to include in `myLib/proto` folder:

```txt
Telescope 1.0.3
? [pkg] which packages do you want to support? (Press <space> to select, <a> to toggle all, <i> to invert selection)
❯◯ amino
 ◯ akash
 ◯ bcna
 ...
 ◯ cosmos
 ...
```

Say `cosmos` is selected and entered, there will be a Cosmos folder and its dependencies (like gogoproto, Google, etc.) in `myLib/proto` folder.

</CodeGroupItem>
<CodeGroupItem title="NonInteractive">

```sh
$ npx telescope install @protobufs/cosmos@0.0.10  @protobufs/confio
```

You can add wanted proto packages after `telescope install`, then you do not have to select them one by one.

Then there will be the packages and its dependencies (like gogoproto, Google, etc.) in the `myLib/proto` folder.

</CodeGroupItem>
<CodeGroupItem title="Docker non-interactive">

```sh
$ docker run --rm -it \
    -v $(pwd):/myLib \
    -w /myLib \
    ts-telescope \
    npx telescope install @protobufs/cosmos@0.0.10  @protobufs/confio
```

This will create `/myLib/proto` with Cosmos and Confio proto file folders in the Docker instance.

</CodeGroupItem>
</CodeGroup>

### Compilation

You can now compile the Protobuf files using Telescope.

<CodeGroup>
<CodeGroupItem title="Local">

```sh
$ npx telescope transpile
```

</CodeGroupItem>
<CodeGroupItem title="Docker">

```sh
$ docker run --rm -it \
    -v $(pwd):/myLib \
    -w /myLib \
    ts-telescope \
    npx telescope transpile
```

</CodeGroupItem>
</CodeGroup>

You should now see some `.ts` files generated in `./src/codegen`. These are the real source files used in your application.

By default, the command takes `./proto` as input folder and `./src/codegen` as output folder. You can config in and out folders and the way Telescope generates code by using options like these examples:

<CodeGroup>
<CodeGroupItem title="Local">

```sh
$ npx telescope transpile --protoDirs ./proto --outPath gen/src
```

</CodeGroupItem>
<CodeGroupItem title="Docker">

```sh
$ docker run --rm -it \
    -v $(pwd):/myLib \
    -w /myLib \
    ts-telescope \
    npx telescope transpile --protoDirs ./proto --outPath gen/src
```

</CodeGroupItem>
</CodeGroup>

When running the command, Telescope takes a proto folder as input, and generates files in a 'gen/src' folder.

Each time `telescope transpile` has run, a config file, `.telescope.json`, will be created in the folder where it is running:

```json
//.telescope.json
{
  "protoDirs": [
    "./proto"
  ],
  "outPath": "gen/src",
  "options": {
    // telescope options
    "addTypeUrlToDecoders": true,
    "addTypeUrlToObjects": true,

    "typingsFormat": {
        "num64": "bigint",
        "customTypes": {
            "useCosmosSDKDec": true
        }
    },
    ...
  }
}
```

* `protoDirs`: root directory that contains folders with Protobuf files. For example, if `protoDirs` is set with `proto` and `proto-common`, when importing from `cosmos/base/query/v1beta1/pagination.proto`, Telescope will check whether there is `proto/cosmos/base/query/v1beta1/pagination.proto` or `proto-common/cosmos/base/query/v1beta1/pagination.proto`, and then generate a dependency based on the file.
* `outPath`: contains TypeScript files structured by the folder structure of Protobuf files.

You can modify the config file according to [Telescope options](https://github.com/cosmology-tech/telescope#options), and then use the file by the option `--config`:

<CodeGroup>
<CodeGroupItem title="Local">

```sh
$ npx telescope transpile --protoDirs ../../proto-common --config .telescope.json
```

</CodeGroupItem>
<CodeGroupItem title="Docker">

```sh
$ docker run --rm -it \
    -v $(pwd):/myLib \
    -w /myLib \
    ts-telescope \
    npx telescope transpile --protoDirs ../../proto-common --config .telescope.json
```

</CodeGroupItem>
</CodeGroup>

<HighlightBox type="note">

* `protoDirs` are taken both from `--protoDirs` arguments and the `protoDirs` field in the configuration file. It means proto files inside `proto-common` and `proto` will be taken in this case.
* `outPath` is taken only from the configuration file, `gen/src` in this case. It means that even if the `--outPath` option is provided to the command, the value will be ignored.

</HighlightBox>

After it has run successfully, there will be a message:

```txt
✨ files transpiled in '/gen/src'
✨ transpilation successful!
```

<HighlightBox type="note">

`outPath` is created automatically during the transpilation. This means that you will get no "missing dir" error, for instance if your `outPath` is incorrect.

</HighlightBox>

---

You should now see your files transpiled into TypeScript. They have been correctly filed under their respective folders and contain both types and services definitions. It also created the transpiled versions of your third party imports.

### A note about the result

Your `tx.proto` file may have contained the following:

```protobuf [https://github.com/cosmos/cosmos-sdk/blob/v0.45.4/proto/cosmos/bank/v1beta1/tx.proto#L11-L17]
service Msg {
      rpc Send(MsgSend) returns (MsgSendResponse);
      //...
}
```

If so, you find its service declaration in the compiled `tx.ts` file (with Telescope option `rpcClients.inline: true`) or in `tx.rpc.msg.ts` file (with Telescope option `rpcClients.inline: false`):

```typescript [https://github.com/confio/cosmjs-types/blob/v0.8.0/src/cosmos/bank/v1beta1/tx.ts#L473]
export interface Msg {
    Send(request: MsgSend): Promise<MsgSendResponse>;
    //...
}
```

It also appears in the default implementation:

```typescript [https://github.com/confio/cosmjs-types/blob/v0.8.0/src/cosmos/bank/v1beta1/tx.ts#L495]
export class MsgClientImpl implements Msg {
    private readonly rpc: Rpc;
    constructor(rpc: Rpc) {
        this.rpc = rpc;
        this.Send = this.Send.bind(this);
        //...
    }
    Send(request: MsgSend): Promise<MsgSendResponse> {
        const data = MsgSend.encode(request).finish();
        const promise = this.rpc.request("cosmos.bank.v1beta1.Msg", "Send", data);
        return promise.then((data) => MsgSendResponse.decode(new _m0.Reader(data)));
    }
    //...
}
```

The important points to remember from this are:

1. `rpc: RPC` is an instance of a Protobuf RPC client that is given to you by CosmJS. Although the interface appears to be [declared locally](https://github.com/confio/cosmjs-types/blob/v0.8.0/src/helpers.ts#L180), this is the same interface found [throughout CosmJS](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/queryclient/utils.ts#L35-L37). It is given to you [on construction](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/queryclient/queryclient.ts). At this point you do not need an implementation for it.
2. You can see `encode` and `decode` in action. Notice the `.finish()` that flushes the Protobuf writer buffer.
3. The `rpc.request` makes calls that are correctly understood by the Protobuf compiled server on the other side.

You can find the same structure in [`query.ts`](https://github.com/confio/cosmjs-types/blob/v0.8.0/src/cosmos/bank/v1beta1/query.ts#L1583). (or `query.rpc.Query.ts` with telescope option rpcClients.inline: false)

### Proper saving

Commit the extra `.proto` files and the compiled ones to your repository so you do not need to recreate them. Add an npm run target to keep track of how this was done and easily reproduce it in the future when you update a Protobuf file:

```json
"scripts": {
    "codegen": "telescope transpile --config .telescope.json",
}
```

## Add convenience with types

CosmJS provides an interface to which all the created types conform, [`TsProtoGeneratedType`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/proto-signing/src/registry.ts#L12-L18), which is itself a sub-type of [`GeneratedType`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/proto-signing/src/registry.ts#L32). In the same file, note the definition:

```typescript [https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/proto-signing/src/registry.ts#L54-L57]
export interface EncodeObject {
    readonly typeUrl: string;
    readonly value: any;
}
```

The `typeUrl` is the identifier by which Protobuf identifies the type of the data to serialize or deserialize. It is composed of the type's package and its name. For instance (and see also [here](https://github.com/cosmos/cosmos-sdk/blob/3a1027c/proto/cosmos/bank/v1beta1/tx.proto)):

```protobuf
package cosmos.bank.v1beta1;
//...
message MsgSend {
    //...
}
```

In this case, the `MsgSend`'s type URL is [`"/cosmos.bank.v1beta1.MsgSend"`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/modules/bank/messages.ts#L6).

Each of your types is associated like this. You can also find constant strings like those in generated files if `prototypes.addTypeUrlToObjects` is set to true, take [tx.ts](https://github.com/confio/cosmjs-types/blob/v0.8.0/src/cosmos/bank/v1beta1/tx.ts#L82) as an example:

```typescript
export const MsgSend = {
  // there's no this field in the given link, but there will be if prototypes.addTypeUrlToObjects is set to true.
  typeUrl: "/cosmos.bank.v1beta1.MsgSend",
  ...
}
```

### For messages

Messages, sub-types of `Msg`, are assembled into transactions that are then sent to CometBFT. CosmJS types already include types for [transactions](https://github.com/confio/cosmjs-types/blob/v0.8.0/src/cosmos/tx/v1beta1/tx.ts#L10-L24). These are assembled, signed, and sent by the [`SigningStargateClient`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/signingstargateclient.ts#L280-L298) of CosmJS.

The `Msg` kind also needs to be added to a registry. To facilitate that, you should prepare them in a nested array:

```typescript [https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/modules/bank/messages.ts#L4-L7]
export const bankTypes: ReadonlyArray<[string, GeneratedType]> = [
    ["/cosmos.bank.v1beta1.MsgMultiSend", MsgMultiSend],
    ["/cosmos.bank.v1beta1.MsgSend", MsgSend],
];
```

Add child types to `EncodeObject` to direct TypeScript:

```typescript [https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/modules/bank/messages.ts#L9-L12]
export interface MsgSendEncodeObject extends EncodeObject {
    readonly typeUrl: "/cosmos.bank.v1beta1.MsgSend";
    readonly value: Partial<MsgSend>;
}
```

In the previous code, you cannot reuse your `msgSendTypeUrl` because it is a value not a type. You can add a type helper, which is useful in an `if else` situation:

```typescript [https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/modules/bank/messages.ts#L14-L16]
export function isMsgSendEncodeObject(encodeObject: EncodeObject): encodeObject is MsgSendEncodeObject {
    return (encodeObject as MsgSendEncodeObject).typeUrl === "/cosmos.bank.v1beta1.MsgSend";
}
```

### For queries

Queries have very different types of calls. It makes sense to organize them in one place, called an extension. For example:

```typescript [https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/modules/bank/queries.ts#L9-L18]
export interface BankExtension {
    readonly bank: {
        readonly balance: (address: string, denom: string) => Promise<Coin>;
        readonly allBalances: (address: string) => Promise<Coin[]>;
        //...
    };
}
```

Note that there is a **key** `bank:` inside it. This becomes important later on when you _add_ it to Stargate.

1. Create an extension interface for your module using function names and parameters that satisfy your needs.
2. It is recommended to make sure that the key is unique and does not overlap with any other modules of your application.
3. Create a factory for its implementation copying the [model here](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/modules/bank/queries.ts#L20-L59). Remember that the [`QueryClientImpl`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/modules/bank/queries.ts#L4) implementation must come from your own compiled Protobuf query service.

## Integration with Stargate

`StargateClient` and `SigningStargateClient` are typically the ultimate abstractions that facilitate the querying and sending of transactions. You are now ready to add your own elements to them. The easiest way is to inherit from them and expose the extra functions you require.

If your extra functions map one-for-one with those of your own extension, then you can publicly expose the extension itself to minimize duplication in [`StargateClient`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/stargateclient.ts#L143) and [`SigningStargateClient`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/signingstargateclient.ts#L109).

For example, if you have your `interface MyExtension` with a `myKey` key and you are creating `MyStargateClient`:

```typescript
export class MyStargateClient extends StargateClient {
    public readonly myQueryClient: MyExtension | undefined

    public static async connect(
      endpoint: string,
      options: StargateClientOptions = {},
  ): Promise<MyStargateClient> {
        const tmClient = await Tendermint34Client.connect(endpoint)
        return new MyStargateClient(tmClient, options)
    }

    protected constructor(tmClient: Tendermint34Client | undefined, options: StargateClientOptions) {
        super(tmClient, options)
        if (tmClient) {
            this.myQueryClient = QueryClient.withExtensions(tmClient, setupMyExtension)
        }
    }
}
```

You can extend [`StargateClientOptions`](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/stargateclient.ts#L139-L141) if your own client can receive further options.

You also need to inform `MySigningStargateClient` about the extra encodable types it should be able to handle. The list is defined in a registry that you can [pass as options](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/signingstargateclient.ts#L139).

Take inspiration from the [`SigningStargateClient` source code](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/signingstargateclient.ts#L76-L80) itself. Collect your new types into an array:

```typescript
import { defaultRegistryTypes } from "@cosmjs/stargate"

export const myDefaultRegistryTypes: ReadonlyArray<[string, GeneratedType]> = [
    ...defaultRegistryTypes,
    ...myTypes, // As you defined bankTypes earlier
]
```

Taking inspiration from [the same place](https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/signingstargateclient.ts#L118-L120), add the registry creator:

```typescript
function createDefaultRegistry(): Registry {
    return new Registry(myDefaultRegistryTypes)
}
```

Now you are ready to combine this into your own `MySigningStargateClient`. It still takes an optional registry, but if that is missing it adds your newly defined default one:

```typescript
export class MySigningStargateClient extends SigningStargateClient {
    public readonly myQueryClient: MyExtension | undefined

    public static async connectWithSigner(
        endpoint: string,
        signer: OfflineSigner,
        options: SigningStargateClientOptions = {}
    ): Promise<MySigningStargateClient> {
        const tmClient = await Tendermint34Client.connect(endpoint)
        return new MySigningStargateClient(tmClient, signer, {
            registry: createDefaultRegistry(),
            ...options,
        })
    }

    protected constructor(tmClient: Tendermint34Client | undefined, signer: OfflineSigner, options: SigningStargateClientOptions) {
        super(tmClient, signer, options)
        if (tmClient) {
            this.myQueryClient = QueryClient.withExtensions(tmClient, setupMyExtension)
        }
    }
}
```

You can optionally add dedicated functions that use your own types, modeled on:

```typescript [https://github.com/cosmos/cosmjs/blob/v0.28.3/packages/stargate/src/signingstargateclient.ts#L180-L196]
public async sendTokens(
    senderAddress: string,
    recipientAddress: string,
    amount: readonly Coin[],
    fee: StdFee | "auto" | number,
    memo = "",
): Promise<DeliverTxResponse> {
    const sendMsg: MsgSendEncodeObject = {
        typeUrl: "/cosmos.bank.v1beta1.MsgSend",
        value: {
            fromAddress: senderAddress,
            toAddress: recipientAddress,
            amount: [...amount],
        },
    };
    return this.signAndBroadcast(senderAddress, [sendMsg], fee, memo);
}
```

Think of your functions as examples of proper use, that other developers can reuse when assembling more complex transactions.

You are ready to import and use this in a server script or a GUI.

<HighlightBox type="tip">

If you would like to get started on building your own CosmJS elements on your own checkers game, you can go straight to the exercise in [CosmJS for Your Chain](/hands-on-exercise/3-cosmjs-adv/index.md) to start from scratch.

More specifically, you can jump to:

* [Create Custom Objects](/hands-on-exercise/3-cosmjs-adv/1-cosmjs-objects.md), to see how to compile the Protobuf objects.
* [Create Custom Messages](/hands-on-exercise/3-cosmjs-adv/2-cosmjs-messages.md), to see how to create messages relevant for checkers.
* [Backend Script for Game Indexing](/hands-on-exercise/3-cosmjs-adv/5-server-side.md), to see how this can be used also to listen to events coming from the blockchain.
* [Integrate CosmJS and Keplr](/hands-on-exercise/3-cosmjs-adv/4-cosmjs-gui.md), to see how to use and integrate what you prepared into a preexisting Checkers GUI.

</HighlightBox>

<HighlightBox type="synopsis">

To summarize, this section has explored:

* How CosmJS's out-of-the-box interfaces understand how messages of standard Cosmos SDK modules are serialized, meaning that your unique modules will require custom CosmJS interfaces of their own.
* How to create the necessary Protobuf objects and clients in TypeScript, the extensions that facilitate the use of these clients, and any further level of abstraction that you deem useful for integration.
* How to integrate CosmJS with Ignite's client and signing client, which are typically the ultimate abstractions that facilitate the querying and sending of transactions.

</HighlightBox>

<!-- ## What next?

The Interchain is vast, with lots of projects, people and concepts to discover:

* Reach out to the community.
* Contribute to the Cosmos SDK, IBC, and CometBFT development.
* Get support for enterprise solutions which you are developing.

Head to the [What's Next](/academy/whats-next/index.md) section to find useful information to launch your journey into the Interchain universe. -->

<!-- INCLUDE AFTER NEW STRUCTURE IS IMPLEMENTED: Head right into the [next section](/hands-on-exercise/3-cosmjs-adv/1-cosmjs-objects.md) to begin creating custom objects for your checkers blockchain.
So what's next?  -->
