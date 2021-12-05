---
layout: default
---

## Terminal commands for ESP32
#### 2018/01/29 GuntherVD
Terminal commands let us controll our ESP32 true a serial line. Below a list of commands you can use in the terminal window. If you dont know how to connect to your ESP32 in Linux i sugest you read my guide: [Connect ESP32 with Linux](connect-esp32-with-linux).
In the Terminal window you should see a prompt `:>` if you don't see this press Enter. I repeat the terminal in the examplles. First i show the command followed with the reply the ESP32 gives.

### SSC Commands

#### help
The help command refers to the command list file. Unfortunaly the official Esperiff document is no longer online.
```
:>help

supported command:

Please refer to document ssc_commands.xlsx
```

#### op
The op command query or set the module.
The synthax looks like `op <parrameter> <mode>`
The parrameter's are:

`-Q` Query Wi-Fi mod. This command returns the current state of the module.

`-S` Set Wi-Fi mode folowed by the <mode>. This command set the module in the wanted state.
Mode options are:

`-o 1`=STA mode
  
`-o 2`=AP mode

`-o 3`=STA+AP mode
```
:>op -Q

+CURMODE:2

+MODE:OK

:>op -S -o 1

mode : sta(30:ae:a4:80:0c:90)

+MODE:OK

+WIFI:AP_STOP

+WIFI:STA_START
```


More to come...

[back](./)

{% include disqus.html %}
