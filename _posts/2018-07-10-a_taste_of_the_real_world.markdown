---
layout: post
title:      "A taste of the Real World"
date:       2018-07-10 06:17:26 +0000
permalink:  a_taste_of_the_real_world
---


Whenever learning something, particularly in a “school” environment with lessons, quizzes and projects, it is often difficult to differentiate between concepts that are exclusive to the lessons and meant as building blocks versus those one will use in a job setting.  Sinatra and ActiveRecord made this dichotomy clearer as the concepts helped me to better visualize the broad strokes of a typical website with users logging in, creating and/or editing data and traffic through the site.

The lessons concluded with the Sinatra Portfolio Project.  With the exclusion of tests, guidelines and instructions we were given free reign to develop a functional, albeit simple website.  Additionally, the project requirements included a user’s ability to sign up, add/modify data and to view content, in other words, CRUD.

Having done a Twitter, blog and Playlister applications, I chose to build a “Video Game Collection” site where users are able to keep track of their video games.  

This CRUD app is comprised of three models, “Player”, “Game” and “Game_Console,” with the Game model ‘belongs_to the Player model and also the Game model ‘belongs_to the Game_Console model.  The Game_Console has_many Games and the Player has many Games.

I used “Moniker” and “Password” attributes for signup purposes, with Moniker being unique value in the database.  

The signup screen consists of email, moniker, password and password confirmation fields with each field being validated as to prevent blank field submissions as well as the uniqueness of moniker.  Additionally, password and password confirmation fields must match and be of at least 8 characters.

I used a helper method of logged_in? to ensure a player is logged in as well as to aid the controller in routing the player to the proper pages.  The helper method, current_player was used to safeguard against a player modifying a game that is not his or her own:

		i.e. if current_player.id == @game.player_id

The view pages were comprised of html tables for easier viewing with column names being ‘clickable’ to view a game sorted by a particular attribute, for example, all games by a particular moniker, or sorted by an object such as all games belonging to “Nintendo’.  

The app also uses ‘layouts’ pages to be DRY and for easier code readability.  This allowed much of the html to be in the layout pages providing the view pages to have clear differentiation.  Flash messages were added to confirm changes to an entry or following a deletion.

If you are interested this app is available at https://github.com/mohibhashimi786?tab=repositories

Additionally, a video walkthrough is available at 
https://www.youtube.com/watch?v=gJz1Lyb_yP8


