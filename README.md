---
layout: ModuleLandingPage
main: true
order: 0
hideResources: true
tools:
  - title: Cosmos SDK
    description: A framework for building public blockchains.
    links:
      - name: Learn more
        url: https://v1.cosmos.network/sdk
      - name: Documentation
        url: https://docs.cosmos.network/
    image: /cosmos-sdk-icon.svg
  - title: CometBFT
    description: Blockchain consensus engine using the Tendermint algorithm.
    links:
      - name: Learn more
        url: https://docs.cometbft.com/v0.37/
      - name: Documentation
        url: https://docs.cometbft.com/v0.37/core/
    image: /tendermint-icon.svg
  - title: Ignite CLI
    description: All-in-one platform to build, launch, and maintain apps on a sovereign and secured blockchain.
    links:
      - name: Learn more
        url: https://ignite.com/cli
      - name: Documentation
        url: https://docs.ignite.com
    image: /ignitecli-icon.svg
  - title: IBC
    description: Industry standard protocol for inter-blockchain communication.
    links:
      - name: Learn more
        url: https://ibcprotocol.org/
      - name: Documentation
        url: https://ibc.cosmos.network/
    image: /ibc-icon.svg
  - title: CosmWasm
    description: Smart contracting platform built for the Interchain Ecosystem.
    links:
      - name: Learn more
        url: https://cosmwasm.com/
      - name: Documentation
        url: https://docs.cosmwasm.com/docs
    image: /cosmwasm-icon.svg
  - title: Cosmos Hub
    description: Software powering Cosmos Hub, the heart of the Interchain network, and home of the ATOM token.
    links:
      - name: Documentation
        url: https://hub.cosmos.network/
    image: /generic-star-icon.svg
articles:
  - title: How-to Cosmos Pt. 4- Building your first application with CosmJS
    date: Sunday, June 4
    time: 8
    url: https://bit.ly/43wVQne
    image: /article-banner-02.png
  - title: The Mind, Body, and Soul of Cosmos.
    date: Friday, May 27
    time: 6
    url: https://blog.cosmos.network/the-mind-body-and-soul-of-cosmos-140ee7cec0cd
    image: /article-banner-01.png
  - title: "ELI5: What is IBC?"
    date: Tuesday, Nov 15
    time: 7
    url: https://medium.com/the-interchain-foundation/eli5-what-is-ibc-def44d7b5b4c
    image: /article-banner-04.jpg
intro:
  - overline: Begin Your Interchain Journey
    title: Developer Portal
    image: /graphics-sdk-course.png
    description: |
      The Interchain is a network of interoperable blockchains built on BFT consensus.<br/><br/>
      The ever-expanding ecosystem provides an SDK, tokens, wallets, applications, and services. Discover the Cosmos SDK to develop application-specific blockchains.<br/><br/>
      Ready to begin your journey?
    action:
      label: Start
      url: /academy/1-what-is-cosmos/index.html
    secondtext: |
      This is a beta version of the Developer Portal that will help you take your first steps with the Cosmos SDK.
      <br>We would be grateful for your feedback. At the end of each are three icons to rate the page and a small box where you can give us feedback about things to improve. Enjoy your journey through the portal and good luck with the HackAtom!
overview:
  overline: Frequently Asked Questions
  title: FAQ
  items:
    - title: Is this portal for me?
      description: |
        This portal offers an overview of the Interchain Stack (the Cosmos SDK, Ignite, CosmJS, etc.), as well as examples and exercises to help developers get a quick start.
    - title: How should I go through the portal?
      description: |
        The portal content unfolds the Interchain universe, beginning with a big-picture perspective and getting into how to create your application-specific blockchain with the Cosmos SDK. The introductory chapters give you a solid overview of the Cosmos SDK, its concepts, and dApps built on the Interchain Stack, while the hands-on exercises put theory into practice – showcasing how to address application concerns when developing such as managing gas fees, working with Ignite CLI, running a node, and making a chain IBC-enabled. A look into all chapters is recommended, as a basic understanding helps when things get tricky. <br/>
        You can follow your own path or go through the content chronologically. To follow your own path, select the tags for the subjects you are interested in and all the chapters and sections relating to that subject will be displayed. <br/>
        Additionally, you can select a version of the portal if you want to explore the content in a specific Interchain Stack version. For more information on which stack version is used in the different versions of the portal, please <a href="/versions.html">go to the page on Platform Versions</a>. In general, we do recommend using the most current version.
    - title: What do I need to know?
      description: |
        For the conceptual, high-level sections, you should have a solid understanding of blockchain technology and be familiar with decentralized applications. The deep dives are specially designed for experienced developers.
    - title: How long will it take?
      description: |
        With the concept sections, you will gain a solid understanding of Cosmos and the Interchain Stack. This might be a matter of just a couple of hours. The time you spend on the other chapters and the hands-on exercise depends on you – there are endless possibilities to discover. <br/><br/>
        Welcome to the Interchain!

customModules:
  - title: All there is to discover
    description: |
      The portal has three different sections, with content to take you through the fundamentals of the Interchain, dive deeper in the tutorials, and apply your conceptual knowledge in the hands-on exercise chapters.
    useNarrowCards: true
    hideFilter: true
    image: /lp-images/ida_dev_portal_lp_background-00.png
    imageLightMode: /lp-images/ida_dev_portal_lp_background-00.png
    sections:
      - image:
        title: Introduction to the Interchain
        href: /academy/1-what-is-cosmos/
        description: |
          From a high-level overview of the Interchain Ecosystem to the main concepts of the Cosmos SDK and IBC, take a look at these more conceptual introductions to begin your journey.
        tags:
          - concepts
      - image:
        title: Hands-on Exercise
        href: /hands-on-exercise/1-ignite-cli/
        description: |
          Work with the full Interchain Stack while developing a checkers game blockchain with Ignite CLI, CosmJS and IBC, and preparing your chain to run in production.
        tags:
          - guided-coding
      - image:
        title: Tutorials
        href: /tutorials/1-tech-terms/
        description: |
          Dive into these tutorials to help get you started and to take a more detailed look at IBC, CosmJS, and some of the other SDK modules. The tutorials walk you through code examples to translate interchain concepts into practice.

  - title: Introduction to the Interchain
    description: |
      Discover the interchain ecosystem, Cosmos SDK, and IBC. Feel free to start with the first chapter, or select one or more tags to filter the available content.
    sections:
      - image: /lp-images/ida_dev_portal_lp_hero-05.png
        title: What is the interchain?
        href: /academy/1-what-is-cosmos/
        description: |
          Get a fast overview of:
        links:
          - title: Blockchain technology and the interchain
            path: /academy/1-what-is-cosmos/1-blockchain-and-cosmos.html
          - title: The interchain ecosystem
            path: /academy/1-what-is-cosmos/2-cosmos-ecosystem.html
          - title: Interchain use cases
            path: /academy/1-what-is-cosmos/3-interchain-use-cases.html
          - title: Getting ATOM and staking it
            path: /academy/1-what-is-cosmos/4-atom-staking.html
      #- {} Example of section break
      - image: /lp-images/cosmos_dev_portal_module-03-lp-new.png
        title: Interchain concepts
        href: /academy/2-cosmos-concepts/
        description: |
          This chapter includes an introduction to the main concepts underlying the Interchain Stack and Cosmos SDK:
        links:
          - title: Blockchain app architecture
            path: /academy/2-cosmos-concepts/1-architecture.html
            tags:
              - concepts
              - cometbft
              - cosmos-sdk
          - title: Accounts
            path: /academy/2-cosmos-concepts/2-accounts.html
          - title: Transaction, messages, and modules
            path: /academy/2-cosmos-concepts/3-transactions.html
          - title: Protobuf
            path: /academy/2-cosmos-concepts/6-protobuf.html
          - title: Multistore and keepers
            path: /academy/2-cosmos-concepts/7-multistore-keepers.html
          - title: BaseApp
            path: /academy/2-cosmos-concepts/8-base-app.html
          - title: Queries, events, and context
            path: /academy/2-cosmos-concepts/9-queries.html
          - title: Testing
            path: /academy/2-cosmos-concepts/12-testing.html
          - title: Relaying with IBC
            path: /academy/2-cosmos-concepts/13-relayer-intro.html
          - title: Interchain Security
            path: /academy/2-cosmos-concepts/14-interchain-security.html
          - title: Bridges
            path: /academy/2-cosmos-concepts/15-bridges.html
          - title: Migrations
            path: /academy/2-cosmos-concepts/16-migrations.html
      - image: /lp-images/ida_dev_portal_lp_hero-04-b.png
        title: Introduction to IBC
        href: /academy/3-ibc/
        description: |
          This chapter will give you an understanding of the Inter-Blockchain Communication Protocol including:
        links:
          - title: What is IBC?
            path: /academy/3-ibc/1-what-is-ibc.html
          - title: IBC/TAO - connections, channels, and clients
            path: /academy/3-ibc/2-connections.html
          - title: Light client development
            path: /academy/3-ibc/5-light-client-dev.html
          - title: Solo machine client
            path: /academy/3-ibc/6-solomachine.html
          - title: IBC token transfer
            path: /academy/3-ibc/7-token-transfer.html
          - title: Interchain accounts
            path: /academy/3-ibc/8-ica.html
          - title: IBC middleware
            path: /academy/3-ibc/9-ibc-mw-intro.html
          - title: IBC tooling
            path: /academy/3-ibc/12-ibc-tooling.html
  - title: Tutorials
    description: |
      Get started and explore IBC, CosmJS, and Cosmos SDK modules. Feel free to start with the first chapter, or select one or more tags to filter the available content.
    sections:
      - image:
        title: Good-to-know dev terms
        href: /tutorials/1-tech-terms/
        description: |
          From gRPC to Protobuf - a brief overview of dev terms you encounter in the interchain ecosystem.
        links:
      - image:
        title: Set up your work environment
        href: /tutorials/2-setup/
        description: |
          Find out what you should install on your machine to work with the Cosmos SDK and work on the hands-on exercises.
      - image:
        title: Run a node, API, and CLI
        href: /tutorials/3-run-node/
        description: |
          Take your first steps with simapp and get your first node for a Cosmos blockchain running.
      - image:
        title: Golang introduction
        href: /tutorials/4-golang-intro/
        description: |
          Take your first steps with Go to discover the basics, including a look at Go interfaces, structures, arrays, slices, and much more.
      - image:
        title: Docker introduction
        href: /tutorials/5-docker-intro/
        description: |
          Learn enough about Docker so as to conveniently do the hands-on exercises.
      - image:
        title: IBC developers
        href: /tutorials/6-ibc-dev/
        description: |
          Want to become an IBC developer? Take a closer look at IBC denoms.
      - image: /lp-images/ida_dev_portal_lp_hero-09.png
        title: Introduction to CosmJS
        href: /tutorials/7-cosmjs/
        description: |
          Want to integrate frontend user interfaces and backend servers with your Cosmos chain? Take a closer look at:
        links:
          - title: What is CosmJS?
            path: /tutorials/7-cosmjs/1-cosmjs-intro.html
          - title: Your first CosmJS actions
            path: /tutorials/7-cosmjs/2-first-steps.html
          - title: Compose complex transactions
            path: /tutorials/7-cosmjs/3-multi-msg.html
          - title: Learn to integrate Keplr
            path: /tutorials/7-cosmjs/4-with-keplr.html
          - title: Create custom CosmJS interfaces
            path: /tutorials/7-cosmjs/5-create-custom.html
      - image: /lp-images/ida_dev_portal_lp_hero-06.png
        title: Understand SDK modules
        href: /tutorials/8-understand-sdk-modules/
        description: |
          Discover several tutorials on specific SDK modules:
        links:
          - title: The authz module
            path: /tutorials/8-understand-sdk-modules/1-authz.html
          - title: The feegrant module
            path: /tutorials/8-understand-sdk-modules/2-feegrant.html
          - title: The group module
            path: /tutorials/8-understand-sdk-modules/3-group.html
          - title: The gov module
            path: /tutorials/8-understand-sdk-modules/4-gov.html
      - image: /lp-images/ida_dev_portal_lp_hero-07.png
        title: The path to production
        href: /tutorials/9-path-to-prod
        description: |
          Prepare your blockchain to run in production:
        links:
          - title: Run in production
            path: /tutorials/9-path-to-prod/1-overview.html
          - title: Prepare the software to run
            path: /tutorials/9-path-to-prod/2-software.html
          - title: Prepare a validator and keys
            path: /tutorials/9-path-to-prod/3-keys.html
          - title: Prepare where the node starts
            path: /tutorials/9-path-to-prod/4-genesis.html
          - title: Prepare and connect to other nodes
            path: /tutorials/9-path-to-prod/5-network.html
          - title: Configure, run, and set up a service
            path: /tutorials/9-path-to-prod/6-run.html
          - title: Prepare and do migrations
            path: /tutorials/9-path-to-prod/7-migration.html
  - title: Hands-on Exercise
    description: |
      From zero to hero - work with the full Interchain Stack while developing a checkers game blockchain. Feel free to start with the first chapter, or select one or more tags to filter the available content.
    sections:
      - image: /lp-images/cosmos_dev_portal_module-04-lp.png
        title: SDK v0.50 native
        href: /hands-on-exercise/0-native/
        description: |
          Do the minimum to develop a checkers blockchain natively with Cosmos SDK v0.50:
        links:
          - title: Preparation
            path: /hands-on-exercise/0-native/1-preparation.html
          - title: Build your module
            path: /hands-on-exercise/0-native/2-build-module.html
          - title: Add your first object
            path: /hands-on-exercise/0-native/3-add-game.html
          - title: Add your first message
            path: /hands-on-exercise/0-native/4-add-message.html
          - title: Add your first query
            path: /hands-on-exercise/0-native/5-add-query.html
      - image: /lp-images/cosmos_dev_portal_module-05-lp.png
        title: Ignite CLI basics
        href: /hands-on-exercise/1-ignite-cli/
        description: |
          Get started with Ignite CLI and begin developing a checkers blockchain:
        links:
          - title: Introduction to Ignite CLI
            path: /hands-on-exercise/1-ignite-cli/1-ignitecli.html
          - title: First steps for your checkers blockchain
            path: /hands-on-exercise/1-ignite-cli/2-exercise-intro.html
          - title: Store object
            path: /hands-on-exercise/1-ignite-cli/3-stored-game.html
          - title: Create custom messages
            path: /hands-on-exercise/1-ignite-cli/4-create-message.html
          - title: Create and save a game
            path: /hands-on-exercise/1-ignite-cli/5-create-handling.html
          - title: Add a way to make a move
            path: /hands-on-exercise/1-ignite-cli/6-play-game.html
          - title: Emit game information
            path: /hands-on-exercise/1-ignite-cli/7-events.html
          - title: Record the winners
            path: /hands-on-exercise/1-ignite-cli/8-game-winner.html
      - image: /lp-images/ida_dev_portal_lp_hero-02.png
        title: Ignite CLI advanced
        href: /hands-on-exercise/2-ignite-cli-adv/
        description: |
          Continue developing your checkers blockchain with Ignite by:
        links:
          - title: Keep an up-to-date game deadline
            path: /hands-on-exercise/2-ignite-cli-adv/1-game-deadline.html
          - title: Keep track of moves being played
            path: /hands-on-exercise/2-ignite-cli-adv/2-move-count.html
          - title: Put your games in order
            path: /hands-on-exercise/2-ignite-cli-adv/3-game-fifo.html
          - title: Enforce auto-expiring of games
            path: /hands-on-exercise/2-ignite-cli-adv/4-game-forfeit.html
          - title: Let players set a wager
            path: /hands-on-exercise/2-ignite-cli-adv/5-game-wager.html
          - title: Handle wager payments
            path: /hands-on-exercise/2-ignite-cli-adv/6-payment-winning.html
          - title: Integration tests
            path: /hands-on-exercise/2-ignite-cli-adv/7-integration-tests.html
          - title: Incentivize players
            path: /hands-on-exercise/2-ignite-cli-adv/8-gas-meter.html
          - title: Help find a correct move
            path: /hands-on-exercise/2-ignite-cli-adv/9-can-play.html
          - title: Play with cross-chain tokens
            path: /hands-on-exercise/2-ignite-cli-adv/10-wager-denom.html
      - image: /lp-images/ida_dev_portal_lp_hero-08.png
        title: CosmJS advanced
        href: /hands-on-exercise/3-cosmjs-adv/
        description: |
          Apply your knowledge of CosmJS to the checkers blockchain exercise. You will:
        links:
          - title: Create custom objects
            path: /hands-on-exercise/3-cosmjs-adv/1-cosmjs-objects.html
          - title: Create custom messages
            path: /hands-on-exercise/3-cosmjs-adv/2-cosmjs-messages.html
          - title: Get an external GUI
            path: /hands-on-exercise/3-cosmjs-adv/3-external-gui.html
          - title: Integrate CosmJS and Keplr
            path: /hands-on-exercise/3-cosmjs-adv/4-cosmjs-gui.html
          - title: Use CosmJS for game indexing
            path: /hands-on-exercise/3-cosmjs-adv/5-server-side.html
      - image: /lp-images/universe.png
        title: From code to MVP to production
        href: /hands-on-exercise/4-run-in-prod/
        description: |
          Show your checkers blockchain to investors and progressively the world:
        links:
          - title: Simulate a production setup with Docker Compose
            path: /hands-on-exercise/4-run-in-prod/1-run-prod-docker.html
          - title: Tally player info after production
            path: /hands-on-exercise/4-run-in-prod/2-migration-info.html
          - title: Add a leaderboard as a module
            path: /hands-on-exercise/4-run-in-prod/3-add-leaderboard.html
          - title: Migrate the leaderboard module after production
            path: /hands-on-exercise/4-run-in-prod/4-migration-leaderboard.html
          - title: Simulate a migration in production with Docker Compose
            path: /hands-on-exercise/4-run-in-prod/5-migration-prod.html
      - image: /lp-images/ida_dev_portal_lp_hero-03.png
        title: IBC - Advanced
        href: /hands-on-exercise/5-ibc-adv/
        description: |
          Test your IBC knowledge by developing your checkers blockchain further:
        links:
          - title: Go relayer
            path: /hands-on-exercise/5-ibc-adv/1-go-relayer.html
          - title: Hermes relayer
            path: /hands-on-exercise/5-ibc-adv/2-hermes-relayer.html
          - title: IBC app development introduction
            path: /hands-on-exercise/5-ibc-adv/3-ibc-app-intro.html
          - title: Make a module IBC-enabled
            path: /hands-on-exercise/5-ibc-adv/4-ibc-app-steps.html
          - title: Add packet and acknowledgement data
            path: /hands-on-exercise/5-ibc-adv/5-ibc-app-packets.html
          - title: Extend the checkers game with a leaderboard
            path: /hands-on-exercise/5-ibc-adv/6-ibc-app-checkers.html
          - title: Create a leaderboard chain
            path: /hands-on-exercise/5-ibc-adv/7-ibc-app-leaderboard.html
---

This repo contains the code and content for the [Developer Portal](https://developers.cosmos.network/) and the [Interchain Developer Academy](https://academy.cosmos.network/).

Note: The layout metadata at the top of the README.md file controls how the tutorial page is published. Write permissions are limited to preserve the structure and contents.

These tutorials guide you through actionable steps and walk-throughs to teach you how to use the Interchain Stack. The Interchain Stack is the world’s most popular framework for building application-specific blockchains, it consists of several products:

* **Cosmos SDK**, a modular framework to build blockchain applications
* **IBC**, the Inter-Blockchain Communication protocol that allows blockchains to communicate
* **Tendermint**, the algorithm that provides the consensus and networking layer for your blockchain application through CometBFT

The Developer Portal contains three types of content:

* **Concepts**, informational content explaining how the Interchain Stack functions
* **Individual Tutorials**, short tutorials to get you up to speed with individual components
* **Checkers Game**, a modular tutorial that covers the full stack and teaches you every element from set-up to launching in production with a front-end application attached.

Going through the entire content will teach you about:

* Blockchain technology and cryptography
* Developing with the Cosmos SDK & Ignite CLI
* The CometBFT consensus algorithm
* The Inter-Blockchain Communication Protocol
* Building front- and backends with CosmJS
* Integrating wallets such as Keplr
* Relaying in the interchain network

The code and docs for each tutorial are based on a specific version of the software. Be sure to follow the tutorial instructions to download and use the right version.

Use the tutorials landing page as your entry point to articles on the [Interchain blog](https://blog.cosmos.network/), videos on [Interchain YouTube](https://www.youtube.com/c/CosmosProject/videos), and ways to get help and support.

This repo manages and publishes the developer platform. **If you would like to contribute**, please take a look at our [CONTRIBUTION GUIDELINES](CONTRIBUTING.md) and [/developer-portal/TECHNICAL-SETUP](TECHNICAL-SETUP.md) documents.
