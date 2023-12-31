## The Locations file format

The format of `Locations.xml` is the following:

```xml
<gweather format="1.0">
  <region>
    <_name>North America</_name>
    <country>
      <_name>United States</_name>
      <iso-code>US</iso-code>
      <state>
        <_name>Alabama</_name>
        <tz-hint>America/Chicago</tz-hint>
        <location>
          <!-- Translators: This is in Alabama in the United States. -->
          <_name>Alabaster</_name>
          <code>KEET</code>
          <zone>ALZ019</zone>
          <radar>bhm</radar>
          <coordinates>33-10-42N 086-46-54W</coordinates>
        </location>
        <city>
          <!-- Translators: This is in Alabama in the United States. -->
          <_name>Mobile</_name>
          <location>
            <!-- Translators: This is in Mobile, Alabama in the United States. -->
            <_name>Mobile Downtown Airport</_name>
            <code>KBFM</code>
            <zone>ALZ061</zone>
            <radar>bix</radar>
            <coordinates>30-36-50N 088-03-48W</coordinates>
          </location>
        </city>
        ...
```

Translatable elements are prefixed with an underscore (`_`) in the
`Locations.xml` file, to allow localization.

Most of the data in the file appears inside `<location>` entries.
However, various larger geographic divisions exist to make things
easier for both users and maintainers.

At the top level are `<region>`s. These mostly correspond to continents,
but not entirely. They are arbitrary, and could be changed in the
future if we wanted.

Each `<region>` is divided into `<country>` elements. Every internationally-
recognized country for which at least one `<location>` is defined should
have its own `<country>`. For the most part, "dependencies",
"territories", "protectorates" and the like are listed as `<location>`s
within their ruling country if they are in the same `<region>`, but
separately if they are in a different `<region>`. This is not followed
100% consistently.

Every `<country>` must have an `<iso-code>` tag giving its ISO 3166-1
alpha-2 code. Sub-country `<location>`s can also specify their own
`<iso-code>` if they have one.

A `<country>` MAY specify a `<tz-hint>`, giving the default time zone name
for the country. Countries that only have one timezone (or where the
majority of the country is covered by a single timezone) should list it at
the `<country>` level. Countries with multiple timezones and no obvious
"default" should not list anything here. (See the [time zones
documentation](./time-zones.md) for more information about time zones in
`Locations.xml`.)

A `<country>` can contain `<city>`s and `<location>`s directly, or can be
split into `<state>`s which contain `<city>`s and `<location>`s. The name
"state" comes from the US states, but it can be used to represent any
sort of well-defined sub-country region that has a name which will be
familiar to local users. This could for example be the primary geographical
or administrative division of a country, like the US State, a Region in France
or a Province in China. A `<state>` may specify a `<tz-hint>` which will
override the `<country>`'s `<tz-hint>` for `<location>`s within the state.

`<city>` is an optional element used to group together multiple
`<location>`s within the same city.

Finally, a `<location>` represents a location for which weather data can
be retrieved. Its fields are:

- `<_name>`: required, the name of the location as a translatable string
- `<iso-code>`: optional, the ISO 3166 code of the location, if not the same
  as its parent `<country>`
- `<tz-hint>`: optional, the timezone of the location, if not the same as
  its parent `<state>` or `<country>`
- `<code>`: required, the METAR code identifying this location
- `<zone>`: optional, secondary weather source information:
  - US: the NOAA IWIN zone
  - UK: the Met Office region name, prefixed with ":"
  - AU: the BOM forecast name, prefixed with "@"
- `<radar>`: optional, the Weather.com radar map name for the location
  (North America only)
- `<coordinates>`: optional, the latitude and longitude of the location, as
  `"[-]ddd.dddddd [-]ddd.dddddd"`:
  - positive values are North and East, respectively
  - negative values are South and West, respectively
