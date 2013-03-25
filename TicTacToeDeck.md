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
	@show_board.board.each { |array| array.length } == 3		
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
describe "player_position" do 
	it "should establish a position on the board, using 'x' or 'o'" do
		@show_board.player_position('x', 0, 1) == @show_board.board[0][1]
	end
end
~~~~

I'm having trouble with this, the test passes with just the following code
since it include the correct number of agruments...

~~~~
@@@ ruby
def player_position(position, first_element, second_element)

end
~~~~

...will need to write the test, do I want 'x' or 'o' to append into the board? Or do we just want to know how to identify a position?...

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

* `we've got the board controller class, so we'll be adding methods to set the board, just setting a new one for now...wait, nothing in routes.rb, ok it's probably time to generate something to the model so we can then do stuff with the controllers`
* `back to the models, what should my board model include? - player_position:string and...`
* `keep to board as an array with default nil values?`
* `list out other items for board needs...i.e., idenitfying the position`
* `test the views and controller for the board`
* `generate the model and run a migration? need to decide what the datatypes should be on the board`
* `creating branches`
