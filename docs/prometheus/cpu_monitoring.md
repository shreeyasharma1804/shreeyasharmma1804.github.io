---


---

<h2 id="cpu-monitoring">CPU Monitoring</h2>
<h3 id="node_cpu_seconds_total"><strong>node_cpu_seconds_total</strong></h3>
<p>Type: <strong>Monotonic Counter</strong></p>
<p>Fields:</p>
<ol>
<li>cpu</li>
<li>mode{idle, iowait, irq, nice, softirq, steal, system, user}</li>
</ol>
<pre><code>node_cpu_seconds_total{mode="idle"}
# Returns an instant vector for all the timeseries
</code></pre>
<pre><code>node_cpu_seconds_total{mode="idle", cpu="0"}[5m]
# Returns a range vector
List item
</code></pre>

