# qyoobs-vnc

Opens a VNC viewer to another Qube. This is useful for facilitating screensharing of another qube
through Google Meet, Zoom, etc.

Download the latest `.rpm` from the [Releases page](https://github.com/inahga/qyoobs-vnc/releases/latest),
and install it with `dnf` to both the source and destination qube. It is preferable to install it
your templates.
```bash
sudo dnf install -y /path/to/downloaded/rpm
```

In Dom0, apply a policy that permits `qyoobs-vnc` to work. Use the Qubes Policy Editor, or whatever
strategy you're using to apply Qubes policies.
```bash
# Configure this policy to your taste.
qyoobs.VNC * @anyvm @anyvm ask
```

Then, run `qyoobs-vnc connect`. Let's try connecting to the `personal` VM.
```bash
qyoobs-vnc connect personal
```

You'll get a dom0 prompt to allow the operation. Allow it, then a VNC window with the contents of
the target qube should appear.

Maximize and drag the window to another workspace. You should then be able to screenshare the VNC
window from your source qube.

Note that if you have multiple monitors, you'll probably want to limit the capture to a single monitor.
Use the `--monitor` option.
```bash
qyoobs-vnc connect personal --monitor 0
```

You can also interactively select an entity to capture using the `--choose` flag.
```bash
qyoobs-vnc connect personal --choose
```

## Building

To build from source, install the dependencies `libX11-devel` and `libXinerama-devel`.

To build the RPM, install `cargo-generate-rpm` with `cargo install cargo-generate-rpm`. Then, build
it.
```bash
cargo build --release
cargo generate-rpm
```

The RPM will be present in `target/generate-rpm`.

## Alternatives

### Manually

This is basically a wrapper around https://www.d10.dev/blog/qubes-vnc-screenshare/. You can follow
those steps instead, if you want.

### qubes-video-companion

If [qubes-video-companion](https://github.com/QubesOS/qubes-video-companion) works for you, you
should use that instead. I found it consumed a lot of CPU, and having the screen shared through
a camera device is kind of awkward.

Maybe I could have improved that instead, but I don't like writing Python ;)
