---
layout: post
title:      "Things to Take Note of:  REST, Rendering, Redirect, and Protecting Routes"
date:       2020-12-01 04:42:26 +0000
permalink:  things_to_take_note_of_rest_rendering_redirect_and_protecting_routes
---


As I'm moving along in Rails, I've come to understand that certain concepts are important to revisit and have a have a firm understaning of. 
## REST

Render tells Rails which view or asset to show a user, without losing access to any variables defined in the controller action.

From what I've learned about REST (REpresentational State Transfer) so far in the Rails module, it is a standard methodology of how web applications handle and structure URLs. It also provides a cleaner way for developers to map out the connections between controller actions and its corresponding URL resource. Lastly, REST provides a handful of tools and methods for developers so that as they develop their apps, handling HTTP verbs would not only be simpler, but also conventional in a way that other developers can follow along in backend side of their application. 

## Rendering vs Redirecting

Rendering communicates with your Rails server and selects the view page to show the user of the application. At the same time, it allows for the page to maitain access to instance variable provided in the controller action.

**Example:
**
``` 

get '/posts' do  
        @posts = Post.all.reverse
        erb :'/post/post_index'
    end 
		
```

The code snippet above would be an example of a controller action providing the page `'/post/post_index' ` access to the `@post` variable.

Redirecting on the other hand sends a fresh request to another URL without unneccessarily providing the page  access to a variable defined in the controller that it would not need.

```

def update
  @post = Post.find(params[:id])
  if @post.update(post_params)
    redirect_to(@post)
    else
    render “edit”
  end
end

```
In the code snippet above, the code tells Rails that the variable is not needed in the first past of our conditional code. On the other hand, the` render "edit"` line allows the the page to have access to the params stored in the variable, which would would be used to prepopulate the form(s) on the edit page.

## HTTP : Stateless Protocol 

 HTTP  is a stateless protocol which, simply put, means that connection between the server side and a client's browser is temporal. What this means is that each request is made without any knowledge of the requests that were made before it, and that connection is dropped after the request is rendered to the client.
 
## Protecting Routes

 Lastly, another important concept I learned from the Sinatra Portfolio Project was the idea of protecting the manipulation of views from users without the right credentials. 
 
  **Exmaple**:
 
 ```
 
 patch '/drinks/:id' do
        @drink =  Drink.find(params[:id])
        if current_user != @drink.user
            redirect '/drinks'
        elsif !params["drink"]["name"].empty? && !params["drink"]["instructions"].empty?
            @drink.update(params["drink"])
            redirect "/drinks/#{params["id"]}"
        else
            @error = "Data invalid. Please try again."
            erb :"/drinks/edit"
        end
    end 
		
 ```
 
 In the code snippet above, I took away the ability of users sending a patch request if they were not the owner of the drink.
 
 ```
 
     @drink =  Drink.find(params[:id])
        if current_user != @drink.user
            redirect '/drinks'
						
 ```
 
 Here would be an example of me providing a layer of protection over user data by making sure the current user's id matched the user_id of the drink in order to edit the properties of the params of `@drink`.
 
 ### All in All
 
Ultimately, there are alot more concepts to take note of as I progress through this module, but these would be the few  "gems"  I found to be important to understand before moving forward.
