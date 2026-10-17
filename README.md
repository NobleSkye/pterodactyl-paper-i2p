# pterodactyl-paper-i2p

Docker image for a Pterodactyl egg that runs a [Paper](https://papermc.io/) Minecraft server and an [i2pd](https://i2pd.website/) router in the same container, so the server can be reached over I2P at a `.b32.i2p` address.

The image is the official Pterodactyl Java 21 yolk (`ghcr.io/pterodactyl/yolks:java_21`) with `i2pd`, `curl` and `ca-certificates` added. The egg's start script launches i2pd in the background and then starts Paper.

## Image

```
ghcr.io/nobleskye/yolks-java21-i2pd:latest
```

Each build is also tagged with the short commit SHA (for example `ghcr.io/nobleskye/yolks-java21-i2pd:abc1234`) if you want to pin a specific build.

Put the `:latest` name in the egg's `docker_images` field:

```json
"docker_images": {
    "Java 21 + i2pd": "ghcr.io/nobleskye/yolks-java21-i2pd:latest"
}
```

## How it's built

`.github/workflows/build.yml` builds the image and pushes it to GitHub Container Registry using the repository's built-in `GITHUB_TOKEN`, so no personal access token is needed. It runs:

- on every push to `main`
- on manual dispatch (Actions tab -> "Build and publish image" -> Run workflow)
- weekly on a schedule, so i2pd and base image updates get picked up

## Make the package public (one-time manual step)

New GHCR packages are private. After the first successful workflow run, make the package public, otherwise the Wings node cannot pull it without credentials:

1. Go to your GitHub profile -> **Packages** -> `yolks-java21-i2pd`.
2. Open **Package settings**.
3. Under **Danger Zone**, click **Change visibility** and choose **Public**.

You only need to do this once; later builds keep the same visibility.
