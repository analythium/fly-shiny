# Deploy Shiny Apps to Fly.io

This repository is an appendix to [Fly related posts on the Hosting Data Apps website](https://hosting.analythium.io/tag/fly/):

- [Make Your Shiny App Fly](https://hosting.analythium.io/make-your-shiny-app-fly/)
- [Auto-scaling Shiny Apps in Multiple Regions with Fly.io](https://hosting.analythium.io/auto-scaling-shiny-apps-in-multiple-regions-with-fly-io/)

## What is Fly

> Deploy App Servers Close to Your Users
>
> Run your full stack apps (and databases!) all over the world. No ops required.

## Setup

Following the Fly.io intro: <https://fly.io/docs/hands-on/start/>

**Step 1.** Install `flyctl` <https://fly.io/docs/hands-on/installing/>

**Step 2.** Sign up or sign in <https://fly.io/docs/hands-on/sign-up/>

```bash
flyctl auth login
```

**Step 3.** Create an app

We will use the Docker image from the <https://gitlab.com/analythium/shinyproxy-hello> project:

```bash
export DOCKER_IMAGE="analythium/shinyproxy-demo:latest"
docker pull $DOCKER_IMAGE
```

Now create the app and follow instructions (you'll need credit card info too):

```bash
flyctl launch --image $DOCKER_IMAGE
```

Now open the newly created `fly.toml` and edit (see <https://fly.io/docs/reference/configuration/> for detailed config options):

```toml
...
[[services]]
...
  internal_port = 3838
...
```

**Step 4.** Deploy the app

```bash
flyctl deploy
```

Now the image gets pushed into the Fly registry. Once the release is healthy, you have a shiny app.

**Step 5.** Check the app status. The output will give the Hostname (`app.fly.dev` where `app` is the app name from the TOML file)

```bash
flyctl status
```

**Step 6.** Visit the app

Copy the Hostname and paste in the browser tab, or use `flyctl open`.

## CICD and GitHub Actions

See the `.github/workflows/main.yml` file for an example.

## Resources

- [Continuous Deployment with Fly and GitHub Actions](https://fly.io/docs/app-guides/continuous-deployment-with-github-actions/)
- [Set up Custom Domains for your App](https://fly.io/docs/app-guides/custom-domains-with-fly/)
- [Scale your App on Fly](https://fly.io/docs/scaling/)
