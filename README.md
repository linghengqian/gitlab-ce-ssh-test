# gitlab-ce-ssh-test

- For SDO BloodLine Community.

- Execute the following commands under Ubuntu WSL 22.04.4.
  Docker Engine `27.1.1` must be installed in advance.

```shell
git clone git@github.com:linghengqian/gitlab-ce-ssh-test.git
cd ./gitlab-ce-ssh-test/
docker compose --file ./docker-compose-success.yml pull 
docker compose --file ./docker-compose-success.yml up -d
```

- To remove the Docker Compose unit, execute the following command.
```shell
docker compose --file ./docker-compose-success.yml down --volumes
```

## Additional Topics

- The contents of the configuration file are explained below.

- `hostname` and `external_url` will change the URL where GitLab can be accessed to `127.0.0.1:12345`.
  `ports` exposes the container port of `12345:12345`.
  See https://gitlab.com/gitlab-org/omnibus-gitlab/-/blob/17.3.0+ce.0/files/gitlab-config-template/gitlab.rb.template?ref_type=tags#L32 .

- `gitlab_rails['time_zone']` and `TZ` change the time zone inside the container.
  See https://gitlab.com/gitlab-org/omnibus-gitlab/-/blob/17.3.0+ce.0/files/gitlab-config-template/gitlab.rb.template?ref_type=tags#L68 .

- `gitlab_rails['gitlab_shell_ssh_port']` exposes the SSH service on port `22345`.
  `ports` exposes the container port of `22345:22`.
  See https://docs.gitlab.com/17.3/ee/install/docker/#install-gitlab-using-docker-compose
  and https://gitlab.com/gitlab-org/omnibus-gitlab/-/blob/17.3.0+ce.0/files/gitlab-config-template/gitlab.rb.template?ref_type=tags#L698 .

- `gitlab_rails['initial_root_password']` changes the password of the `root` user.
  See https://docs.gitlab.com/17.3/ee/install/docker/#install-gitlab-using-docker-swarm-mode
  and https://gitlab.com/gitlab-org/omnibus-gitlab/-/blob/17.3.0+ce.0/files/gitlab-config-template/gitlab.rb.template?ref_type=tags#L733 .

- `registry_external_url` configures container registry under an existing GitLab domain.
  We can log in via `docker login 127.0.0.1:32345`.
  `ports` exposes the container port of `32345:32345`.
  See https://docs.gitlab.com/17.3/ee/administration/packages/container_registry.html#configure-container-registry-under-an-existing-gitlab-domain .

- The following configuration is
  from https://docs.gitlab.com/17.3/omnibus/settings/memory_constrained_envs.html#configuration-with-all-the-changes to
  run in a memory constrained environment.

```shell
puma['worker_processes'] = 0
sidekiq['concurrency'] = 10
prometheus_monitoring['enable'] = false
```
