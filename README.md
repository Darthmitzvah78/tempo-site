# tempo.jacob.fyi

The download page for Tempo. **Do not edit anything here directly** - it is a
delivery target. The page is authored in the Tempo repository under `site/`, and
`Tools/publish-site.sh` copies it across, so anything changed here is overwritten
the next time that runs.

## What is in it

| | |
|---|---|
| `index.html`, `style.css` | the page. Vanilla CSS on the `vichy` palette |
| `*.gif`, `*-still.png` | the walkthroughs, recorded by `Tools/help-gifs.sh` |
| `Tempo.dmg` | the signed, notarised build, kept here only for builds up to 249 |
| `latest.json` | what the app's update check reads, including where the image is |
| `CNAME` | tells GitHub Pages the custom domain |

## Releasing a new version

From the Tempo repository:

    git tag -a v1.1 -m "Tempo 1.1"                   # the version lives in the tag
    ./notarize.sh                                    # signs and notarises
    ./Tools/release-feed.sh "1.1" "What changed." > site/latest.json
    ./Tools/publish-site.sh --push

`latest.json` reads its build number off the app that was just built, and refuses
if that build is not from the current commit, so the announcement always describes
the download it links to.

## Where the download actually lives

Since build 251 the image is a **release asset on this repository**, and that is
what the page's button points at, through the `/releases/latest/download/` alias
so the link never has to change.

The reason is counting. A file served by Pages is fetched with no record kept
anywhere you can see: no logs, no counter, and the app sends no telemetry by
design. A release asset carries a `download_count`, which counts requests and
nothing else - no cookie, no script on the page, nothing about who asked.

    ./Tools/downloads.sh        # per release, and the total

It counts requests, not people: one person downloading twice is two, and each
release includes one fetch made by `publish-site.sh` itself, which checks the
published image against the built one. `downloads.sh` says so rather than
quietly subtracting it.

`latest.json` names the exact release it announces, not the latest alias, so a
copy that checks the feed a month from now is handed the build it was told about.

## The copy of the image still in this repository

`Tempo.dmg` is also published beside the page, and only for the builds that came
before any of this. Up to build 249 the app's own download button fetched
`https://tempo.jacob.fyi/Tempo.dmg`, and those copies will ask for that address
for as long as they run; 251 onwards is told where to look by `latest.json`.

Once no old copy is in use, drop the `cp` in `publish-site.sh` and this repository
stops taking 10 MB of history per release. Those downloads are not counted, which
is the other reason to retire it.
