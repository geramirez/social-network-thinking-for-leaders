# What Engineering Leaders Can Learn from Social Network Analysis

My academic background is in Sociology, Anthropology, and Arabic. The only reason I write code is because I fell into a cross-disciplinary collaboration with some people in the Computer Science department in graduate school — but that’s a story for another time. What matters here is that this unusual combination of fields led me to apply a research method called Social Network Analysis to my engineering team, and what I found has changed how I manage.

This post is based on a talk I gave at DEVWorld 2025. The slides are in [`slides.md`](./slides.md) and the data collection scripts are at [geramirez/gh-graph-explorer](https://github.com/geramirez/gh-graph-explorer).

-----

## What is Social Network Analysis?

Social Network Analysis (SNA) is a research method for studying social structures through the use of network and graph theory. It came about in the 1930s, during the same period that saw the rise of mass survey techniques and the large-scale application of quantitative psychology — think IQ testing entire populations, or compiling survey responses across thousands of respondents. At the time, a lot of sociologists and psychologists were genuinely excited about these new methods. Others were more hesitant, worried that the act of turning people into data points would strip away the very thing that made them human.[^1]

That tension matters, and I’ll come back to it.

SNA evolved alongside computing technology. What started as manual network mapping gradually became tractable on early computers, and then computationally powerful in the 2010s as Web 2.0 made human interaction data abundant and observable at scale. The image below is a real example — a visualization of a polarized political conversation on Twitter, where two ideological clusters talk almost exclusively within themselves.

![A polarized Twitter conversation mapped as a social network](./images/pew-center-american-politics.png)

-----

## A New Lens for an Old Problem

As managers, we navigate a constant tension between two roles. On one hand, we are accountable for performance. On the other, we are responsible for cultivating the conditions under which people can do their best work together — distributing opportunities, clearing blockers, keeping communication lines open. These roles are not just different; they are often in direct opposition.[^2]

For this reason, I want to focus on the second role. And I think SNA offers something genuinely useful here: a different point of view on a team that the typical management literature doesn’t quite capture.

When you join an organization, the first thing you receive is the organizational chart. The message is clear: the CEO sits at the top, and power and influence trickle down to individual contributors. But this is a mechanical projection of a much more complex, organic system. A social network graph shows you something different — who the real influencers are, who the connectors are, where the relationships that cross formal organizational lines actually lie, and who is quietly central to everything.

The org chart shows reporting lines. The social network shows how work actually gets done.

With that lens, I was able to start asking questions I had always felt but never been able to quantify:

- What causes collaboration to drop?
- Can we actually see context silos in data?
- Can individuals be too central to a network?

![Two years of my team’s network connectivity over time](./images/network-graph-for-my-people.png)

-----

## How I Applied It

I work at GitHub, where we joke that if it doesn’t happen on GitHub, it doesn’t really exist. That made data collection practical. Using scripts from [gh-graph-explorer](https://github.com/geramirez/gh-graph-explorer), I started collecting interaction data in January 2024, storing it as a CSV edge list of usernames, interactions, and GitHub resources.

![An example CSV edge list](./images/edge-list.png)

I analyzed this data in two ways. The first is what network scientists call a **bipartite network** — a people-to-resources graph. GitHub User A created Pull Request 1; User B commented on it; User C approved it. Everyone is connected through the work artifacts they touch. I like this view because it shows you what people are actually working on and surfaces the hidden structure of how the team’s attention is distributed.

The second is a **unipartite network**, collapsing the bipartite graph into a people-to-people view. This graph is smaller and denser, and it shows you something simpler but important: who is working with whom, and how often. No resource noise — just the collaboration pattern itself.

For visualization I used [Gephi](https://gephi.org/), [vis-network](https://github.com/visjs/vis-network), [Cosmograph](https://cosmograph.app/), and [Neo4J](https://neo4j.com/) — each for different reasons. Neo4J has an excellent Cypher query engine that lets you extract exactly the slice of the network you need. Cosmograph is elegant for high-level exploration and zooming into specific clusters.

![1.5 years of the GitHub Notifications Team network visualized](./images/notifications-team-graph.png)

I also ran computational analysis every two weeks, capturing network density, node count, and mean node connectivity. What emerged was a narrative graph — a timeline of how my team was doing, one I could interpret in real time because I understood the context behind the numbers.

-----

## Four Patterns from Two Years of Data

Over two years of consistent analysis, four patterns stood out — each one teaching me something I had suspected but never been able to show.

### 1. Team On- and Offboardings

It is fairly well documented that adding a large cohort of people to a team initially tanks velocity before it improves it.[^3] What I found is that the same thing happens in the network: when several engineers join at once, connectivity drops sharply. The two marked dips in the graph below correspond to exactly those moments.

![Network connectivity graph showing drops during large onboarding events](./images/network-graph-for-my-people-onboarding.png)

I don’t know exactly why. My best theories involve the disruption of daily rituals, the cost of introducing new people into existing communication patterns, and the overhead of getting newcomers oriented. But whatever the mechanism, it happens reliably enough to plan around.

The mitigations I found useful were a buddy system and onboarding round robins — formalizing the expectation that each new person would meet every member of the team in the first weeks. Even short conversations gave people enough rapport to reach out later when they needed to. I also started putting real weight on team rituals: questions of the day, short games at the start of meetings. I resisted this at first. A staff engineer had to convince me that building the team *is* the work, not an interruption of it. He was right. Getting people talking at the start of a meeting — even about something silly — made them noticeably more willing to collaborate afterward. And the simplest mitigation of all: don’t let so many people join at once. I learned that one the hard way, twice.

### 2. Cliques and Silos

Looking at a snapshot of the network one month, I saw something striking: two distinct groups on either side of the graph, connected only through a single node in the middle. That node was me.

![Two distinct cliques visible in the team network graph](./images/cliq-silos.png)

My immediate instinct was to intervene — everyone should be able to work on everything, right? That’s the agile playbook. A staff engineer pushed back. He said: I’m doing a complex database migration. Someone else is working on an AI chatbot. These are entirely different problems, and knowing everything about the other side of the codebase isn’t going to help me. It’s just going to add context I can’t hold in my head.

That conversation reframed how I think about silos. The first question I ask now, when I see a clique in the data, is whether it’s actually a problem. Silos can be positive when the problem is genuinely deep and the context cost of sharing would hurt more than the isolation. They become detrimental when the group is too small, or when a bottleneck is actively blocking the rest of the team.

When they do need to be broken up, the most effective tools I found were controlled rotations — moving one person at a time, not disrupting the whole network — and cross-team projects on generic topics like observability or availability, which create new connections that tend to persist long after the project ends.

### 3. Seniority Bottlenecks

In one network snapshot, I saw two nodes tightly clustered at the center surrounded by work, and two others clearly on the periphery. After some one-on-ones, the picture clarified: the two junior engineers didn’t know how to get in. The senior engineers were available and willing; the juniors just couldn’t find the entry point.

![Senior engineers at the center of the network, junior engineers on the periphery](./images/central-senior.png)

This is a catch-22 that I’ve seen in every team I’ve worked on. To become more senior, a junior engineer needs to do senior things. But to do senior things, you need to be given senior work. And organizations, often for understandable reasons, are reluctant to give junior engineers senior-level responsibility. The result is that junior engineers get stuck doing work at a scale that doesn’t build the leadership and communication muscles they need.

Two things helped. The first was mob sessions — getting a group of engineers together to work through a single problem, usually a customer escalation. With a good facilitator, these sessions become genuine knowledge transfer: a senior engineer walking through their diagnostic process is teaching the whole room how to access certain databases, what the error logs mean, how to approach the problem. The second was junior-only task forces: real problems, not time-bound, handed entirely to a small group of junior engineers with no senior involvement. Counterintuitive, and it made my senior leadership nervous — but it gave junior people the chance to practice accountability and communication at a scale that wasn’t dangerous to the organization.

After about six weeks of both, the before-and-after graphs were striking:

|Before                                                                      |After                                                            |
|----------------------------------------------------------------------------|-----------------------------------------------------------------|
|![Senior engineers central, juniors peripheral](./images/central-senior.png)|![All engineers well connected](./images/seniors-and-juniors.png)|

You couldn’t tell who was senior and who was junior anymore. Everyone was just operating as a team.

### 4. Manager Bottlenecks

This was the hardest pattern to notice, because it was about me.

The first time I took vacation, the team’s network connectivity dropped. My initial reaction was exactly what you’d expect: *of course it dropped, I’m the manager, I’m the glue.* Then I took a slightly longer vacation, and it dropped further.

![Network connectivity graph showing drops during my vacations](./images/network-graph-for-my-manager.png)

I sat with that for a while. And eventually I came to a different conclusion: this wasn’t a team problem. It was a management problem. I was too central, and my centrality was holding the team back from moving forward when I wasn’t there. That’s not a sign of strong management — it’s the opposite.

The shift I made was to stop focusing on technical solutions and process, and start focusing on *why*. I spent my one-on-ones helping people understand the direction we were headed and why it mattered — not telling them how to do their work. Then I held them accountable to outcomes, not process. After a few weeks, the team had what I can only describe as an internal compass. They didn’t need me to assign tickets. Everyone understood what was important and could make decisions in the same direction without me in the room. I also started actively discouraging the instinct to wait for me — or for anyone else — before moving forward. If the staff engineer wasn’t there and a decision needed to be made, someone else made it. If it turned out to be wrong, we reversed it. It is not the end of the world.

-----

## Did Any of This Work?

Looking at the connectivity trend over two years, I believe it did. The lows got higher, the highs got higher, and a trend line would clearly slope upward — even through a period of significant team growth. I also had something the data can’t provide: people telling me, unprompted, that the team felt like a team.

![Two-year connectivity trend showing upward progress](./images/network-graph-for-my-people.png)

-----

## Social Network Analysis with AI

At some point I built what I started calling a second brain for this work: a Neo4J database with two years of anonymized data, connected to Claude via an MCP server.

![Neo4J, MCP, and Claude Desktop logos](./images/neo4j-logo.svg)

I could ask it questions about specific time periods and get quick insights about group dynamics. It was genuinely useful.

![A Claude prompt exploring team network data](./images/claude-prompt.png)

And then I asked it something that gave me pause. I asked it to give me a list of the lowest-ranked collaborators. It returned one. And when I cross-referenced that list against my own understanding of who was struggling on my team, it was uncomfortably close.

![Claude returning a list of low collaborators](./images/claude-explorer.png)

For a moment I thought: could this be a performance management tool? Could I automate some of this?

I stepped back from that idea. And I want to be clear about why.

-----

## Ethics and Limits

When SNA was being developed in the 1930s, it emerged alongside a serious intellectual debate about whether turning people into data was actually good for humanity.[^1] Some researchers worried that quantifying people would remove the very thing that made them human, and that the data would encode the biases of whoever designed the collection method. I think they were right to worry. The same concern applies here.

Using SNA for individual performance management would be a mistake — not because the data isn’t interesting, but because the data can’t carry that weight. Someone with high connectivity might be a leader and influencer; they might be doing crucial glue work connecting teams that wouldn’t otherwise talk; or they might be someone who opens issues they can’t finish and creates noise that others have to clean up. All three look similar in the graph. Someone with low connectivity might be on vacation, doing deep research that requires concentration, or experiencing a personal conflict that needs a completely different kind of intervention. The graph doesn’t know which is which. Only a manager who has built actual relationships with these people can know.

To keep myself honest, I follow three personal ethical principles:

**Transparency.** The people whose interactions I’m analyzing know about it. I shared the metrics and insights with the team every two weeks.

**Groups, not individuals.** SNA is for understanding team dynamics — how collaboration flows, where it’s blocked, whether things are improving. It’s called *social* network analysis for a reason; it’s not a tool for ranking people.

**Bias and fairness.** I happened to work at a company where almost everything happened on GitHub. If important work happens in Slack, in meetings, or in conversations your tooling can’t capture, your data will miss it. Know the limits of your collection method before drawing conclusions.

-----

## The Network Still Needs a Manager

Social network analysis has given me a lens I didn’t have before. It showed me — in data — things I had long suspected but never been able to quantify. It gave me evidence that interventions were working, or not. It made the invisible visible.

But the graph can’t tell me why someone went silent for three weeks. It can’t tell me why, after two months of saying nothing in mob sessions, a junior engineer finally found the courage to offer to drive. Those things don’t live in the data. They live in the relationships you build over time, in the one-on-ones, in the trust that makes people willing to be vulnerable in a room full of people more senior than them.

![The full team network graph](./images/notifications-team-network-graph.png)

Social network analysis clarifies a snapshot. It doesn’t tell you why, or what to do about it.

That’s why I always come back to the same conclusion: **the network still needs a manager.**

-----

*Slides: [`slides.md`](./slides.md) · Data scripts: [geramirez/gh-graph-explorer](https://github.com/geramirez/gh-graph-explorer)*

[^1]: This tension runs through a lot of critical theory from the period. Adorno and Horkheimer’s *Dialectic of Enlightenment* (1944) is the sharpest articulation: as we develop ever more powerful methods for studying and optimizing human systems, are we also stripping away the things that make those systems human?

[^2]: I’ve written about this tension before — the performance manager and the environment shaper are genuinely in opposition, and most management literature doesn’t reckon seriously with that.

[^3]: This is sometimes called Brooks’ Law, from Fred Brooks’ *The Mythical Man-Month*: adding manpower to a late software project makes it later. The same logic applies to team cohesion more broadly.