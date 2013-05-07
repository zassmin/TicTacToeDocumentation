# TicTacToe

Learn Rails through TicTacToe. It's test driven, fill in the blanks and...Have fun!

# Getting Started

* `Find the Repo here: https://github.com/zmontesd/TicTacToeSkeleton...`
* `Fork it, instructions here: https://help.github.com/articles/fork-a-repo`
* `Clone it to your computer, follow this: ....`
* `Open the App in your text editor, i.e. Sublime or Komodo Edit`

# Gemfile

* `listed gem version defaults since each have different environments`

run...

~~~~
@@@ ruby

$ bundle install        
// installs any gems that you need, which are listed in your Gemfile

~~~~

# Migrations

~~~~
@@@ ruby

$ rake db:migrate       
// runs any migrations you have and sets up your database and datamodel

~~~~

take a look: /db/

Table: Game with a text column board

# Game.board

let's see our board 

~~~~
@@@ ruby

Array.new(3) { Array.new(3) }

~~~~

we'll view with the board next...

# Rails Console / Pry

* `gem 'Pry'`
* `syntax highlighting`
* `pry commands`

~~~~
@@@ ruby

$ rails c
>> wtf?
>> g = Game.new
>> g.board
>> g.display_board
>> ^d

~~~~

methods used here are found in app/helpers/games_helper.rb

# Board Validator

* `app/validators/game/board.rb`
* `makes sure board length is => Array.new(3) { Array.new(3) } and input 'o', 'x', nil

# Players

meet 'o'

meet 'x'

# Running Tests...

~~~~
@@@ ruby
$ rspec spec/          
// runs all the tests in the project
~~~~

if you get errors, run 

~~~~
@@@ ruby
$ rake db:test:prepare
$ rspec spec/
~~~~

# Game model

* `app/models/game.rb`
* `spec/models/games_spec.rb`

# Resources

* `WWC google group`
* `Ruby Tuesday Core Team`
* `Railsbridge`
* `RubyMonk`
* `Codecademy`
* `Learn to Program`
* `Beginning Ruby`
* `RailsGuides`

!SLIDE

Come Again

# Updating routes.rb

in config/routes.rb

~~~~
@@@ ruby

resources :games
~~~~

~~~~
@@@ ruby

TicTacToe$ rake routes
    games GET    /games(.:format)          games#index
          POST   /games(.:format)          games#create
 new_game GET    /games/new(.:format)      games#new
edit_game GET    /games/:id/edit(.:format) games#edit
     game GET    /games/:id(.:format)      games#show
          PUT    /games/:id(.:format)      games#update
          DELETE /games/:id(.:format)      games#destroy
~~~~

# REST

# GameController

# erb

# params

# partials