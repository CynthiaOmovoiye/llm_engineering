Do the same for this:

Okay, well, here we are, back in cursor, one of my very favorite places to be.

And we're going into the week.

One directory on the left.

Check the LM Engineering's in block caps here.

So you're in the right project.

And then go to day two, which is where we find ourselves right now.

And I need to remind you before you do anything, you have to go to the top right to where it says Select

kernel for you.

You click on that.

You click in the Python Environments option.

The first option you pick the dot VM.

That's Python 3.12 that is matching this VM right here.

And that means your kernel is set.

Welcome to the day two lab.

So I just wanted to mention here before we start that there is a page of resources on my home page and

I've linked to it here.

I'll try and keep adding useful resources.

So do keep that bookmarked.

You can go there and check it out right now if you wish.

Okay okay so we're now going to go back again to doing what we did yesterday, but do it with a little

bit more discipline.

And I want to tell you about this thing called the Chat Completions API.

And this is the name of it's the simplest way that you can call an LLM, particularly a frontier LLM.

And it's called chat completions because it's a nod to what you're actually doing.

When you call an LLM, you're giving it a chat a conversation so far, and you're asking it to predict

the most likely message to come next.

And you can think of that as as like completing a chat.

That's what the LLM thinks it's doing.

It's it's it's like predictive text.

It's just trying to predict the most likely words to come next.

And as a side effect of that, it happens to be really good at answering whatever questions it's asked.

But all it's actually trying to do is to predict the most likely next few words, or as we'll shortly

explain, the most likely next tokens.

Okay, so it was actually this chat completions API.

This this approach was invented by OpenAI first and foremost.

But it was so popular the structure and style of this type of request became so popular that it's become

ubiquitous.

All of the providers offer the Chat Completions API.

It is the kind of standard way to interact with an LLM.

And we're going to start again with OpenAI.

I know some of you are antsy to get off OpenAI and use free models.

Your time is coming any second now.

Just watch this if you don't want to use OpenAI, and we'll get to you in a second.

All right.

So first of all, I'm going to do a repeat of last time where we use this thing called load dot EMV,

which is going to load in the secrets in our EMV file and check that we have OpenAI key set.

So this load EMV function is something which which makes which loads in anything you've saved in your

EMV.

And then we'll just check that the key is good.

It says API key found and it looks good so far.

And if yours doesn't say that, then you know what to do.

Check the EMV, check everything, and look in the look in the troubleshooting and ask me if you get

stuck.

Okay, let's talk about end points.

Now, I imagine most of you know exactly what an end point is.

It's one of those words people throw around all the time.

But you might not.

And for people that don't know about it, you should quickly find out.

I explain it all in the Technical Foundations guide.

There's nothing special at endpoint.

It's an http url which you can call to, to to to make an API request by hitting some web address.

And that web address would be known as an endpoint.

It's a way that you have an API.

But if things like API's and endpoints are new to you, then read that guide.

Okay.

There's an endpoint in particular which might interest you, which I want to show you right now.

And it's an endpoint, an API endpoint offered by open AI.

So in order to call this endpoint, which I will just do, we first have to set a couple of things which

are very common for any HTTP request.

There's HTTP headers, the headers that go down in that request.

You normally you probably if you know anything about web requests.

You specify what content type, what type of thing you want to come back.

And that's a way of saying, I want Jason to come back, please.

And then this is a fairly standard way of, of, of sending down a secret in an HTTP message that authorizes

you, you put in a header called authorization and the value of that header, there's the word bearer

and then a space and then some secret that identifies you to the third party.

And in this case we are going to stuff the OpenAI API key.

The thing that I got just here, we're going to stuff that into the header.

And then I've got something here called payload.

Payload is just a chunk of JSON.

And it's going to be a dictionary a dictionary with two keys.

One of them is model, which is going to be GPT five nano, the tiny version of GPT five.

The latest model, the one of the strongest on the planet right now.

And then the second field in this dictionary has a key of messages.

The value is, you know, this is a list of dictionaries.

It's a list you can see there.

It is a list.

Each dictionary, let's put that on another line, has a key role with value user a key content.

And the value is tell me a fun fact okay.

Nothing special here.

Let's look at this.

Uh, there is the payload JSON.

It's is just what I said.

Model GPT five nano messages, a list of dictionaries, role user content.

Tell me a fun fact.

Okay, that's a chunk of JSON.

Uh, headers and payload.

What we're now going to do is we're going to send that to an endpoint.

This is the endpoint API.

V1.

It's chat completions.

And sometimes when you make a Post request people think of that as like creating a new resource for

people that are like rest, rest people.

Uh, that's that's sometimes called creating a resource.

So you could think of this as being like chat completions create some familiar.

Uh, so chat completions create.

We're passing in our headers with our API key.

And the payload is this JSON blob right here.

So let's run that.

And what comes back is a bunch of JSON.

We asked for JSON back.

Let's see what we get back.

Let's see what kind of JSON we get.

Here it is.

Uh so it's a chunk of JSON as like an ID object, various stuff.

And then it has a field called choices and Choices.

In this response, choices is a list.

And the first item of that choices with index zero is something which has a field message.

And that message is itself a JSON dictionary.

And that has something called content, which is fun fact.

There are possible unique games, more possible unique games of chess than there are atoms in the observable

universe.

About ten to the power of 120 There is a fun fact.

And so that fun fact came back in the JSON response from our call to GPT five to that endpoint, that

URL that I just gave you.

And so there's obviously there's another way I could do this.

Let me get a new code thing here.

Let's say, uh, um, we said response JSON.

That was the JSON that we just were just looking at right up here.

Uh, I spelt response wrong.

That's that's what you get if you make me type Response.json.

So we could say, okay, let's look in the choices field.

I guess it knows what I'm doing here.

Look at the first element in there.

Element index zero at the field message at the field content okay.

You see that.

That's you know what that's going to do.

We're just going to choices the first one message content.

And let's see what that prints.

It prints that that very fun fact of course that we just looked at uh, the more possible unique games

of chess than there are atoms in the observable universe.

Okay, but you know what?

Uh, so so this is a perfectly good way to call OpenAI in the cloud using an HTTP endpoint.

And it's fine.

We could do this.

We could we could always type this.

But it is kind of messy fussing around with JSON, looking in dictionaries at keys and things like that.

It's kind of hokey, and it would be a real pain if every time we wanted to call GPT or any frontier

model, we had to stitch together these HTTP requests, call that slash chat completions with a post

or create and then be be sort of navigating our way through JSON objects like this.

It would be a pain.

It would be nice if there were a better way to do it.

Yes, yes, I know you get the joke.

You know what I'm gonna say?

There is a better way to do it.

And OpenAI created this better way.

They made a package called OpenAI, and that package is what's known as a Python client library.

Python client library is nothing fancy at all.

A Python client library that you often use for APIs all over the place for you.

Name it for APIs, for sending emails, for APIs to do so much.

Um, and these things are typically very lightweight libraries that manufacture an HTTP request to an

endpoint.

And with what comes back, it turns it into Python objects so that you're not messing around with this

kind of stuff with with these, uh, ploughing your way through JSON dictionaries.

But you can just write some nice, elegant Python code, and that is all the OpenAI library is.

It's a Python client library that wraps a call to an HTTP endpoint.

It's very simple.

It's completely open source.

You can open it, look at it, look at all the code.

It's perfectly simple.

Some people, the first time they come across this, think that when you're dealing with OpenAI, the

object and the code, that somehow we're sort of running GPT and we have some some fancy code from OpenAI.

Not at all.

It's vanilla code that just wraps making a web request.

And we'll we'll quickly use that code now, but it's going to be much more familiar to you.

  Do same for this:
All right, so I'll do some more typing since since the haters will yell at me if I don't, so I'll

do some typing.

Let's do it the proper way.

Let's create an OpenAI Python client.

You just say OpenAI equals OpenAI.

Oops, like so.

And it by default looks in the uh, in the environment variable OpenAI API key by default if you don't

specify one.

So that's just just hard coded in there.

And I have to say from OpenAI import OpenAI to be able to do that.

Okay.

And now what we're going to say is, uh, response equals I guess it just fills it in.

What's the point in me typing OpenAI chat dot completions dot create.

So that is basically saying I want to go to slash chat completions and I want to do a post request,

which is like doing a create.

And I'm going to pass in like I don't have to manufacture JSON.

I can just say model equals GPT five nano five nano.

Oh, I could just press tab.

You know it.

You know what I'm gonna say?

messages is that list of dictionaries that I passed in right there.

And then instead of this fussy JSON plowing, I can just say response dot choices.

Zero choices, zero dot message content.

And I'm just going through Python fields.

It's actually pedantic object of course, for people that know this stuff.

Uh, and uh, as a result, it's got all of the, all of the nice features of using proper Python methods

and attributes like, like the ID can check, I've spelt it right and things like that.

I'm not just guessing, uh, values to to look into the JSON.

I know that I've got this right.

And so I run this and we should get ourselves a new fun fact.

And the thing to, to understand, uh, here we go.

Bananas are berries, but strawberries aren't.

Strawberries are not.

And and there we go.

We've got our explanation for that.

A lovely, fun fact from GPT five nano.

I hope you've been enjoying yours too.

Of course, the point I'm trying to make to you is that this is exactly the same as doing it the boring

way.

It's just this looks a little bit more elegant and simple.

And the reason I show you this is so that hopefully, and maybe this is all obvious to you, in which

case, well, hang on in there.

But if not, it just shows you that's all that this library is.

At the end of the day, it's just a nice, simple wrapper around making an HTTP call to an API in the

cloud.

And it by default it looks for OpenAI API key.

And that's what it puts in that bearer in that that authorization field in the headers of the HTTP request.

And it's as simple as that.

We have made our call to OpenAI on the cloud.

It looks like we did it with code.

In fact, it was an HTTP call over an endpoint.

Over an endpoint.

What I meant to say was it's an endpoint call over HTTP.

All right.

So I've already typed all this.

We can get rid of that.

Get rid of that.

So so then something cool happened.

So then OpenAI's chat completions API was so popular that all the other providers started to offer an

identical end point.

So you could use exactly the same web request and and just call a different model instead, because

people sort of converged on OpenAI's approach as a way to do it.

So everyone else offered them to Gemini.

Google, for example, they had one endpoint that was very specific to Gemini, and they were like,

oh, I know what, we'll just give a second one that's identical for anyone that wants it.

And out of all of them, anthropic held out the longest.

They they obviously their enemy OpenAI.

They didn't want that to become the standard.

So they held out.

But in the end they relented.

And they also created an OpenAI compatible endpoint.

So now all of them have it.

And for example, this is Google's one.

It's Https generative.

OpenAI.

It's got OpenAI in its name.

That is their endpoint for making OpenAI compatible request.

And because everybody did this, OpenAI decided, oh, I guess we'll be good corporate citizens, good

AI citizens.

Reasons.

And we'll say to people, look, you can use the same OpenAI library.

The same client library will allow you to say, hey, I don't actually want to talk to OpenAI's endpoint.

I want to switch to a different endpoint like this.

You can say OpenAI base URL equals and switch to a different URL and pass in a different API key.

And if you do that, then you can talk to Gemini using OpenAI's client library because it's just simple,

lightweight code.

That's it.

And so yeah, just to be crystal clear, even though OpenAI is is in our code here, we're not actually

using anything to do with OpenAI's models.

We're just using their lightweight code to make an endpoint request.

And you might know all this back to front.

This might be super obvious to you, but you'd be amazed how many people are confused by this and that

this can be really refreshing to understand exactly how this works.

So if you're in that category, then then, then I'm pleased to hear it and it's going to hopefully

make a lot of sense what we do next.

Okay.

So uh, we are going to look at Gemini's base URL.

That's the one I just showed you.

It's like a Google URL with OpenAI stuffed at the end of it.

In my EMV file, I have a Google API key.

Now you might not do it, in which case, don't worry, just just skip this next one.

But if you do, if you've got a Gemini or a Gemini API key, then then hopefully you've added Google

API key to your EMV file.

Uh, then then you can run this cell and uh, I'm just going to run it and it's going to confirm that

it found a Google API key in my EMV, and that it has the right format R and Z.

And if you yeah, if you want to get yourself an API key, you can you can head over.

The instructions are in guide nine.

That will tell you exactly what you need to do.

Okay.

So now look at this line Gemini is OpenAI.

Remember it's just the lightweight Python client library.

We give it the Gemini base URL and we give it the Google API key.

And that has now given me a, a basically like a class that's ready to connect to Gemini.

Okay.

So what we can basically do is take exactly the same code that we had before.

Come back up here.

I can just copy this.

I can come down here and I can use the identical code.

The only difference is I'm going to change the word OpenAI to be the word Gemini.

And I'm obviously also going to change the model.

If I try and talk to Gemini with GPT five, it'll say what it's not.

That's not our stuff.

So we have to make this something like Gemini 2.5 Pro.

We'll get a fun fact from the top range model.

Let's check.

This looks right.

It does.

Let's give this a whirl.

So we are now asking Google's strongest model on the planet.

As of now, you might be a Gemini three user, in which case you will have great fun getting a hopefully

a more profound fun fact than my one.

Uh, and here is the answer a group of flamingos is called a flamboyance.

Uh, there you go.

Uh, if you knew that, then you're obviously very good at trivia.

Um, but there is the short, uh, and punchy fun fact from Google's Gemini 2.5 Pro, and I hope you

enjoyed it.

    do same for this:

Okay.

And now we also can use the same trick to connect to a llama running on our local box, because llama,

the product also gives an endpoint locally that is compatible with OpenAI as well.

So first of all, before we do anything, let's just make sure llama is actually running.

If you if you installed it before yesterday like you were meant to, then it will be.

And if you run this little test here, you should see a llama is running.

If you don't see a llama is running, then make sure that you've installed llama.

There are instructions in guide nine if you if you missed that one, or of course in the Readme for

the project.

And then you might need to run a llama.

Serve in a terminal, open a terminal and type llama serve if you need to, or just double click on

the llama application to launch it.

Any of those things might be needed if you do llama serve, and you get an error that says that the

port is already in use, it means it's already running.

You're in great shape.

Okay, so the next thing we're going to do is download the model called llama 3.2.

And if you already tried this yesterday.

Then you don't need to do it again.

And if you've already done it, it will just run really, really fast.

So if I run this now, it should run nice and quick for me because I already have it.

But for you, it probably took a little minute.

If it's too big for you, this this takes up a few gigabytes.

Um, and it might be bigger for most machines.

It should be fine.

But if not, just add colon one b to get the really small version of llama.

But assuming that this is good, then you've got that.

Okay.

And now we use the same trick and llamas base URL looks like this localhost 11434 slash v1.

That v1 is like a call out to the fact that OpenAI had slash v1 chat completions create.

Uh.

So, uh, yeah, this is, this is V1 by analogy.

And we can create OpenAI.

The base URL is llama base URL, and the API key can be anything you want.

It gets ignored.

This is a local a local model.

There's no your credit card isn't there?

There's no no secret needed, of course, but I think it's a it's a mandatory field.

So I think you have to pass in something you can pass in any, any, any word that you like.

Um, and now we're going to just take the same code as before, the same ones we've used a couple of

times now.

Copy.

And let's get a fun fact here.

So we'll, we'll put it down here in this cell.

I'll paste.

And of course we're going to not do, um, I'll run that cell and then we're not going to do Gemini,

we're going to do Olama.

And for the model you need to put the name of the model that you just downloaded.

So we'll use llama 3.2, uh, and uh, otherwise exactly the same.

Now if you've got the smaller variant then you do colon one b there.

But you need to have done a pull to have brought that model in or you'll get an error message.

And so ready let's run this and see what happens.

Let's run it, run it, run it.

And here we go.

Did you know there's a type of jellyfish that is immortal?

The turret, the turritopsis, also known as the immortal jellyfish.

There you go.

If you didn't know it, you know it.

Now, by the end of this, you're going to win at all trivia challenges.

There is your your next fun fact.

But the cool thing is that the last two fun facts we retrieved from Frontier models OpenAI in the cloud

and from Gemini and Google's Cloud.

This one was just created on your local machine by llama running there, even though it was pretty quick

for me.

Uh, that's that what was actually happening there.

And that's very satisfying.

There's something nice about that.

And if you if you care a lot about data privacy and you want to be absolutely sure that that the data

isn't leaving your computer, then this is the way to do it.

You could unplug from the internet and I would stop talking to you.

Uh, but but you would still be able to do this.

Uh, so, uh, so so that's that's a great thing to know.

And I just put here the obvious.

I mean, we'll talk much more about this in due course, but obviously the big benefits, there's no

API charges, it's running locally and data doesn't leave your box.

The downside, of course, is that llama is something like a thousand times smaller than using most

frontier models, and you'll see that that's reflected in in the.

How intelligent it is and and how capable it is at solving difficult tasks.

So obviously those are the trade offs that you have to make.

And there are many more trade offs that we will talk about in time.

Okay.

So with that, it's time for you to experiment and start playing with with different models.

And I want to show you one more open source model.

I want to show you a variant of Deep Seek.

You remember I told you that deep seek came in much smaller sizes.

The main one would never run on my computer.

The smaller sizes would.

And in particular, I want to show you one that's called deep seek R1 colon 1.5 B it's very small 1.5

billion parameters, a small version of deep seek.

And as I explained before, this is in fact not deep seek.

Originally, what they've done is they took Quene the model from Alibaba Cloud.

They took the 1.5 billion size version of Quene, and they trained it more with extra data.

And that extra data was data that was generated by deep seq.

The big model.

It generated tons and tons of data to sort of show its intelligence, and it trained the little quon

to be able to try and replicate deep seq.

That's how it worked.

Known as distillation distilling.

So we can we need to start by making sure that we pull this.

So I do a lama pull.

There we go.

And kindly cursor fills in for me.

Now I haven't downloaded that.

So this is now going to run.

So you can see how long it takes for me.

Uh and it's downloading.

Hopefully my computer isn't going to free freeze up, but but the trace is coming there.

So once that's downloaded, we'll be able to then run some code.

So let's let's get started on this.

We're going to, to basically be able to take the same thing that we did before this exact code.

We don't need to we'll still be using Lama locally.

So we don't need to do anything new there.

And we can just change this here.

The model name to be the model name of deep seq R1 1.5 B.

So I'm just going to be able to replace that in there like so.

And as soon as this is finished pulling so that we have the model locally, I will now be it.

There we go.

It says success.

I should now be able to call Deep seek R1 1.5 be deep seek distilled into Quen running on my local computer

with the same message.

Tell me a fun fact.

Let's see what happens.

Off it goes.

Uh, here's a fun fact for you.

Did you know that the sun looks like a golden crown during sunset, with its gold colored reflection

on the horizon?

The rest of it is pale yellow.

This interesting correlation change coloration, not correlation coloration, changes as the weather

changes.

When the sun is high, it looks colder, and when it sets it appears hotter yellow.

So.

Aha!

So fair enough.

Not such a riveting fun fact, but not no surprise, this is twice as small as llama, and it's several

thousand times smaller than the frontier models we worked at before.

So to be honest, it's super impressive that it even answered the question and came up with something

credible in the first place.

But this gives you, first of all, a good sense of how you can run deep seq and any any open source

model locally, and also some perspective on the different abilities of the different sizes of models.

And now it's over to you to experiment with different models, pull them and run them.

Keep experimenting.

It's all about experimentation and have fun asking questions like fun facts or whatever question you

want to open source models running locally courtesy of Olama using the OpenAI compatible endpoint.

And now the assignment for you please a homework assignment for day two.

And that homework assignment is look at what we did in day one, the summarization of a web page, and

replicate that using Olama using any open source model of your choosing, using Olama, running it locally

so that you have a web page Summarizer that gives you a summarized markdown version of a website using

an open source model via a llama.

That's what you should complete.

Put that in the cell right here.

Uh, there is a solution and the solution is folder.

But don't look there.

You can do it.

Easy peasy.

Copy and paste from day one.

Make a change to the to the the open AI Python client library, just as we did above, and figure out

how to make that work.

And when you've done so, then by all means put it in community contributions.

Put your change in this folder community contributions, and then look to make a PR so it can be included

with the others.

And particularly try to see if you can't do something fun with it.

Maybe mess around with the system prompt, see if you could maybe change it to a different language,

or use a different tone, or have some aspect of it that makes it unique to you.

A different take, a different take on summarizing a web page using an open source model.

That's your assignment.

I can't wait to see what you come up with.

And with that.

With that, your 5%, your 5% along the journey.

It's the end of day two.

You've got got two days in and already hopefully you've got a good handle on what it's going to take

to get you to being proficient on the steps along the path.

You've got your first impression of the frontier models from our experience with OpenAI and Gemini,

and you've also got an impression of the open source models, and you've used the OpenAI API, and you've

used Olama to make web page summaries.

You've done your task.

I hope your assignment, you're feeling more confident with this.

And for me, most important of all, if you didn't already have this clear in your mind about what a

Python client library is, what a what an endpoint is, and how using OpenAI the chat completions API

simply a web request then.

Now hopefully that is crystal clear for you.

What we're going to do next time we're going to be comparing frontier models.

We're going to be to be playing with them through the products, through through the their websites.

We're going to be getting a sense for what are they good at and what are they not so good at as an interesting

way to get a deeper perspective on them?

I can't wait to show it all to you.

Lots to get done.

See you on day three.

  