!SLIDE 
# Erlang and Batman.js #
## Tristan Sloughter (tristan@mashape.com @t_sloughter)

![Mashape](mashape-logo.png)

!SLIDE bullets 
# Contents #

* Types of Web Frameworks
* Javscript Frameworks
* Erlang for RESTful API

!SLIDE center
# Cowboy-Batman! #
![Cowboy-Batman](cowboy-batman.jpg)

!SLIDE bullets 
#  Why My Way? #

* Right tool for the job
* API from the beginning 
* Total separation

!SLIDE bullets 
# Frameworks #

* MVC (Rails, Django, Play!...)
* Totalitarian? (Opa, Meteor...)
* Other (Yesod, Nitrogen...)

!SLIDE center
# Confusion #
![Confsion](confused.jpg)

!SLIDE center
# Erlang for Web Development? #

!SLIDE center
# I know, I'll convert Erlang terms to HTML and functions to generate Javascript! #

!SLIDE center
# Now you have 2 problems! #
![Facepalm](facepalm.jpg)

!SLIDE center
# Throw it all away #

![Trash](trash.jpg)

!SLIDE center
# Simplify! #

!SLIDE center
# Browser \<- JSON -> Backend #

!SLIDE center
![Maru](maru_v1.png)

!SLIDE execute
# Nginx Config #
    @@@ Javascript
    server {
      listen 80;
      server_name localhost;
    
      location / {
        root .../bcmvc/lib/bcmvc_web/priv/;
    
      if ($request_method ~* POST) {
        proxy_pass http://localhost:8080;
      }
    
      if ($http_accept ~* application/json) {
        proxy_pass http://localhost:8080;
      }
    }

    
!SLIDE center
# JS MVC Advantages #

![Trash](someone_else.png)

!SLIDE bullets
# JS MVC Advantages #

* Backend developer safety!
* Nothing new to learn for frontend devs
* No waiting on updates to template engine
* Node.js...

!SLIDE center
# Batman.js Example #

![Batman.js](batmanjs.png)

!SLIDE center
# TodoMVC #
![TodoMVC](todomvc.png)

!SLIDE code
# View #

!SLIDE code
# Model #

!SLIDE code
# Controller #

!SLIDE bullets
# Erlang Apps #

* Webmachine or Cowboy for REST
* JSX or Jiffy for JSON
* Maru for extras

!SLIDE center
# Cowboy Example #

!SLIDE code
# Dispatch #

!SLIDE code
# Authorize #

!SLIDE code
# CRUD #

!SLIDE bullets
# References #

* Batman.js (http://batmanjs.org/)
* Cowboy (https://github.com/extend/cowboy)
* Webmachine (https://github.com/basho/webmachine)
* JSX (https://github.com/talentdeficit/jsx)
* Jiffy (https://github.com/davisp/jiffy)
* Maru (https://github.com/tsloughter/maru)

!SLIDE center
![Fab](fab.jpg)
