---
layout: post
title:      "Some of What Sinatra Has Taught me"
date:       2020-11-16 23:09:28 +0000
permalink:  some_of_what_sinatra_has_taught_me
---


![](https://media.giphy.com/media/a5viI92PAF89q/giphy.gif)


 From this module, I've relearned the importance of syntax, the MVC model, associations, and coffee. I've learned 
the importance of  purposeful coding through work flow and anticipating user friendly navigation. Using the MVC model, developers establish  blueprints of the user experience, and also a layout of what data to show user at a giving point in time. I used this model to develop an application that allowed users to sign up, and establish a session for an application that allowed them to see list of cocktail drinks that had been created by other users. 



```
    post '/login' do
        if params["username"].empty? || params["password"].empty?
          @error  = "Must fill in both input fields"
          erb :'/users/login'
        else 
          if user = User.find_by(username: params["username"], password: params["password"])
                session[:user_id] = user.id
                redirect to '/drinks'
          else
            @error = "Account not found"
            erb :'/users/new' 
          end
        end
    end
```


I would say one of the most important parts of a Sinatra application that has a database of users, is the logic behind user authentication and the sessions controller. Here is a piece of code I had in my sessions controller that allowed me to validate user input in the login form. 

One proud moment in particular I had during project development, was incorporating what I learned from the CLI Project to include an API in my application, whose purpose was to provide seed data.
```

        def find_drink_by_name
            drinksdb = JSON.parse(RestClient.get(BASE_URL))
           
            drinksdb["drinks"].each do |drink|
                @drink = Drink.new(name: drink["strDrink"], pic: drink["strDrinkThumb"], instructions: drink["strInstructions"]
                )
                @drink.save
            end 
        end
```

Also among the concepts I learned during this project, was the importance of having different controllers to serve the different models in your program. This reminded me of the concept of having different class and folders to serve different purposes of a CLI. An exmaple of this in my project would be my drink controller and how I wanted my application to handle user input when they reached a certain part of the website.
```
    post '/drinks' do
        filter = params.reject{|key, value| key == "pic" && value.empty?}
        drink = current_user.drinks.build(filter)
        drink.pic = nil if drink.pic.empty?
        if !drink.name.empty? && !drink.instructions.empty?
            drink.save
            redirect to '/drinks'
         else
            @error = "Data invalid. Please try again."
         erb :"/drinks/new"
        end
    end```
	
There is so much more I learned during the development of my Sinatra application, while on the other hand, the CLI project helped me use what I learned about API  to feed some data into my application. Remebering the importance, of API, learning the beauty of ActiveRecord, and exploring concepts such as sessions, have left me grasping a much larger understanding of how web application work.

Mood:

![](https://media.giphy.com/media/l0Iy69RBwtdmvwkIo/giphy.gif)

