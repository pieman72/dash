dash
===
`dash` is a command-line dashboard for the `bash` shell. It can display local
weather, an email inbox synopsis, system stats, and other data streams, each in
individual panes in the terminal. The size and number of panes adjusts
automatically to fit the content requested and the available terminal screen
space. This works really well with terminal servers like
[`tmux`](http://manpages.ubuntu.com/manpages/trusty/man1/tmux.1.html) and
[`Screen`](http://manpages.ubuntu.com/manpages/trusty/man1/screen.1.html).

Compatibility
---
I test `dash` only on [Ubuntu 14.04 Trusty](http://packages.ubuntu.com/trusty/kernel/linux-image-3.13.0-46-generic).
Though `dash` is written in pretty generic bash script, I make no claims of
compatibility on other systems :-) . A few 'not-totally-standard' packages
(e.g. curl) are needed for some panes. To what extent I know about them, I list
them in the help sections for each pane. If you discover any overlooked
dependencies, please post an [issue](https://github.com/pieman72/dash/issues).
`dash` is really only meant for use with the `bash` shell, however with a few
modifications, it can probably be adapted for use with other shells. If this is
of interest to you, feel free to leave an issue, or by all means, a pull
request!

Installation
---
This repository only has two code files. The executable `dash` script, and the
configuration (`.ini`) file. After downloading these files, he script can be
placed anywhere, but the config file should go in your home directory (much
like a `.bashrc` file would). Next, read the following section on configuration
(in case the default `dash` settings do not match your needs) then create a
useful shortcut to the `dash` script:

    `$ alias bash-dash=/your/path/to/dash`

In the example above, I created a shortcut called `bash-dash`, but you can call
it whatever you like.

Configuration
---
`dash` has a number of configurable settings, which can be set in the config
file `dash.ini`. They are separated into global, display, and specific
settings, and are outlined below:

*General settings* These control the overall behavior of the `dash` script:
* `DBG` ([bit]) - This is a debugging flag. You probably don't want to use it.
* `REFRESH_RATE_SECONDS` ([int]) - The default number of seconds between pane
refreshes. Refreshes are also triggered anytime the screen is resized. Note
that refreshes are somewhat intelligent. Each pane has an individual minimum
time between data fetches, and also checks the current screen size. During a
refresh, panes with current data will not fetch new data, and if the screen
size has not changed, they will reuse their old rendering.
* `SECTION_MIN_COLS` ([int]) - The minimum number of characters-wide a pane
needs to be.
* `SECTION_MIN_ROWS` ([int]) - The minimum number of characters-tall a pane
needs to be.

*Display settings* These change the way `dash` renders content, based on your
system's available settings:
* `DISPLAY_ENCODING` (`UTF8`|`ASCII`) - If UTF-8 encoding is used, output will
display fancy line-drawing and other characters (like &#x2502;, &#x2500;,
&#x253c;, and &#x2744;), otherwise somewhat ugly ASCII substitutes will be used
(like |, -, +, and &#x2a;).
* `DISPLAY_COLOR_PALETTE` (`256`|`16`|`8`|`1`) - The number of terminal colors
available for your display output. Most terminals support at least 16, or have
an [option](https://push.cx/2008/256-color-xterms-in-ubuntu) to enable 256.
Only go monochrome if you really have to...
* `DISPLAY_SEPARATOR_V` ([char]) - Character to use for vertical pane
separations
* `DISPLAY_SEPARATOR_H` ([char]) - Character to use for horizontal pane
separations
* `DISPLAY_SEPARATOR_C` ([char]) - Character to use for pane separation
intersections
* `DISPLAY_SEPARATOR_CA` ([char]) - Character to use for pane separation
intersections with no lower pane (won't occur until 7 panes?)

*Specific settings* These settings are specific to individual panes
_*Email*_
* `EMAIL_SECONDS_PER_POLL` ([int]) - The number of seconds data is considered
to be "fresh". A larger number may mean fewer distractions :-)
* `EMAIL_SOURCE` (`Gmail`) - Currently only Gmail is supported, but I intend to
add other mail sources eventually. If you use Gmail, you will need to [enable
access](#https://www.google.com/settings/security/lesssecureapps).
_*Weather*_
* `WEATHER_SECONDS_PER_POLL` ([int]) - The number of seconds data is considered
to be "fresh". Weather.gov data updates only once an hour, and is subject to a
request limit, so it's advisable to make this number no smaller than 30 minutes
(1800 seconds).
* `WEATHER_NOAA_STATION_CODE` ([string]) - This is your [ICAO code](http://en.wikipedia.org/wiki/International_Civil_Aviation_Organization_airport_code),
and is used to search the weather API. ICAO codes in the USA start with 'K'
followed by the value from the "CALL-ID" column in
[this list](http://www.weather2000.com/1st_order_wbans.txt) (e.g. central park,
New York City gets "KNYC").
* `WEATHER_TEMPERATURE_UNITS` (`F`|`C`) - Get weather temperature readout in
degrees Fahrenheit or Celsius.
_*System Stats*_
* `STATS_SECONDS_PER_POLL` ([int]) - The number of seconds data is considered
to be "fresh". It's generally safe to keep this pretty low.
* `STATS_TEMPERATURE_UNITS` (`F`|`C`) - Get core temperature readout in degrees
Fahrenheit or Celsius.
