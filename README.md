# gitlab-locally

## Extra config

After run.

```sh
docker-compose up -d
```

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
