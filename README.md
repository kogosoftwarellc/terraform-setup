# terraform-setup
> Facilitating concise terraform scripts.

Terraform projects can require a lot of variables.  This gets cumbersome.  While
working on a lot of terraform projects, I got sick of repeating the same stuff
over and over again.  I also got tired of my local `.terraform*` files getting out
of sync with remote state.  This project put an end to that.

## Goals

* Infrastructure collaboration with other devs.
* Predictable terraform execution.
* Rapid infrastructure development.

## Installing

Place `terraform-setup` somewhere on your `PATH`.  Ensure `terraform` is on your
`PATH` as well.  E.G.

```sh
cd "$HOME/bin"
curl https://raw.githubusercontent.com/kogosoftwarellc/terraform-setup/master/terraform-setup > terraform-setup
chmod +x terraform-setup
```

## Usage

Here's a sample script named `stg` in one of my projects.  When this file is
executed in a bash shell, it will setup my infrastructure for a staging environment:

```bash
#!/bin/bash

set -e

# State is stored in s3.  These 2 variables must be exported before sourcing.
export bucket_prefix=kogo-tf-states-
export bucket_region=us-west-1

source "`which terraform-setup`"

# terraform-setup exports env (filename of script), stack_name (name of this
# script's parent directory), and cmd (defaults to plan).

terraform "$cmd" \
  -var env="$env" \
  -var region=$bucket_region \
  -var stack_name="$stack_name" \
  -var some_other_var="foo" \
```

I can now setup my staging environment simply by running `./stg apply`.  I don't have
to type variables in each time either, and `terraform-setup` will keep all state
in s3.  Nothing is saved or stored locally.
