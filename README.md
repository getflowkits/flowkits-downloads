# FlowKits Downloads

Public download host for FlowKits companion applications. **Releases only — no source code lives here.**

Companion apps are small local helpers that let a FlowKits widget or profile talk to
desktop software on your own machine. They run entirely on your PC.

## Current downloads

- **Xeneon LrC Companion** — the Lightroom Classic plug-in for the FlowKits
  *Lightroom Classic Control* widget on the Corsair Xeneon Edge.
  Setup guide: <https://getflowkits.com/lightroom-icue-companion>

- **Star Citizen Edge Control Companion** — the keystroke helper for the FlowKits
  *Star Citizen Edge Control* widget on the Corsair Xeneon Edge.
  Setup guide: <https://getflowkits.com/setup-companion/star-citizen>

See the [Releases](../../releases) tab.

---

## ⚠️ Release rules — read before publishing

This repository serves **more than one product**, and GitHub's `latest` is **repo-wide**,
not per product. That single fact drives everything below.

### Only the Lightroom companion may be marked "latest"

Its setup page links to a version-agnostic URL:

```
/releases/latest/download/Xeneon-LrC-Companion-Plugin.zip
```

If any other product's release is marked latest, that link resolves to a release which does
not contain that file, and **the Lightroom download 404s for every customer**.

So: publish every other product's release with `--latest=false`.

### Every other product uses a rolling tag

Each gets one release on a fixed tag, re-used for every version, so its download URL is
version-agnostic without touching `latest`:

| Product | Tag | Download URL |
| --- | --- | --- |
| Star Citizen Edge Control | `sc-edge-latest` | `/releases/download/sc-edge-latest/FlowKits-SC-Edge-Companion.zip` |

**The asset filename must never carry a version number**, or the URL stops being stable.
Put the version in the release title instead.

To publish a new version of a rolling-tag product:

```
gh release edit   sc-edge-latest --title "Star Citizen Edge Control Companion X.Y.Z"
gh release upload sc-edge-latest "FlowKits-SC-Edge-Companion.zip" --clobber
```

### One good version at a time

Superseded builds are replaced, not archived. A rolling tag does this by itself; for the
Lightroom companion, delete the old release rather than leaving it published. A customer
who finds an old download and installs it becomes a support case that did not need to exist.

### After publishing

Check the URL actually redirects to the file you just uploaded, for **both** products:

```
curl -sI -o /dev/null -w "%{http_code} %{redirect_url}\n" \
  "https://github.com/getflowkits/flowkits-downloads/releases/latest/download/Xeneon-LrC-Companion-Plugin.zip"
curl -sI -o /dev/null -w "%{http_code} %{redirect_url}\n" \
  "https://github.com/getflowkits/flowkits-downloads/releases/download/sc-edge-latest/FlowKits-SC-Edge-Companion.zip"
```

A release that builds fine and serves the wrong file is indistinguishable from a broken
product, and the only way to know is to ask for the URL a customer would use.
