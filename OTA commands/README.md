# OTA server / client commands

## Setting up the Zigbee gateway

Start the gateway:
> ./Z3GatewayHost_664.exe -n 0 -p /dev/ttyS36

- -n = handshaking to use
- -p = port to use

## Server commands

Check what images are on the server
> plugin ota-storage printImages

Start the network
> plugin network-creator start 1

Open then network for joining
> plugin network-creator-security open-network

## Client commands

> plugin ota-storage-common printImages
>
> plugin ota-storage-common delete 0
>
> plugin network-steering start 0
>
> plugin ota-client start
>
> plugin ota-client bootload 0

## Image builder commands

Create an OTA file from an application with specific version number, image type, manufacturer ID and tag ID.

> C:\SiliconLabs\SimplicityStudio\v5\developer\sdks\gecko_sdk_suite\v3.1\protocol\zigbee\tool\image-builder\image-builder-windows --create=update_v6.ota --version=0x00000005 --image-type=0x4206 --manuf-id=0x117C --tag-id=0000 --string="signed" --tag-file=myapp.gbl

Create a combined OTA image of application and bootloader 

> C:\SiliconLabs\SimplicityStudio\v5\developer\sdks\gecko_sdk_suite\v3.1\protocol\zigbee\tool\image-builder\image-builder-windows --create=combined_v6_slots.ota --version=0x00000006 --image-type=0x4206 --manuf-id=0x117C --tag-id=0x0000 --string="combined_app_bl" --tag-file=combined_app_bl.gbl

## Building GBL files

Create a standard GBL file from an S37 file:
> c:\SiliconLabs\SimplicityStudio\v5\developer\adapter_packs\commander\commander gbl create myapp.gbl --app myapp.s37

Create a GBL file containg application and bootloader: 
> C:\SiliconLabs\SimplicityStudio\v5\developer\adapter_packs\commander\commander.exe gbl create combined_app_bl.gbl --app Z3LightSoc.s37 --bootloader bootloader-storage-internal-single.s37

Create a signing key for encryption / signing GBL files:

> c:\SiliconLabs\SimplicityStudio\v5\developer\adapter_packs\commander\commander util genkey --type ecc-p256 --privkey signing_key.pem --pubkey signing_pubkey.pem

Create a signed GBL file from an S37 file:

> c:\SiliconLabs\SimplicityStudio\v4_3\developer\adapter_packs\commander\commander gbl create myapp-signed.gbl --app myapp.s37 --sign signing-key

Verify the signed image:

> c:\SiliconLabs\SimplicityStudio\v4_3\developer\adapter_packs\commander\commander util verifysign myapp-signed.gbl --verify signing-key.pub

Write the signing key to the tokens:

> c:\SiliconLabs\SimplicityStudio\v5\developer\adapter_packs\commander\commander flash --tokengroup znet --tokenfile signing-key-tokens.txt

## Deprecated

> c:\SiliconLabs\SimplicityStudio\v4_3\developer\adapter_packs\commander\commander gbl keygen --type ecc-p256 --outfile signing-key