# Hands-on Session: Exploring Network Namespace

## Objectives:
- Gain practical experience working with network namespaces.

---

### Network Namespace
Open an ubuntu shell

**Exercise**:

#### 1. Create a new network namespace
1. Create the  namespace.
   ```bash
   sudo unshare --net --mount-proc bash
   ```
   **Explanation**:
    - `--net`: Creates a new network namespace.
    - `--mount-proc`: Mounts a new `/proc` filesystem inside the namespace.
    - `bash`: Starts a new shell inside the namespace.

2. List the network interfaces available in the network namespace.
   ```bash
   ip a
   ```  
   **Question** Do you see the same network interfaces as on the host?

#### 2. Create network connectivity between the host and the network namespace
1. **Open a new ubuntu shell (It will be referred later as the host shell)** Create a veth pair:
   ```bash
   sudo ip link add veth-host type veth peer name veth-ns
   ```

   **Explanation**:
    - `veth-host`: This will stay in the host network namespace.
    - `veth-ns`: This will be moved to the newly created namespace.

2. **From the new network namesapce** Get the PID of the shell:
   ```bash
   echo $$
   ```

3. **From the host shell** Move `veth-ns` to the PID from step 2:
   ```bash
   sudo ip link set veth-ns netns <PID>
   ```

4. **From the host shell** Configure the network interfaces.
   ```bash
   sudo ip addr add 192.168.15.1/24 dev veth-host
   sudo ip link set veth-host up
   ```
5. **From the new network namesapce** Configure the network interfaces.
   ```bash
     # Assign ip address
     ip addr add 192.168.15.2/24 dev veth-ns
     ip link set veth-ns up
     ip link set lo up
   ```
6. View the network interfaces in the network namespace:
   ```bash
   ip a
   ```
7. Rename veth-ns to eth0 (List the network interface to verify it was changed successfully)
   ```bash
   <try to find the command yourself>
   ```

- Test network connectivity from the network namespace:
   ```bash
   ping 192.168.15.1
   ```


<details>
  <summary>Bonus - Connect the container to the internet (This is a challenging one!)</summary>

- Setup the host:
   - **From the host shell**:
      ```bash
      sudo apt install iptables
      sudo sysctl -w net.ipv4.ip_forward=1
      sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
      ```
   <details>
     <summary>Add the missing command to the network namespace</summary>

  - **Inside the network namespace**
      ```bash
      <Put the missing command. if you give up you can find the command in the next section :)>
      ```
   </details>
   <details>
     <summary>Reveal the answer</summary>

   - **Inside the network namespace**
       ```bash
       ip route add default via 192.168.15.1
       ```
   </details>

2. **From the new network namesapce** Check connectivity to the internet:
    ```bash
    curl https://httpbin.org/ip
    ```
</details>

#### Cleanup

   ```bash
   exit
   # uncomment below if the bonus section was done
   #sudo sysctl -w net.ipv4.ip_forward=0
   #sudo iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
   ```


