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
| `Tempo.dmg` | the signed, notarised build the button downloads |
| `latest.json` | what the app's update check reads |
| `CNAME` | tells GitHub Pages the custom domain |

## Releasing a new version

From the Tempo repository:

    git tag -a v1.1 -m "Tempo 1.1"                   # the version lives in the tag
    ./notarize.sh                                    # signs and notarises
    ./Tools/release-feed.sh "1.1" "What changed." > site/latest.json
    ./Tools/publish-site.sh --push

`latest.json` reads its build number off the app that was just built, and refuses
if that build is not from the current commit, so the announcement always
describes the download sitting beside it. The file is named `Tempo.dmg` with no
version in it, so this page's download link never has to change.

## If the disk image gets big

It is committed here, which puts about 8 MB into the history per release. That is
fine for a handful and less fine for fifty. The move at that point is GitHub
Releases and an absolute link in `index.html`, which keeps binaries out of git
entirely.
