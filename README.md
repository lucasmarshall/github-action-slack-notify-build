# Slack Notify Build

This action prints your GitHub Action build status to Slack. It takes an opinionated approach by showing attachments for metadata like branch, pull request, and event. Heavily-inspired by [Post Slack messages](https://github.com/marketplace/actions/post-slack-message).

A [Slack bot token](https://api.slack.com/docs/token-types) is required to use this action, and the associated app must be granted permission to post in the channel, private group or DM you specify.

## Usage

```yaml
uses: voxmedia/github-action-slack-notify-build@master
with:
  channel: app-alerts
  status: STARTED
  color: good
env:
  SLACK_BOT_TOKEN: ${{ secrets.SLACK_BOT_TOKEN }}
```

The Slack notification leverages attachments to group important information together and provide valuable links:

<img src="docs/push.png" alt="Screenshot of the push event" width="540">

When used with the `pull_request` event, a link to the originating pull request is included:

<img src="docs/pr.png" alt="Screenshot of the pull_request event" width="540">

### Reporting Success or Failure

You can use the `success()` and `failure()` conditional checks within your workflow to determine which build notification to send:

```yaml
- name: Run tests
  # ... your test step here

- name: Notify slack success
  if: success()
  env:
    SLACK_BOT_TOKEN: ${{ secrets.SLACK_BOT_TOKEN }}
  uses: voxmedia/github-action-slack-notify-build@master
  with:
    channel: app-alerts
    status: SUCCESS
    color: good

- name: Notify slack fail
  if: failure()
  env:
    SLACK_BOT_TOKEN: ${{ secrets.SLACK_BOT_TOKEN }}
  uses: voxmedia/github-action-slack-notify-build@master
  with:
    channel: app-alerts
    status: FAILED
    color: danger
```

## Inputs

### `channel`

**Required** The ID or name of the channel to post the message to.

### `status`

**Required** The status to show for the action, e.g. `STARTED` or `FAILED`.

### `color`

The color to use for the notification. Can be a hex value or any [valid Slack color level](https://api.slack.com/docs/message-attachments#color) (e.g. `good`). Defaults to `#cccccc`.

## Setup

To use this GitHub Action, you'll need a [Slack bot token](https://api.slack.com/docs/token-types). A bot token must be associated with a Slack app.

### Creating a Slack App

1. **Create a Slack App.** Go to Slack's developer site then click "Create an app". Name the app "GitHub Action" (you can change this later) and make sure your team's Slack workspace is selected under "Development Slack Workspace".
1. **Add a Bot user.** Browse to the "Bot users" page listed in the sidebar. Name your bot "GitHub Action" (you can change this later) and leave the other default settings as-is.
1. **Set an icon for your bot.** Browse to the "Basic information" page listed in the sidebar. Scroll down to the section titled "Display information" to set an icon.
1. **Install your app to your workspace.** At the top of the "Basic information" page, you can find a section titled "Install your app to your workspace". Click on it, then use the button to complete the installation.
