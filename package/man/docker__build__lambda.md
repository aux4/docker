#### Description

The `lambda` command builds a Docker image for deploying aux4 commands as AWS Lambda functions. It uses `aux4/aws-lambda` as the base image and supports all three execution modes: `RAW`, `HTTP`, and `STEP_FUNCTIONS`.

The `.aux4` file can be baked into the image by placing it in the build directory, or loaded at runtime via S3 Files mount by omitting it from the build context.

#### Usage

```bash
aux4 aux4 docker build lambda <command> [--tag <tag>] [--mode <RAW|HTTP|STEP_FUNCTIONS>] [--packages <packages>] [--dir <path>]
```

--tag       The tag for the image (can be specified multiple times)
--base      The base image to use (default: aux4/aws-lambda)
--mode      The Lambda execution mode: RAW, HTTP, or STEP_FUNCTIONS
--packages  Comma-separated aux4 packages to install
--command   The Lambda command to execute (positional argument)
--dir       The directory to build the image from (default: .)

#### Example

```bash
aux4 aux4 docker build lambda hello --tag my-lambda:latest --mode HTTP --dir .
```

This builds a Lambda image that runs the `hello` command in HTTP mode. The `.aux4` file in the current directory is copied into the image.

To build without baking the `.aux4` (for use with S3 Files):

```bash
aux4 aux4 docker build lambda hello --tag my-lambda:latest --mode STEP_FUNCTIONS --dir /dev/null
```
