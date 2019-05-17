---


---

<h1 id="continuum---tag-manager">Continuum - Tag Manager</h1>
<h2 id="why-all-of-this-">Why all of this ?</h2>
<p>The goal was to track client navigation and send Google Analytics data through all the order process and until he successfully paid (to track conversion).</p>
<p>'Cause of 4escape iframe’s behavior, referrer was lost when user goes through the iframe and if <strong>tag manager</strong> proceeds as expected in the <a href="https://4escape.groovehq.com/knowledge_base/topics/configurer-google-tag-manager-pour-analytics-adwords-et-facebook-pixel">4escape documentation</a>.</p>
<p>So we had to work with <a href="1">postMessage</a> method to send data <strong>from</strong> the iframe <strong>to</strong> the continuum reservation’s page. In this way we can’t loose referrer.</p>
<p>To make all this stuff working, <a href="https://datarunsdeep.com.au/blog/how-track-iframes-google-tag-manager">this post</a> inspired me.</p>
<pre class=" language-sequence"><code class="prism  language-sequence">Andrew-&gt;China: Says Hello
Note right of China: China thinks\nabout it
China--&gt;Andrew: How are you?
Andrew-&gt;&gt;China: I am good thanks!
</code></pre>
<h2 id="new-triggers">New triggers</h2>
<p>Complexity here was to create some <strong>triggers</strong> which divide:</p>
<ul>
<li>iframe’s URLs from <strong>4escape</strong></li>
<li><strong>4escape’s</strong> URLs not in iframe</li>
<li><strong>Continuum’s</strong> URLs.</li>
</ul>
<h3 id="why-only-iframes-urls-">Why only Iframe’s URLs ?</h3>
<p>Unfortunatly when the order is successed, 4escape <strong>loose the iframe behavior</strong> and send the user to a successful page they own… And from here user can navigate on other 4escape’s pages…<br>
So we have to tracks, as <strong>Continuum</strong> does, URLs <strong>from 4escapes</strong> which are <strong>not</strong> in Iframe.<br>
What is <strong>not a problem</strong> here: referrer is not lost when user goes in 4escape’s successful page.</p>
<h3 id="triggers-list">Triggers list</h3>
<h4 id="triggers-depending-on-user-localisation">Triggers depending on user localisation</h4>
<ul>
<li><strong>URL Iframe</strong>: checks if <code>Page URL</code> contains <code>iframe=1</code></li>
<li><strong>4escape - iframe</strong>: combines <strong>URL Iframe</strong> and checks if <code>Page Hostname</code> contains <code>4escape.io</code></li>
<li><strong>4escape - Payment Success Page</strong>: check if <code>Page Path</code> contains <code>checkout/thankyou</code> AND if the var <code>4escape - Payment Success</code> = <code>1</code></li>
<li><strong>Continuum Page Reservation</strong>: check if <code>Page Path</code> contains <code>/reservation.php</code></li>
</ul>
<h4 id="triggers-depending-on-custom-behaviors">Triggers depending on custom behaviors</h4>
<ul>
<li><strong>Custom - postMessage - Page</strong>: this check if event <code>custom.postMessage.page</code> is sent from the <code>dataLayer</code> (see below how this event is triggered). It is receiving from <strong>Continuum reservation’s page</strong> and notify that data are sent from the iframe</li>
<li><strong>Custom - Wait For Var - OrderId</strong>: this check if event <code>custom.waitForVar.OrderId</code> is sent from the <code>dataLayer</code> (see below how this event is triggered). It notifies current page that <code>window.orderId</code> is ready to use.</li>
</ul>
<h2 id="new-tags">New Tags</h2>

<table>
<thead>
<tr>
<th align="center">Tags</th>
<th>What’s for</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center"><em>IFRAME - Html - PostMessage - Send</em></td>
<td>Print JS script which sends the custom event <code>custom.postMessage.page</code> to the parent thanks to the <strong>famous</strong> <a href="1">postMessage</a>  method.</td>
</tr>
<tr>
<td align="center"><em>IFRAME - Html - PostMessage - Receive</em></td>
<td>Print JS script which retrieve the message from  the <a href="1">postMessage</a>  method and push data to the <code>dataLayer</code>. That’s what creates the <code>custom.postMessage.page</code> event.</td>
</tr>
<tr>
<td align="center"><em>GA - Pageview - Virtual - postMessage</em></td>
<td>This tag is waiting for the <code>custom.postMessage.page</code> event. It retrieves data from <code>dataLayer</code> to build <strong>GA PageView hit</strong> with the path of the <strong>Iframe’s URL</strong></td>
</tr>
<tr>
<td align="center"><em>4escape - Wait for Var - OrderId is set</em></td>
<td>Print JS when  <code>4escape - Payment Succcess Page</code> is triggered. The script waits until <code>window.orderId</code> var is defined and push <code>custom.waitForVar.OrderId</code> event to the <code>dataLayer</code>. This is a necessary step before sending <em>GA - Conversion</em>, <em>Pixel Facebook - Conversion</em> and <em>Adwords - Conversion</em></td>
</tr>
<tr>
<td align="center"><em>Google Analytics - Pageview - Not Iframe</em></td>
<td>Create a GA PageView hit for all pages (<strong>4escape</strong> and <strong>Continuum</strong> pages included) which are not called in Iframe</td>
</tr>
</tbody>
</table><blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

