# Alert Manager

## Setup

- Add alert manager config

    ```
    global:
      resolve_timeout: 5m
    
    route:
      group_by: ['alertname']
      group_wait: 10s
      group_interval: 10s
      repeat_interval: 1h
      receiver: default
    
    receivers:
      - name: default
    ```
    
    [alertmanager.yml](alertmanager.yml)

    refer this link for complete config https://prometheus.io/docs/alerting/latest/configuration/

- Add alert manager in compose

  ```
  alertmanager:
      image: prom/alertmanager
      container_name: alertmanager
      command:
        - '--config.file=/etc/alertmanager/alertmanager.yml'
        - '--storage.path=/alertmanager'
      ports:
        - 9093:9093
      restart: unless-stopped
      volumes:
        - ./prometheus/alertmanager:/etc/alertmanager
  ```

    [compose.yaml](../../compose.yaml)

- Run compose up
  
- Validate the UI
        
  http://localhost:9093/#/alerts
  
  ![am.png](am.png)

## Email Receiver

- Add new receiver for gmail

```
# A list of notification receivers.
receivers:
  - name: default
  - name: email-gmail
    email_configs:
      - smarthost: 'smtp.gmail.com:587'
        to: "arut.selvan15@gmail.com"
        from: "arut.selvan15@gmail.com"
        auth_username: "arut.selvan15@gmail.com"
        auth_password: ""  # create gmail app password and use here
```

[alertmanager.yml](alertmanager.yml)

- Add route matcher

Any alert match the label "severity=warning" will be notified.  You can have multiple receivers with different conditions.

```
  # custom receivers
  routes:
    - receiver: email-gmail
      matchers:
        - severity="warning"
    - receiver: email-gmail
      matchers:
        - severity="high"
```

[alertmanager.yml](alertmanager.yml)

- Restart compose
- Validate alert in alert manager
    ![gmail-alert.png](gmail-alert.png)
- Validate email
    ![gmail-notify.png](gmail-notify.png)