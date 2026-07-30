# Grafana Alerting

## 4 Things are need for grafana alerting
    - Alert rules
    - Contact points
    - Notification policies
    - Notification Templates

### What is Alert rules ? 

```grafana
An alert rule is a query + condition that Grafana evaluates on a schedule. 
If the condition is met, the rule fires and moves into the Alerting state, 
which then triggers notifications through contact points 
(based on notification policies).
```
#### Alert states
    - Normal – condition not met
    - Pending – condition met, but still inside the "for" duration
    - Alerting – condition met and duration satisfied → notifications fire
    - No Data – query returned no data (configurable behavior)
    - Error – query/evaluation failed (configurable behavior)

### What is a Contact Point?

```grafana
A contact point (formerly called "notification channel") defines where an alert 
notification gets sent — the actual destination and how the message is delivered there.
```
#### Common contact point types (integrations)
    - Email
    - Slack
    - Microsoft Teams
    - PagerDuty
    - OpsGenie
    - Webhook (generic HTTP POST — most flexible, integrates with anything)
    - Discord
    - Telegram
    - VictorOps / Splunk On-Call
    - Google Chat
    - Alertmanager (forward to another Alertmanager instance)