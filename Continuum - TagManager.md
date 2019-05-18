---
extensions:
	mermaid: {
	    enabled: true
	}

---

<h1 id="continuum---tag-manager">Continuum - Tag Manager</h1>
<h2 id="why-all-of-this-">Why all of this ?</h2>
<p>The goal was to track client navigation and send Google Analytics data through all the order process and until he successfully paid (to track conversion).</p>
<p>'Cause of 4escape iframe’s behavior, referrer was lost when user goes through the iframe and if <strong>tag manager</strong> proceeds as expected in the <a href="https://4escape.groovehq.com/knowledge_base/topics/configurer-google-tag-manager-pour-analytics-adwords-et-facebook-pixel">4escape documentation</a>.</p>
<p>So we had to work with <a href="https://developer.mozilla.org/fr/docs/Web/API/Window/postMessage">postMessage</a> method to send data <strong>from</strong> the iframe <strong>to</strong> the continuum reservation’s page. In this way we can’t loose referrer.</p>
<p>To make all this stuff working, <a href="https://datarunsdeep.com.au/blog/how-track-iframes-google-tag-manager">this post</a> inspired me.</p>
<h2 id="needed-sequences">Needed sequences</h2>
<div class="mermaid"><svg xmlns="http://www.w3.org/2000/svg" id="mermaid-svg-UV2FSIKfUpqaNiqy" height="100%" width="100%" style="max-width:870px;" viewBox="-60 -10 870 617"><g></g><g><line id="actor8" x1="75" y1="5" x2="75" y2="606" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="0" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="75" dy="0">Continuum</tspan></text></g><g><line id="actor9" x1="275" y1="5" x2="275" y2="606" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="200" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="275" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="275" dy="0">GA</tspan></text></g><g><line id="actor10" x1="475" y1="5" x2="475" y2="606" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="400" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="475" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="475" dy="0">4Escape (Iframe)</tspan></text></g><g><line id="actor11" x1="675" y1="5" x2="675" y2="606" class="actor-line" stroke-width="0.5px" stroke="#999"></line><rect x="600" y="0" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="675" y="32.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="675" dy="0">4Escape</tspan></text></g><defs><marker id="arrowhead" refX="5" refY="2" markerWidth="6" markerHeight="4" orient="auto"><path d="M 0,0 V 4 L6,2 Z"></path></marker></defs><defs><marker id="crosshead" markerWidth="15" markerHeight="8" orient="auto" refX="16" refY="4"><path fill="black" stroke="#000000" stroke-width="1px" d="M 9,2 V 6 L16,4 Z" style="stroke-dasharray: 0, 0;"></path><path fill="none" stroke="#000000" stroke-width="1px" d="M 0,1 L 6,7 M 6,1 L 0,7" style="stroke-dasharray: 0, 0;"></path></marker></defs><g><text x="175" y="93" class="messageText" style="text-anchor: middle;">Hit page (Reservation.php)</text><line x1="75" y1="100" x2="275" y2="100" class="messageLine0" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="fill: none;"></line></g><g><text x="275" y="128" class="messageText" style="text-anchor: middle;">load Iframe</text><line x1="75" y1="135" x2="475" y2="135" class="messageLine0" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="fill: none;"></line></g><g><rect x="400" y="170" fill="#EDF2AE" stroke="#666" width="150" height="34" rx="0" ry="0" class="note"></rect><text x="396" y="194" fill="black" class="noteText"><tspan x="416" fill="black">Page is loaded</tspan></text></g><g><text x="275" y="232" class="messageText" style="text-anchor: middle;">says to parent "I'm loaded" (PostMessage)</text><line x1="475" y1="239" x2="75" y2="239" class="messageLine1" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="stroke-dasharray: 3, 3; fill: none;"></line></g><g><rect x="70" y="241" fill="#f4f4f4" stroke="#666" width="10" height="121" rx="0" ry="0"></rect></g><g><rect x="400" y="249" fill="#EDF2AE" stroke="#666" width="150" height="34" rx="0" ry="0" class="note"></rect><text x="396" y="273" fill="black" class="noteText"><tspan x="416" fill="black">see note (1)</tspan></text></g><g><rect x="0" y="293" fill="#EDF2AE" stroke="#666" width="150" height="34" rx="0" ry="0" class="note"></rect><text x="-4" y="317" fill="black" class="noteText"><tspan x="16" fill="black">see note (2)</tspan></text></g><g><text x="177.5" y="355" class="messageText" style="text-anchor: middle;">Hit page (iframe page URL)</text><line x1="80" y1="362" x2="275" y2="362" class="messageLine0" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="fill: none;"></line></g><g><line x1="-10" y1="145" x2="560" y2="145" class="loopLine"></line><line x1="560" y1="145" x2="560" y2="372" class="loopLine"></line><line x1="-10" y1="372" x2="560" y2="372" class="loopLine"></line><line x1="-10" y1="145" x2="-10" y2="372" class="loopLine"></line><polygon points="-10,145 40,145 40,158 31.6,165 -10,165" class="labelBox"></polygon><text x="-2.5" y="160" fill="black" class="labelText"><tspan x="-2.5" fill="black">loop</tspan></text><text x="275" y="160" fill="black" class="loopText" style="text-anchor: middle;"><tspan x="275" fill="black">[ Navigating In Iframe ]</tspan></text></g><g><text x="575" y="425" class="messageText" style="text-anchor: middle;">Got to page "checkout/thankyou"</text><line x1="475" y1="432" x2="675" y2="432" class="messageLine0" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="fill: none;"></line></g><g><rect x="670" y="434" fill="#f4f4f4" stroke="#666" width="10" height="77" rx="0" ry="0"></rect></g><g><rect x="600" y="442" fill="#EDF2AE" stroke="#666" width="150" height="34" rx="0" ry="0" class="note"></rect><text x="596" y="466" fill="black" class="noteText"><tspan x="616" fill="black">see note (3)</tspan></text></g><g><text x="472.5" y="504" class="messageText" style="text-anchor: middle;">Hit page (current URL) + GTM Event order:success</text><line x1="670" y1="511" x2="275" y2="511" class="messageLine0" stroke-width="2" stroke="black" marker-end="url(#arrowhead)" style="fill: none;"></line></g><g><line x1="265" y1="382" x2="760" y2="382" class="loopLine"></line><line x1="760" y1="382" x2="760" y2="521" class="loopLine"></line><line x1="265" y1="521" x2="760" y2="521" class="loopLine"></line><line x1="265" y1="382" x2="265" y2="521" class="loopLine"></line><polygon points="265,382 315,382 315,395 306.6,402 265,402" class="labelBox"></polygon><text x="272.5" y="397" fill="black" class="labelText"><tspan x="272.5" fill="black">alt</tspan></text><text x="512.5" y="397" fill="black" class="loopText" style="text-anchor: middle;"><tspan x="512.5" fill="black">[ Payment Success ]</tspan></text></g><g><rect x="0" y="541" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="75" y="573.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="75" dy="0">Continuum</tspan></text></g><g><rect x="200" y="541" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="275" y="573.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="275" dy="0">GA</tspan></text></g><g><rect x="400" y="541" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="475" y="573.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="475" dy="0">4Escape (Iframe)</tspan></text></g><g><rect x="600" y="541" fill="#eaeaea" stroke="#666" width="150" height="65" rx="3" ry="3" class="actor"></rect><text x="675" y="573.5" dominant-baseline="central" alignment-baseline="central" class="actor" style="text-anchor: middle;"><tspan x="675" dy="0">4Escape</tspan></text></g></svg></div>
<p>Notes:</p>
<ul>
<li>(1) send <code>custom.postMessage.page with</code> URL data</li>
<li>(2) wait for <code>custom.postMessage.page</code> retrieve data from <code>dataLayer</code></li>
<li>(3) wait until <code>window.OrderId</code> is defined  and send <code>custom.waitForVar.OrderId</code> event</li>
</ul>
<h2 id="new-triggers">New triggers</h2>
<p>Complexity here was to create some <strong>triggers</strong> which divide:</p>
<ul>
<li>iframe’s URLs from <strong>4escape</strong></li>
<li><strong>4escape’s</strong> URLs not in iframe</li>
<li><strong>Continuum’s</strong> URLs.</li>
</ul>
<h3 id="why-only-iframes-urls-">Why only Iframe’s URLs ?</h3>
<p>Unfortunatly when the order is successed, 4escape <strong>looses the iframe behavior</strong> and sends the user to a successful page it own… And from here user can navigate on other 4escape’s pages…<br>
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
<li><strong>Custom - Wait For Var - OrderId</strong>: this checks if event <code>custom.waitForVar.OrderId</code> is sent from the <code>dataLayer</code> (see below how this event is triggered). It notifies current page that <code>window.orderId</code> is ready to use.</li>
</ul>
<h2 id="new-tags">New Tags</h2>

<table>
<thead>
<tr>
<th align="center">id</th>
<th align="center">Tags</th>
<th>What’s for</th>
<th>Depends On</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center"><strong>1</strong></td>
<td align="center"><em>IFRAME - Html - PostMessage - Send</em></td>
<td>Print a JS script which sends the custom event <code>custom.postMessage.page</code> to the parent thanks to the <strong>famous</strong> <a href="https://developer.mozilla.org/fr/docs/Web/API/Window/postMessage">postMessage</a>  method.</td>
<td></td>
</tr>
<tr>
<td align="center"><strong>2</strong></td>
<td align="center"><em>IFRAME - Html - PostMessage - Receive</em></td>
<td>Print a JS script which retrieves the message from  the <a href="https://developer.mozilla.org/fr/docs/Web/API/Window/postMessage">postMessage</a>  method and push data to the <code>dataLayer</code>. That’s what creates the <code>custom.postMessage.page</code> event.</td>
<td><strong>2</strong></td>
</tr>
<tr>
<td align="center"><strong>3</strong></td>
<td align="center"><em>GA - Pageview - Virtual - postMessage</em></td>
<td>This tag is triggered when the <code>custom.postMessage.page</code> event is fired. It retrieves data from <code>dataLayer</code> to build <strong>GA PageView hit</strong> with the path of the <strong>Iframe’s URL</strong></td>
<td><strong>3</strong></td>
</tr>
<tr>
<td align="center"><strong>4</strong></td>
<td align="center"><em>4escape - Wait for Var - OrderId is set</em></td>
<td>Print a JS when  <code>4escape - Payment Succcess Page</code> is triggered. The script waits until <code>window.orderId</code> var is defined and push <code>custom.waitForVar.OrderId</code> event to the <code>dataLayer</code>. This is a necessary step before sending <em>GA - Conversion</em>, <em>Pixel Facebook - Conversion</em>, <em>Adwords - Conversion</em>  and <em>Google Analytics - Pageview - Not Iframe</em></td>
<td></td>
</tr>
<tr>
<td align="center"><strong>5</strong></td>
<td align="center"><em>Google Analytics - Pageview - Not Iframe</em></td>
<td>Create a GA PageView hit for all pages (<strong>4escape</strong> and <strong>Continuum</strong> pages included) which are not called in Iframe</td>
<td><strong>4</strong></td>
</tr>
</tbody>
</table><h2 id="improvements">Improvements</h2>
<p>Should be great to track every activities on the cart process. Because <strong>4escape</strong> exposes an object in JS with all the checkout data, I create yet a bunch of script to allow it.<br>
Problems is: <code>orderId</code> before and after payment is not the same, so we can’t rely data on it, it shouldn’t be consistent.<br>
Maybe <strong>4escape</strong> can do something for it ?</p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

