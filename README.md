# Secure React

ReactJS is a popular JavaScript library for building user interfaces. It enables client-rendered, “rich” web apps that load entirely upfront, allowing for a smoother user experience.

ReactJS is quite safe by design as long as it is used the way it’s meant to be used. For example, string variables in views are escaped automatically. However, as with all good things in life, it’s not impossible to mess things up.

So, most of these issues are introduces by bad programming practices -

- Creating React components from user-supplied objects

- Rendering links with user-supplied href attributes, or other HTML tags with injectable attributes (link tag, HMTL5 imports)

- Explicitly setting the `dangerouslySetInnerHTML` prop of an element

- Passing user-supplied strings to `eval()`
