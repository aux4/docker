# aux4/docker

Run aux4 commands inside a Docker container. Useful for running aux4 in isolated environments or deploying aux4-based services.

## Installation

```bash
aux4 aux4 pkger install aux4/docker
```

## Usage

Given a `.aux4` file with a `hello` command, run it inside Docker:

```bash
> aux4 aux4 docker run hello
```
```text
Hello, World!
```

For more details, see [aux4 aux4 docker](./commands/aux4/docker).
