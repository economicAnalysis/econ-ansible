######Ansible explained

Quickstart code

```
docker run -d -P --name {{ ident }}_econapp -v /econ/econ-server:/src/
```

This works by using the `-v` flag to mount a host directory `/econ/econ-server`
into the container at `/src/`. The Dockerfile expects to find the `server.js`
file in the `/src` directory. 