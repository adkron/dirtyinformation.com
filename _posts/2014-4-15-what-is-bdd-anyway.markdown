---
layout: post
title: What is BDD anyway?
---

The term BDD or Behavior Driven Development has been thrown around for a
while. There were many promises of it changing the way that we write our
software and think about how we test it. Unfortunately when I first
started experimenting with the idea I didn't get it. I tried, but all of
my specs ended up looking just like my old TDD ways.

I went back and forth, but the tools like [RSpec](https://github.com/rspec)
kept dragging me back. They weren't always the fastest, but the matchers
were very cool and allowed me to replace test name (glorified comments)
with actual code that was expressive. I was still writing my "specs" in
the moth-eaten and dusty ways of my past.

Most developers I find doing "BDD" are still using those old ways. These
ways work fair, but I find that they don't communicate well. There were
often cumbersome setups, test names, and useless test cases.

I will get to what I believe is the heart of BDD very soon, but let's
start with an example of the old way. He are some "BDD" specs for a
stack, written the old way.

~~~ruby

describe Stack do
  describe "#empty?" do
    context "when there are no elements in the stack" do
      it { subject.should be_empty }
    end

    context "when there is an element in the stack" do
      before do
        subject.push :element
      end

      it { subject.should_not be_empty }
    end
  end

  describe "#pop" do
    context "when there are no elements in the stack" do
      it do
        expect{ subject.pop }.to raise_error(Stack::EmptyError)
      end
    end

    context "when there is more than one item" do
      before do
        subject.push :first
        subject.push :second
      end

      it "pops the last element placed on" do
        subject.pop.should == :second
      end

      it "popping multiple times moves down" do
        subject.pop
        subject.pop.should == :first
      end

      it do
        expect{ subject.pop }.to change{ subject.size }.by(-1)
      end
    end
  end
end
~~~

Ok that isn't complete, but I think we have enough to see how this is
going. So we describe each method in turn. Each method then has a list
of states that go along with it to determine how we travel through that
method in each case. This makes complete sense, or does it.

This is going to be the whole secret. Wait for it...wait for it...use
the force, Luke, trust your instincts. Sorry, I got a little side
tracked. BUTTERFLY! Move the context to the outside. Describe the
"behavior" with in a context. That is what makes it BDD.

~~~ruby
describe Fifo do
  context "when it is new" do
    it do
      is_expected.to be_empty
    end

    specify do
      expect(subject.size).to be_zero
    end

    specify do
      expect{ subject.pop }.to raise_error Fifo::EmptyError
    end

    specify do
      expect{ subject.push :element }.to change{ subject.size }.by(1)
    end
  end

  context "with one element" do
    before do
      subject.push :element
    end

    it do
      is_expected.to_not be_empty
    end

    specify do
      expect(subject.size).to eql(1)
    end

    describe "popping an element" do
      specify do
        expect{ subject.pop }.to change{ subject.size }.by(-1)
      end

      specify do
        expect(subject.pop).to eql(:element)
      end
    end
  end

  context "with more than one element" do
    before do
      subject.push :first_element
      subject.push :second_element
    end

    specify do
        expect(subject.pop).to eql(:second_element)
    end

    specify do
      subject.pop
      expect(subject.pop).to eql(:first_element)
    end
  end
end
~~~

That is the big change. If you'd like to look at this is a better light
then you can view it as a
[gist](https://gist.github.com/adkron/10799883#file-bdd-rb). Bonus: The
gist includes the implementation to pass the specs.

When attempting this approach make sure that each spec you write fails.
Make sure that each step you take is only enough to change the error.
Never go beyond what you need. I also find it is nice to implement each
context fully before moving on to the next context.

Another advantage to this type of testing has to do with the contexts
being easily upgraded to top level classes when they diverge enough. As
an example you may have a user class that has a flag for admin. A
version with the state of admin has some differing behavior than that of
a user without admin. At the beginning of the project you use a flag and
it works great. As the project matures there becomes the need for an
AdminUser class. If you are doing things the old way you must step
through each method description pulling out the specs that apply to the
admin class and hope you don't miss any. The "real" BDD way allows you
to pull the one "as an admin" context and promote it to its very own
test class.

We attempted this as pairs in [STLRuby](http://www.meetup.com/stlruby/)
and we have multiple test suites done by pairs on
[github](https://github.com/stlruby/tdd_bdd_stacks). Compare the test
suites from the retro style vs the new and improved BDD style. See which
you prefer.
