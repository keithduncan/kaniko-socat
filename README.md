# kaniko-socat

kaniko:debug image with socat binary

Allows binding a unix domain socket for a remote shell into the kaniko container:

```
# Create shared volume
docker volume create --label kanikoctl

# Start server
docker run \
	--rm \
	-it \
	-v $PWD:/workspace \
	-v kanikoctl:/kanikoctl \
	kaniko-socat \
		unix-listen:/kanikoctl/socket,fork,unlink-early \
		'exec:sh -vx,stderr'
    
# Connect client
docker run \
  --rm
  -it \
  -v kanikoctl:/kanikoctl
  alpine/socat
    STDIO,ignoreeof \
		unix-connect:/kanikoctl/socket
    
# Send build command to server shell
executor --no-push
```
