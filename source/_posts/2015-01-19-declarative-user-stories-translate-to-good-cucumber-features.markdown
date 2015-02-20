---
layout: post
title: "Declarative user stories translate to good cucumber features"
date: 2015-01-19 15:58:19 -0600
comments: true
categories: [Cucumber, Testing]
author: Jon Kinney
---

A lot of the FUD you read about Cucumber involves teams abandoning the tool
because they think it adds a layer of extra work to the testing process.
However, teams that go that route are missing a big benefit of plain text user
stories: executable documentation **that is readable by "the business"**. The
ultimate goal is to have the business (aka the stakeholders) actually
collaborate on user stories so that a shared understanding of what needs to be
built can be developed across the whole team.

In order to achieve that goal, however, Cucumber scenaios need to be written in
a certain way. They need to read as conversational requirements, not a QA test
plan check list. This style of writing has a name: declarative.

{% blockquote k12reader, http://www.k12reader.com/declarative-sentences-are-the-most-common-type%E2%80%A6and-with-good-reason/ %}
"Possibly the most common sentence type in the English language, declarative
sentences are used when you want to make a statement. Whether it's a bold
statement or a simple fact, the sole purpose of a declarative sentence is to
give information. It always ends with a simple period. And if you'd like to see
an example of a declarative sentence, you don't need to look any further.
Actually, every sentence in this paragraph is a declarative sentence."
{% endblockquote %}

The most important part of that last paragraph is this: "the sole purpose of
a declarative sentence is to give information". This is why Cucumber is best 
suited to this style of writing. A user story's entire purpose is also to give 
information.

Perhaps the easiest way to show a good user story style is by first showing an
antipattern. Here is a user story written in an imperative style. If you have
been using Cucumber for a long time, you might recgonize these steps as being
matched by the [now defunkt](http://aslakhellesoy.com/post/11055981222/the-training-wheels-came-off)
`features/support/web_steps.rb` file.

```gherkin
Given I am on the home page
 When I fill in "Username" with: "jondkinney"
  And I fill in "Password" with: "SuperSecret123"
  And I check "Remember me"
  And I press "Log in"
 Then a user session should be persisted
  And I should be on my dashboard
  And I should see "You have successfully logged in."
```

Sometimes this style can be useful. It can help tease out an implementation
detail that the only documented way to log in is clicking the 'Log in' button,
for instance. However, I think stories that are structured in a declarative
method overall provide more value more of the time.

A declarative version of the story above might read like this:

```gherkin
Given I am on the home page 
 When I login as an admin
 Then I should be on my dashboard 
  And I should see "You have successfully logged in."
```

The implementation details are largely gone from the story, and it reads a lot
better.

Another example where an admin is merging two user accounts in an asset
management system might read something like this:

```gherkin
Given two users with matching addresses and different spellings for full name
 When I edit one of the user names to match the other and submit the form
 Then the assets for both user records should exist under a single user 
  And the duplicate user record should no longer exist
```
 
Here's another shorter example:

```gherkin
Given I am logged in as an admin 
 When I unlock a user account
 Then the user should receive an email with a link to reset their password
```

## Other resources on declarative user stories

This style isn't a new idea anymore. However, many of the teams we work with 
still fall into the imperative trap when a deadline looms or it's unclear how to
extract several steps into a more declarative one. For some more inspiration on
the subject, take a look at a few posts from other experts on testing and BDD.

* [Imperative vs declarative scenarios in user stories](http://benmabey.com/2008/05/19/imperative-vs-declarative-scenarios-in-user-stories.html)
* [Declarative vs imperative gherkin scenarios for cucumber](http://itsadeliverything.com/declarative-vs-imperative-gherkin-scenarios-for-cucumber)

And from the author of cucumber itself: [The training wheels came off](http://aslakhellesoy.com/post/11055981222/the-training-wheels-came-off)

We've had good luck with this more declarative style at Faster Agile.
Stakeholders can better read and understand the requirements in a user story,
and with this conversational and plain language tool to describe how the system
is supposed to work, they also help write the stories more often too!

## Taking it a step further

After conquering the verbosity and repetativeness of imperative stories,
there's still a nagging issue with the resulting step defintions. Using bare
capybara as the underlying mechanism to interact with the DOM can lead to
mutliple ways of targeting elements to fill-in and click. Even if a project
tries to standardize on targeting _only_ IDs or _only_ label text, that code is
still repeated in many places leading to a brittle test suite that is locked
pretty heavily to the existing ID/class structure and/or label text. 

In our next post, we'll show how Faster Agile uses the Site Prism gem and the
concept of "page objects" to encapsulate the DOM for any given page making it
much easier to keep a Cucumber suite more decoupled from the DOM it's driving.
