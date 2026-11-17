# Bedrock Survival SMP — Railway

This project deploys a Minecraft Bedrock Dedicated Server using the `itzg/minecraft-bedrock-server` container.

## Pinned server version

`VERSION=1.26.3.1`

This is the **server** version, not a client update. The container downloads the Bedrock Dedicated Server package at startup; the game server binary is not bundled in this ZIP.

## Railway setup

1. Create a new Railway project and deploy this project from a GitHub repository containing these files.
2. In the `bedrock` service, open **Variables** and add:
   - `EULA` = `TRUE`
   - `VERSION` = `1.26.3.1`
   - `GAMEMODE` = `survival`
   - `DIFFICULTY` = `easy`
   - `MAX_PLAYERS` = `10`
   - `ALLOW_LIST` = `false` (temporary first-boot setting; enable later if you want a whitelist)
   - `SERVER_PORT` = `19132`
3. Add a Railway Volume mounted at `/data`. Do not remove this volume after the world is created; it stores the world.
4. Deploy and inspect the logs. Confirm the log says `Downloading Bedrock server version 1.26.3.1` and then `Server started.`

## Networking note

Bedrock 1.26.3.1 is an older release and uses the older Bedrock networking transport (RakNet). Configure your UDP tunnel to forward to the `bedrock` service on UDP port `19132`. Do not configure a TCP-only tunnel. The public tunnel address and port are provided by your tunnel provider.

If the tunnel provider requires an internal target hostname, use the Railway private hostname for the `bedrock` service, with destination port `19132`. Do not guess the hostname; copy it from Railway's service networking page.

## Important

- This ZIP does not include Mojang's server binary. It is downloaded by the container when the service starts.
- Back up `/data` before changing server versions or replacing the service.
- If the exact version cannot be downloaded, check the deployment log for the download error. Do not silently change `VERSION` to `LATEST`.
