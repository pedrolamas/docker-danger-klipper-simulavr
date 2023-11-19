# docker-danger-klipper-simulavr

[![Project Maintenance](https://img.shields.io/maintenance/yes/2023.svg)](https://github.com/pedrolamas/docker-danger-klipper-simulavr 'GitHub Repository')
[![License](https://img.shields.io/github/license/pedrolamas/docker-danger-klipper-simulavr.svg)](https://github.com/pedrolamas/docker-danger-klipper-simulavr/blob/master/LICENSE 'License')

[![Release](https://github.com/pedrolamas/docker-danger-klipper-simulavr/workflows/Release/badge.svg)](https://github.com/pedrolamas/docker-danger-klipper-simulavr/actions 'Build Status')

[![Follow pedrolamas on Twitter](https://img.shields.io/twitter/follow/pedrolamas?label=Follow%20@pedrolamas%20on%20Twitter&style=social)](https://twitter.com/pedrolamas)
[![Follow pedrolamas on Mastodon](https://img.shields.io/mastodon/follow/109365776481898704?label=Follow%20@pedrolamas%20on%20Mastodon&domain=https%3A%2F%2Fhachyderm.io&style=social)](https://hachyderm.io/@pedrolamas)

Simple Docker image running [Danger Klipper](https://github.com/DangerKlippers/danger-klipper/) with Simulavr, [Moonraker](https://github.com/Arksine/moonraker/), and [mjpg-streamer](https://github.com/jacksonliam/mjpg-streamer).

This repo will run a GitHub action every hour to check for new code on the "master" branches of the Danger Klipper and Moonraker repositories, and creates a new Docker image if there are any modifications.

## Usage

Create and run the new container as you would normally do:

```sh
docker run -d \
  --name danger-klipper-simulavr \
  --net=host \
  ei99070/docker-danger-klipper-simulavr
```

This will start Klipper with a simulated Atmel ATmega micro-controller, Moonraker on port 7125, and mjpg-streamer on port 8080.

If you need to remap the default ports, run the container under the bridge network instead:

```sh
docker run -d \
  --name danger-klipper-simulavr \
  -p 7125:7125 \
  -p 8080:8080 \
  ei99070/docker-danger-klipper-simulavr
```

The default configuration files used can be found on the [klipper_config](/klipper_config) folder.

This is the runtime folder structure:

```txt
/printer
  /klipper
  /klippy-env
  /mjpg-streamer
  /moonraker
  /moonraker-env
  /printer_data
    /config
      /moonraker.conf
      /printer.cfg
    /database
    /gcodes
    /logs
      /klippy.log
      /moonraker.log
      /supervisord.log
    /moonraker.asvc
  /pysimulavr
```

Any of these files can be overrided by mapping the folder or the specific file.

For example, here is how to override the default `printer.cfg`:

```sh
docker run -d \
  --name danger-klipper-simulavr \
  --net=host \
  -v my-printer.cfg:/printer/printer_data/config/printer.cfg \
  ei99070/docker-danger-klipper-simulavr
```

## Klippy Extras

Some Klipper extra modules are included as part of this image, specifically:

- `virtual_pins` - https://github.com/pedrolamas/klipper-virtual-pins

## Available tags

- `latest`: points to Danger Klipper and Moonraker "master" branches
- `klipper-sha-<hash>`: points to the Danger Klipper GitHub commit hash
- `moonraker-sha-<hash>`: points to the Moonraker GitHub commit hash

## License

MIT
