1. Download the Alpine Linux miniroot filesystem
   ```bash
    wget https://dl-cdn.alpinelinux.org/alpine/v3.18/releases/x86_64/alpine-minirootfs-3.18.4-x86_64.tar.gz
    ```
2. Create a directory to hold the root filesystem
    ```bash
    mkdir alpine-rootfs
    ```
3. Extract the Alpine filesystem into the new directory
    ```bash
    tar -xzf alpine-minirootfs-3.18.4-x86_64.tar.gz -C alpine-rootfs
    ```
4. Use `unshare` to create a new namespace environment and enter it
    ```bash
    sudo unshare --mount --uts --pid --fork --net --user --ipc --mount-proc /bin/bash
    ```
5. Inside the unshared environment, mount the root filesystem and use `chroot`
    ```bash
    mount --bind alpine-rootfs alpine-rootfs
    chroot alpine-rootfs /bin/sh
    ```



