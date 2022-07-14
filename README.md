# :fox_face: gitlab-locally

Run locally your gitlab-ce and a gitlab runner with docker.

## :flags: Runnning Gitlab & Runner

To run locally, you must have docker installed.

After this, just run the `docker-compose.yml`.

```shell
docker-compose up -d
```

Check the container health with.

```shell
docker container ps
```

Check your local GitLab in http://gitlab.localhost/ 

## :joystick: Register Runner

To register the gitlab runner into gitlab, run this command and fill the form:

```shell
$ docker exec -it gitlab_runner gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=35 revision=2a70a833 version=15.1.1
Running in system-mode.                            
                                                   
Enter the GitLab instance URL (for example, https://gitlab.com/):
http://gitlab/
Enter the registration token:
<get your token from GitLab > Admin Area > Overview > Runners > Register an instance runner>
Enter a description for the runner:
[d0dd48cad767]: 
Enter tags for the runner (comma-separated):

Enter optional maintenance note for the runner:

Registering runner... succeeded                     runner=5jaZNWRS
Enter an executor: custom, docker-ssh, shell, ssh, docker-ssh+machine, docker, parallels, virtualbox, docker+machine, kubernetes:
docker
Enter the default Docker image (for example, ruby:2.7):
ruby:2.7
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 
```

## :game_die: Extra config

Edit the `./config/gitlab-runner/config.toml` file and add `extra_hosts = ["gitlab.localhost:172.17.0.1"]` extra args into `[runners.docker]` section, like this.

```toml
[[runners]]
  name = "d0dd48cad767"
  url = "http://gitlab/"
  token = "KyHvAgn4mmPG4CKEBCac"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    tls_verify = false
    image = "ruby:2.7"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    extra_hosts = ["gitlab.localhost:172.17.0.1"]
    volumes = ["/cache"]
    shm_size = 0
```

Save and restart the gitlab runner
