# Architect Rules

An architect **guide** and not **specify** technology choices. Choosing React.js is a technical decision not architectural decision. An architect should say and explain what and why some architecture decision are taken (reactive design, browser support, etc...) and let the team choose Angular, Elm, React.js

**Architect should always ask themselves if a decision help guide the team to make the right choice, or makes the choice for them.**

### Laws

#### First Law of Software Architecture

> Everything in software architecture is a trade-off

#### Corollary 1

> If an architect thinks they have discovered something that isn’t a trade-off, more likely they just haven’t identified the trade-off yet.

#### Second Law of Software Architecture

> Why is more important than how.

### Tasks

#### Continually analyze the architecture

Even when a decision has been taken 2 or 3 years ago, they should always reassess their choices to see if it's still viable today given changes in business and technology

#### Keep current with latest trends

If developers need to stay current with the technologies they use daily, architect needs to stay current on the latest industry and technology trends. 

The decisions they take tend to be long lasting and difficult to change lasting and difficult to change. 

Staying current let them prepare for future and make better decisions.

#### Ensure compliance with decisions

An architect is expected to ensure compliance with architecture decisions and design principles.

#### Diverse exposure and experience

```text
   / \
  / 1 \
 /  2  \
/   3   \
```

1. Know-Known: is the technical depth, what is mastered. It's what a developer mainly focus on
2. Known-Unknown: is the technical breadth, what we know exists but don't master
3. Unknown-Unknown: 🤷

Architect should focus on Known-Unknown to expose themselves to more context and have a greater range of tools to look at when a decision has to be taken. It's better to know a bit 10 caching solutions (pros and cons) than to master one.

#### Have business domain knowledge

Without business domain knowledge, it is difficult to understand the business problem, goals, and requirements making it hard to take decisions and suggest good solutions. 

Moreover architect needs to talk with stakeholders and developers and must understand / be able to speak the language of both.

#### Possess interpersonal skills

> No matter what they tell you, it's always a people problem. 
>
> --- _Gerald Weinberg_

Leadership skills (lead team, mentor/coach, communicate idea/architecture) is at least half the skill needed to become a good architect.

#### Understand and navigate politics

Negotiation skills is really important. Developers take decision every day and no-one cares (e.g. I will apply a strategy pattern here). As architect every decision will have a broader impact on the team but also other team or the company. It needs to be negotiated with all the other people and also need to take into account other people's agenda.
