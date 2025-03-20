---
title: "Story of a Parameter Specific XSS"
date: Tue, 19 Sep 2017 13:25:00 +0000
draft: false
type: posts
categories: 
- 
---
# Story of a Parameter Specific XSS

<br/>

<br/>
REDIRECTING TO THE NEW BLOG ...
===============================

setTimeout(()=> location="https://blog.noob.ninja/story-of-a-parameter-specific-xss/",3000); Hello Infosec folks!  

                    So I am going to start writing posts related to my bug hunting findings and share it with the community starting with this post.

  

So, this post is about a Reflected XSS I found in a Private Program which has been previously tested many times.This XSS was present on nearly every page of the domain (let's call this private-bounty.com) but wasn't found by anyone before.

  

When I was going through the Application, I found an endpoint which had following in URL:

https://www.private-bounty.com/Deactivate?view=**aaa**&utm\_content=**foo**&utm\_medium=**bar**&utm\_source=**baz**

  

I checked the source code to see if the parameter "view" was reflected somewhere in the page and it was found that the whole URL was reflected in Javascript context(inside Script tags) but except for the parameter "view" and its value.  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhrVZc-FCL1ZfYWkmJABhXwkJ0G-rLAlizbKAx7ck9iIdNmASLuZuWjTciA9fJXvSMfMiVq76ZP1yur6M2OsSRQqlX3WlFWxGPi0D9RJUxjM-dbt9xt0LxaIMKMjlmCyD3MorWzx7YBoHbN/s1600/Screenshot+from+2017-09-19+17-00-24.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhrVZc-FCL1ZfYWkmJABhXwkJ0G-rLAlizbKAx7ck9iIdNmASLuZuWjTciA9fJXvSMfMiVq76ZP1yur6M2OsSRQqlX3WlFWxGPi0D9RJUxjM-dbt9xt0LxaIMKMjlmCyD3MorWzx7YBoHbN/s1600/Screenshot+from+2017-09-19+17-00-24.png)

  

  

  

It got reflected as - 

https://www.private-bounty.com/Deactivate?utm\_content=**foo**&utm\_medium=**bar**&utm\_source=**baz**

  

Then I tried to break out since **foo, bar** and **baz** values were also reflected in the page that but unfortunately, everything was properly encoded, 

  

https://www.private-bounty.com/Deactivate?utm\_content=**foo****'"><>\\**&utm\_medium=**bar****'"><>\\**&utm\_source=**baz****'"><>\\**

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQi4-wK0k4qOWaW8jHheqkQQpJ9MkWSX8yDQrC-EnNnC5MI8Ufr5tVgPniIp6xZE1572Q4N0Pa8lY3F2TsxT2kyT9G06ReKpxUntj-yqCIDeF7QiJlUoaqvphy92U3HWzg02tH5QqmSVKP/s1600/Screenshot+from+2017-09-19+17-05-48.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiQi4-wK0k4qOWaW8jHheqkQQpJ9MkWSX8yDQrC-EnNnC5MI8Ufr5tVgPniIp6xZE1572Q4N0Pa8lY3F2TsxT2kyT9G06ReKpxUntj-yqCIDeF7QiJlUoaqvphy92U3HWzg02tH5QqmSVKP/s1600/Screenshot+from+2017-09-19+17-05-48.png)

  

  

Then I tried to add quotes in the **path itself** but it was also encoded well, so I moved ahead to find something else after not being able to XSS this due to the proper encoding of user input.

  

https://www.private-bounty.com/Deactivate/**'"**?utm\_content=foo'"><>\\&utm\_medium=bar'"><>\\&utm\_source=baz'"><>\\  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj9UKwpZChxGoJ6UqIQGTfXK0oBuDcjGppBU-A4ufBh4k1En-QWsvfJo9f-JtwOEUxbBC2x_OXJKxBFzagU0yS3R6BmXvVF8_s7EZ5WUGBJjgoe7tvR0CTbPbS2Z_DB-4LuYb5fHCO-ntMy/s1600/Screenshot+from+2017-09-19+17-09-38.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj9UKwpZChxGoJ6UqIQGTfXK0oBuDcjGppBU-A4ufBh4k1En-QWsvfJo9f-JtwOEUxbBC2x_OXJKxBFzagU0yS3R6BmXvVF8_s7EZ5WUGBJjgoe7tvR0CTbPbS2Z_DB-4LuYb5fHCO-ntMy/s1600/Screenshot+from+2017-09-19+17-09-38.png)

  

  

I found this pattern of "utm\_content=**foo**&utm\_medium=**bar**&utm\_source=**baz**" on every endpoint getting reflected and no other parameter will be reflected. I tried to append a custom parameter myself to see if it gets reflected, but it didn't work

  

https://www.private-bounty.com/Deactivate?view=**aaa**&utm\_content=**foo**&utm\_medium=**bar**&utm\_source=**baz**&test=xxxxx

After that, I tried to append a parameter named utm\_foobarbaz=xxxxx 

  

https://www.private-bounty.com/Deactivate?utm\_content=**foo**&utm\_medium=**bar**&utm\_source=**baz**&**utm\_foobarbaz=xxxxx**

and it was reflected! into the page, so the application only reflected the parameters beginning with "utm"  
  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgy8kh1JmKbgAc6RBKTQ8gYbaEn7lLCx0iXpQ1zXf_s56t6Q6zE_QupboDzW8p1LyX2P_-V81sOMeJM7UXg0-2SUnMOFRJcL3P1hWWIPzT4oHmlW1IgF0TClQgZ062iym5PjDJf6_hIsnUl/s1600/Screenshot+from+2017-09-19+17-11-40.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgy8kh1JmKbgAc6RBKTQ8gYbaEn7lLCx0iXpQ1zXf_s56t6Q6zE_QupboDzW8p1LyX2P_-V81sOMeJM7UXg0-2SUnMOFRJcL3P1hWWIPzT4oHmlW1IgF0TClQgZ062iym5PjDJf6_hIsnUl/s1600/Screenshot+from+2017-09-19+17-11-40.png)

  

so I tried again to break the context to achieve XSS using this parameter's value but it was also encoded well :(

  

Then the last try I did was to break the context by putting the payload in the parameter name itself 

  

https://www.private-bounty.com/Deactivate?utm\_content=**foo**&utm\_medium=**bar**&utm\_source=**baz&utm\_foobarbaz'"</>=xxxxx**

  

and boom! it worked :D, the parameter names beginning with "utm" were not being encoded when reflected in the page.

  

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjnUe_hx0J-isgzU24cypZAtMyrFW8HC8aMS2FHgOE38Ty8gvJWHjE9gLOW9BqljuQnW-Om8OZDuj2MPEFWu4IiITOzf-AdAWyDLtewUEi3ZEBpQp5Cv_zCSrPKHojR602MAUMZcWlLo7KS/s1600/Screenshot+from+2017-09-19+17-13-47.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjnUe_hx0J-isgzU24cypZAtMyrFW8HC8aMS2FHgOE38Ty8gvJWHjE9gLOW9BqljuQnW-Om8OZDuj2MPEFWu4IiITOzf-AdAWyDLtewUEi3ZEBpQp5Cv_zCSrPKHojR602MAUMZcWlLo7KS/s1600/Screenshot+from+2017-09-19+17-13-47.png)

  

  

and That's how we alert :p , 

  

https://www.private-bounty.com/Deactivate?utm\_content=foo&utm\_medium=bar&utm\_source=baz&**utm\_foobarbaz');alert(1)//**

The lesson is that we should also always try to inject/fuzz the parameter names themselves and this was just a special case of such an XSS in parameter name beginning with a specific keyword "utm".

\- Rahul Maini

#### [Source](https://www.noob.ninja/2017/09/story-of-parameter-specific-xss.html)

<br/>
---
