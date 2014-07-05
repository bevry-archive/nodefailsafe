# Workshop

This document contains advice for those running Error School as an in-person workshop.

Currently, we have only done 7 exercises of our many awesome envisioned ones. They are also not yet implemented using the node school executable, so must be worked through manually. As answers are contained in the markdown files, a mentor should issue the problems separately from the answers.

Here is a possible flow for an in-person workshop:

1. Hello.
1. Node School is a community run event.
1. Thank you to the event sponsors.
1. Provide the students with the first problem description via a short url, or via a USB key.
1. Once each student has completed, review their answer, provide feedback, if okay, then issue them the next problem. Repeat.
1. The last exercise `07-fetchall` will take a lot of time. Use this time to prepare for the rest of the workshop.
1. Take a lunch break when people are hungry, kick everyone out, and resume together.
1. Once a student has completed `07-fetchall`, sit with them individually and go over debugging:
 	1. Cover missing completion callback with zero urls by using [tracegl](https://github.com/traceglMPL/tracegl)
	1. Cover step by step debugging to verify the issue with [node-inspector](https://github.com/node-inspector/node-inspector)
  1. If another student finishes while you are debugging, get them to consider the `Bonus Points` section, using [joyent errors](http://joyent.com/developers/node/design/errors) as a reference
1. Once a student has completed debugging and the `Bonus Points` section, that is, understanding the unexpected state that may occur, get them to consider how they could optimise their code, handle the errors better, and perhaps use a flow library to solve these issues.
  1. [TaskGroup](https://github.com/bevry/taskgroup) is a flow control library that handles errors correctly
1. Reinforce the lessons and skills the student has now gained
1. Finish up, thank the students, compliment them on their awesome new skills, and wrap up