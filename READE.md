# MQTT Broker
## Docker Compose
```yaml
version: '3.5'

services:

    emos:
        container_name: emos
        image: eclipse-mosquitto
        restart: always
        logging:
            options:
                max-size: 10m
        ports:
            - 1883:1883
            - 1884:1884
        volumes:
            - ./configs:/mosquitto/config
```
```bash
docker compose up -d
```

## Configuration File
The broker configurations are on [mosquitto.conf](configs/mosquitto.conf)

```txt
allow_anonymous true

listener 1883
protocol mqtt

listener 1884
protocol websockets
```

To add authentication modify the following lines
```txt
allow_anonymous false
password_file /mosquitto/config/users
```

## Add User Authentication

Create a text file like this 
```txt
user1:passw1
user2:passw2
...
```

Then encrypt the passwords in the file run
```bash
docker run -v $PWD:/workspace eclipse-mosquitto mosquitto_passwd -U /workspace/configs/users
```

