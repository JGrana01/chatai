# chatai

chatai will start an interactive "chat" session with either Googles Gemini AI or Anthropics Claude (or both at once) bots.
chatai also supports an analyze mode for files such as small shell scripts, log files etc.

## Installation

For Asuswrt-merlin based routers running Entware, using your preferred SSH client/terminal, copy and paste the following command, then press Enter:

/usr/sbin/curl --retry 3 "https://raw.githubusercontent.com/JGrana01/chatai/master/chatai" -o "/jffs/scripts/chatai" && chmod 0755 /jffs/scripts/chatai && /jffs/scripts/chatai install

Before actually using chatai for the first time, you will need to get an API_KEY from Google and/or Anthropic.

Get the API key for Googles Gemini from https://aistudio.google.com/app/apikey

![image](https://github.com/JGrana01/genailogs/assets/11652784/e0b13ae5-cb94-405c-842f-9acf43c63056)

Get the API key for Anthropics Claude by going to this website:

https://console.anthropic.com/login



Edit the /jffs/addons/chatai/chatai.conf file and put the api key string in the line:
```
API_KEY="Put your API key here"
```

## Usage

```
chatai (Ver 0.1.1) - start an interactive chat session with Googles Gemini AI

Usage: chatai [log] [show] [help] [install] [uninstall] [update]

        log - log the session for later viewing
        show - show all the saved chat log sessions
        help - show this message
        install - install chatai and create addon dir and config file
        uninstall - remove chatai and its directory, chatlogs and config file
        update - check for and optionally update

```
Typical usage is to just start chatai:

```
$ chatai
```
When chatai begins, it shows you in chat mode and mentions to enter "q" when done:

```
GeminiAI Chat mode. Enter a line of text and get a response.
    Enter q to exit this mode

chat>
```

Ask questions and get (typically) a response in bold

Chatai supports a logging function where it will save a number of chat sesstions in a log file for later viewing. The number of chat sessions saved is configurable (NUMLOGS) in the /jffs/addons/chatai/chatai.conf file.
You can also change the location of the chat session logs to a different directory (LOGDIR), again by editing /jffs/addons/chatai/chatai.conf.
To enable logging for a session, use the "log" argument:

```
$ chatai log
```

To view the chat session, you can use any editor or text viewer to look at the logs/session stored in /jffs/addons/chatai/logs. The files will have the date and time when the chat session started, like this:
```
chatlog_2024-04-09-12-05-14
```

As new chat logs are created, chatai will remove the earlist logs to keep the number of files to NUMLOGS. This is configurable.

There is also an argument (show) that will display all the chat sessions (using "more) from the earlist to the most recent.

Here is a typical /jffs/addons/chatai/chatai.conf file:
```
#
# chatai conf file
#

API_KEY="AIzaSyB1y4Nn4rwrSVxH0a3KEXXXXXXXXXXXXXX"          # Put here ;-)
LOGDIR="/jffs/addons/chatai/logs"                          # location to store chat log sessions
NUMLOGS="5"                                                # number of log sessions to save for later viewing

```
Following good Asuswrt-merlin Addon methods, chatai has install, uninstall and update functions.

Install - will install chatai in /jffs/scripts directory with a symbolic link to /opt/sbin. It also will download any additional applications from Entware that it requires (jq and fold).

A default chatai.conf file is created in /jffs/addons/chatai.

Unistall removes chatai, the symbolic link and the /jffs/addons/chatai directory (and all logs).

Update will query the latest version on github and offer to download and install it.



