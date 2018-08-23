
# Secure React

ReactJS is a popular JavaScript library for building user interfaces. It enables client-rendered, “rich” web apps that load entirely upfront, allowing for a smoother user experience.

ReactJS is quite safe by design as long as it is used the way it’s meant to be used. For example, string variables in views are escaped automatically. However, as with all good things in life, it’s not impossible to mess things up.

https://snyk.io/test/npm/react/15.3.0

So, most of these issues are introduced by bad programming practices -

- Creating React components from user-supplied objects

- Rendering links with user-supplied href attributes, or other HTML tags with injectable attributes (link tag, HMTL5 imports)

- Explicitly setting the `dangerouslySetInnerHTML` prop of an element

- Passing user-supplied strings to `eval()`

### Let's see this in action

**1. Creating React components from user-supplied objects**

JSX syntax is converted into JS code by the transpiler like this,

    // JSX

	const element = (  
	  <h1 className=”greeting”>  
	    Hello, world!  
	  </h1>  
	);

	// Transpiled to createElement() call

	const element = React.createElement(  
	  ‘h1’,  
	  {className: ‘greeting’},  
	  ‘Hello, world!’  
	);

New React elements are created from component classes using the `createElement()` function:

	
	React.createElement(  
	  type,  
	  [props],  
	  [...children]
	)
	
So, the vulnerable code may look like,

	attacker_supplied_value = JSON.parse(some_user_input)
	
	React.createElement(
	  "span", 
	  null, 
	  attacker_supplied_value
	);
	
	attacker_props = JSON.parse(stored_value)
	React.createElement("span", attacker_props};

***Developers: Don't parse user supplied json and never use `eval()` function. This may lead to XSS attack.***

**2. Rendering links with user-supplied `href` attributes, or other HTML tags with injectable attributes (`link` tag, HMTL5 imports)**
		
	<a href={userinput}>Link</a>
	<button form="name" formaction={userinput}>
	<link rel=”import” href={user_supplied}>
	

**3. Explicitly setting the `dangerouslySetInnerHTML` prop of an element**

	

    <div dangerouslySetInnerHTML={user_supplied} />

Example of dangerous payload,

	`{"dangerouslySetInnerHTML" : { "__html": "<img src=x/ onerror=’alert(localStorage.access_token)’>"}}`

**4. Passing user-supplied strings to `eval()`**

	function antiPattern() {  
	  eval(this.state.attacker_supplied);  
	}
The danger of `eval()` is when it is executed on unsanitised values, and can lead to a [DOM Based XSS](https://www.owasp.org/index.php/DOM_Based_XSS) vulnerability.

e.g. consider the following code in your HTML (rather contrived, but it demonstrates the issue I hope)

	
	<script>

	eval('alert("Your query string was ' + unescape(document.location.search) + '");');

	</script>
	

Now if the query string is `?foo` you simply get an alert dialog stating the following: `Your query string was ?foo`

But what this code will allow a user to do is redirect users from their site to a URL such as `http://www.example.com/page.htm?hello%22);alert(document.cookie+%22`, where www.example.com is your website.

This modifies the code that is executed by `eval()` to

	
	alert("Your query string was hello");
	alert(document.cookie+"");
	


### Additionally, here are few checklists for frontend technologies -

https://html5sec.org/

https://checkmarx.gitbooks.io/js-scp/
