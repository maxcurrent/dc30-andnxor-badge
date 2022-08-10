# RTFM #

So you got an AND!XOR DC30 badge? Here's what you should know...

# License #

Made with beer and late nights in California.

(C) Copyright 2017-2022 AND!XOR LLC (https://andnxor.com/).

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

**ADDITIONALLY:**

If you find this source code useful in anyway, use it in another electronic
conference badge, or just think it's neat. Consider buying us a beer
(or two) and/or a badge (or two). We are just as obsessed with collecting
badges as we are in making them.

**Contributors:**

* @andnxor
* @zappbrandnxor
* @hyr0n1
* @bender_andnxor
* @lacosteaef
* @f4nci3
* @Cr4bf04m
* @5n4ck3y
* @humanrevision
* @kur3us

# Features #

**Hardware:**

* LilyGo T-WATCH 2020V3 (removing Arduino and hacking together a full Micropython v1.18 implemenation)
* ESP32, 16MB flash, 4MB RAM, 80mhz (using one core)
* Wifi and Bluetooth
* Infrared LED
* 240x240 color display (with capacitive touch)
* Nearly 24 hours battery life (rechargable with Micro USB)
* Vibration motor
* Microphone (unused, powered down)

**Applications**

* Watch Faces
* TV-B-Gone
* BLE Scanner
* Ninja
* B.E.N.D.E.R
* Bling
* Example
* RTFM
* Upgrayedd
* About
  

# Power #

To power on or off, hold the button for 4 seconds.

# Micropython #

Your badge has a semi-custom build of Micropython 1.18! We've added a few "frozen" python files, a custom C module display driver (for maximum bling), and optimized ESP32 config settings.

## mpy vs py ##

From: [https://docs.micropython.org/en/latest/reference/mpyfiles.html]

MicroPython defines the concept of an .mpy file which is a binary container file format that holds precompiled code, and which can be imported like a normal .py module. The file foo.mpy can be imported via import foo, as long as foo.mpy can be found in the usual way by the import machinery. Usually, each directory listed in sys.path is searched in order. When searching a particular directory foo.py is looked for first and if that is not found then foo.mpy is looked for, then the search continues in the next directory if neither is found. As such, foo.py will take precedence over foo.mpy.

Compiling python files is easy:

`lib/micropython/mpy-cross/mpy-cross foo.py`

For performance and size .mpy files are preferred, however, .py files as noted above, take precedence. It is a best practice to develop with .py files, test, and when complete compile to .mpy. We have left some code as .py files intentionlly so you can see and modify our code.

# App Framework #

Yes we built our own. Checkout `app_example.py` for full documentation.

## Watch Faces ##

What's a watch without watch faces? You get three types: Digital, Binary, and Analog. Swipe up or down to change the face types.

## TV-B-Gone ##

It works, usually, in some situations. 226 TV codes have been put into it's database. The LED is extremely weak and subject to interference from strong IR sources like the sun. 

## BLE Scanner ##

The tool is built out for you to build upon. Touch to scan for all BLE devices, touch to select the nearest device, then begin to interrogate.

## Ninja ##

A lightweight wireless game inspired by the OG badge hackers, the Ninja's. Scan for nearby...Ninjas to attack. If you succeed, you will level up and your victim's watch will vibrate letting them know of their doom and failure.

## Badge Enabled Non Directive Enigma Routine (B.E.N.D.E.R) ##

Our badge flavour of CTF. Every year is a different challenge, presented in the style of a dungeon crawling text based adventure game. But its not a game, its a hacking challenge. 

We hope you enjoy it, if you happen to complete it then... REDACTED things witll REDACT

This requires use of a serial terminal, we reccomend picocom. Plug the watch in to a computer via USB then...
```
$ picocom -b 115200 /dev/ttyACM0
```

## Bling ##

The watch supports two modes of bling, primary method streaming RGB8 raw files. This has the best FPS performance. Secondarily, the watch will play GIF files. The difference being trading storage for FPS.

Bling is played full screen 240x240. However, to save storage RGB files are encoded at 120x120 and 8-bit color. To convert any video or GIF file to RGB including scaling and letterboxing use the following ffmpeg command:

```
INPUT = bender.gif
OUTPUT = bling_bender.rgb
ffmpeg -i $INPUT -y -vf "scale=120:120:force_original_aspect_ratio=decrease,pad=120:120:(ow-iw)/2:(oh-ih)/2"  -r 5 -f rawvideo -s 120x120 -pix_fmt rgb8 $OUTPUT
```

The bling app is written to autodiscover RGB files with the following format bling_FOO.rgb. Replace FOO with whatever you want. Upload with

```
$ ampy put bling_FOO.rgb
```

NOTE: ampy is very slow to upload. 100k can take a minute or more and it will _not_ output to the screen.

See `app_bling.py` for how to play a raw RGB8 file or to add your own. 

While the watch supports GIF files, they are slow (around 10FPS). Resize your GIF(s) to 60x60 and ensure there is no transparency. See `dance60.gif` for an example that works. To play the GIF use the code below:

```
def cb():
    print("GIF callback :)")
    
watch.display.gif("/dance60.gif", cb)
```

## Example ##

A fully stubbed out example application to teach you the foo bar hello world whatever you want to call it ropes.

Checkout `app_example.py` for full documentation.

Oh yeah to do that...you need a tool and to download it from the watch
```
$ pip3 install adafruit_ampy
$ ampy get app_example.py > app_example.py
```

## RTFM ##

A QR code link to an online version of this manual.

## Upgrayedd ##

We may push new features and bug fixes, with two D's, to resolve  double dose of downtime. The upgrayedd app helps you do this. Ensure the badge has wifi nearby that it likes (see config.json below) and it will update itself and reboot. Note: this will overwrite any changes you might have made to standard issue `.py` and `.mpy` files.

## About ##

Information about the watches current build, firmware details, and thanks to our sponsors: Urbane Security, w00w00, & Philanthropists

# Misc #

## 5n4ck3y (aka Snacky) ##

The newest member of AND!XOR, fully responsible for handing out free badges. Intern 4 life. Look for them in the conference... 

If your summercamp-fam are jealous and want to earn a free badge through hacking challenges, go see 5n4ck3y.

## Microphone ##

The watch contains a microphone. We were not able to come up with any hackery things for it. But it's there. However, to save power and maximize bling we have disabled power to the audio circuit. You will need to enable power in your app to AXP202_LDO4 to use it.

## Wifi ##

DO NOT use the watch to de-auth WiFi. We cannot stress this enough, Las Vegas takes this very seriously. A lifetime ban is the best outcome you could hope for. We want to continue to see our frends in Vegas.

The watch checks NTP periodically to get accurate time. Once 15 minutes after powering on and then every 24 hours after that. If known wifi cannot be found, the watch will try again in 6 hours. Default SSID/PWD is "Matt Damon" / "WEmustSAVEhim" with no quotes. You can change the SSID and PWD in config.json file with the following "ssid" and "pwd" keys.

## config.json ##

Badge settings are handled in the `config.json` file. Below describes each value, a badge update will not overwrite the file. 

  | Setting   | Default       | Description                                 |
  | :-------: | :-----------: | ------------------------------------------- |
  | offset    | -7            | Offset in hours from UTC time, Vegas is -7  |
  | inverted  | false         | Invert the watch (useful for swithing arms) |
  | name      | Ma77D@m0n     | Name for the user of the badge              |
  | wifi_ssid | Matt Damon    | SSID of wifi to use for periodic NTP        |
  | wifi_pwd  | WEmustSAVEhim | WPA password to use for the wifi            |
  | current   | false         | Display the current mA being used           |

## Default config.json file in case you FIU ##

```
{
    "offset": -7,
    "inverted": false,
    "name": "Ma77D@m0n",
    "wifi_ssid": "Matt Damon",
    "wifi_pwd": "WEmustSAVEhim",
    "current": false
}
```

or

```
$ ampy rm config.json
```

## ampy ##
Adafruits micropython tool. You've seen it mentioned half a dozen times and how to get it. Curious how to use it? Super difficult
```
$ ampy
```
