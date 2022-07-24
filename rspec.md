# rspec

test are kept in spec folder

application code is kept in lib folder

tests for hello.rb would be hello_spec.rb

# require dependencies

```
require 'rspec'
require 'hello'

describe '#hello_world' do

end
```

# hello world

```
# hello_spec.rb

require 'rspec'
require 'hello'

describe "#hello_world" do
  it "returns 'Hello, World!'" do
    expect(hello_world).to eq("Hello, World!")
  end
end
```
## hello world code

```
# hello.rb

def hello_world
  "Hello, World!"
end
```

## describe and it

takes strings as arguments

### describe and it

```"#method"``` for instance methods

```"::method"``` for class methods

## context

```
describe Student do
  context 'when a current student' do
    ...
  end

  context 'when graduated' do
    ...
  end
end
```

# expect

describe and it organize tests and give them descriptive labels

expect will actually do the work of testing your code

```
expect(test_value).to ...
expect(test_value).to_not ...
```


## argument construction
```
describe Integer do
  describe '#to_s' do
    it 'returns string representations of integers' do
      expect(5.to_s).to eq('5')
    end
  end
end
```

## block construction

when you want to test that a certain method call will throw an error

```
describe '#sqrt' do
  it 'throws an error if given a negative number' do
    expect { sqrt(-3) }.to raise_error(ArgumentError)
  end
end
```

# matchers

```expect(test_value).to eq(expected_value)``` will see if ```test_value == expected_value```. ```expect(test_value).to be(expected_value)``` will test if ```test_value``` is the same object as ```expected_value```.

```
expect('hello').to eq('hello') # => passes ('hello' == 'hello')
expect('hello').to be('hello') # => fails (strings are different objects)
```

# install rspec

```
gem install rspec      # for rspec-core, rspec-expectations, rspec-mocks
gem install rspec-core # for rspec-core only
rspec --help
```

[rspec required reading 1](https://github.com/rspec/rspec-core)

## example

describe an order and sum up the prices of its line items

```
RSpec.describe Order do
  it "sums the prices of its line items" do
    order = Order.new

    order.add_entry(LineItem.new(:item => Item.new(
      :price => Money.new(1.11, :USD)
    )))
    order.add_entry(LineItem.new(:item => Item.new(
      :price => Money.new(2.22, :USD),
      :quantity => 2
    )))

    expect(order.total).to eq(Money.new(5.55, :USD))
  end
end
```

## nested groups

```
RSpec.describe Order do
  context "with no items" do
    it "behaves one way" do
      # ...
    end
  end

  context "with one item" do
    it "behaves another way" do
      # ...
    end
  end
end
```

### it, specify, example

```
RSpec.shared_examples "collections" do |collection_class|
  it "is empty when first created" do
    expect(collection_class.new).to be_empty
  end
end

RSpec.describe Array do
  include_examples "collections", Array
end

RSpec.describe Hash do
  include_examples "collections", Hash
end
```

#### shared_context, include_context, shared_examples, include_examples

# example

```
RSpec.describe "Using an array as a stack" do
  def build_stack
    []
  end

  before(:example) do
    @stack = build_stack
  end

  it 'is initially empty' do
    expect(@stack).to be_empty
  end

  context "after an item has been pushed" do
    before(:example) do
      @stack.push :item
    end

    it 'allows the pushed item to be popped' do
      expect(@stack.pop).to eq(:item)
    end
  end
end
```

# Rspec expectations

[rspec required reading 2](https://github.com/rspec/rspec-expectations)

lets you express expected outcomes on an object in an example


```
expect(account.balance).to eq(Money.new(37.42, :USD))
```

# install

``` gem install rspec```

## comparisons

```
expect(actual).to be >  expected
expect(actual).to be >= expected
expect(actual).to be <= expected
expect(actual).to be <  expected
expect(actual).to be_within(delta).of(expected)
```

## regular expressions

```expect(actual).to match(/expression/)```

## types/classes

```
expect(actual).to be_an_instance_of(expected) # passes if actual.class == expected
expect(actual).to be_a(expected)              # passes if actual.kind_of?(expected)
expect(actual).to be_an(expected)             # an alias for be_a
expect(actual).to be_a_kind_of(expected)      # another alias
```

## truthiness

```
expect(actual).to be_truthy   # passes if actual is truthy (not nil or false)
expect(actual).to be true     # passes if actual == true
expect(actual).to be_falsy    # passes if actual is falsy (nil or false)
expect(actual).to be false    # passes if actual == false
expect(actual).to be_nil      # passes if actual is nil
expect(actual).to_not be_nil  # passes if actual is not nil
```

## expecting errors

```
expect { ... }.to raise_error
expect { ... }.to raise_error(ErrorClass)
expect { ... }.to raise_error("message")
expect { ... }.to raise_error(ErrorClass, "message")
```

## yielding

```
expect { |b| 5.tap(&b) }.to yield_control # passes regardless of yielded args

expect { |b| yield_if_true(true, &b) }.to yield_with_no_args # passes only if no args are yielded

expect { |b| 5.tap(&b) }.to yield_with_args(5)
expect { |b| 5.tap(&b) }.to yield_with_args(Integer)
expect { |b| "a string".tap(&b) }.to yield_with_args(/str/)

expect { |b| [1, 2, 3].each(&b) }.to yield_successive_args(1, 2, 3)
expect { |b| { :a => 1, :b => 2 }.each(&b) }.to yield_successive_args([:a, 1], [:b, 2])
```

# predicat matchers
```
expect(actual).to be_xxx         # passes if actual.xxx?
expect(actual).to have_xxx(:arg) # passes if actual.has_xxx?(:arg)
```

## link

see link 
[copied from this link](https://github.com/rspec/rspec-expectations)

# before

```
describe Chess do
  let(:board) { Board.new }

  describe '#checkmate?' do
    context 'when in checkmate' do
      before(:each) do
        board.make_move([3, 4], [2, 3])
        board.make_move([1, 2], [4, 5])
        board.make_move([5, 3], [5, 1])
        board.make_move([6, 3], [2, 4])
      end

      it 'should return true' do
        expect(board.checkmate?(:black)).to be true
      end
    end
  end
end
```


```before(:each)``` or ```before(:all)```

also there is ```after(:each)``` and ```after(:all)```

## pending specs

if you simply leave the test bodies empty, it'll look like they are all passing

leave off the do...end from the it

```
describe '#valid_move?' do
  it 'should return false for wrong colored pieces'
  it 'should return false for moves that are off the board'
  it 'should return false for moves that put you in check'
end
```

### predicate syntatic sugar

```expect(Array).to be_empty```

```expect(my_hash).to have_key(my_key)```

see link 
[rspec rspec-core link](https://relishapp.com/rspec/rspec-core/v/2-4/docs)
