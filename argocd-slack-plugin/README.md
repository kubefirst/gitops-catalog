# Argo CD Slack Notifications Plugin

## Overview

This plugin enables Slack notifications for Argo CD application events. It configures Argo CD to send notifications to Slack channels when applications are synced, deployed, or experience health issues.

## Features

- Real-time Slack notifications for Argo CD events
- Customizable notification templates with rich formatting
- Support for multiple event types:
  - Application created/deleted
  - Sync started/succeeded/failed
  - Application health degraded
  - Deployment completed

## Required Vault Configuration

The gitops catalog implementation will add these 2 secrets to your Vault,
and bind them to your app using external secrets. For local development you
can add these secrets manually.

```bash
vault kv put secret/argocd-slack-plugin \
  SLACK_TOKEN="xoxb-your-slack-token-here" \
  SLACK_CHANNEL="#deployments"
```

Note: While the channel name isn't sensitive, it's stored in Vault as a user input value.

## Slack Setup

1. Create an Incoming Webhook in Slack
2. Use the webhook URL as the SLACK_TOKEN value

## Configuration

### Default Channel

The default Slack channel is configured via the `SLACK_CHANNEL` value in Vault. All notifications will be sent to this channel by default.

### Custom Subscriptions

You can configure per-application notifications by adding annotations:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  annotations:
    notifications.argoproj.io/subscribe.on-sync-succeeded.slack: "#deployments"
    notifications.argoproj.io/subscribe.on-sync-failed.slack: "#alerts"
```

## Notification Events

The following events trigger notifications:

- `on-created`: Application created
- `on-deleted`: Application deleted  
- `on-deployed`: Application successfully deployed
- `on-health-degraded`: Application health degraded
- `on-sync-failed`: Sync operation failed
- `on-sync-running`: Sync operation started
- `on-sync-succeeded`: Sync operation succeeded

## Customization

### Modifying Templates

Edit the `argocd-notifications-cm` ConfigMap to customize notification templates and add new ones.

### Adding New Triggers

Add new triggers in the ConfigMap following the pattern:

```yaml
trigger.on-your-event: |
  - when: your.condition
    send: [your-template]
```

## Troubleshooting

Check the notification controller logs:

```bash
kubectl logs -n argocd deployment/argocd-notifications-controller
```

Verify the secret is created:

```bash
kubectl get secret -n argocd argocd-notifications-secret
```

Test notifications:

```bash
kubectl exec -n argocd deployment/argocd-notifications-controller -- \
  /app/argocd-notifications template notify app-sync-succeeded <app-name> --recipient slack:<channel>
```
