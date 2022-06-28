# Programming as an engineering practice

Software is a difficult topic, we need to approach it thoughtfully. If treated
lightly, as it was only a mere sequence of instructions, it is doomed to
failure in most cases.

We should take inspiration from science and work by doing experimentations:
- Characterize: observe the current state
- Hypothesize: create a theory explaining the observation
- Predict: make a prediction based on the hypothesis
- Experiment: test the prediction

The only way we can work with experimentation is to work in small steps, to be
able to understand the impact of each one of them. To reduce this feedback loop
as a developer we have only one main practice to follow: continuous delivery.

We must remember that continuous delivery defines an outcome, not a mechanism.
The outcome is: "software must always be releasable" and "optimize for fast
feedback".

When followed carefully, it helps define a framework of thinking, enabling us
to find better answers or solutions to problems we don't know how to solve yet.
To achieve this continuous flow of changes, we need to organize all aspects of
our development practice accordingly:
- organization: minimize dependencies between teams, teams must be autonomous
- automation: team must be able to assess quickly if a change is safe through
  testing

If we take an iterative process to "always improve for faster feedback", it
will ultimately lead us to agile theory, lean theory, continuous delivery and
DevOps practices. This give us a concrete lead to follow instead of blindly
following rituals or development processes.

A software is considered deployable if it is fast enough, secure enough,
resilient enough and working,... When the deployment pipeline says the change
is good, there is no more testing to do, no more sign-off, or integration
testing or whatever. It doesn't mean the software has to go to production right
away but it is ready, it doesn't require any other validation.

If we want to achieve this degree of confidence and still maintaining a very
fast feedback, we need to be able to create new features over a series of
deployment. It's not because some code has been merged and is deployed that the
code is released / visible to the end user.

The other aspect to have this degree of confidence and fast feedback loop, is
the testability of the software, mainly how to easy it is to get the system in
a particular state to be tested.

Let's be clear: if a test is difficult to write, the design of the code is poor
and needs to be improved.

One technique that helps a lot is TDD. TDD is not about writing tests but is a
tool to better design our software. The objective is to write code that is easy
to test and put in a state to be tested. When this technique is not followed,
it usually leads to test that cuts some corners because some parts are
untestable or force us to break encapsulation or tight our test to the code we
already wrote.

If we wait until we've finished to test our software, we won't get the full
benefit of it. We won't get high quality timely fast feedback we expect.
Testing will loose all the little nuance we would have grabbed if done along
the way. It's going to become a chore and that's when organizations starts to
hire people to do hit for us.
