1. Open an ubuntu shell
2. Run as root
   ```bash
   sudo su
   ```
3. Download the Alpine Linux miniroot filesystem
   ```bash
   cd /
   wget https://dl-cdn.alpinelinux.org/alpine/v3.18/releases/x86_64/alpine-minirootfs-3.18.4-x86_64.tar.gz
   ```
4. Create a directory to hold the root filesystem
   ```bash
   mkdir /alpine-rootfs
   ```

5. Extract the Alpine filesystem into the new directory
   ```bash
   tar -xzf alpine-minirootfs-3.18.4-x86_64.tar.gz -C /alpine-rootfs
   ```
6. Use `unshare` to create a new namespace environment and enter it
   ```bash
   sudo unshare --mount --uts --pid --fork --net --user --map-root-user --ipc --mount-proc /bin/bash
   ```
7. **Question** What linux distribution and version are you running on now? find the linux cli command to find the linux distribution and version.

8. Inside the unshared environment, mount the root filesystem and use `chroot` to checnge the root filesystem
   ```bash
   mount --bind /alpine-rootfs alpine-rootfs
   chroot alpine-rootfs /bin/sh
   ```
9. **Question** What linux distribution and version are you running on now? Use the same command you used in section 6.
10. **Question** What network devices do you see? Can you ping google.com? if not, why?
11. **Question** Is there any security concern in terms of user namespace in the new container?
12. **Question** If you were to create a second identical container by running the same commands, what major security issue may occur?




