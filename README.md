# Docker Registry Test Instance

* Install virtualbox
* Install vagrant
* startup the machine

```
vagrant up
```

## Credentials

Default credentials

```
Username: testuser
Password: testpassword
```

To create further credential sets, just enter the machine and use the htpasswd command

```
vagrant ssh
htpasswd -bnB __USERNAME__ __PASSWORD__ >> /vagrant/htpasswd_backup/htpasswd
```

## Test the registry

```
docker login registry.lokal
docker pull alpine
docker tag alpine registry.lokal/testuser/alpine
docker push registry.lokal/testuser/alpine
```

Open the Docker Registry UI, to see your newly created image.

* http://registry.lokal/

## Fixing insecure registries

Highly likely, you will get an error response, when trying to login to the registry.

```
Error response from daemon: Get "https://registry.lokal/v2/": dial tcp 192.168.56.66:443: connect: connection refused
```

run "docker info" to have a look at the "Insecure Registries".

```
Client:
 Version:    24.0.5
 Context:    default
 Debug Mode: false
...
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
```

To work around that problem, create the file /etc/docker/daemon.json and add the following content:

```
{
    "insecure-registries" : [ "registry.lokal" ]
}
```

Afterwards reload and restart the docker service

```
sudo systemctl daemon-reload
sudo systemctl restart docker
```

When running "docker info" again, you should now find the added insecure registry

```
Client:
 Version:    24.0.5
 Context:    default
 Debug Mode: false
...
 Insecure Registries:
  registry.lokal
  127.0.0.0/8
 Live Restore Enabled: false
```
