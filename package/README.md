# docker
This is a tool to run aux4 on Docker.

## Usage

.aux4

```json
{
  "profiles": [
    {
      "name": "main",
      "commands": [
        {
          "name": "hello",
          "execute": [
            "echo 'Hello, World!'"
          ],
          "help": {
            "text": "Say hello"
          }
        }
      ]
    }
  ]
}
```

To run the command `hello` on Docker, use the following command:

```bash
> aux4 aux4 docker run hello
```
```text
Hello, World!
```

Check the command [aux4 aux4 docker](./commands/aux4/docker) for more information.
