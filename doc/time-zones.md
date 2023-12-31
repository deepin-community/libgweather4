Notes on assigning `<tz-hint>`s
-----------------------------

The `<tz-hint>` field in `Locations.xml` is used by GNOME Clocks to
guess the correct time zone that goes with a particular location. As
the tag name suggests, it is just a hint; the user could be allowed
to override it if the guess is wrong. Still, it is nice to have
them be as close to correct as possible.

The timezone names come from the tzdata database; you should have a
complete list of the current timezones names in
`/usr/share/zoneinfo/zone.tab`. If you are going to be figuring out the
timezones for a region, it may also be useful to grab the source data
from ftp://elsie.nci.nih.gov/pub/. (The `tzdataXXXXX.tar.gz` file)

The most important thing to realize about tzdata is that it has a separate
time zone name for every region that has had its own distinct timezone rules
*at any point since 1970* (the start of UNIX `time_t`).  This means that
many of the timezones listed are no longer in use and can mostly be ignored.
Eg, `zone.tab` lists 11 timezones for Argentina, even though as of 2008 all
of Argentina is in the same timezone.

In the cases where tzdata has more timezones for a country than the
government of that country recognizes, `Locations.xml` tries to pick one
tzdata timezone to correspond to each government-defined timezone, and uses
those timezones throughout the country rather than using the
historically-more-specific ones. (This will make it easier to localize the
names of the timezones in the future.) So, eg, in the mainland United
States, the four official timezones (Eastern, Central, Mountain, and
Pacific) are mapped to "America/New\_York", "America/Chicago",
"America/Denver", and "America/Los\_Angeles", respectively. Regions that
have switched from one timezone to another in the past (such as parts of
Kentucky and Indiana) are simply listed according to whichever timezone they
are *currently* in, rather than picking an appropriate "micro-timezone" such
as "America/Indiana/Indianapolis".

Finally, the names of timezones will occasionally change between releases of
tzdata. (Eg, "Asia/Calcutta" was recently renamed to "Asia/Kolkata" to match
the new preferred spelling of that city.) `Locations.xml` should always use
the most recent names, because distributions should always be shipping the
most recent tzdata, to ensure that the daylight-savings-time rules for
different countries match the latest government decrees.
