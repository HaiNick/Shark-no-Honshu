# Sonstiges

## PulseAudio keine Ausgabe (fix)

Nach korrupter Deinstallation von pulseaudio keine Tonausgabe und Anzeige des einzigen Audiogerätes (Dummy Output)

```bash
$ sudo touch /usr/share/pipewire/media-session.d/with-pulseaudio
$ systemctl --user restart pipewire-session-manager 
```
