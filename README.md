# Black Hole Architecture

The [Black Hole Architecture](https://agnt.one/blog/black-hole-architecture) is a cron-first self-driving agent flow

Its crons and markdown

The simplest form of this would be an agent that wakes up every x hours, compares the current codebase with the vision of the project (a VISION.md file) and makes and executes a plan to close the gap. 

Now instead of 1 agent, figure out how to cleanly make 10 agents do that in parallel every hour. Or 100. Or.. you see where this is going

The approach here in this skill is a pretty opinionated way to scale the above. 

Every agent gets a persistent identity and owns a slice of the codebase. No agent is allowed to touch anyy of another role’s code. Sounds crazy but it keeps changes small

At hour 0 every role runs makes a plan. At hour 1 they execute the plan. Then at hour 2 they start planning again and the cycle repeats

Plans and learnings are committed to the repo

Ideally you should run this using a cloud harness but local works too

Thats basically it, have fun


