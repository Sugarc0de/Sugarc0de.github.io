## Building a Vocabulary Highlighting App

In this blog post, I am excited to share about my recent web app: VocabAssist. The app eases the pain for ESL students like me to look up each unfamiliar English words in an article. Once you specify the English text and your level in [CEFR](<https://en.wikipedia.org/wiki/Common_European_Framework_of_Reference_for_Languages>) framework, difficult vocabulary will be auto highlighted with their meanings. 

Below is the snippet of my app, of which the text was imported from the exerpt I provided. 



<img src="/images/blog7/app_page.png" width="90%">



>In case you can't wait to dive into the app yourself, the spoiler is here: www.vocabassist.com

Otherwise, I will go over the tech stack I used for this project. 

### Nuxt.js + Vue.js + Element UI

[Vue.js](<https://vuejs.org/>) is a JavaScript framework to build Single-Page Applications (SPA)s, apps that run on the client-side and grab data from the server. Compared to writing plain JavaScript code, Vue.js takes care of lots of things for you, such as creating structures, routing through Vue-router, and states shared across different components in Vuex. 

[Nuxt.js](<https://nuxtjs.org/>) is a framework made for Vue.js. I have yet to explore all features in Nuxt.js, but it allows developers to work with Vue more easily. Although one of the selling points of Nuxt.js is server-side rendering (SSR), I chose to develop my app as SPA. Nuxt.js still provides search engine optimization (SEO) for SPA as I can generate static web pages in the development phase. The picture below shows that Google can crawl my web content without waiting for the Javascript to load. 



<img src="/images/blog7/google_crawl.png">



Check out this post: <https://snipcart.com/spa-seo> if you are interested in learning SEO for SPA. 

Finally, [Element UI](<https://element.eleme.cn/2.0/#/en-US>) provides various components from buttons to dropdowns, with custom styling, and are ready to use in Vue.js. 

### Flask + WSGI server + NGINX

As I write my backend logic using Python, I chose [Flask](<https://flask.palletsprojects.com/en/1.1.x/>) to be the web framework. It has the advantage of being flexible, which allows developers to use different extensions of choice for an ORM, authentication and authorization, etc. Like most web frameworks, core Flask comes with URL routing, views, and error handling, things that sufficient for the need of my app. 

However, Flask is not suitable for directly handling requests in production. By default, it does not respond to more than one user at the same time. And while it can serve static files such as images and CSS, it will be slower than servers dedicated to production. 

[NGINX](<https://www.nginx.com/>) is a free, open-source, high-performance HTTP server that can act as a reverse proxy and serve static content. It can determine whether a client has requested a static file or some dynamic content based on the URL. 

There needs to be a middle man to communicate between NGINX and Flask, that is, an application server. I used WSGI server provided by [gevent](<http://www.gevent.org/intro.html>) to wrap my Flask app and send the HTTP response back to NGINX. 

### EC2 + DynamoDB

AWS offers a 12-month free tier for its services, including [EC2](<https://aws.amazon.com/ec2/>) (cloud computing), [S3](<https://aws.amazon.com/s3/>) (data storage), and [DynamoDB](<https://aws.amazon.com/dynamodb/>) (webscale NoSQL). It is cheap enough to run my app on EC2 and store the user input in DynamoDB for later processing. 

DynamoDB is almost schema-less, and all you have to define beforehand is the attributes that function as keys. 

After some planning, I decided to store user IP as the partition key and the date as the range key. Hopefully, this can make data query a breeze. 

### NLP baseline + StarDict

So far, I have talked about the software aspect of this project. But how does my app knows what words in a paragraph are at the B2 level (intermediate) ? 

The way I implemented this, as a starting point, is to extract A1 to C2 level wordlists from [here](<https://www.toe.gr/course/view.php?id=27>). Then I did an 80-20 split among them, using the training data to build a linear regression classifier. The only feature I used is lexical frequency.  It outperforms many other features combined, such as orthographic features (the number of unique letters in a word, the kinds of phenomes, etc.), according to many papers that tackle a similar problem. 

As for the dictionary, I found one of the many dictionary files from [StarDict](<http://download.huzheng.org/>), an open-source project that allows for quick lookup definitions in a GUI interface. 

### Going forward

I hope you enjoyed this article and can use my app to rocket up your English vocabulary. In the meantime, I am trying to develop an OCR tool to detect highlighters from books or newspapers.

I am looking forward to it, so yeah, stay virtually connected! 