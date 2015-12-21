---
layout: post
title: "I don't test controllers, and that's ok"
keywords: "controller,rails,test,ruby"
location: London
short_url: 
---

I know that's controversial, but I don't see value on writing tests for controllers on almost every scenario. I recently changed job and although I'm happy, I noticed my new team has a "unwritten policy" of writing controller tests, so I decided to write the reasons why I think controller tests are unnecessary.

<!-- more -->

#### You should be doing BDD

Feature tests do not need to test every possible detail, but they do need to test every possible flow (successes and failures that lead to different paths).

#### Testing simple delegations has little value

{% highlight ruby %}

class Foo
  # ...

  def bar_attribute
    bar.attribute
  end
end

describe Foo, "#bar_attribute" do
  it "calls bar's attribute" do
    bar = double(:bar, attribute: :such_value)
    foo = Foo.new(bar: bar)
    expect(foo.bar_attribute).to eq(:such_value)
  end
end

{% endhighlight %}

The test for the code above has no value. Reasons:
- It is extremely simple.
- It is completely coupled to the implementation; no matter what changed you make, the test would have to change as well.
- The design is trivial, so TDD-ing it is useless.

I would only write a test like this in one scenario: there is no higher level test executing this code. That's because Ruby isn't compiled so I have to "make sure" I didn't make any typo. 

I used quotes above because, since I would probably use a double on the test, there is always a chance that both the test and the code has a typo. I can't really be sure my code works without coupling the test of `Foo` with `Bar` or having an higher level test that execute my code in a integrated way.

#### Controllers should be slim

{% highlight ruby %}

class SomeController ...
  ...
  def create
    result = ABusinessRule.call(...params...)
    if result
      redirect_to url, notice: success_message
    else
      flash[:notice] = failure_message
      render :new
    end
  end
  ...
end

describe SomeController, "POST create" do
  it "calls a business rule" do
    expect(ABusinessRule).to receive(:call).with(...params...)
    post :create, params
  end
  
  it "redirects to url when success" do
    allow(ABusinessRule).to receive(:call).with(...params...).and_return(true)
    post :create, params
    expect(response).to redirect_to(url)
  end

  it "renders new when failure" do
    allow(ABusinessRule).to receive(:call).with(...params...).and_return(false)
    post :create, params
    expect(response).to render_template(:new)
  end
end

{% endhighlight %}

The code above represents what I believe to be well written, slim controller. It receives a request, delegates it to a method object on the domain of the application and then handles the two possible outcomes. Almost anything besides that should not be on a controller.

However, the controller tests above are not very good. They test only two things: delegation to another object and the two possible outcomes of the action.

As discussed before, testing the delegation has very little value, but testing the two possible outcomes is important. However, since I always have a higher level test covering my feature, those two controller tests are unnecessary: that behavior is already covered by the feature tests.

#### Tests are expensive

Take time to write.
Even if you do TDD by the book, you should not maintain low value tests - they have to be maintained by future developers.
Tests take time to run.

### When should you write controller tests

When for some reason you don't have a feature test. Ex: integrating with external services, like Facebook authenticator.

Conclusion

Consider the value and the cost of your tests. Writing tests during TDD and deleting after refactoring is ok. Not writing because you feel comfortable without them is also ok. Don't blindly follow rules.