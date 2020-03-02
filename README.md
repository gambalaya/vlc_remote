# vlc_remote
Home assistant interface using VLC's HTTP API for controlling VLC on a remote box.

Initial version borrowed heavily from Squeezebox setup by Kingo55.
Added play_media url option by gambalaya (required for HA text to speech).

No documentation yet. Simply:

1. Enable HTTP interface in VLC, taking note of user/pass and port.

2. Add the following under "media_player":
```
  - platform: vlc_remote
    host: 192.168.0.2
    port: 8080
    name: "HTPC VLC" # Optional
    username: "something" # Optional
    password: "xyz" # Optional
```
3. Copy integration files to <homeassistant config directory>\custom_components\vlc_remote

To Do:
 - Async
 - Syncing
 - More media info and typing
 - Handle closing/stop states better
