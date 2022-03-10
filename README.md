# github-ssh-key-deprecation-notes

Making sense of relevant information around [the GitHub SSH Key Deprecation](https://github.blog/2021-09-01-improving-git-protocol-security-github/)

## CircleCI users

For CircleCI users, we may want to refer to this Discussion post:
https://discuss.circleci.com/t/github-ssh-deprecation-information-resolutions/42887

However, I tend to be more of a visual person.
So, I wonder if we can craft the information above to a flowchart to determine if we need to take action for any CircleCI project.


```mermaid

flowchart TD
    project[Project] --> key_exists{Has User/Deploy key?}
    key_exists -- NO --> ok((OK))
    key_exists -- YES --> key_type{What key type?}
    key_type -- DSA --> action_required((Action required))
    key_type -- others --> within_window{Project created between 2021-11-02 and 2022-01-13?}
    within_window -- NO --> when_created{When was it created?}
    when_created -- After 2022-01-13 --> ok
    when_created -- Before 2021-11-02 --> action_required
    within_window -- YES --> executor_type{Uses Docker / Machine?}
    executor_type -- Docker --> legacy_docker_img{Legacy circleci/ image?}
    executor_type -- Machine --> set_machine_img{Specified image?}
    executor_type -- Others --> ok
    legacy_docker_img -- YES --> action_required
    legacy_docker_img -- NO --> ok
    set_machine_img -- NO --> action_required
    set_machine_img -- YES --> newer_than_ubuntu14{newer than Ubuntu 14.04?}
    newer_than_ubuntu14 -- YES --> ok
    newer_than_ubuntu14 -- NO --> action_required
```

**Note:** for action required, it can include verifying the SSH key type as a first step.
