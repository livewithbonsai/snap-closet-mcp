# Snap Closet connector

Plugin manifests that connect Claude and ChatGPT to a [Snap Closet](https://snap-closet.com)
account over MCP. One remote server, `https://mcp.snap-closet.com/mcp`, serves every client;
this repository only packages it. After you sign in, the assistant can look through the garments
you keep in Snap Closet, show them as cards, suggest outfits made only from clothes you own, put
an outfit on the app's calendar, log what you wore, read the closet diagnosis, and add new
garments from photos. No gateway or app code lives here.

> **Status:** the service is in preparation. Until `https://mcp.snap-closet.com/mcp` is live,
> the steps below register the connector but it will not connect.

## What it can do

| Ask for | What happens |
| --- | --- |
| Help | Explains what the connector is, what the assistant receives and how to get started |
| Account | Your plan, closet capacity, remaining photo matches and credits |
| Overview | How your closet is distributed by category, colour and season |
| Search | Finds garments by category, colour, pattern, season, occasion, style or your own tags |
| Details | Full details of chosen garments, with the images you saved shown in a card |
| Show | Displays any grouping of your garments as rows of cards, for example outfits the assistant composes |
| Outfits | Your worn, planned and saved outfits from the app's calendar |
| Diagnosis | The closet diagnosis: gaps, garments with few partners, duplicates and new combinations |
| Suggestions | Outfit suggestions made only from garments you own, for an occasion, a temperature range or a wish |
| Add | Adds garments from photos; the server removes the background and tags them as the app does |
| Similar | Checks whether you already own something like a photographed garment |
| Upload link | A short-lived link to upload photos from your phone or computer |
| Plan | Puts an outfit on a future day of the app's calendar |
| Log | Records what you wore on a day |

Garment images appear only inside the tool card; the assistant never receives image links.

## Install

### Claude (web, desktop, mobile)

Open the prefilled connector form and select **Add**, then sign in to Snap Closet when asked:

`https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=Snap%20Closet&connectorUrl=https%3A%2F%2Fmcp.snap-closet.com%2Fmcp`

Without the link: **Customize > Connectors > Add custom connector**, name `Snap Closet`,
URL `https://mcp.snap-closet.com/mcp`.

### Claude Code

As a server (then `/mcp` in a session to sign in):

```bash
claude mcp add --transport http snap-closet https://mcp.snap-closet.com/mcp
```

As a plugin from this repository (your Git credentials must be able to read it):

```bash
claude plugin marketplace add zhuduji-tech/snap-closet-mcp
claude plugin install snap-closet@snap-closet
```

### ChatGPT (web, desktop, mobile)

1. Open [chatgpt.com/plugins](https://chatgpt.com/plugins). If the option in step 2 is missing,
   turn on Developer mode first: **Settings > Security and login > Developer mode**.
2. Select the plus button, then **Add custom MCP server**.
3. Name `Snap Closet`, URL `https://mcp.snap-closet.com/mcp`.
4. Review the warning and continue, then **Create as a plugin**. Sign in to Snap Closet when asked.

The plugin then shows up in the ChatGPT mobile apps signed in to the same account.

When ChatGPT asks before a tool that changes your closet (adding garments, logging an outfit),
choose **Allow once**. **Always allow** currently does nothing for tools that render a card; it is
a ChatGPT-side issue, and either way the choice only lasts for the current conversation.

### Codex

As a server:

```bash
codex mcp add snap-closet --url https://mcp.snap-closet.com/mcp
codex mcp login snap-closet
```

As a plugin from this repository:

```bash
codex plugin marketplace add zhuduji-tech/snap-closet-mcp
codex plugin add snap-closet@snap-closet
```

## Privacy

Everything goes through `mcp.snap-closet.com` with your own Snap Closet sign-in; the plugin
files store nothing and carry no credentials. The assistant receives garment tags and the
processed garment images, never photos of people. A photo you upload to find a similar garment
is deleted once the garment has been cut out of it. Policy:
[snap-closet.com/privacy](https://snap-closet.com/privacy). Questions and account requests:
support@snap-closet.com.

## Layout

```
.claude-plugin/marketplace.json   Claude marketplace for this repository -> ./claude
.agents/plugins/marketplace.json  OpenAI marketplace for this repository -> ./codex
claude/                           Claude plugin: .claude-plugin/plugin.json, .mcp.json
codex/                            OpenAI portable plugin: plugin.json, mcp.json, .app.json, assets/
```

Version `0.1.0` is set in four places: `claude/.claude-plugin/plugin.json`,
`.claude-plugin/marketplace.json`, `codex/plugin.json` and `.agents/plugins/marketplace.json`.
Change them together.

## License

MIT, see [LICENSE](LICENSE).
