# Loxone RGB to Philips HUE value convertor

This program is written to control a Philips Hue RGB light bulb with a Loxone miniserver. 

Loxone has program function blocks where picoc code gets interpreted. 

## Requirements
- Loxone miniserver admin access
- Philips Hue Bridge access

## How to

### Add an application block

- In the top ribbon when on your rooms page: Function block -> General -> Program
- Double click the new block to open the program field
- Paste the content of program.picoc in the field
- OK to close

### Retrieve an API key from Hue bridge

Following the guide available on <https://developers.meethue.com/develop/get-started-2/> 

- open `https://<bridge ip address>/debug/clip.html`
- press the link button on the bridge
- send POST command to `/api` with body `{"devicetype":"my_hue_app#loxone"}` 
- save the "username" to use later

### Create Virtual Output in Loxone Config

Following the guide available on <https://www.loxone.com/enen/kb/virtual-inputs-outputs/>

#### Create the output (where to send to)

- Select "virtual outputs" in the miniserver section of devices
- On the ribbon the section "virtual outputs" is activated
- Select "virtual output" to create a new one
- Give it a name and set `http://<bridge ip address>` as address

#### Create the command (what to send)

- In the top ribbon you can select "virtual output command" when sellecting the previously created output
- Give a name
- At "Command for ON" you give the path for the command `/api/<username frome before>/lights/<number of the light>/state`
- HTTP Body for ON is the value "`<v>`" which will be filled in with the value the program creates
- HTTP Method is `PUT`

### Connect it all

- Connect the desired input to the application blocks I1 input (eg. lightning control block)
- Connect the output txt1 to the Virtual Output Command
- Save and test 

## Sources

The following sources helped building this script and can be used as reference: 

- <https://developers.meethue.com/develop/get-started-2/>
- <https://www.loxone.com/enen/kb/virtual-inputs-outputs/>
- <http://updatefiles.loxone.com/KnowledgeBase/Online/Common/Documents/CustomScriptProgramming.pdf>
