# Using PhotoVision with Romi

[Install PhotonVision on the Romi](https://github.com/PhotonVision/photonvision-docs/blob/ec29ff49e4a03f8d06e00818885efa774126c24c/source/docs/getting-started/installation/romi.rst).

Most of the documentation is there but here's a few things to check on.

- If Romi is setup as an access point you must plug in an ethernet cable before running the install.
- Set the Romi to **Writable** mode before installing.
- After editing the CameraServer `runCamera` script remember to put it back into **Read-Only** mode before rebooting.
- The install on the Romi sets up the `photonvision.service` file in `/lib/systemd/system` to start PhotonVision on reboot.  You can also start it manually: `systemctl stop/start photonvision.service`
- The Raspberry Pi must be in **Writable** mode to create a new pipeline.  I believe it can go back into **Read-Only** mode after the pipeline is created and you are executing your java program.
- While creating the pipeline you must adjust the filtering controls until you've aquired one or more targets, otherwise you won't get anything in the Network Tables.

- *Pipeline Type* must be **Reflective** mode otherwise there's no data.


## References

- PhotonVision - [Install on the Romi](https://github.com/PhotonVision/photonvision-docs/blob/ec29ff49e4a03f8d06e00818885efa774126c24c/source/docs/getting-started/installation/romi.rst)

- PhotonVision [Documentation](https://docs.photonvision.org/_/downloads/en/latest/pdf/)
