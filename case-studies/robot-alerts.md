# Catching robot failures before users do

## Context

At a previous job, I supported around 8 UiPath robots running HR, reporting and customer service processes for several clients.

## The problem

To catch errors, I went through each Orchestrator one by one and checked the screens and logs by hand, looking for robots that had failed or were running longer than usual. It was slow and didn't add real value. Problems were often noticed late, and it was hard to see which errors kept coming back. While I was doing it, I kept asking myself: what is the point of business process automation if I'm not automating my own job?

## What we built

My manager came up with the idea of an alert system, and we built it. The system followed robot runs and reported problems as they happened.

- Followed robot jobs and errors through the UiPath Orchestrator API
- Sent errors to Slack in real time, grouped by error category
- Reported stuck processes and processes running longer than expected
- For critical errors, opened a ticket automatically in Asana, assigned it to the right person and sent an email notification

## My part

I designed and developed the system, including the connection between the Orchestrator API and Slack.

## What changed

- Errors were noticed as soon as they happened, not when someone went looking for them
- Grouping the errors by reason helped even more than fast alerts. We could see which errors were chronic and quickly decide what needed action
- We found where frequently stuck processes were getting stuck. Fixing these made the processes cheaper and more reliable for the client

## How I would build it today

Monitoring is easier now than it was then. Orchestrator already handles retries and sends error emails, and known errors can still be handled by simple rules, which is also cheaper. So I would add an agent only where rules aren't enough.

What is still hard is catching a process that is stuck or running much longer than it should, and turning all those error emails into tickets someone can act on. An agent could help here:

- Read error emails and logs, and group similar errors
- Notice runs that don't look normal
- Open a ticket with a short summary of the likely cause and a suggested fix
- Keep the client informed: explain the error, answer their questions about it, and share the action plan once a person has approved it

A person should still make the call. The agent's explanation is a guess, and sometimes a very confident wrong one.
