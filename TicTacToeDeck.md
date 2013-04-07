# TicTacToe

Creating TicTacToe as a tool to learn how to make a rails app. Users will be WWC, RubyTuesday ladies. Documenting TicTacToe as it's built. Building project one feature at a time. And of course TestTestTest!

Please note - this is a rough draft. 

# test Board.rb

Starting with the test...checking whether board.rb and Board class exist

~~~~
@@@ ruby
require 'spec_helper'
require 'board'

describe Board do 
	
end
~~~~

# watch it fail

~~~~
@@@ ruby
TicTacToe$ rspec spec
/Users/zmontesd/.rvm/gems/ruby-1.9.3-p194@global/gems/bundler-1.1.4/lib/bundler/runtime.rb:31:in `block in setup': You have already activated rspec-core 2.12.2, but your Gemfile requires rspec-core 2.10.1. Using bundle exec may solve this. (Gem::LoadError)
~~~~

we wanted the tests to fail, they didn't even run....

# error handling

running bundle exec...just trying to specify a particular rspec version to use...
...this is where pairing comes in handy... 

~~~~
@@@ ruby

	TicTacToe$ bundle exec rspec spec
	WARNING: Cucumber-rails required outside of env.rb.  The rest of loading is being defered until env.rb is called.
  	To avoid this warning, move 'gem cucumber-rails' under only group :test in your Gemfile

~~~~

!SLIDE

Yes, googled error,
...don't know why I would need to reinstall cucumber, but here we go...

~~~~
@@@ ruby
$ rails g cucumber:install
$ rails g cucumber:install capybara
$ rake cucumber
~~~~

!SLIDE

ran...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec
~~~~

and again... 

~~~~
@@@ ruby
WARNING: Cucumber-rails required outside of env.rb.  The rest of loading is being defered until env.rb is called.
  To avoid this warning, move 'gem cucumber-rails' under only group :test in your Gemfile
~~~~

!SLIDE

google again...I need expert help here, I don't know why this is necessary...

~~~~
@@@ ruby
gem 'cucumber-rails', :require => false
~~~~

and of course...

~~~~
@@@ ruby
TicTacToe$ bundle install
~~~~

!SLIDE

and...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec
Rack::File headers parameter replaces cache_control after Rack 1.5.
/Users/zmontesd/.rvm/gems/ruby-1.9.3-p194@rails3tutorial2ndEd/gems/activesupport-3.2.11/lib/active_support/dependencies.rb:251:in `require': cannot load such file -- board.rb (LoadError)
~~~~

...yay, that's because it doesn't exist yet!!! My initial expected error!

# Creating Board model

via file... board.rb
keeping it empty for now...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec
Rack::File headers parameter replaces cache_control after Rack 1.5.
/Users/zmontesd/Sites/current/TicTacToe/spec/models/board_spec.rb:4:in `<top (required)>': uninitialized constant Board (NameError)
~~~~

# Adding Board class

~~~~
@@@ ruby
class Board

end
~~~~

running rspec...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec
Rack::File headers parameter replaces cache_control after Rack 1.5.
No examples found.


Finished in 0.00011 seconds
0 examples, 0 failures
~~~~

# Adding Tests, initializing the board

~~~~
@@@ ruby
describe Board do 
	before do
		@show_board = Board.new
	end

	it "should be initialized with nil values inside of the array, [[]]" do
		@show_board.board.should == [[nil, nil, nil], 
									 [nil, nil, nil], 
									 [nil, nil, nil]] 
	end
end
~~~~

!SLIDE

watch it fail...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec
Rack::File headers parameter replaces cache_control after Rack 1.5.
F

Failures:

  1) Board should be initialized with nil values inside of the array, [[]]
     Failure/Error: @show_board.board.should == [[nil, nil, nil],
     NoMethodError:
       undefined method `board' for #<Board:0x007fd0fe0b2878>
     # ./spec/models/board_spec.rb:10:in `block (2 levels) in <top (required)>'

Finished in 0.01067 seconds
1 example, 1 failure
~~~~

!SLIDE

yay! But again where pair programming would be useful...I don't know if we want the
default value to be nil for the array...or if it should be something else, keeping it nil until we decide otherwise.

~~~~
@@@ ruby
class Board
	attr_accessor :board

	def initialize 
		@board = [[nil, nil, nil], 
				  [nil, nil, nil], 
				  [nil, nil, nil]]
	end
end
~~~~

# Passing test on board initialization

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec
Rack::File headers parameter replaces cache_control after Rack 1.5.
.

Finished in 0.01145 seconds
1 example, 0 failures
~~~~

# Now our board array's length is 3...

We are expecting this to pass since we already the array set to 3

~~~~
@@@ ruby
	
	it "should always have a length of 3" do 
		@show_board.board.length.should == 3
	end

~~~~

~~~~
@@@ ruby

	TicTacToe$ bundle exec rspec spec
	Rack::File headers parameter replaces cache_control after Rack 1.5.
	..

	Finished in 0.0145 seconds
	2 examples, 0 failures

~~~~

# Since there is an array within the an array, let's make sure their length is always 3...

also expecting this to pass without changes

~~~~
@@@ ruby
	
	it "should always have the array within the array length of 3" do
		@show_board.board.each { |array| array.length.should == 3 } 		
	end
~~~~

~~~~
@@@ ruby
	TicTacToe$ bundle exec rspec specify
	Rack::File headers parameter replaces cache_control after Rack 1.5.
	...

	Finished in 0.01379 seconds
	3 examples, 0 failures
~~~~

# Now that we have the details of our board, let's establish the details of placing a player onto the board

~~~~
@@@ ruby

	describe "assign_player_position" do 
		it "should establish a position on the board, using 'x' or 'o'" do
			p = @show_board.assign_player_position('x', 0, 1)
			p.should == @show_board.board[0][1]
		end
	end

~~~~

running test...

~~~~
@@@ ruby

	TicTacToe$ bundle exec rspec spec/models/
	Rack::File headers parameter replaces cache_control after Rack 1.5.
	...F

	Failures:

  	1) Board assign_player_position should establish a position on the board, using 'x' or 'o'
     	Failure/Error: p.should == @show_board.board[0][0]
       		expected: nil
            got: "x" (using ==)

~~~~

!SLIDE

yay!...it failed! Now for the method to assign a position...

~~~~
@@@ ruby
	
	def assign_player_position(player, row, column)
		@board[row][column] = player
	end

~~~~

~~~~
@@@ ruby
	
	TicTacToe$ bundle exec rspec spec/models/
	Rack::File headers parameter replaces cache_control after Rack 1.5.
	....

	Finished in 0.01381 seconds
	4 examples, 0 failures

~~~~

beautiful....

!SLIDE

we've got to make something to dispay the board now...of course, another method, test first..

~~~~
@@@ ruby

	describe "display_board" do 
		it "should display board" do 
			@show_board.display_board.should == " | | \n" + 
												"_ _ _\n" +
												" | | \n" +
     											"_ _ _\n" +
												" | | \n"
		end
	end
~~~~

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/models/
Rack::File headers parameter replaces cache_control after Rack 1.5.
....F

Failures:

  1) Board display_board should display board
     Failure/Error: " | | \n"
       expected: " | | \n_ _ _\n | | \n_ _ _\n | | \n"
            got: nil (using ==)
     # ./spec/models/board_spec.rb:36:in `block (3 levels) in <top (required)>'

Finished in 0.01675 seconds
5 examples, 1 failure
~~~~

!SLIDE

it failed! so nice!...let's back up, how do we make the board look that way. Well, we know it's going to be a string but it's either player ('x' or 'o') is going to have to appear on the board. let's break this down even further, we want another method to display the element in the array, for now, let's call this display element.

~~~~
@@@ ruby

describe "display_element" do
		it "should display @board element" do
			mock_player = @show_board.assign_player_position('x', 0, 1)

			to_test = @show_board.display_element(0,1)
			to_test.should == mock_player
		end
~~~~

!SLIDE

alright...we made a mock_player to assign the play position, we want to specify at which point and how that player position should be displayed. In the first test, we want the player we assign to be displayed rather than nil. Let's watch it fail...


# watching the test above fail is missing. TODO

!SLIDE

...now let's test make it so whenever there is a nil element in the array, we make a space. 

~~~~
@@@ ruby

		it "should display a space if the element is nil" do
			@show_board.display_element(0,0).should == ' '
		end
~~~~

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/models/
Rack::File headers parameter replaces cache_control after Rack 1.5.
.....F

Failures:

  1) Board display_element should display a space if the element is nil
     Failure/Error: @show_board.display_element(0,0).should == ' '
       expected: " "
            got: nil (using ==)
     # ./spec/models/board_spec.rb:39:in `block (3 levels) in <top (required)>'

Finished in 0.01616 seconds
6 examples, 1 failure
~~~~

!SLIDE

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/models/
Rack::File headers parameter replaces cache_control after Rack 1.5.
.....

Finished in 0.0151 seconds
5 examples, 0 failures

it "should display a space if the element is nil" do
	@show_board.display_element(0,0).should == ' '
end
~~~~

# if the element is not a space, let's make sure it a player we want, 'x' or 'o'...

~~~~
@@@ ruby

		it "should display an 'x' or an 'o' if the element is not a space" do
		  @show_board.assign_player_position('x', 0, 1)
		  @show_board.display_element(0,1).should == 'x'
		end
~~~~

# TODO show the failing test...

!SLIDE

~~~~
@@@ ruby

	def display_element(row, column)
		element = @board[row][column]
		if element
			element
		else
			' '
		end
	end
~~~~

# TODO so the passing test...


...alright. back to think about displaying the board on a high level. We've specify when to display a player and space...let's make a board...

~~~~
@@@ ruby

	def display_board
		puts "#{display_element(0,0)}|#{display_element(0,1)}|#{display_element(0,2)}\n" +
	         "- - -\n" +
		     "#{display_element(1,0)}|#{display_element(1,1)}|#{display_element(1,2)}\n" +
		     "- - -\n" +
		     "#{display_element(2,0)}|#{display_element(2,1)}|#{display_element(2,2)}\n"
	end

~~~~

# TODO more for the board...

!SLIDE

refactoring Board#display_board - added a display_line method:

~~~~
@@@ ruby
def display_line(num)
	"#{display_element(num,0)}|#{display_element(num,1)}|#{display_element(num,2)}\n"
end
~~~~

and the new display_board method:

~~~~
@@@ ruby
def display_board
  display_line(0) +
  "- - -\n" +
  display_line(1) +
	"- - -\n" +
  display_line(2)
end
~~~~


# Note:
Eventually this display_board function won't actually be necessary.  In the views, we can draw a permanent grid and insert the value of each cell into it.
For now, let's test the display line in tictactoe. the test...

~~~~
@@@ ruby

	describe "display_line" do 
		it "should display player marks or a space (if nil) in the designated row" do 
			@show_board.assign_player_position('x', 0, 2)

			to_test = @show_board.display_line(0)
			to_test.should == " | |x\n"
		end
	end
~~~~

!SLIDE

and it fails...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/models/
Rack::File headers parameter replaces cache_control after Rack 1.5.
.......F

Failures:

  1) Board display_line should display player marks or a space (if nil) in the designated row
     Failure/Error: to_test = @show_board.display_element(0,2).display_row(0)
     NoMethodError:
       undefined method `display_line' for "x":String
     # ./spec/models/board_spec.rb:52:in `block (3 levels) in <top (required)>'

Finished in 0.02304 seconds
8 examples, 1 failure
~~~~

let's make it pass....

~~~~
@@@ ruby

    def display_line(row)
   		"#{display_element(row,0)}|#{display_element(row,1)}|#{display_element(row,2)}\n"
    end
~~~~

!SLIDE

now we need to test that line shows up on the board, we have the board method already, let's unpend the display board tests and add a new test....

~~~~
@@@ ruby

    	it "should display player marks on the board" do
    		@show_board.assign_player_position('x', 0, 0)
    		@show_board.assign_player_position('o', 1, 1)
    		@show_board.assign_player_position('x', 2, 2)

    		@show_board.display_board.should == " | | \n" + 
	 											"- - -\n" +
	 											" | | \n" +
      											"- - -\n" +
	 											" | | \n"
		end
~~~~

let's just see if it's working, it should fail, once it does, we'll update the test.

!SLIDE

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec/models/
Rack::File headers parameter replaces cache_control after Rack 1.5.
.........F

Failures:

  1) Board display_board should display player marks on the board
     Failure/Error: " | | \n"
       expected: " | | \n- - -\n | | \n- - -\n | | \n"
            got: "x| | \n- - -\n |o| \n- - -\n | |x\n" (using ==)
       Diff:
       
       
       @@ -1,6 +1,6 @@
       - | | 
       +x| | 
        - - -
       - | | 
       + |o| 
        - - -
       - | | 
       + | |x
     # ./spec/models/board_spec.rb:76:in `block (3 levels) in <top (required)>'

Finished in 0.02126 seconds
10 examples, 1 failure
~~~~

!SLIDE

Yay! failing test. Now, I'm going to rewrite it so it can pass...

~~~~
@@@ ruby

    		@show_board.display_board.should == "x| | \n" + 
	 											"- - -\n" +
	 											" |o| \n" +
      											"- - -\n" +
	 											" | |x\n"
~~~~

and...

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec/models/
Rack::File headers parameter replaces cache_control after Rack 1.5.
..........

Finished in 0.02526 seconds
10 examples, 0 failures
~~~~
 
!SLIDE

Changing things up a bit and will shift methods in board.rb to game.rb, first thing will be to generate a model with players (x and o), current_player, board, created_at, status, and updated_at. Then we'll rewrite the tests for the board class and test for some of the methods. It will now be a game class. Keeping this in my repo until we decide we want this to be it! Pumped!!!

!SLIDE

~~~~
@@@ ruby

TicTacToe$ rails generate model Game board:text player_o:string player_x:string current_player:string status:string created_at:datetime updated_at:datetime
      invoke  active_record
      create    db/migrate/20130406222125_create_games.rb
      create    app/models/game.rb
      invoke    test_unit
      create      test/unit/game_test.rb
      create      test/fixtures/games.yml
~~~~

!SLIDE

now migration time...

~~~~
@@@ ruby

TicTacToe$ rake db:migrate
==  CreateGames: migrating ====================================================
-- create_table(:games)
   -> 0.0026s
==  CreateGames: migrated (0.0028s) ===========================================
TicTacToe$ rake db:test:prepare

~~~~

# adding defaults to the schema...

~~~~
@@@ ruby

ActiveRecord::Schema.define(:version => 20130406222125) do

  create_table "games",          :force => true do |t|
    t.text     "board",          :default => "[[],[],[]]"
    t.string   "player_o"
    t.string   "player_x"
    t.string   "current_player"
    t.string   "status",         :default => "in_progress"
    t.datetime "created_at",     :null => false
    t.datetime "updated_at",     :null => false
  end

end
~~~~

# updating game.rb by adding the Board class and changing it to the game class, using inheritance to call on ActiveRecord

~~~~
@@@ ruby

class Game < ActiveRecord::Base
~~~~ 

changing attr_accessor to...

~~~~
@@@ ruby

attr_accessible :board, :player_o, :player_x, :current_player, :status, :created_at
~~~~

...since we now have a migrated db

# assigning defaults to board and players...will assign defaults to status, created_at, and current player as they are needed in the method

~~~~
@@@ ruby

	def initialize 
		@board = [[nil, nil, nil], 
				  [nil, nil, nil], 
				  [nil, nil, nil]]
		@player_o = 'o'
		@player_x = 'x'
	end
~~~~

little things, deleting board.rb and updated the spec/models/board_spec.rb to spec/models/game_spec.rb

~~~~
@@@ ruby

TicTacToe$ touch spec/models/game_spec.rb

~~~~

Updating Board content to Game content, such as:

~~~~
@@@ ruby

describe Game do 
	before do
		@test_game = Game.new
	end

~~~~

updating assign_player_position

~~~~
@@@ ruby

	def assign_player_position(player, row, column)
		player = @player_o, @player_x
		@board[row][column] = player
	end

~~~~ 

running to tests now just to see what's failing and what should change next...

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec
Rack::File headers parameter replaces cache_control after Rack 1.5.
FFFF..FF.F

Failures:

  1) Game should be initialized with nil values inside of the array, [[]]
     Failure/Error: @test_game.board.should == [[nil, nil, nil],
     NoMethodError:
       undefined method `has_key?' for nil:NilClass
     # ./spec/models/game_spec.rb:9:in `block (2 levels) in <top (required)>'
~~~~

!SLIDE

~~~~
@@@ ruby

  2) Game should always have a length of 3
     Failure/Error: @test_game.board.length.should == 3
     NoMethodError:
       undefined method `has_key?' for nil:NilClass
     # ./spec/models/game_spec.rb:15:in `block (2 levels) in <top (required)>'

  3) Game should always have the array within the array length of 3
     Failure/Error: @test_game.board.each { |array| array.length.should == 3 }
     NoMethodError:
       undefined method `has_key?' for nil:NilClass
     # ./spec/models/game_spec.rb:19:in `block (2 levels) in <top (required)>'

  4) Game assign_player_position should establish a position on the board, using 'x' or 'o'
     Failure/Error: to_test.should == @test_game.board[0][1]
     NoMethodError:
       undefined method `has_key?' for nil:NilClass
     # ./spec/models/game_spec.rb:25:in `block (3 levels) in <top (required)>'
~~~~

!SLIDE

~~~~
@@@ ruby

  5) Game display_element should display an 'x' or an 'o' if the element is not a space
     Failure/Error: @test_game.display_element(0,1).should == 'x'
       expected: "x"
            got: ["o", "x"] (using ==)
     # ./spec/models/game_spec.rb:43:in `block (3 levels) in <top (required)>'

  6) Game display_line should display player marks or a space (if nil) in the designated row
     Failure/Error: to_test.should == " | |x\n"
       expected: " | |x\n"
            got: " | |[\"o\", \"x\"]\n" (using ==)
       Diff:
       @@ -1,2 +1,2 @@
       - | |x
       + | |["o", "x"]
     # ./spec/models/game_spec.rb:52:in `block (3 levels) in <top (required)>'
~~~~

!SLIDE

~~~~
@@@ ruby

  7) Game display_board should display player marks on the board
     Failure/Error: " | |x\n"
       expected: "x| | \n- - -\n |o| \n- - -\n | |x\n"
            got: "[\"o\", \"x\"]| | \n- - -\n |[\"o\", \"x\"]| \n- - -\n | |[\"o\", \"x\"]\n" (using ==)
       Diff:
       
       
       @@ -1,6 +1,6 @@
       -x| | 
       +["o", "x"]| | 
        - - -
       - |o| 
       + |["o", "x"]| 
        - - -
       - | |x
       + | |["o", "x"]
     # ./spec/models/game_spec.rb:74:in `block (3 levels) in <top (required)>'

Finished in 0.04731 seconds
10 examples, 7 failures
~~~~

alright, this is good...we've got a bunch to update...not to bad though...

# fixed the simplest error for now, which was to change assign_player_position to it's orginial form

~~~~
@@@ ruby

	def assign_player_position(player, row, column)
		@board[row][column] = player
	end

~~~~ 

now only the first 4 tests are failing at 

~~~~
@@@ ruby

undefined method `has_key?' for nil:NilClass

~~~~ 

I wanted to know why the computer thinks there is an undefined method called `has_key?...I'm remembering that becuase it likely wants it's own #board such as, 

~~~~
@@@ ruby

	def board
		@board
	end
~~~~

!SLIDES

running tests...

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec
Rack::File headers parameter replaces cache_control after Rack 1.5.
..........

Finished in 0.02252 seconds
10 examples, 0 failures
~~~~

yay! that was all we needed!

# running rollback

As I was going to through the Micheal Hartl tutorial I learned that created_at and updated_at are automatically generated. I could also add columns as I go. For now, will only have players, board, status and current player. I will add other columns was needed. 

~~~~
@@@ ruby

TicTacToe$ rake db:rollback
~~~~

updated 

~~~~
@@@ ruby

class CreateGames < ActiveRecord::Migration
  def change
    create_table :games do |t|
      t.text :board,            {:default => Array.new(3).map{[]}}
      t.string :player_o,       {:default => 'o'}
      t.string :player_x,       {:default => 'x'}
      t.string :current_player
      t.string :status,         {:default => 'in_progress'}

      t.timestamps
    end
  end
end
~~~~
 
in db/migrate/20130406222125_create_games.rb
ran rake db:migrate and rake db:test:prepare

!SLIDE

now schema.rb looks like this...

~~~~
@@@ ruby

 ActiveRecord::Schema.define(:version => 20130406222125) do

  create_table "games", :force => true do |t|
    t.text     "board",          :default => "'---\n- []\n- []\n- []\n'"
    t.string   "player_o",       :default => "o"
    t.string   "player_x",       :default => "x"
    t.string   "current_player"
    t.string   "status",         :default => "in_progress"
    t.datetime "created_at",                                              :null => false
    t.datetime "updated_at",                                              :null => false
  end

end
~~~~

don't understand why the board formatting looks the way it does, we'll need to inquire about this...running all the test and they each passed. 

# TODO refactor methods in #Game

updated @board to 

~~~~
@@@ ruby

@board = Array.new(3).map{[nil, nil, nil]}
~~~~

for now...

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

# now onto game controller

creating a spec/controllers/game_controller_spec.rb

~~~~
@@@ ruby
require 'spec_helper'
require 'games_controller'

describe GamesController do

end
~~~~

!SLIDE

running it...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
/Users/zmontesd/.rvm/gems/ruby-1.9.3-p194@rails3tutorial2ndEd/gems/activesupport-3.2.11/lib/active_support/dependencies.rb:251:in `require': cannot load such file -- games_controller (LoadError)
~~~~

!SLIDE

yes, expected response, making the games_controller.rb. notice, I only ran the controller test - avoiding the model tests for now...

~~~~
@@@ ruby
class GamesController < ApplicationController

end
~~~~

now...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
No examples found.


Finished in 0.00014 seconds
0 examples, 0 failures
~~~~

!SLIDE

# alright, what do controllers need? Starting with tests

~~~~
@@@ ruby

describe GamesController do
	describe "GET games#index" do
		before(:each) do 
			get :index
		end

		it 'should set the @games instance variable to a set of all Games' do
        	assigns[:games].should_not be_nil
        	assigns[:games].all? {|game| game.kind_of?(Game)}.should be_true
    	end
    end
end

~~~~

# running tests...

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
F

Failures:

  1) GamesController should set the @games instance variable to a set of all Games
     Failure/Error: get :index
     NameError:
       uninitialized constant GamesController::Games
     # ./app/controllers/games_controller.rb:4:in `index'
     # ./spec/controllers/games_controller_spec.rb:8:in `block (2 levels) in <top (required)>'

Finished in 0.06245 seconds
1 example, 1 failure
~~~~

# now, creating an index method in Games

~~~~
@@@ ruby

	def index
		@games = Game.all
	end
~~~~

# re-running test, expecting an error since views are missing...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
F

Failures:

  1) GamesController GET games#index should set the @games instance variable to a set of all Games
     Failure/Error: get :index
     ActionView::MissingTemplate:
       Missing template games/index, application/index with {:locale=>[:en], :formats=>[:html], :handlers=>[:erb, :builder, :coffee]}. Searched in:
         * "#<RSpec::Rails::ViewRendering::EmptyTemplatePathSetDecorator:0x007fb754d123c0>"
     # ./spec/controllers/games_controller_spec.rb:9:in `block (3 levels) in <top (required)>'

Finished in 0.11067 seconds
1 example, 1 failure
~~~~

# adding a views/layouts/games/index.html.erb 

~~~~
@@@ ruby

TicTacToe$ mkdir app/views/games
TicTacToe$ touch app/views/games/index.html.erb
~~~~

for fun, adding 

~~~~
@@@ ruby

<h1>TicTacToe is coming soon!</h1>
~~~~

into app/views/games/index.html.erb

!SLIDE

ran controller test again and voilà!

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
.

Finished in 0.11957 seconds
1 example, 0 failures
~~~~

# now test for setting a new Game

~~~~
@@@ ruby

    describe "GET games#new" do
    	before(:each) do
      		get :new
    	end

    	it "should set the @game variable to a new game" do
      		assigns[:game].try(:kind_of?, Game).should be_true
    	end
	end
~~~~

!SLIDE

running rspec on controllers

~~~~
@@@ ruby

TicTacToe zmontesd$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
.F

Failures:

  1) GamesController GET games#new should set the @game variable to a new game
     Failure/Error: get :new
     AbstractController::ActionNotFound:
       The action 'new' could not be found for GamesController
     # ./spec/controllers/games_controller_spec.rb:20:in `block (3 levels) in <top (required)>'

Finished in 0.12993 seconds
2 examples, 1 failure
~~~~

creating a new method

~~~~
@@@ ruby

	def new
		@games = Game.new
	end
~~~~

running tests...

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
.F

Failures:

  1) GamesController GET games#new should set the @game variable to a new game
     Failure/Error: get :new
     ActionView::MissingTemplate:
       Missing template games/new, application/new with {:locale=>[:en], :formats=>[:html], :handlers=>[:erb, :builder, :coffee]}. Searched in:
         * "#<RSpec::Rails::ViewRendering::EmptyTemplatePathSetDecorator:0x007f9d24662da8>"
     # ./spec/controllers/games_controller_spec.rb:20:in `block (3 levels) in <top (required)>'

Finished in 0.13208 seconds
2 examples, 1 failure
~~~~

!SLIDE

we expected that failure, base on what we saw before, so let's create more views...

~~~~
@@@ ruby

TicTacToe$ touch app/views/games/new.html.erb
~~~~

running the tests again...

~~~~
@@@ ruby

TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
..

Finished in 0.12696 seconds
2 examples, 0 failures
~~~~

### TODOs, notes and to think about...

* `test and make the views and controller on games`
* `more methods to actually play the game`
* `bootstrap to enhance views?`
