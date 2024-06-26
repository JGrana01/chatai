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

Setup a user account using your email address, then go to your Dashboard and select API Keys

![Anth1](https://github.com/JGrana01/chatai/assets/11652784/987c06ef-f66a-4096-bade-d503aed5876b)

![Anth2](https://github.com/JGrana01/chatai/assets/11652784/fccfe3b0-3232-43b8-84eb-9c757935c730)

Edit the /jffs/addons/chatai/chatai.conf file and put the api key string in the line:
```
GAPI_KEY="Put your API key here"        # Google Gemini API key
CAPI_KEY="Put your API key here"        # Anthropics Claude API
```

You only need one of the AI API keys for chatai to work. It depends on which AI bot you want to converse with.
Googles Gemini is the default. This can be overriden either in the command line or in the chatai.conf file.
If you want answers from both, you will need both companies API key.

## Command and Command Line Arguments

```
chatai (Ver 0.2.2) - start an interactive chat session with Googles Gemini or Anthropics Claude AI

Usage: chatai [gemini|claude|both] [analyze filename] [log] [show] [help] [install] [uninstall] [update]

        gemini - use Googles Gemini for chat (default)
        claude - use Anthropics Claude for chat
        both - use both AI and display the results in different color output
        analyze filename - analyze the suppled filename contents
        log - log the session for later viewing
        show - show all the saved chat log sessions
        help - show this message
        install - install chatai and create addon dir and config file
        uninstall - remove chatai and its directory, chatlogs and config file
        update - check for and optionally update

```

## Command Line Options

**gemini** - use Google Gemini for the chat session (overrides .conf file WHICHAI setting)

**claude** - use Anthropics Claude for the session (overrides .conf file WHICHAI setting)

**both** - show answers from both AI bots. Geminis output will be cyan and Claude in blue (overrides .conf file WHICHAI setting)

**analyze** _filename_ - rather then start a chat session, send _filename_ to AI for analysys.

**log** - record a session of the chat session. This will show the questions and responses. The files at date/time stamped and stored in /jffs/addons/chatai/logs.

**show** - display all the latest log sessions (using the Linux "more" command)

**help** - show a help screen

**install** - install chatai, create the addons directory and create a chatai.conf file

**uninstall** - remove chatai and its directories and files (including log files!!!)

**update** - check github for a new version. If so, offer to install it.

Here is a typical /jffs/addons/chatai/chatai.conf file:
```
#
# chatai conf file
#

GAPI_KEY="AIzaSyB1y4Nn4rwrSVxH0a3KE7_XXXXXXXX8"          # Put Google Gemini API key here ;-)
CAPI_KEY="sk-ant-api03-PQGJE_F-LlsovCnChPXBGag_sioN8aX8JAkaM2-XyTFgZq4kZnTp8zFY-p6G6kCUE7wytdyoZXXXXXXXXXXw-z6CFJgAA"          # Put Anthropics Claude
WHICHAI="0"                         # which ai bot - 0: Gemini 1: Claude  2: both
LOGDIR="$SCRIPTDIR/logs"            # location to store chat log sessions
NUMLOGS="5"                          # number of log sessions to save for later viewing
MAXLINES="200"                      # Maximum number of lines in a file to analyze
                                    # use cautin in making it too large...
MAXSIZE="32767"                     # Maximum number of characters in any request

```
_GAPI_KEY_ - the key created by you when you asked for a Gemini API key
_CAPI_KEY_ - the key created by you when to setup Antropics Claude developer
_WHICHAI_ - set to 0 (default) for Gemini, 1 for Claude or 2 to have both respond to your questions or analysis
_LOGDIR_ - where you want chatai to store log files of the session. Default is in the /jffs/addons/chatai/logs
_NUMLOGS_ - the number of log files to store at a time. Its a circular method - a new log file overrides the oldest
_MAXLINES_ - the maximium number of lines of text to send. This can be increased - but should be cautiously - more lines - more tokens used.
_MAXSIZE_ - this is the maximium number of characters to send. Again, can be increased - more dependent on shell variable length :-)

## Usage

Typical usage is to just start chatai:

```
$ chatai
```
When chatai begins, it shows you in chat mode, which AI bot and mentions to enter "q" when done:

```
GeminiAI Chat mode. Enter a line of text and get a response.
    Enter q to exit this mode

chat>
```
Ask questions and get (typically) a response in either cyan if using Gemini or blue if Claude

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

There is also an argument (show) that will display all the chat sessions (using "more") from the earlist to the most recent.

Commands line arguments can be combined. For example, to have both bots answer your questions (or analyze your _file_) and log the session:

```
$ chatai both log
```

There is a **special mode** while in chat - analyze while in chat. To invoke this mode, while at the chat prompt, enter just the word "analyze":
```
Enter text and get both responses.

    Enter q to exit this mode

chat> analyze

What type of file to analyze:
  1) text
  2) code
  3) log

(1 or 2 or 3) ?
```
chatai will ask what type of file you plan to have analyzed (makes for more accurate analysis). Enter either 1 (text), 2 (code) or 3 (log)

chatai will then ask for the filename. If it is too large (> MAXSIZE) it will tell you and not send on.
If the file has more lines in it then MAXLINES, chatai will ask if you want it to reduce the number of lines to MAXLINES (using tail).
Note below about special characters, especially with shell scripts. I'm working on solutions...

Once the file is analyzed and displayed, chatai returns to chat mode. Useful for asking more questions about its analysis.

## Notes

Be careful with JSON special characters - especially escape sequences in code (i.e. "\\\n"). chatai attempts to clean up files and questions by
replacing "returns" to "\\n" and escaping double quotes.

Following good Asuswrt-merlin Addon methods, chatai has install, uninstall and update functions.

Install - will install chatai in /jffs/scripts directory with a symbolic link to /opt/sbin. It also will download any additional applications from Entware that it requires (jq and fold).

A default chatai.conf file is created in /jffs/addons/chatai.

Unistall removes chatai, the symbolic link and the /jffs/addons/chatai directory (and all logs).

Update will query the latest version on github and offer to download and install it.



