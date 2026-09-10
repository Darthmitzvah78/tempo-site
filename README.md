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
| `Tempo-1.0.dmg` | the signed, notarised build the button downloads |
| `latest.json` | what the app's update check reads |
| `CNAME` | tells GitHub Pages the custom domain |

## Releasing a new version

From the Tempo repository:

    ./notarize.sh                                    # signs and notarises the DMG
    ./Tools/release-feed.sh "1.1" "What changed." > site/latest.json
    ./Tools/publish-site.sh --push

`latest.json` takes its build number from the commit count, the same place the
app's own does, so the announcement and the build it describes cannot drift apart
by being typed in twice.

## If the disk image gets big

It is committed here, which puts about 8 MB into the history per release. That is
fine for a handful and less fine for fifty. The move at that point is GitHub
Releases and an absolute link in `index.html`, which keeps binaries out of git
entirely.
