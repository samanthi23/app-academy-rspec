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

