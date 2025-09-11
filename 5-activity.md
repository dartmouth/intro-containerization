# Activity

## Goal

Our task is to build a base image for a PHP web app that will use Apache's mod_auth_cas plugin.

You should learn:
- How to pick a base image for your base image
- How to incrementally build up up the commands for the image
- How to use environment variables to configure the image
- How to create an entry point script


## Pick a base image for your base image
## Incrementally build up up the commands for the image

Steps
- Evaluate options for a base image and make a justification for the one you pick
- Run a command to pull that image down
- Run a command to start a container based on that image and drop into a shell
- Install Apache and PHP if it was not included in your base image
- Install mod_auth_cas

## Use environment variables to configure the image
## Create an entry point script

Steps
- Move from interactive mode to using a Dockerfile with a build step
- Hand edit the config file and get a fully working app protected by Dartmouth SSO
- Create environment variables to fill in the config file
- Create a start up script that reads the environment variables and updates the config file

Next: [Where to next?](6-where-to-next.md)