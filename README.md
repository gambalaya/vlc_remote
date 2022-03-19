# vlc_remote
Home assistant interface using VLC's HTTP API for controlling VLC on a remote box.

This is an alternative to the internal "VLC media player via Telnet" interface. This
HTTP API version has the advantage that there is no dependency on a long-running
telnet socket.

Note that the internal home assistant "RESTful Command" called from the "Universal Media Player" can
replace this custom integration if preferred:

1. Example rest command:
```
rest_command:
  play_media:
    url: 'http://bookshelf.local:9090/requests/status.xml?{{ command }}'
    username: ""
    password: "test"
```

2. Example rest sensor:
```
sensor:
  - platform: rest
    resource: http://:test@bookshelf.local:9090/requests/status.xml
    name:     vlc
    scan_interval: 60
    value_template: '{{ value_json.root.state }}'
    json_attributes_path: "$.root"
    json_attributes:
      - volume
```

3. Minimal Universal Media Player configuration:
```
media_player:
- platform: universal
    name: vlc
    attributes:
      state: sensor.vlc
      volume_level: sensor.vlc|volume
    commands:
      play_media:
        service: rest_command.play_media
        data:
          command: "command=in_play&input={{ media_content_id }}"
      media_stop:
        service: rest_command.play_media
        data:
          command: "command=pl_stop"
```

The initial version of this integration was borrowed heavily from Squeezebox by Kingo55.
Since the Universal Media Player can accomplish the same thing, I no longer maintain it.

To configure the custom integration:

1. Enable HTTP interface in VLC, taking note of user/pass and port.

2. Add the following:
```
media_player:
- platform: vlc_remote
    host: 192.168.0.2
    port: 8080
    name: "HTPC VLC" # Optional
    username: "something" # Optional
    password: "xyz" # Optional
```
3. Copy integration files to <homeassistant config directory>\custom_components\vlc_remote
