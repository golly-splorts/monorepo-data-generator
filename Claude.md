This file contians instructions for Claude and other coding agents.

# Environment Setup

## Virtual python environments

The "vp" directory contains a Python virtual environment, so anytime
you want to run a python command, you should use "vp/bin/python" from
the root of this repository.

Likewise, the "vpp" directory (if present) contains a Pypy virtual environment,
so anytime you want to run a pypy3 command, you should use "vp/bin/pypy3" from
the root of this repository.

Pypy3 is mainly used when running gollyx-python tests/simulations, but gollyx-maps and
gollyx-backend-generator are also required to be compatible with Pypy3.

## Installing packages

The gollyx-python and gollyx-maps and gollyx-backend-generator libraries can be installed
via the pyproject.toml with the following command, run from any of their subdirectories:

```
pip install .
```

Ensure this pip refers to the virtual environment referred to above. If modifying the
library's source, use the -e flag. If running tests, include the `[dev]` packages.

```
pip install -e .[dev]
```

## Tests

The gollyx-python and gollyx-maps and gollyx-backend-generator libraries all have
pytest test suites. To run, install the package:

```
pip install -e .[dev]
```

Then run the tests with the `pytest` command.


# How To Work

## Planning - Look Before You Leap

When you are asked to perform a task, start by gathering information first.

Read code and collect information before you try to install packages, run tests, or suggest changes.

Formulate a plan based on the information you collected, and execute on it. If you see signs that the
plan does not match reality or that there were decisiosn that were unaccounted for, circle back and
ask the user for feedback.

The intent is not to overwhelm you with information before you ever lift a finger. The intent is to
increase your understanding and context, and make you better at performing complex tasks.

If we were to use smaller and less powerful models, they would not have the ability to assimilate the
entire context into a plan, and would make multiple poor decisions due to lack of context
that would all compound into a big waste of time.

## Troubleshooting - Information First, Action Second

When debugging or troubleshooting, avoid overwhelming the user by limiting the number of suggested
debugging steps and commands for the user to try. If there are multiple alternatives, and each alternative
has multiple steps and multiple commands, the user will become completely overwhelmed, and it will pollute
the context. Instead, ask the user to run commands to collect troubleshooting information and provide you
with valuable data about the real world to inform your decision.

Do not rush to judgement about the cause of a problem. Do not assume you know the cause of the problem
just because it is similar to one you have seen before in your training data. If you do not see evidence
of the cause, do not assume you know the cause.

Do not forget that the user can run this code in different operating systems and environments, so it is
important to know where the code is running when helping troubleshoot problems. Code is typically run
on Mac laptops for local development and testing, and run on Amazon Linux or Ubuntu Linux on virtual
private servers, or alternatively in a Lambda function in the AWS cloud.

## Documentation

Planning step: When you are given a large task and you are formulating a substantial plan, you should
always output an approved plan to a markdown file in the root of the repository. Give it a descriptive
CamelCase name, with a multi-word description at the beginning and the word "Plan" at the end. For example,
InfiniteRomansPlan.md or RepairHorsewithearsPlan.md.

Implementation step: When you are writing new code or modifying existing code, ensure the docstrings for
Python functions describe the input parameters, their types, and the return values. This is critical for
helping future AI coding agents navigate the code.

Completion step: When you have completed your feature, put a short writeup summarizing the feature into a
summary file in the repo root. Give it a similar name to the plan, except with "Summary" instead of "Plan".
For example, InfiniteRomansSummary.md or RepairHorsewithearsSummary.md.

