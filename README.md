# ISFE event JSON

Daily event data for the ISFE event sign-in / sign-out survey.

**Generated. Do not edit by hand.** Every file under `events/` is compiled from the semester master workbook by `tools/publish_events.py` in the `qualtrics` project. A hand edit here is overwritten by the next publish and leaves no trace of who changed what.

## Layout

```
events/<YYYY-M-D>.json
```

**The date is UNPADDED** — `2026-9-22.json`, not `2026-09-22.json`. The survey requests the file by piping `${date://CurrentDate/DS}`, which produces an unpadded date. A zero-padded filename is never requested and would silently serve nothing, on every single-digit month or day.

## Shape

```json
{
  "date": "2026-9-22",
  "generated": "2026-09-22T07:06:52",
  "events": [
    {
      "occurrence_id": "2026_09_22_01",
      "event_name": "Book Discussion (How to Think About the Economy — Tuesdays)",
      "location_code": "gab_435",
      "window_open": "07:00",
      "start_time": "10:30",
      "end_time": "11:45",
      "window_close": "12:20",
      "assigned_readings": "YES",
      "reading": "Chapter 1",
      "extra_credit": []
    }
  ]
}
```

`window_open` and `window_close` decide **which** event a scan resolves to and are deliberately wider than the event itself. `start_time` and `end_time` are the official times, used for the on-time determination. They are not the same thing.

`extra_credit` lists the instructor ids offering extra credit for that event. Empty is the common case, and an empty list means the survey asks nothing about it.

## What is deliberately absent

**No coordinates.** The runtime geofence was removed: the survey captures raw GPS and presence is computed afterwards, against a better anchor than existed on the day. So no venue anchors are published here.

**No student data, ever.** These files describe events. The publisher emits an explicit allowlist of fields, so a new bookkeeping column in the master cannot leak into a public file.

## Why it is public

Qualtrics fetches this server-side through a Web Service element. A private URL would need a credential embedded in the survey definition, which is exportable by anyone with survey access. Public with no sensitive content is the safer arrangement.
