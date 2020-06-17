# OWASP Juice Shop Solutions
###### Compiled by [Agam Jolly](https://www.agamjolly.com)
Unique solutions to the OWASP Juice Shop vulnerability challenge. 

## Deployment 

My website has been deployed to Heroku, and can be accessed [here](http://www.agamjolly-juiceshop.herokuapp.com).

You can directly deploy this app on your own Heroku account using the following link. 

[some link](lorem_ipsum)

## Common Approaches

### Routes
To extract vulnerabilites, having a comprehensive list of all directories can be a huge boon. The commonly used <b>Inspect Element</b> can be particularly helpful for extracting a list of all sources and external libraries or scripts being used by a webpage. 

In the toolbox, it is easy to notice that a compiled Angular file has some valuable source code in it. Among most of these files is the `main-es2015.js` file. Google Chrome provides pretty-print to automatically clean compressed source files, including standard HTML and JS files.

After cleaning up the main `main-es2015.js` file, you can find different routes that point to distinct paths. A JSON file contains all these paths. Pasting these paths to the standard endpoint [here](agamjolly-juiceshop.herokuapp.com/#/) opens up these unique URLs. 

Alternatively you can use directory bruteforce with a Python Selenium script to see which pages return a `200 OK` response and which ones return a `404 NOT FOUND` response. We can simply neglect all `404 NOT FOUND` responses, and return a list of routes that point to paths that exist. 

### XSS Attacks

Since search queries are passed as standard HTML code in the app, it can be easily manipulated to import a new frame in the DOM. This is a major security vulnerability.

## Solutions

### Scoreboard

Using the common route approach discussed above, we can find that scoreboard can be accessed <a href="agamjolly-juiceshop.herokuapp.com/#/score-boar">here</a>. 

This solves the scoreboard problem. 

### Confidential Document (FTP Server)

Port 21 on web servers is mostly reserved for FTP requests that allow for really fast file transmissions due to dedicated transmission protocols. Here, without an extra port, the FTP browser client-side could be reached using a statoc route.

Using the common route approach discussed above, the client-side FTP server could be found <a href="agamjolly-juiceshop.herokuapp.com/ftp">here</a>.

### DOM XSS Attack

This follows from the second type of common approaches talked about in the first half of this document. 

Pasting the following `iframe` into the search bar could exploit the XSS vulnrability in the app, since the search bar uses a mere `input` HTML tag to send GET/POST requests to the web server: 

```HTML 
<iframe src="javascript:alert(`xss`)">
```

### Bonus Payload

Follows the same logic and approach as the <b>DOM XSS Attack</b>, with the only change this time being the inclusion of a different script, which happens to be a call to the SoundCloud API to play a cringe song. 

```HTML
<iframe width="100%" height="166" scrolling="no" frameborder="no" allow="autoplay" src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/771984076&color=%23ff5500&auto_play=true&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true"></iframe> 
```

### Error Handling

Works by invoking one type of error that is not documented by the creator of the app. An example of this could be making an app of your own but forgetting to make a custom 404 page. 

This challenge can be solved using basic exception inputting at the login page. Passing just about any invalid input, like `^` as the username/email should do the job.

### Privacy Policy

You can access the privacy policy by logging in and then opening the privacy policy in the account menu on the central dashboard. 

I don't know why this challenge really exists. 


### Metrics

Can be solved using the routes approach, although automated directory bruteforcing could be particularly easy. 

The URL can be accessed <a href="https://agamjolly-juiceshop.herokuapp.com/metrics">here</a>. 


### Missing Encoding (Bjoern's Cat)

There might be other ways to solve this, but HTTrack can clone the relevant static parts of the website locally. 

Upon cloning, the `assets` folder has all sorts of files. It took me only a matter of seconds to figure out that `%F0%9F%98%BC-` was the encoding being used. The actual path can be found <a href="https://agamjolly-juiceshop.herokuapp.com/assets/public/images/uploads/%F0%9F%98%BC-%23zatschi-%23whoneedsfourlegs-1572600969477.jpg">here</a>. 


### Outdated Whitelist

This works by manipulating redirect whitelists using path encoding. The server throws a `406` or `500` request upon forced redirects. To avoid this, with a little bruteforcing, one can simply come up with simple patterns to obfuscate the whitelisting protocol. 

To start with, a redirect link needs to be found which can be partiularly found in the GitHub SVG in the navbar. This points to the GitHub repository where the actual source for the project is hosted. 

One way to do this could be to keep a whitelisted URL in the path while including a non-whitelisted path. This should confuse the redirect protocol to believe that a whitelisted URL is present, even though technically it is of no use. 

The actual path can be found <a href="https://agamjolly-juiceshop.herokuapp.com/redirect?to=https://www.agamjolly.com?to=https://github.com/bkimminich/juice-shop">here</a>.

# Other Challenges

These are other challenges I solved but didn't complete any documentation for.

- Admin Section
- Login Admin
- View Basket
- Security Policy
- Five-Star Feedback
- Upload Size 
- CAPTCHA Bypass
- Zero Star Review
- Admin Registration
- Reset Bender's Password
- Bjoern's Favourite Pet
- Login Jim
