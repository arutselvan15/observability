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