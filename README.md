# Calendars and Scheduling

The common scheduling vocabulary -- calendars, the events on them, the bookings people make, the availability they are made against, and the kinds of appointment on offer -- distilled from the APIs that already exist. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Scheduling, Calendar in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| calendar | 13 | 12 | id 67%, name 50% | calendars 7, calendar 6 |
| calendar_event | 9 | 9 | description 89%, attendees 78%, id 78%, location 78%, end_at 67%, start_at 67%, name 56%, recurrence 56% | events 9, event 3 |
| appointment | 29 | 26 | id 88%, start_at 69%, status 62%, end_at 58% | appointments 16, bookings 12, meetings 4, booking 2 |
| availability | 13 | 11 |  | slots 5, availability 5, available_slots 2, availabilities 2 |
| service | 15 | 15 | id 93%, name 87%, description 60%, duration 53% | services 6, appointment_types 4, event-types 2, appointment-types 2 |

Cohort: 112 providers with harvested OpenAPI, 38 crosswalks. Of the providers exposing a calendar: 38% create, 92% list, 31% fetch, 31% update, 23% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- Calendar events agree more than anything else here: description, attendees, location, start and end, and recurrence are all core -- the iCalendar shape every calendar API inherits.
- Availability barely agrees: only end_at reaches half of the providers that expose it. Some return free/busy blocks, some bookable slots, some opening hours.
- The cohort is thin for its size (112 tagged providers, 37 mapped). Most scheduling products publish no OpenAPI of their own.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- The Booking tag is not used: it is mostly travel booking, which has its own context.
- An appointment is `/appointments`, not `/bookings`. Appointments lead 16 providers to 14, and the word collides with nothing in other contexts.
- A calendar event is `/calendars/{calendar_id}/events`. A bare `/events` is a webhook feed or a ticketed event elsewhere, so here it counts only at a provider that also exposes calendars (`requires: [calendar]`).
- What can be booked is a `service` (`/services`), covering Calendly's event types and Acuity's appointment types. `services` leads (7) and is the plain word.
- Infrastructure `/services` (Nomad, Cronitor) are excluded by name.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
