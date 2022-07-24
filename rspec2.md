# subject and let

to test a class you need to instantiate an instance of the object to test it out

```
describe Robot do
  subject { Robot.new }

  it "satisfies some expectation" do
    expect(subject).to # ...
  end
end
```


you can also dclare a subject with a name

```
describe Robot do
  subject(:robot) { Robot.new }

  it "position should start at [0, 0]" do
    expect(robot.position).to eq([0, 0])
  end

  describe "move methods" do
    it "moves left" do
      robot.move_left
      expect(robot.position).to eq([-1, 0])
    end
  end
end
```

the it block is a test

the test fails if the expect fails

# let

subject is the focus of the test, let defines helper objects

```
describe Robot do
  subject(:robot) { Robot.new }
  let(:light_item) { double("light_item", :weight => 1) }
  let(:max_weight_item) { double("max_weight_item", :weight => 250) }

  describe "#pick_up" do
    it "does not add item past maximum weight of 250" do
      robot.pick_up(max_weight_item)

      expect do
        robot.pick_up(light_item)
      end.to raise_error(ArgumentError)
    end
  end
end
```

see link

[when to use rspec let](https://stackoverflow.com/questions/5359558/when-to-use-rspec-let)

see link 
[subject and let blocks](https://benscheirman.com/2011/05/dry-up-your-rspec-files-with-subject-let-blocks/)

## let does not persist state

```
class Cat
  attr_accessor :name

  def initialize(name)
    @name = name
  end
end

describe "Cat" do
  let(:cat) { Cat.new("Sennacy") }

  describe "name property" do
    it "returns something we can manipulate" do
      cat.name = "Rocky"
      expect(cat.name).to eq("Rocky")
    end

    it "does not persist state" do
      expect(cat.name).to eq("Sennacy")
    end
  end
end

# => All specs pass
```

