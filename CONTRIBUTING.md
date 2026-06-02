Contributing
============

Before anything else, thank you for taking some of your precious time to help
this project move forward. ❤️

If you're new to open source and feeling a bit nervous, we understand! We
recommend watching [this excellent guide](https://egghead.io/talks/git-how-to-make-your-first-open-source-contribution) to give you a grounding in some
of the basic concepts. We want you to feel safe to make mistakes, and ask
questions.

If anything in this guide or anywhere else in the codebase doesn't make sense to
you, please let us know! It's through your feedback that we can make this
codebase more welcoming, so we'll be glad to hear thoughts.

You can chat with us in the `#committers` channel in our
[Community Discord](https://cucumber.io/community#discord), or feel free to
[raise an issue](https://github.com/cucumber/common/issues/new?template=developer_experience.yaml)
or [start a discussion](https://github.com/orgs/cucumber/discussions) if you're
experiencing any friction trying to make your contribution.

## About to contribute?

Great! Before making a significant contribution, consider discussing the outline
of your solution first. We don't want to waste your or our time working on
something we might not accept.

Please also read this guide all the way through.

## AI Usage

Cucumber is developed and maintained by humans. We read every discusison, issue
and pull reqeust. As such we appreciate it if you take the time to communicate
with us.

When you use AI, we require that you do the following:

* **All AI usage in any form must be disclosed**
  You must state the tool you used along with the extent that the work was
  AI-assisted.
* **You must fully understand all code.** 
  You should be able to explain what your changes do and how they interact with
  the greater system without the aid of AI tools.
* **You must be in the loop**.
  Any content generated with AI must have been reviewed and edited by you
  (a human) before submission.

## Project organization

_Note: This is the catch-all contributing guide and not all repositories are
organized the same. So this guide can't help you with the specifics. The
repository might have a dedicated `CONTRIBUTING.md`. If so, you'll want to
consult that instead of the next two sections._

In general there are two types. Polyglot and monoglot.

### Polyglot

Polyglot repositories contains identical implementations of a component in
different languages. It typically looks like this:

```
.
├── README.md
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
├── dotnet          # the dotnet implementation
├── java            # the java implementation
├── javascript      # the javascript implementation
├── python          # the python implementation
├── ruby            # the ruby implementation
└── testdata        # test data for acceptance tests
```

Generally speaking, each implementation can be build independently of the
others using the tools typical for that language. If you don't know what those
are `.github/workflows/test-<language>.yaml` would be a good place to start.

### Monoglot

If the repository you're looking at doesn't look like a polyglot repo it 
probably is for a single language. In this case it can probably be build using
the tools typical for that language. If you don't know what those  are then
again `.github/workflows/test-<language>.yaml` would be a good place to start.
