---


---

<h2 id="docker-storage">Docker Storage</h2>
<h3 id="volumes">Volumes</h3>
<p>Use docker volumes for data persistence, even when a container is deleted.</p>
<p>A given volume can be mounted into multiple containers simultaneously.</p>
<p>Mounting a non-empty volume hides the container’s existing files.<br>
Mounting an empty volume over a non-empty directory, docker copies existing files into the new volume. To avoid this, use<code>volume-nocopy</code></p>
<p>Create a volume:</p>
<pre><code>docker volume create
</code></pre>
<p>List all volumes</p>
<pre><code>docker volume ls
</code></pre>
<p>Inspect a volume</p>
<pre><code>docker inspect &lt;volume&gt;
</code></pre>
<p>Mount a volume</p>
<pre><code>docker run --volume &lt;volume-name&gt;:&lt;mount-path&gt;
</code></pre>
<p>Mount a volume in read only mode</p>
<pre><code>docker run --volume &lt;volume-name&gt;:&lt;mount-path&gt;:ro
</code></pre>
<p>Specifying <code>volume-subpath</code> is useful if you only want to share a specific portion of a volume with a container.</p>
<p>NFS volumes can be used to share a directory  with multiple containers</p>
<pre><code>docker volume create \
  --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.1.100,rw \
  --opt device=:/srv/nfsdata \
  nfsdata
</code></pre>

