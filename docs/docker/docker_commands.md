---


---

<h1 id="commands">Commands</h1>
<ul>
<li>Create container from image (download if image does not exist)</li>
</ul>
<pre><code>docker run &lt;image&gt;
</code></pre>
<p>tag: the version of the image. Default is the :latest tag</p>
<p>Container only runs as long as a process is executing inside it, os containers exit imediately with the above command.</p>
<p>To run the container in interactive mode:</p>
<pre><code>docker run -it &lt;image&gt; bash
</code></pre>
<p>Port mapping</p>
<pre><code>docker run -p host_port:docker_port &lt;image&gt;
</code></pre>
<p>Cannot change port mappings on a running or stopped container - you must recreate it</p>
<p>Data persistence</p>
<pre><code>docker run -v host_location:docker_location &lt;image&gt;
</code></pre>
<p>Detached mode</p>
<pre><code>docker run -d  &lt;image&gt;
</code></pre>
<p>Define environment variables</p>
<pre><code>docker run -e ENV_VAR=var  &lt;image&gt;
</code></pre>
<p>Run containers in remote hosts</p>
<pre><code>docker -H=host:port run &lt;image&gt;
</code></pre>
<ul>
<li>Only pull image from registry</li>
</ul>
<pre><code>docker pull &lt;image&gt;
</code></pre>
<ul>
<li>List all available images</li>
</ul>
<pre><code>docker images
</code></pre>
<ul>
<li>Remove an image</li>
</ul>
<pre><code>docker rmi &lt;image&gt;
</code></pre>
<ul>
<li>List containers</li>
</ul>
<pre><code>docker ps # All running containers
docker ps -a # All available containers
</code></pre>
<ul>
<li>Stop a container</li>
</ul>
<pre><code>docker stop &lt;container id/ container name&gt;
</code></pre>
<ul>
<li>Start a container</li>
</ul>
<pre><code>docker start &lt;container id/ container name&gt;
</code></pre>
<ul>
<li>Pause a container: pause all processes inside a container</li>
</ul>
<pre><code>docker pause &lt;container id/ container name&gt;
</code></pre>
<ul>
<li>Remove a container</li>
</ul>
<pre><code>docker rm &lt;container id/ container name&gt;
</code></pre>
<ul>
<li>More details about container<br>
Also shows the environment variables</li>
</ul>
<pre><code>docker inspect &lt;container id/ container name&gt;
</code></pre>
<ul>
<li>Execute command in running container</li>
</ul>
<pre><code>docker exec -it container-name sh
i: Open input to the container
t: sudo terminal
</code></pre>
<ul>
<li>Docker stdout for detached mode</li>
</ul>
<pre><code>docker logs &lt;container_name_or_id&gt;
</code></pre>
<p>Create a new image from a container’s current state</p>
<pre><code>docker commit &lt;container_id&gt; &lt;new_image&gt;:&lt;tag&gt;
</code></pre>
<ul>
<li>Dockerfile commands</li>
</ul>
<ol>
<li>FROM: The image to be used</li>
<li>Copy: For deploying the code</li>
<li>ADD: COPY + other file formats like tar</li>
<li>RUN: Execute a command (Executes at buildtime)</li>
<li>CMD: Default process for the container (Executes at runtime). This command is overwritten by docker run container_name_or_id command. Add defualt arguments to the main docker process</li>
<li>ENV: Set an environment variable</li>
<li>VOLUME: Persistent storage location</li>
<li>EXPOSE: Expose a port</li>
<li>WORKDIR: Change to a directory</li>
<li>ENTRYPOINT: Default process for the container (Executes at runtime). This command is appended to the docker run container_name_or_id command</li>
</ol>
<p>Use multistage builds for different build and deploy images</p>
<ul>
<li>Limit CPU and RAM usage of a container through cgroups</li>
</ul>
<pre><code>docker run --cpu=0.5 &lt;container&gt; # for limiting cpu usage to 50%
docker run --memory=100m &lt;container&gt;
</code></pre>
<ul>
<li>Storage<br>
Docker stores all images, containers etc at <code>var/lib/docker</code>on host system</li>
<li>Registry</li>
</ul>
<pre><code>docker login &lt;registry&gt;
docker push &lt;registry/image&gt;
docker pull &lt;registry/image&gt;
</code></pre>
<p>docker cli and docker daemon connect using REST APIs</p>
<ul>
<li>Networking</li>
</ul>
<p>Network types: bridge, host, macvlan</p>
<p>Bridge:</p>
<p>All containers in a bridge network can be resolved using their container name due to in built in dns.(Works only for user defined bridge and not the default bridge)</p>
<p>docker0 is the default docker bridge network interface(Used when no network is defined).</p>
<p>Any new bridge docker network created using below command/ network: in docker compose file creates a new bridge with a different subnet. (user defined bridge)</p>
<p>Docker copies the /etc/resolv.conf file from the host for dns.</p>
<p>In bridge networks, docker containers can ping any server, but the docker ports are not exposed automatically.</p>
<pre><code>ip address
bridge link
</code></pre>
<pre><code>sudo docker network create &lt;&gt;
sudo docker network ls
sudo docker network inspect &lt;network-id&gt;
</code></pre>
<p>Host:</p>
<p>Use host network when no network isolation is required. No port exposure required.</p>
<p>MacVlan</p>
<p>Specifying the container ip and gateway ip is required (no dhcp service in docker).<br>
Docker creates a virtual mac address for the container<br>
To create a macvlan network and add a container to it:</p>
<pre><code>└❯ sudo docker network create -d macvlan \
└❯ --subnet 192.168.31.0/24 \
└❯ --gateway 192.168.31.1 \
└❯ -o parent=wlo1 \
└❯ macvlan_network

sudo docker run -itd --network macvlan_network --ip 192.168.31.4 alpine
</code></pre>
<p>macvlan requires promiscous mode to be turned on.<br>
Promiscuous mode in networking is a setting for a network interface card (NIC) that allows it to intercept and read all network packets that arrive, rather than just those addressed to it.</p>
<pre><code>sudo ip link set wlo1 promisc on
</code></pre>
<p>This mode only works for ethernet since wifi interface does not support multiple mac addresses from same nic.</p>
<p>IPVlan:<br>
Shares the same mac address of the parent host:</p>
<pre><code>sudo docker network create -d ipvlan \
--subnet 192.168.31.0/24 \
--gateway 192.168.31.1 \
-o parent=wlo1 \
ipvlan_network
</code></pre>
<ul>
<li>Secrets</li>
</ul>
<pre><code>docker secret create
</code></pre>

