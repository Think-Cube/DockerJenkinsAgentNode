# Docker Jenkins Agent Node

A lightweight and extensible Docker image for running a Jenkins build agent over SSH. It creates a dedicated non-root user, sets up a secure SSH server, and is ready to be used as an SSH agent in Jenkins pipelines.

## Features

- Creates dedicated jenkins user (with customizable UID/GID)
- Installs and configures openssh-server
- Allows injection of SSH public keys via environment variables or container arguments
- Automatically generates SSH host keys if missing
- Secure by default: disables root login and password authentication
- Easily extensible with tools like Node.js, npm, or Yarn

## Getting Started

#### Build the Image

```bash
docker build -t jenkins-agent-node .
```

#### Run the Container
Using environment variable:

```bash
docker run -d \
  -e JENKINS_AGENT_SSH_PUBKEY="ssh-rsa AAAA..." \
  -p 2222:22 \
  jenkins-agent-node
```

Or pass the SSH public key as a parameter:  

```bash
docker run -d \
  -p 2222:22 \
  jenkins-agent-node "ssh-rsa AAAA..."
```

## Connect from Jenkins

In Jenkins:

- Host: IP or hostname of your Docker host
- Port: `2222` (or the mapped SSH port)
- Credentials: Use a matching SSH private key
- Remote root directory: `/home/jenkins`

## Environment Variables

| Variable                   | Description                                  |
|----------------------------|----------------------------------------------|
| `JENKINS_AGENT_SSH_PUBKEY` | SSH public key to authorize in the container |
| `JENKINS_AGENT_HOME`       | Home directory for the Jenkins user          |
| `user`, `group`            | Username and group name (default: `jenkins`) |
| `uid`, `gid`               | UID and GID of the user (default: `1000`)    |

## License

This project is open source and available under the [MIT License](https://opensource.org/licenses/MIT).
