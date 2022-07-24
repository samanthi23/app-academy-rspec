# rspec order of operations

don't try to declare our subject at the top of our it block

rspec requires that the subject be declared outside of your it blocks

```
RSpec.describe Deck do
  describe '#initialize' do
    subject(:deck) { Deck.new } # yup

    it 'initializes with 52 cards' do
      expect(deck.count).to eq(52)
    end
  end
end
```

order matters

especially for describe, it, expect, subject, let, or before

```
RSpec.describe Sloth do
  subject(:sloth) { Sloth.new("Herald") }

  describe "#run" do
    context "when a valid direction is given" do
      it "returns a string that includes the direction" do
        expect(sloth.run("north")).to include("north")
      end
    end

    context "when an incorrect direction is given" do
      it "raises ArgumentError" do
        expect { sloth.run("somewhere") }.to raise_error(ArgumentError)
      end
    end

  end
end
```

# Test doubles

when we write unit test we want each of our specs to do only one thing

Example: 

```
class Order
  def initialize(customer)
    @customer = customer
  end

  def send_confirmation_email
    email(
      to: @customer.email_address,
      subject: "Order Confirmation",
      body: self.summary
    )
  end
end
```

Customer class Order class

# Test doubles

also called a mock

a test double is a fake object that we can use to create the desired isolation

a double takes the place of outside objects such as the Customer object. 

```
#IMPLEMENTATION
class Order
    def initialize(customer, product)
        @customer = customer
        @product = product
    end

    def send_confirmation_email
      email(
        to: @customer.email_address,
        subject: "Order Confirmation",
        body: self.summary
      )
    end

    def charge_customer
        @customer.debit_account(@product.cost)
    end
end

#RSPEC FILE
RSpec.describe Order do
  let(:customer) { double("customer") }
  subject(:order) { Order.new(customer) }

  it "sends email successfully" do
    allow(customer).to receive(:email_address).and_return("ned@appacademy.io")

    expect do
      order.send_confirmation_email
    end.to_not raise_exception
  end
end
```

this creates an instance of RSpec::Mocks::Mock

the double is a blank slate waiting for us to add behaviors to it

a method stub stands in for a method

Order needs customer's email_address method

```allow(double).to receive(:method)```

```and_return``` takes the return value that the stubbed method will return when called as its parameter

the customer double simulates the Customer#email_address method

this totally isolates the test from the Customer class

customer object is not a real Customer it's an instance of Mock

```let(:customer) { double("customer", :email_address => "ned@appacademy.io") }```

# Method expectations

test that the proper methods are called

to do this we use method expectations

```
RSpec.describe Order
  let(:customer) { double('customer') }
  let(:product) { double('product', :cost => 5.99) }
  subject(:order) { Order.new(customer, product) }

  it "subtracts item cost from customer account" do
    expect(customer).to receive(:debit_account).with(5.99)
    order.charge_customer
  end
end
```

expecatations need to be set up in advance

## integration tests

unit tests how an object should interact with other objects

we use real objects instead of mocks




