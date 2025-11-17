---


---

<h2 id="cpu-monitoring">CPU Monitoring</h2>
<ul>
<li><strong>node_cpu_seconds_total</strong></li>
</ul>
<p>Type: <strong>Monotonic Counter</strong></p>
<p>Fields:</p>
<ol>
<li>cpu</li>
<li>mode{idle, iowait, irq, nice, softirq, steal, system, user}</li>
</ol>
<pre><code>node_cpu_seconds_total{mode="idle"}
# Returns a counter
</code></pre>
![alt text](<Screenshot from 2025-11-17 13-14-50.png>)

<pre><code>node_cpu_seconds_total{mode="idle", cpu="0"}[5m]
# Returns 

List item

a range vector
</code></pre>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUxMjQ4NDUwNF19
-->
