# Hands-on Session: Exploring User Namespace

## Objectives:
- Gain practical experience working with user namespace.

---

### User Namespace


#### 1. Create a new User namespace with root privileges
Open an ubuntu shell

1. Create a new user namespace.
   ```bash
   sudo unshare --user --map-root-user bash
   ```

   **Arguments Explanation**:
   - `--user`: Unshares the user namespace.
   - `--map-root-user`: Maps your user ID (UID 0) to the root user (UID 0) inside the namespace.
   - `bash`: Spawns a new shell inside the namespace.

2. Inside the new shell, check the current UID.
    ```bash
    id
    ```

3. Experiment with privileged operations:
    ```bash
    mkdir /test-directory
    ```
4. **Question** Did the command succeed?
5. **Question** Does the new directory exists outside the new user namespace? (You can open a second terminal to verify)
6. **Question** Why did the user in the new user namespace managed to write to the host filesystem?
7. **Question** is there a security vulnerability in this setup??
8. Exit the namespace.
   ```bash 
   rm -rf /test-directory
   exit
   ```

#### 2. Create a new User namespace with "fake" root
1. Check the current UID.
    ```bash
    id
    ```
2. Create a new user namespace.
    ```bash 
    unshare --user --map-user=0 bash
    ```

   **Arguments Explanation**:
   - `--user`: Unshares the user namespace.
   - `--map-user=0`: Maps your user ID (UID 1000) to the root user (UID 0) inside the namespace.
   - `bash`: Spawns a new shell inside the namespace.

3. Inside the new shell, check the current UID:
    ```bash
    id
    ```  
4. **Question** Is the running user a root user?

5. Experiment with privileged operations:
    ```bash
    mkdir /test-directory
    ```
6. **Question** Did the command succeed and why?

7. You can view the current user namespace mapping by running the following command:
   ```bash 
   cat /proc/self/uid_map
   # output format - <start of UID in parent namespace> <start of UID in current namespace> <number of posiible UIDs in the current namespace>
   ```

7. Exit the namespace.
   ``` bash
    exit
   ```