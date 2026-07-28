# Screen Streaming Ansible role

This role installs and configures a Linux systemd service that captures a local X11 display with ffmpeg and publishes it as an RTSP stream. It is intended for kiosk-style setups on Raspberry Pi OS and similar distributions.

## Requirements

- Ansible 2.2 or newer
- A target system running Linux with systemd
- A working graphical session or X server
- A user account that can access the target display
- ffmpeg, which is installed by this role

## What the role does

- Validates that the target is Linux and uses systemd
- Installs ffmpeg
- Deploys a systemd unit that runs ffmpeg and publishes the stream via RTSP
- Enables and starts the kiosk service
- Adjusts the kernel video mode in /boot/firmware/config.txt when applicable

## Role variables

The role exposes the following variables (see defaults/main.yml):

- screen_streaming_user: The user account that runs the service (default: pi)
- screen_streaming_group: The group used by the service (default: pi)
- screen_streaming_display: The X11 display to capture (default: :0)
- screen_streaming_resolution: The captured video resolution (default: 1920x1080)
- screen_streaming_framerate: The frame rate of the capture (default: 15)
- screen_streaming_bitrate: The video bitrate (default: 2M)
- screen_streaming_port: The RTSP port (default: 8554)
- screen_streaming_path: The RTSP path (default: /kiosk)
- screen_streaming_systemd_after: Optional systemd units to start after
- screen_streaming_systemd_requires: Optional systemd units required by the service

## Example playbook

Using the role from Ansible Galaxy:

```yaml
---
- hosts: servers
  become: true
  roles:
    - role: supcik.screen_streaming
```

Using the role directly from GitHub:

```yaml
---
- hosts: servers
  become: true
  roles:
    - name: ansible-role-screen-streaming
      src: https://github.com/supcik/ansible-role-screen-streaming.git
      scm: git
```

You can override variables if needed:

```yaml
---
- hosts: servers
  become: true
  vars:
    screen_streaming_resolution: "1280x720"
    screen_streaming_framerate: 30
  roles:
    - role: supcik.screen_streaming
```

## Notes

- The service publishes the stream at rtsp://localhost:8554/kiosk by default.
- The role expects a working X11 display and Xauthority for the configured user.
- It is commonly used together with a media server or another RTSP consumer.

## License

MIT

## Author information

Jacques Supcik <mailto:jacques@supcik.net>
