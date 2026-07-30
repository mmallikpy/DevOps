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

### What is a Notification Policy?

```grafana
A notification policy is the routing logic that decides which contact point handles 
which alert. It matches alerts based on their labels and routes them to the appropriate 
contact point, with control over grouping, timing, and repetition.
```

### What is a Notification Template?
```grafana
A notification template controls what the notification message actually looks like — 
the text, formatting, and content structure of the alert notification sent to a contact 
point. Grafana uses Go templating (the same engine as Prometheus Alertmanager) to build 
these
```

### Examples
- Step 1: Create a rule
<img src="Image/Granfan_alert.png" alt="Project Logo" width="600" align="center">
<img src="Image/Granfan_alert_2.png" alt="Project Logo" width="600" align="center">
<img src="Image/Granfan_alert_3.png" alt="Project Logo" width="600" align="center">
<img src="Image/Granfan_alert_4.png" alt="Project Logo" width="600" align="center">

- Step 2: Create the Title "dynamic_title"

```grafana

{{ define "dynamic_title" }}

{{ if eq .CommonLabels.alertname "High_CPU" }}
    {{ if eq .Status "firing" }}
		🔥 HIGH CPU ALERT
    {{ else }}
		🟢 NORMAL CPU ALERT
    {{ end }}
{{ end }}

{{ end }}
```

- Step 3: Create message "telegram_master"

```grafana
{{ define "telegram_master" }}

{{ if eq .CommonLabels.alertname "High_CPU" }}
  {{ template "High_CPU" . }}

{{ end }}

{{ end }}
```

- Step 4: Create template "High_CPU"

```grafana
{{ define "High_CPU" }}

{{ $cpu := (index .Alerts 0).Values.A }}

{{ if eq .Status "firing" }}

Device: {{ (index .Alerts 0).Labels.instance }}{{ "\n" }}
Job: {{ (index .Alerts 0).Labels.job }}{{ "\n" }}
CPU Usage: {{ printf "%.1f" $cpu }}%{{ "\n" }}
Triggered at: {{ (index .Alerts 0).StartsAt }}{{ "\n" }}

Status: FIRING

{{ else }}

Device: {{ (index .Alerts 0).Labels.instance }}{{ "\n" }}
Job: {{ (index .Alerts 0).Labels.job }}{{ "\n" }}
CPU Usage: {{ printf "%.1f" $cpu }}%{{ "\n" }}
Recovered at: {{ (index .Alerts 0).EndsAt }}{{ "\n" }}

Status: RESOLVED

{{ end }}

{{ end }}
```