##

##Useful details
The name of the application is econapp-latest. The application is composed
of two Docker containers. 

####Docker network ports
######Defaults
By default if you include the `-P` flag docker will map ports on the Docker
container to high ports (49153-65535) on the host. E.g. for a webapp that listens on port
5000, you might see the port the webapp is listening on 5000, mapped to high
port 49155 on the host machine. `-P` is a shortcut for '-p 5000'.

######Configure our own mappings
We can also configure our own mappings. If we want to map port 5000 inside
the container to port 5000 on the host we would include `-p 5000:5000`. This in
the form `-p 127.0.0.1:$HOSTPORT:$CONTAINERPORT`

In our case we're running command 
```
"docker run -d --name {{ ident }}_econapp --link {{ ident }}_db_mongo:db -p {{ port_db }}:5000 -p {{ port_db_ssh }}:22 econ/econapp-latest"
```
e.g. We're mapping port `5000` to port 50001 on the host machine. Note that the
webapp listens on port `5000` and we expose port `5000` in the Dockerfile. 
We're also mapping port `22` (container) to port 50002 on the host machine. 
This should allow us to ssh into the Docker container.


######Finding the public port
If we want to find what port we've mapped to on our host machine we can use

`sudo docker port nostalgic_morse 5000`

####Linking

The link command takes the form `--link name:alias`

The link command makes the named container available to the newly created
continer

In this case we run

```
"docker run -d --name {{ ident }}_econapp --link {{ ident }}_db_mongo:db -p {{ port_db }}:5000 -p {{ port_db_ssh }}:22 econ/econapp-latest"
```
so the link is `sample_db_mongo:db`. The name is `sample_db_mongo` and the 
alias is `db`. `db` is then available in our newly created container 
`sample_econapp`.

######Quickstart explained

Data volumes are using the `-v` flag. For example 

```
sudo docker run -d -P --name web -v /webapp training/webapp python app.py
```

this will create a data volume inside the container at `/webapp`.

In our case we want to mount a host directory inside the container. 

The data volume code in the quickstart-app.yml

```
docker run -d -P --name {{ ident }}_econapp -v /econ/econ-db:/src/
```

Here we're mounting a directory from the host (i.e. Vagrant) in our container
at the location `/src`. Remember that we mount the directory `/econdev` on our
Max into Vagrant at `/econ` (this is in the Vagrantfile)

######Cheat sheet
`-d`: daemonize (i.e. run as background process)