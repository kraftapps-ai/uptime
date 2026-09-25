# uptime

An external check of https://app-essentials.kraftapps.ai/readyz, run by
GitHub Actions every 5 minutes from GitHub's runners (SE-386). The workflow
is `.github/workflows/uptime.yml`; the operations notes are in the
store-essentials repo, `docs/operations/uptime.md`.

A run fails when the answer is not 200, when `ready` is not true or when
`database` is not "ok", after one retry 30 seconds later. A failed run is an
outage.

## Who gets the email

GitHub sends a failed scheduled run to one person: whoever last changed the
`cron:` line in the workflow. Watching this repository does not add you.
That person needs, at https://github.com/settings/notifications, under
"System" then "Actions", "Email" ticked and "Only notify for failed
workflows" on.

## What GitHub does not promise

Scheduled runs can start late under load, and some are dropped. A gap of 10
to 15 minutes between runs is GitHub, not the app. This is a free, coarse
alarm, not an SLA measurement.

## Why this repository is public

Standard runners are free in public repositories. In a private repository
288 runs a day, each billed as a whole minute, come to about 8,640 minutes a
month against the organisation's shared allowance, and the organisation's
Actions budget is $0 with "stop usage", so the monitor would stop all of the
organisation's workflows when the allowance ran out. Nothing here is secret:
the endpoint is public and the workflow uses no secrets.

GitHub disables scheduled workflows in a public repository after 60 days
without activity, so a weekly job re-enables it through the API.

## Run it by hand

```
gh workflow run uptime.yml -R kraftapps-ai/uptime
gh run list -R kraftapps-ai/uptime -L 5
```
