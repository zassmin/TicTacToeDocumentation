# TicTacToe

Creating TicTacToe as a tool to learn how to make a rails app. Users will be WWC, RubyTuesday ladies. Documenting TicTacToe as it's built. Building project one feature at a time. And of course TestTestTest!

Please note - this is a rough draft. 

# test Board.rb

Does a the file exist?
Does the board exist?

Making the first test

~~~~
@@@ ruby
require 'spec_helper'
require 'board.rb'

describe Board do 
	
end
~~~~

# watch it fail

~~~~
@@@ ruby
rspec spec
/Users/zmontesd/.rvm/gems/ruby-1.9.3-p194@global/gems/bundler-1.1.4/lib/bundler/runtime.rb:31:in `block in setup': You have already activated rspec-core 2.12.2, but your Gemfile requires rspec-core 2.10.1. Using bundle exec may solve this. (Gem::LoadError)
~~~~

we wanted the tests to fail, they didn't even run....

# error handling

running bundle exec...just trying to specify a particular rspec version to use...
...this is where pairing comes in handy... 

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec-core 2.10.1
bundler: command not found: rspec-core
Install missing gem executables with `bundle install`
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

and just for kicks...
I haven't generated a model. See this as necessary, but don't think it'll hurt...

~~~~
@@@ ruby
$ rake db:migrate
$ rake db:test:prepare
~~~~

ran...

~~~~
@@@ ruby
$ bundle exec rspec spec
~~~~

!SLIDE
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
$ bundle install
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
default value to be nil for the array, or whether it should have specific coordinates or huh...will it really name it a board?

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
	@show_board.board.length == 3
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
TicTacToe$ bundle exec rspec spec
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
		p.should == @show_board.board[0][0]
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
			to_test = @show_board.display_board.should == " | | \n" + 
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

...alright. back to think about displaying the board on a high level. We've specify when to display a play and space...let's write it

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
Eventually this display_board function won't actually be necessary.  In the views, we can draw a permanent grid
and insert the value of each cell into it.

# switching gears and working on board controller...

creating a spec/controllers/board_controller_spec.rb

~~~~
@@@ ruby
require 'spec_helper'
require 'board_controller'

describe BoardController do

end
~~~~

!SLIDE

running it...

~~~~
@@@ ruby
TicTacToe$ bundle exec rspec spec/controllers/
Rack::File headers parameter replaces cache_control after Rack 1.5.
/Users/zmontesd/.rvm/gems/ruby-1.9.3-p194@rails3tutorial2ndEd/gems/activesupport-3.2.11/lib/active_support/dependencies.rb:251:in `require': cannot load such file -- board_controller (LoadError)
~~~~

!SLIDE

yes, expected response, making the board_controller.rb. notice, I only ran the controller test - avoiding the model tests for now...

~~~~
@@@ ruby
class BoardController < ApplicationController

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

alright, what do controllers need?

### TODOs, notes and to think about...

* `routes for the board`
* `generate models and run a migration? what should my board model include? - player_position:string and...`
* `list out other items for board needs...i.e., idenitfying the position`
* `test and make the views and controller for the board`
* `create a new_board method`
