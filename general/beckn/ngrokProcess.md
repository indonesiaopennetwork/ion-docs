# Process to setup ION Buyer/Seller App using Beckn Starter Pack (Ngrok version)

## Introduction
This document describes the process of setting up ION Buyer App and ION Seller App using the Beckn Starter Kit. In particular this document explains the process to set it up using Ngrok. Ngrok setup is required 
- when you need to call discover call on a external discover service and want it to respond to your local machine.
- when you need the external discover service to crawl catalogs published locally


If the above two are not required (i.e. you use a local discover service or want to test the rest of P2P messages), the process is simpler and explained in the starter kit repo that you will download in the steps below. (Actually the ngrok process is also explained there. This document has been written to make the instructions more linear and to include the ION parts of the configuration)

## Prerequisites
Your laptop needs to have the following software already installed.
1. git client (you should be able to run `git` command in a terminal)
2. Docker desktop (you should be able to run `docker compose` command in a terminal)
3. Postman application (we will be using this to simulate Buyer App/Seller App communications)
4. ngrok client
    - We need to download the ngrok software onto local machine (You should be able to run `ngrok` in terminal)
    - We need to signup at ngrok.com . 
    - After signup, click on the `Your AuthToken` menu on the left panel and copy the Auth Token. This example will refer to it as `NGROK_AUTH_TOKEN` and use the value `3ILskgpCIWJW....` in code listing(The string is around 49 chars long. Due to privacy reason this document truncates this string.) 
    - In the ngrok website, click on the domains menu item. The center panel will show a web address that is reserved for you. This document will refer to this as `NGROK_DEV_DOMAIN` and will use the value `criteria-overheat-modular.ngrok-free.dev`

## Step 1: Clone the starter kit
1. In a terminal run the following command to clone the beckn starter kit `git clone https://github.com/beckn/starter-kit.git`. This command will create a folder called `starter-kit`. 
2. Within the `starter-kit` folder, there is the `generic-devkit` folder. This is effectively the root folder for all our operations.
3. The generic-devkit folder has three subfolders. 
    - `config` - We will be setting the configuration values of the Buyer App (BAP) and Seller App (BPP) in this folder. 
    - `install` - This contains the docker compose file and ngrok configuration file.
    - `postman` - This contains the postman collection we will be using to simulate a Buyer App or Seller App sending messages.
4. When we get to catalog publishing, there will be a fourth temporary folder called `data` that will host our catalogs.

## Step 2: Get your credentials from ION devlabs
In this step, we will signup to ION devlabs, create buyer app and seller app profile (this guide does both, but if you are developing only one of these, you can create only one), create and register Beckn credentials.
1. Signup to ION (https://central.ion.id). In the wizard, this guide chose "I do both - sell and serve buyers" as the role. You can choose the one appropriate for you. 
2. From the ION Central website, click on Devlabs icon on the left side panel. It will take you to devlabs. 
3. Click on the validate icon on the left hand side panel. This is where we can get Keys to transact on the staging network.
4. Click on "Change Key" button. A dialog box pops up where you can add a new key or view details of an existing key.
5. Click on "Generate Key". Type in a
    - `Name`. This can be anything. For this guide, we used BPP_WARUNG_VB when creating for seller and BAP_WARUNG_VB when creating for Buyer app profile. (If you are only working with one profile, you will have to add only one)
    - `Subscriber URL` - This is the NGROK_DEV_DOMAIN you got in Prereq. We used `https://criteria-overheat-modular.ngrok-free.dev/bap/receiver` for Buyer App profile and `https://criteria-overheat-modular.ngrok-free.dev/bpp/receiver` for Seller App profile. (Currently due to a quirk in the UI, you have to use `bap-warung-vb.ion.id` and `bpp-warung-vb.ion.id` as the domain name while creating it and then come back and change to the above)
    - `Subscriber Id` - We used `bap-warung-vb.ion.id` for Buyer App and `bpp-warung-vb.ion.id` for Seller App. 
    - `Public Key` - Click on Generate Key Pair, the public key will be auto-populated and the private key will be downloaded. For this guide our
        - BAP private key was `p1Vn9SDDrv...90awttDYI8I=` (truncated for privacy)
        - BAP public key was `nJa6DgvKuewcmda3pTAWBbGlHRLzX1rQhCbS79f/kUs=`
        - BPP private key was `navmVa+AbJUlJu...SWBTwpTj+4tuEUg=` (truncated for privacy)
        - BPP public key was `pEykb23DAr2kYdQDBMiawE+SkNZo1oikdrbS4/zeoT4=`
    - Click on "Register Key". 
    - Edit the key and fix the Subscriber URL to the ngrok-free.dev values (the quirk we mentioned above)
6. Once the keys are created, click on the View details and note down the following values:

Buyer App 
```
Subscriber Id: bap-warung-vb.ion.id
Subscriber URL: https://criteria-overheat-modular.ngrok-free.dev/bap/receiver
Public Key: nJa6DgvKuewcmda3pTAWBbGlHRLzX1rQhCbS79f/kUs=
KeyId: 76EU8HW89k5QvUavQYShfpPVU3tpmxXyX47Cf12KNeGzEzck4yVAW2
```

Seller App
```
Subscriber Id: bpp-warung-vb.ion.id
Subscriber URL: https://criteria-overheat-modular.ngrok-free.dev/bpp/receiver
Public Key: pEykb23DAr2kYdQDBMiawE+SkNZo1oikdrbS4/zeoT4=
KeyId: 76EU83JyA6N9aAaaKp4wx68JLybsZqvFUvUV16ve5Sx3eLqxaAmuph
```

## Step 3: Configure and start ngrok
1. In the install sub folder of generic-devkit, make a copy of the ngrok.yml.example and call it ngrok.yml.
2. Within the ngrok.yml file, 
    - paste your NGROK_AUTH_TOKEN value (3ILskgpCIWJW....) into the agent.authtoken field. 
    - paste your NGROK_DEV_DOMAIN value (criteria-overheat-modular.ngrok-free.dev) into tunnels.beckn.domain field. (Uncomment key if it is commented)
    - Refer to the prereq section for these values.
3. Start ngrok by typing `ngrok start --all --config ngrok.yml`

## Step 4: Configure and start the ONIX and sandboxes
1. In the `generic-devkit/config/generic-bap.yaml` under the modules.bapTxnReceiver.handler.plugins.keymanager set the following values from Step 2 above.
 
```
            subscriberId: bap-warung-vb.ion.id
            keyId: 76EU8HW89k5QvUavQYShfpPVU3tpmxXyX47Cf12KNeGzEzck4yVAW2
            signingPrivateKey: p1Vn9SDDrv...90awttDYI8I=
            signingPublicKey: nJa6DgvKuewcmda3pTAWBbGlHRLzX1rQhCbS79f/kUs=
            encrPrivateKey: p1Vn9SDDrv...90awttDYI8I=
            encrPublicKey: nJa6DgvKuewcmda3pTAWBbGlHRLzX1rQhCbS79f/kUs=
```
2. Set the same above values also in modules.bapTxnCaller.handler.plugins.keymanager in the same file (IMPORTANT)
3. In the `generic-devkit/config/generic-bpp.yaml` under the modules.bppTxnReceiver.handler.plugins.keymanager set the following values from Step 2 above

```
            subscriberId: bpp-warung-vb.ion.id
            keyId: 76EU83JyA6N9aAaaKp4wx68JLybsZqvFUvUV16ve5Sx3eLqxaAmuph
            signingPrivateKey: navmVa+AbJUlJu...SWBTwpTj+4tuEUg=
            signingPublicKey: pEykb23DAr2kYdQDBMiawE+SkNZo1oikdrbS4/zeoT4=
            encrPrivateKey: navmVa+AbJUlJu...SWBTwpTj+4tuEUg=
            encrPublicKey: pEykb23DAr2kYdQDBMiawE+SkNZo1oikdrbS4/zeoT4=
```
4. Set the same above values also in the modules.bppTxnCaller.handler.plugins.keymanager in the same file (IMPORTANT)
5. Set the same above values also in the modules.catalogPublish.handler.plugins.keymanager in the same file (IMPORTANT). Also under modules.catalogPublish.handler.plugins.catalogPublisher change the `catalogBaseUrl` attribute to your NGROK_DEV_DOMAIN/beckn e.g. `catalogBaseURL: "https://criteria-overheat-modular.ngrok-free.dev/beckn"`
6. In all the config files(3-4 files in generic-devkit/config have this string) find the string `beckn.one/testnet` and replace it with the networkId for ion `ion.id/ion-launch` (in future this will be `ion.id/staging`)
7. In the file config/generic-routing-BAPCaller.yaml, the target.url for the discover endpoint should point to the ION Discover server. Set it to `https://34.47.138.217.sslip.io/beckn` (In future this will be `https://discover.infra.ion.id/beckn`)
8. In the `docker-compose-generic.yml` file, change the image path of sandbox-bpp from `image: fidedocker/sandbox-2.0:latest` to `image: ionregistry/beckn-sandbox-2.0:latest`. Please verify that the container being changed is `sandbox-bpp`.  
9. Start Docker Desktop and in the generic-devkit/install folder run `docker compose -f docker-compose-generic.yml up`

## Step 5: Configure Postman and test messages
1. Open the Postman application.
2. Import the two collections present in generic-devkit/postman. The collections are named BAP Beckn Starter Kit and BPP Beckn Starter Kit
3. In the `variables` tab of the BOTH the collections, change 
    - `bap_id` variable to point to your Buyer app subscriber ID (bap-warung-vb.ion.id)
    - `bpp_id` variable to point to your Seller app subscriber ID (bpp-warung-vb.ion.id)
    - `public_url` variable to point to  `https://{NGROK_DEV_DOMAIN}` (https://criteria-overheat-modular.ngrok-free.dev)
    - `networkId` variable to `ion.id/ion-launch` (In future this will be `ion.id/staging`)
4. In the BAP Beckn Starter Kit collection in the discover call, change the body to the following and send the message. If you notice the logs of the docker containers, you should see the catalogs being returned. You can also see the on_discover in the ngrok log at http://localhost:4040

```
  "message": {
        "intent": {
            "textSearch": "Coffee"
        }
  }
```
5. In the BAP Beckn Starter Kit collection, run the Transaction messages of select, init and confirm and they are working fine (by observing docker log or ngrok log.)
6. In the BPP Beckn Starter Kit collection, for the on_select, on_init and on_confirm messages, the networkId has been hardcoded within the body>>context as `"networkId": "beckn.one/testnet"`. Within postman, open each of these messages and change it to the following: `"networkId": "{{networkId}}"`. Run the on_select, on_init and on_confirm messages.

## Step 6: Test seller app publishing messages
1. Open the BPP Beckn Starter Kit collection and folder Catalog Publishing within it (in Postman). In the body of the message, under message>>catalogs, you will find the id, bppId and bppUri attributes. Change them to the value below.

```
        "id": "{{bpp_id}}/CAT-GENERIC-001",
        "bppId": "{{bpp_id}}",
        "bppUri": "{{bpp_uri}}",
```
Similarly in the same body, under message>>publishDirectives, change the catalogId and visibleTo as below

```
    "publishDirectives": [
      {
        "catalogId": "{{bpp_id}}/CAT-GENERIC-001",
        "visibleTo": [
          "{{networkId}}"
        ],
        "catalogType": "REGULAR"
      }
    ]
```
2. Run the publish request. The response should show status as completed. There could be a warning saying DeDi record does not link to catalog index file. See next item for reason and solution for this. For the time being goto the data folder under generic-devkit and ensure that you see the catalogs and index.
3. Linking the subscriber record to the index file: When publishing you might have seen a warning : "DeDi record for bpp-warung-vb.ion.id does not link catalog index https://criteria-overheat-modular.ngrok-free.dev/beckn/index/becknCatalogs.index.json; add this URL to meta.catalog_index_urls on your DeDi record". This is a temporary warning till the devlabs is updated. Once devlabs is updated, this error should go away. For the time being, the DeDi record will have to be modified to add your index position to be crawled.

```
"meta": {
    "catalog_index_urls": [
      { "url": "https://criteria-overheat-modular.ngrok-free.dev/beckn/index/becknCatalogs.index.json" }
    ]
  }
```
- Contact ION Support for this task. If you see this warning, this has to be fixed before you see the catalog through discover. Once ION Support have added the record, delete the catalogs and index folder in data/beckn folder and try publish again. The warnign should not appear. 

4. Once the catalog has been published, it is time to discover it from the Buyer App(Give a few mins for the sync). Go to the BAP Beckn Starter Kit collection and discover request. Change the content of the intent to the following. This intent will directly search the catalog you added.

```
"intent": {
    "filters": {
        "type": "jsonpath",
        "expression":"$.catalogs[*] ? (@.id == \"{{bpp_id}}/CAT-GENERIC-001\")"  
    }
}
```
You should see the catalog you added in the docker logs and in the ngrok logs. 