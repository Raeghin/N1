<style>
  h3 {
    line-height: 40px;
    color:#00B883;
  }
  h3.padded {
    padding-top: 80px;
    padding-bottom: 25px;
  }
  h3 .number {
    width:40px;
    height:40px;
    margin-right:20px;
    border-radius:50%;
    border:1px solid #00B883;
    float:left;
    color:#00B883;
    text-align: center;
  }
  h3.first {
    padding-top:40px;
  }
  h3.second {
    color: #00A180;
  }
  h3.second .number {
    border:1px solid #00A180;
    color: #00A180;
  }
  h3.third {
    color: #009899;
  }
  h3.third .number {
    border:1px solid #009899;
    color: #009899;
  }
  h4 {
    line-height: 26px;
    font-size: 16px;
    color:#89C9C8;
    margin-top: 30px;
    margin-bottom: 45px;
  }
  h4 .letter {
    width:22px;
    height:22px;
    margin-right:15px;
    font-size: 14px;
    background-color: #89C9C8;
    float:left;
    color:white;
    text-align: center;
  }
  p {
    font-size:1.1em;
    line-height: 1.6em;
    color:rgba(0,0,0,0.5);
  }
  blockquote p {
    font-size: 15px;
  }
</style>


<h2 class="gsg-header">Continue building your plugin!</h2>

<p>
  If you followed the <a href="/N1/getting-started/">first part</a> of our Getting Started Guide,
  you should have a freshly written plugin that places a colored line in the message sidebar. This guide
  picks up where we left off, and goes a bit more in depth explaining what each part of the code does.
</p>
<p>
  We&#39;re going to build on your new plugin to show the sender&#39;s <a href="http://gravatar.com">Gravatar</a>
  image in the sidebar, instead of just a colored line.
</p>
<p>
  If you don't still have it open, find the plugin source in <code>~/.nylas/dev/packages</code> and open the contents in your favorite text editor.
</p>
<blockquote>
  <p>
    We use <a href="https://github.com/jsdf/coffee-react">CJSX</a>, a <a href="http://coffeescript.org/">
    CoffeeScript</a> syntax for <a href="https://facebook.github.io/react/docs/jsx-in-depth.html">JSX</a>, to streamline our plugin code.
    For syntax highlighting, we recommend <a href="https://github.com/babel/babel-sublime">Babel</a> for Sublime, or
    the <a href="https://atom.io/packages/language-cjsx">CJSX Language</a> Atom package.
  </p>
</blockquote>
<h3 id="changing-the-data">Changing the data</h3>
<p>
  Let&#39;s poke around and change what the sidebar displays.
</p>
<p>
  Just like in the last tutorial, you&#39;ll find the code responsible for the sidebar in <code>lib/my-message-sidebar.cjsx</code>. Take a look at
  the <code>render</code> method -- this generates the content which appears in the sidebar.
</p>
<p>
  (How does it get in the sidebar? See <a href="InterfaceConcepts.html">Interface Concepts</a> and look at
  <code>main.cjsx</code> for clues. We&#39;ll dive into this more later in the guide.)
</p>
<p>
  We can change the sidebar to display the contact&#39;s email address as well. Check out the
  <a href="Contact.html">Contact attributes</a> and change the <code>_renderContent</code> method to display more information:
</p>
<pre><code class="lang-coffee">_renderContent: =&gt;
  <span class="hljs-variable">&lt;div className="header"&gt;</span>
  <span class="hljs-variable">&lt;h1&gt;</span>Hi, {@<span class="hljs-keyword">state</span>.contact.name}
  ({@<span class="hljs-keyword">state</span>.contact.email})!<span class="hljs-variable">&lt;/h1&gt;</span>
  <span class="hljs-variable">&lt;/div&gt;</span>
</code></pre>
<p>
  After making changes to the plugin, reload N1 by going to <code>View &gt; Reload</code>.
</p>
<h3 id="installing-a-dependency">Installing a dependency</h3>
<p>
  Now we&#39;ve figured out how to show the contact&#39;s email address, we can use that to generate the
  <a href="http://gravatar.com">Gravatar</a> for the contact. However, as per the
  <a href="https://en.gravatar.com/site/implement/images/">Gravatar documentation</a>, we need to be able
  to calculate the MD5 hash for an email address first.
</p>
<p>
  Let&#39;s install the <code>md5</code> plugin and save it as a dependency in our <code>package.json</code>:
</p>
<pre><code class="lang-bash">$ npm <span class="hljs-operator"><span class="hljs-keyword">install</span>  <span class="hljs-keyword">md5</span> <span class="hljs-comment">--save</span></span>
</code></pre>
<p>
  Installing other dependencies works the same way.
</p>
<p>
  Now, add the <code>md5</code> requirement in <code>my-message-sidebar.cjsx</code> and update the
  <code>_renderContent</code> method to show the md5 hash:
</p>
<pre><code class="lang-coffee">md5 = require <span class="hljs-symbol">'md</span>5'

<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">MyMessageSidebar</span>
  <span class="hljs-keyword"><span class="hljs-keyword">extends</span></span> <span class="hljs-title">React</span>.
  <span class="hljs-title">Component</span>
</span>  <span class="hljs-annotation">@displayName</span>: <span class="hljs-symbol">'MyMessageSideba</span>r'

  ...

  _renderContent: =&gt;
  &lt;div className=<span class="hljs-string">"header"</span>&gt;
  {md5(<span class="hljs-annotation">@state</span>.contact.email)}
  &lt;/div&gt;
</code></pre>
<blockquote>
  <p>
    JSX Tip: The <code>{..}</code> syntax is used for JavaScript expressions inside HTML elements.
    <a href="https://facebook.github.io/react/docs/jsx-in-depth.html">Learn more</a>.
  </p>
</blockquote>
<p>
  You should see the MD5 hash appear in the sidebar (after you reload N1):
</p>
<p>
  <img class="gsg-center" src="images/sidebar-md5.png"/>
</p>
<h3 id="let-s-render-">Let&#39;s Render!</h3>
<p>
  Turning the MD5 hash into a Gravatar image is simple. We need to add an <code>&lt;img&gt;</code> tag to the rendered HTML:
</p>
<pre><code class="lang-coffee"><span class="xml">_renderContent =&gt;
    <span class="hljs-tag">&lt;<span class="hljs-title">div</span>
      <span class="hljs-attribute">className</span>=<span class="hljs-value">"header"</span>&gt;</span>
      <span class="hljs-tag">&lt;<span class="hljs-title">img</span>
        <span class="hljs-attribute">src</span>=</span></span><span class="hljs-expression">{'<span class="hljs-variable">http</span>:/<span class="hljs-end-block">/www.gravatar.com</span><span class="hljs-end-block">/avatar</span>/' + <span class="hljs-variable">md</span>5(@<span class="hljs-variable">state.contact.email</span>)}</span><span class="xml"><span class="hljs-tag">/&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-title">div</span>&gt;</span></span>
</code></pre>
<p>
  Now the Gravatar image associated with the currently focused contact appears in the sidebar. If there&#39;s no image
  available, the Gravatar default will show; you can <a href="https://en.gravatar.com/site/implement/images/">add
  parameters to your image tag</a> to change the default behavior.
</p>
<p>
  <img class="gsg-center" src="images/sidebar-gravatar.png"/>
</p>
<h3 id="styling">Styling</h3>
<p>
  Adding styles to our Gravatar image is a matter of editing <code>stylesheets/main.less</code> and applying the class
  to our <code>img</code> tag. Let&#39;s make it round:
</p>
<p>
<div class="filename">stylesheets/main.less</div>
</p>
<pre><code class="lang-css"><span class="hljs-class">.gravatar</span> <span class="hljs-rules">{
    <span class="hljs-rule"><span class="hljs-attribute">border-radius</span>:<span class="hljs-value"> <span class="hljs-number">45px</span></span></span>;
    <span class="hljs-rule"><span class="hljs-attribute">border</span>:<span class="hljs-value"> <span class="hljs-number">2px</span> solid <span class="hljs-hexcolor">#ccc</span></span></span>;
}</span>
</code></pre>
<p>
<div class="filename">lib/my-message-sidebar.cjsx</div>
</p>
<pre><code class="lang-coffee">_renderContent =&gt;
  gravatar = <span class="hljs-string">"http://www.gravatar.com/avatar/"</span> + md5(@<span class="hljs-keyword">state</span>.contact.email)

  <span class="hljs-variable">&lt;div className="header"&gt;</span>
  <span class="hljs-variable">&lt;img src={gravatar} className="gravatar"/&gt;</span>
  <span class="hljs-variable">&lt;/div&gt;</span>
</code></pre>
<blockquote>
  <p>
    React Tip: Remember to use DOM property names, i.e. <code>className</code> instead of <code>class</code>.
  </p>
</blockquote>
<p>
  You&#39;ll see these styles reflected in your sidebar.
</p>
<p>
  <img class="gsg-center" src="images/sidebar-style.png"/>
</p>
<p>
  If you&#39;re a fan of using the Chrome Developer Tools to tinker with styles, no fear; they work in N1, too. Open
  them by going to <code>Developer &gt; Toggle Developer Tools</code>. You&#39;ll also find them helpful for debugging
  in the event that your plugin isn&#39;t behaving as expected.
</p>
<hr/>

<h2 class="continue"><a href="getting-started-3">Continue this guide: adding a data store to your plugin</a></h2>

<hr/>


<h2 class="gsg-header">Step 3: Adding a Data Store</h2>

<p>
  Building on the <a href="getting-started-2">previous part</a> of our Getting Started guide, we&#39;re going to
  introduce a data store to give our sidebar superpowers.
</p>
<h2 id="stores-and-data-flow">Stores and Data Flow</h2>
<p>
  The Nylas data model revolves around a central <code>DatabaseStore</code> and lightweight <code>Models</code> that
  represent data with a particular schema. This works a lot like ActiveRecord, SQLAlchemy and other
  &quot;smart model&quot; ORMs. See the <a href="database">Database</a> explanation for more details.
</p>
<p>
  Using the <a href="https://facebook.github.io/flux/docs/overview.html#structure-and-data-flow">Flux pattern</a> for
  data flow means that we set up our UI components to &#39;listen&#39; to specific data stores. When those stores
  change, we update the state inside our component, and re-render the view.
</p>
<p>
  We&#39;ve already used this (without realizing) in the <a href="getting-started-2">Gravatar sidebar example</a>:
</p>
<pre><code class="lang-coffee">  componentDidMount: =&gt;
  @unsubscribe = FocusedContactsStore.<span class="hljs-function"><span class="hljs-title">listen</span><span class="hljs-params">(@_onChange)</span></span>
  ...
  _onChange: =&gt;
  @<span class="hljs-function"><span class="hljs-title">setState</span><span class="hljs-params">(@_getStateFromStores()</span></span>)

  _getStateFromStores: =&gt;
  contact: FocusedContactsStore.<span class="hljs-function"><span class="hljs-title">focusedContact</span><span class="hljs-params">()</span></span>
</code></pre>
<p>
  In this case, the sidebar listens to the <code>FocusedContactsStore</code>, which updates when the person selected
  in the conversation changes. This triggers the <code>_onChange</code> method which updates the component state; this
  causes React to render the view with the new state.
</p>
<p>
  To add more depth to our sidebar plugin, we need to:
</p>
<ul>
  <li>Create our own data store which will listen to <code>FocusedContactsStore</code></li>
  <li>Extend our data store to do additional things with the contact data</li>
  <li>Update our sidebar to listen to, and display data from, the new store.</li>
</ul>
<p>
  In this guide, we&#39;ll fetch the GitHub profile for the currently focused contact and display a link to it, using
  the <a href="https://developer.github.com/v3/search/">GitHub API</a>.
</p>
<h2 id="creating-the-store">Creating the Store</h2>
<p>
  The boilerplate to create a new store which listens to <code>FocusedContactsStore</code> looks like this:
</p>
<p>
<div class="filename">lib/github-user-store.coffee</div>
</p>
<pre><code class="lang-coffee">Reflux = <span class="hljs-built_in">require</span> <span class="hljs-string">'reflux'</span>
  {FocusedContactsStore} = <span class="hljs-built_in">require</span> <span class="hljs-string">'nylas-exports'</span>

  <span class="hljs-built_in">module</span>.exports =

  GithubUserStore = Reflux.createStore

  <span class="hljs-attribute">init</span>: <span class="hljs-function">-&gt;</span>
  <span class="hljs-property">@listenTo</span> FocusedContactsStore, <span class="hljs-property">@_onFocusedContactChanged</span>

  <span class="hljs-attribute">_onFocusedContactChanged</span>: <span class="hljs-function">-&gt;</span>
  <span class="hljs-comment"># TBD - This is fired when the focused contact changes</span>
  <span class="hljs-property">@trigger</span>(@)
</code></pre>
<p>
  (Note: You&#39;ll need to set up the <code>reflux</code> dependency.)
</p>
<p>
  You should be able to drop this store into the sidebar example&#39;s <code>componentDidMount</code> method -- all it
  does is listen for the <code>FocusedContactsStore</code> to change, and then <code>trigger</code> its own event.
</p>
<p>
  Let&#39;s build this out to retrieve some new data based on the focused contact, and expose it via a UI component.
</p>
<h2 id="getting-data-in">Getting Data In</h2>
<p>
  We&#39;ll expand the <code>_onFocusedContactChanged</code> method to do something when the focused contact changes.
  In this case, we&#39;ll see if there&#39;s a GitHub profile for that user, and display some information if there is.
</p>
<pre><code class="lang-coffee">request = <span class="hljs-built_in">require</span> <span class="hljs-string">'request'</span>

  GithubUserStore = Reflux.createStore
  <span class="hljs-attribute">init</span>: <span class="hljs-function">-&gt;</span>
  <span class="hljs-property">@_profile</span> = <span class="hljs-literal">null</span>
  <span class="hljs-property">@listenTo</span> FocusedContactsStore, <span class="hljs-property">@_onFocusedContactChanged</span>

  <span class="hljs-attribute">getProfile</span>: <span class="hljs-function">-&gt;</span>
  <span class="hljs-property">@_profile</span>

  <span class="hljs-attribute">_onFocusedContactChanged</span>: <span class="hljs-function">-&gt;</span>
  <span class="hljs-comment"># Get the newly focused contact</span>
  contact = FocusedContactsStore.focusedContact()
  <span class="hljs-comment"># Clear the profile we're currently showing</span>
  <span class="hljs-property">@_profile</span> = <span class="hljs-literal">null</span>
  <span class="hljs-keyword">if</span> contact
  <span class="hljs-property">@_fetchGithubProfile</span>(contact.email)
  <span class="hljs-property">@trigger</span>(@)

  <span class="hljs-attribute">_fetchGithubProfile</span>: <span class="hljs-function"><span class="hljs-params">(email)</span> -&gt;</span>
  <span class="hljs-property">@_makeRequest</span> <span class="hljs-string">"https://api.github.com/search/users?q=<span class="hljs-subst">#{email}</span>"</span>, <span class="hljs-function"><span class="hljs-params">(err, resp, data)</span> =&gt;</span>
  <span class="hljs-built_in">console</span>.warn(data.message) <span class="hljs-keyword">if</span> data.message?
  <span class="hljs-comment"># Make sure we actually got something back</span>
  github = data?.items?[<span class="hljs-number">0</span>] ? <span class="hljs-literal">false</span>
  <span class="hljs-keyword">if</span> github
  <span class="hljs-property">@_profile</span> = github
  <span class="hljs-built_in">console</span>.log(github)
  <span class="hljs-property">@trigger</span>(@)

  <span class="hljs-attribute">_makeRequest</span>: <span class="hljs-function"><span class="hljs-params">(url, callback)</span> -&gt;</span>
  <span class="hljs-comment"># GitHub needs a User-Agent header. Also, parse responses as JSON.</span>
  request({<span class="hljs-attribute">url</span>: url, <span class="hljs-attribute">headers</span>: {<span class="hljs-string">'User-Agent'</span>: <span class="hljs-string">'request'</span>}, <span class="hljs-attribute">json</span>: <span class="hljs-literal">true</span>}, callback)
</code></pre>
<p>
  The <code>console.log</code> line should show the GitHub profile for a contact (if they have one!) inside the
  Developer Tools Console, which you can enable at <code>Developer &gt; Toggle Developer Tools</code>.
</p>
<p>
  You may run into rate-limiting issues with the GitHub API; to avoid these, you can add
  <a href="https://developer.github.com/v3/#authentication">authentication</a> with a
  <a href="https://github.com/settings/tokens">pre-baked token</a> by modifying the HTTP request your store makes.
  <strong>Caution! Use this for local development only.</strong> You could also try implementing a simple cache to
  avoid making the same request multiple times.
</p>
<h2 id="display-time">Display Time</h2>
<p>
  To display this new data in the sidebar, we need to make sure our component is listening to the store, and load the
  appropriate state when it changes.
</p>
<pre><code class="lang-coffee"><span class="hljs-class"><span class="hljs-keyword">class</span>
  <span class="hljs-title">GithubSidebar</span> <span class="hljs-keyword"><span class="hljs-keyword">extends</span></span>
  <span class="hljs-title">React</span>.<span class="hljs-title">Component</span>
</span>  ...
  componentDidMount: =&gt;
  <span class="hljs-annotation">@unsubscribe</span> = <span class="hljs-type">GithubUserStore</span>.listen(@_onChange)

  _onChange: =&gt;
  <span class="hljs-annotation">@setState</span>(@_getStateFromStores())

  _getStateFromStores: =&gt;
  github: <span class="hljs-type">GithubUserStore</span>.getProfile()
</code></pre>
<p>
  Now we can access <code>@state.github</code> (which is the GitHub user profile object), and display the information
  it contains by updating the <code>render</code> and <code>renderContent</code> methods.
</p>
<p>
  For example:
</p>
<pre><code class="lang-coffee">  _renderContent: =&gt;
  <span class="hljs-tag">&lt;<span class="hljs-title">img</span> <span class="hljs-attribute">className</span>=<span class="hljs-value">"github"</span> <span class="hljs-attribute">src</span>=<span class="hljs-value">{@state.github.avatar_url}</span>/&gt;</span> <span class="hljs-tag">&lt;<span class="hljs-title">a</span> <span class="hljs-attribute">href</span>=<span class="hljs-value">{@state.github.html_url}</span>&gt;</span>GitHub<span class="hljs-tag">&lt;/<span class="hljs-title">a</span>&gt;</span>
</code></pre>
<h2 id="extending-the-store">Extending The Store</h2>
<p>
  To make this plugin more compelling, we can extend the store to make further API requests and fetch more data about
  the user. Passing this data back to the UI component follows exactly the same pattern as the barebones data shown
  above, so we&#39;ll leave it as an exercise for  the reader. :)
</p>
<blockquote>
  <p>
    You can find a more extensive version of this example in our <a href="https://github.com/nylas/edgehill-plugins/tree/master/sidebar-github-profile">sample plugins repository</a>.
  </p>
</blockquote>