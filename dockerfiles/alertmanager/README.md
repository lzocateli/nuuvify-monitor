# AlertManager-Alpine

## To run this image, execute:
```
# docker container run -d --name alertmanager -p 9093:9093 lzocateli/alertmanager_alpine
```

Open http://your_ip:9093 to access the interface of AlertManager.

## To view the logs, execute:
```
# docker container logs -f <container_id>
```

## To create a swarm service, execute:
```
# docker service create --name alertmanager -p 9093:9093 lzocateli/alertmanager_alpine
```

PS: Edit /etc/alertmanager/config.yml to put your Microsoft Teams / Slack's config.

## To set up Microsoft Team to receive notifications

Official doc: https://learn.microsoft.com/en-us/microsoftteams/platform/webhooks-and-connectors/how-to/connectors-using?tabs=cURL#send-a-message-through-incoming-webhook-or-connector-for-microsoft-365-groups

