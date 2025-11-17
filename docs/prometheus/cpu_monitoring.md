---


---

<h2 id="cpu-monitoring">CPU Monitoring</h2>
<h3 id="node_cpu_seconds_total"ul>
<li><strong>node_cpu_seconds_total</strong></h3li>
</ul>
<p>Type: <strong>Monotonic Counter</strong></p>
<p>Fields:</p>
<ol>
<li>cpu</li>
<li>mode{idle, iowait, irq, nice, softirq, steal, system, user}</li>
</ol>
<pre><code>node_cpu_seconds_total{mode="idle"}
# Returns an instant vector for  counter

![allt the timeseriesext](<Screenshot from 2025-11-17 13-14-50.png>)

</code></pre>
<pre><code>node_cpu_seconds_total{mode="idle", cpu="0"}[5m]
# Returns 

List item

a range vector
List item
</code></pre>

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA4MDgxMDk2OV19
-->