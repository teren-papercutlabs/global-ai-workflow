# Good material to read

[https://ezyang.github.io/ai-blindspots/](https://ezyang.github.io/ai-blindspots/)  
[Elie's cursor rules](https://x.com/elie2222/status/1907070722960187745?t=qpM7b05p1tMkPnMhte3xSQ&s=19)  
[Tester, Architect & PM walked into a codebase: How to make vibe coding work](https://pixels.place/p/vibe-coding-decoded-taming-the-chaos?utm_source=share&utm_medium=android&r=ql82&triedRedirect=true)
[Beyond the AI MVP - What it really takes](https://blog.lawrencejones.dev/ai-mvp/)

At the start of this year, I decided to try starting my own startup as a solo, bootstrapped SaaS founder. I don't have any formal software engineering education or experience, although I have written extensive amounts of Google Apps Script that effectively runs as software, with Google Sheets as the frontend/database and Google Apps Script as the backend.

My hypotheses were that:

- I could learn whatever I needed to learn from online materials, especially with AI to assist my learning process
- An AI-first coding could make me extremely productive, such that I would be able to run a small startup solo
- The usage of AI to code is in itself a skill - there are good ways and not-so-good ways to do it, and I should double down on being a good _AI-first coder vs. leveraging AI to make me a good traditional software developer_

[As I build more things, use this paragraph to highlight wins and achievements from this approach]

This is not a vibe-coding tutorial. I think my approach is distinct from vibe coding in several ways:

- **Maintaining agency over the architecture.** I remain responsible for the high-level design, organization, and flow of applications, making deliberate technical decisions rather than delegating system architecture to AI. I often brainstorm the technical approach at a high level with Claude, but I make the final decisions as to what approach to use.
- **Critical evaluation.** I try to always read and understand (at a high level) what the AI tools are outputting - if something looks wrong, I don't accept the code - I get it to explain what it's doing [this actually should link to a dedicated section about it]
- **Sustainable development practices.** I integrate some traditional software development practices (comprehensive documentation, version control, structured testing) that create long-term maintainability as opposed to the short-term, disposable nature of vibe coding.

Caveats

- I don't presume to know the best way of doing this - this is a documentation of my journey and what's worked for me. If you have found a better way of doing some of these things, I would love to hear about them. For the sake of brevity, I didn't preface every line in this document with "I think". but **everything in this document is just my opinion** as opposed to some universal truth.
- The code produced via this approach is definitely not the cleanest, best written code, but it has worked so far within the scale I'm building for.
- This is not a guide as to how to write enterprise-level software. I think a lot of things I'm doing break down when working in a team, let alone a thousand-person engineering organization. If you are a developer in that context, read everything here with a bucket of salt.
- I have come to realize that a lot of the details in this document are stack-specific (Node, React). There might be a future effort to make this a stack agnostic workflow (e.g. by setting up stack-specific instructions like using Jest to test Node) in cursor rules during project setup, but the initial draft of this document will remain tailored to the stack I'm on.

# Choice of AI

[Why I chose Claude]

- I like the voice
- Projects that can connect to Gdocs and github

# Tech stack

- Stick to the most popular stacks, where the AI has a lot of training data
  - I use
    - React
      - Mantine UI/ShadCN (still figuring this out)
    - Node.JS
    - Express/Next.JS (still figuring this out)

# Principles

## Learn to code

- Knowing some coding helps a lot
  - You can speak to AI in the right terminology to help it understand what you want
  - You can spot when the AI is seriously going off the rails
    - A lot of repeated code

## Optimize for high-level readability

- The most important thing to understand is the high level - how the codebase is organized, how data flows between the different services, etc.
- AI is quite good at figuring out nitty gritty algorithms and functions. The main issue is that as the codebase gets bigger, it loses context of how everything ties together. This is where your understanding of the high-level allows you to point it in the right direction.
- Try to keep files small, but don't overabstract - if the file is too verbose, it's very hard for you to trace what's happening in the file. At the same time, if there are too many abstractions, you need to jump to too many different methods/files to figure out what is happening also. My rule of thumb - start without abstracting prematurely, and if I try to read through the code, and I am having trouble understanding what a particular method is doing, it's probably time to refactor it.

## Understand the high-level

- Organize your backend logic into service files
- Your codebase should be organized where there is good [separation of concerns](https://en.wikipedia.org/wiki/Single-responsibility_principle). You need to understand what each fi

## Be prepared to dive into the details once in a while

- Cursor will inevitably get stuck in a bug once in a while. Don't just keep asking it to try again - you need to help it along
  - Ask it specifically to write logs that will help it debug

## Don't believe the hype (most of the time)

- Every week, there's something new on X that promises "nothing will ever be the same". 99.9% of the time, this is hyperbole. I think there are very few single things that will completely change the game. More frequently, I think you gain small learnings here and there that slowly aggregate into a giant improvement in your workflow over the course of several months.

## [Use TypeScript](https://www.typescriptlang.org/why-create-typescript/)

- TypeScript is a strongly typed programming language that builds on JavaScript, adding static type definitions that can be checked at compile time. It helps catch errors earlier in the development process while giving you all the features of JavaScript plus additional type safety and developer tooling.
- This vastly improves the quality of AI code output as Cursor traces linter errors to figure out if something needs to be fixed. The strict type-checking of TypeScript ensures that if it changes a piece of code that has implications on other parts of the codebase, the linter errors will trigger Cursor to change the dependencies too.
- If AI is writing the nitty gritty code, you aren't bothered too much by the additional overhead of defining your types.

## Don't be stingy with context

- AI works much better when you give it as much context as you possibly can for each instruction
  - As specifically as possible, what you want it to achieve
  - Specific files it should look at when doing something
  - What you've tried before that didn't work
  - Logs (see here)

## Security

## Document your work

- I recommend doing this manually. See more [here](#bookmark=id.hmqndpygrf91).

# General workflow

## Overview

- High-level discussions in Claude about how we should build something
- Discuss the work breakdown with Claude - do we need to break it down into multiple tickets for Cursor, or will one ticket suffice?
  - This is turning out to be more an art than a science
    - The reliability of Cursor improves when you break down the task into more tickets
    - However, this takes more time and costs more, and Cursor can actually do a lot in a single prompt when given sufficient context and you're using TypeScript (or some other strongly typed language) so it catches its own errors
    - I am typically a bit ambitious with the ticket scope, and if Cursor then isn't implementing it properly, I go back to Claude and ask it to break down the task into more tickets
  - The output of each ticket should be testable - if you need 3 tickets to produce something that you can test, you've broken it down too far - see below for how to test
  - I have read that test-driven development is actually an excellent way to work with Cursor. In theory, it extends the self-catching mechanism like I've described with TypeScript to the code functionality as a whole, which sounds amazing. I haven't tried this, however. In general, my test coverage is a bit spotty, that's probably something that needs to be improved going forward.
- Ask Claude to write a ticket for each piece of work
- Record the ticket in Linear
- Do a manual read-through of the ticket
- Give the ticket to Cursor
- Test the result
  - Frontend - easy to test
  - Backend
    - Ask Cursor to write a test script to test the functionality in isolation
    - For APIs - Ask Cursor to write a curl that you can put in Postman to test
- Update the readme (sometimes, if there are multiple tickets relating to the same piece of work, I update the readme all at once after all the tickets as opposed to updating after every ticket)

## Wavebox

[Wavebox](https://wavebox.io/) is a browser that I use to [add more details]

## WIP - New full-Cursor workflow for planning instead of jumping between Claude Projects and Cursor

### Rule System

The Cursor rule system provides a structured way to maintain consistent coding practices and workflows across projects. It consists of two main components: global rules and project-specific rules.

#### Global Rules Architecture

- **Central Management**: Global rules are managed in a dedicated project called `global-ai-workflow` under `.cursor/rules/global-cursor-rules/`
- **Cross-Project Synchronization**: Other projects maintain synchronization with global rules through:
  - Copying the directory structure - each project's `.cursor/rules/global` syncs with `global-ai-workflow`s `.cursor/rules/global-cursor-rules/` on folder open or can be manually triggered via ctrl-shift-P "Cursor: Sync Global Rules"
  - Using symlinks to reference global rules
  - Management via a custom VSCode extension

This architecture ensures that:

- Rules are maintainable in a single source of truth
- Changes propagate consistently across projects
- Projects can extend global rules with their own specific requirements
- Rule creation follows standardized processes

#### Rule Creation Process

- Global rules must only be created using the agent from within the `global-ai-workflow` project
- Cursor will read and follow the rules for this defined in create-global-cursor-rule and cursor-rule-instructions
- Project-specific rules can be created using the agent from any project, it will follow the rules defined in create-project-cursor-rule and cursor-rule-instructions

### Claude (w/ projects)

### Overview

Claude Projects serves as my high-level development partner, providing organization, context awareness, and strategic guidance. I create a dedicated Claude Project for each software project, connecting relevant GitHub repositories and documentation to maintain comprehensive context throughout development.

### Feature Complexity-Based Workflow

My approach scales based on feature complexity:

### **Simpler Features**

For smaller, straightforward features:

1. **Optional: Cursor Investigation** - If the feature touches parts of the codebase that Claude may not have full context on, I'll use the traceprompt espanso shortcut to generate a research ticket for Cursor.
2. **Direct Implementation Ticket** - For straightforward changes, I skip directly to creating an implementation ticket using the writeticket espanso shortcut, which Claude then crafts for Cursor to implement.

### **Complex Features**

For larger, multi-component features, I follow a more structured approach:

1. **Work Alignment and Context Enrichment** - I start by creating a comprehensive description (espanso: workalignment). This helps articulate the context, requirements, initial thoughts, and constraints. It ends with asking Claude to ask me any questions to complete its understanding of what I'm looking to achieve.
2. **Cursor investigation** - I then get it to generate a research ticket for Cursor (espanso: traceprompt), the output of which will be fed back into Claude to help Claude get all the context it needs.
3. **Epic Creation** - For work requiring multiple tickets, I ask Claude to create a parent epic ticket using my parentepic espanso shortcut. This ticket maintains the big picture, outlines components, defines interfaces, and establishes the integration testing strategy.
4. **Component Tickets** - Once the epic is defined, I start creating individual implementation tickets (espanso: writeticket), each with clear references to the parent epic.
5. **Integration Testing Ticket** - For complex features only, the final ticket (espanso: writeintegration) is dedicated to integration testing, which verifies that all components work together correctly.

### Project Knowledge Management

I maintain comprehensive context in Claude Projects by attaching:

1. **Project Documentation:** Initial project documents that define requirements, technical approaches, and product goals
2. **GitHub Readme Files:** Connected to the relevant repositories, focusing exclusively on README files that document the evolving codebase

This structured, scalable approach ensures Claude has enough context to generate high-quality tickets appropriate to the feature complexity, while Cursor receives focused, implementable tasks with clear boundaries and testing requirements.

## Linear

- Read through the ticket before giving it to Cursor. If you've given Claude enough context, the ticket output is often surprisingly good, but sometimes I catch some stuff:

  - Wrong file paths (although Cursor is pretty good at finding the right file even if you direct it wrongly initially)
  - An implementation plan that doesn't make sense based on how your codebase is structured e.g. asking Cursor to create a new service file when it should be adjusting one that already exists

  Most of these issues arise from a misunderstanding of the current state of the codebase, although this should not be that frequent if you've been good with documentation and syncing it with Claude. When this happens, I ask Claude what's the basis for its misunderstanding, and this sometimes helps me identify outdated parts of my documentation.

- If there are parts of the ticket I don't understand, I give Claude the ticket in a new conversation and ask it to explain those parts of the ticket. After understanding, I make a call whether to keep, remove or modify this part of the ticket. This helps me maintain agency over what Cursor implements.
- If the changes required are more substantial than removing or changing a couple of lines, I give Claude the ticket content in a new chat in the project along with my feedback, and ask it to refine the ticket.
- I won't cover general principles on how to organize your ticket workflow, nothing changes with AI-first coding compared to normal development.

## Cursor

- Use Agent mode for most work
  - Auto mode works well enough for me, my hypothesis is that if you give it enough context and direction it can work well with "auto" model selection, but for the vibest of coding, specifying more advanced models mitigates the vague instructions you give it.
- Use Ask mode when you're… asking

## Testing

- Testing is an integral part of the development workflow, not a separate phase
- Each ticket should include testing requirements alongside implementation
  - This keeps context together and treats testing as a first-class citizen in the development process
  - Ensures the AI implements tests that specifically address the functionality being built
- My ticketprompt espanso includes testing requirements:
  - A dedicated "Testing Requirements" section that specifies behaviors to test, edge cases, and any mocking needs
  - Explicit acceptance criteria that include passing tests
  - Instructions to follow Given-When-Then pattern for all tests
- The Given-When-Then pattern provides a clear structure for tests:
  - **Given (Arrange)**: Set up the preconditions and test data
  - **When (Act)**: Execute the specific action being tested
  - **Then (Assert)**: Verify that the expected outcomes occurred
- Stack-specific testing approaches:
  - Currently using Jest for Node.js/React projects
  - Test files should be placed alongside the code they're testing, for example:

| src/├── services/│ ├── userService.js│ ├── userService.test.js # Tests for userService.js│ ├── paymentService.js│ └── paymentService.test.js # Tests for paymentService.js├── components/│ ├── Button.jsx│ ├── Button.test.jsx # Tests for Button component│ ├── Modal.jsx│ └── Modal.test.jsx # Tests for Modal component |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

- To improve test coverage over time:
  - Start with critical paths and core functionality
  - Add edge case testing as the application matures

## Logging

## Version control (mostly Git)

- Follow general best practices for version control. There's plenty of material on this - read it. I try and commit after every ticket.
- This is especially important when working with AI as once in a while, it will make a big mess that is easier to clean up by starting from scratch. Cursor's "restore checkpoint" generally works, but once in a while it doesn't (especially if you want to revert to a checkpoint from an older chat), so having good Git hygiene is critical.
- Git proxies
  - Mergetomain
  - Git AC

## Espanso

- [Espanso](https://espanso.org/) is an open-source text expander which I use to create shortcuts for prompts that I use often. For example, typing : followed by "amq" will expand the text out into:
  - Before writing anything, ask me all the questions you need to clarify any questions you may have so you have a comprehensive understanding of the output I am looking for.
- This is useful for saving time giving AIs prompts that you find yourself using frequently. I'm sharing a list of prompts I use often [below](#bookmark=id.1a68f9brjfv3).

# Things I haven't categorized yet

## General tips for working with AI

- Ask it to ask you questions.

## Logging

## Readme

- What it should contain
  - High level workflows and which files are involved in what step of the workflow
  - File structure with a short description of what each file does
- Purposes
  - The act of documenting the readme after each significant change forces you to maintain that high-level understanding of your codebase. This is one of the few parts
  - AI can refer to the readmes:
    - Claude project - to gain context over the state of the current codebase. This helps it understand your project status, as well as help write tickets with more guidance for Cursor
    - Cursor - to understand the workflows and which files it needs to look at when doing a specific task
  - For you to remember what the code does - this becomes more useful especially when you're juggling multiple projects and may not be touching

# Refactoring

- Do this now and then before it gets too late
- If you struggle to follow the flow of your code, it's probably a good time to do it

# Testing

# Prompts (w/ Espanso shortcuts) I use often

| Title                             | Espanso trigger | Prompt                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Purpose/remarks                                                                                                                                                                                                                                                                                                                                                                                   |
| :-------------------------------- | :-------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Ask me questions                  | amq             | Before writing anything, ask me all the questions you need to clarify any questions you may have so you have a comprehensive understanding of the output I am looking for.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | When I'm not certain that I've given Claude enough context to do what it needs to do (like write a ticket), this gets it to ask me a laundry list of questions so it triggers me to give it all the context it needs. For best results, it's probably best to use it all the time, but for tasks that you're certain you've given it all the necessary context, you can probably skip doing this. |
| Write a ticket                    | writeticket     | Write ticket(s), the contents of which will be given to Cursor for it to implement. The ticket name should concisely encapsulate what the ticket is for. If the work has been broken into multiple tickets, prefix the ticket name with a number incrementing from 1 representing the order the tickets should be done in. Do not write code, but describe in detail what you would like Cursor to do. IMPORTANT: Tell Cursor exactly what actions to take using direct commands (e.g., 'Create a function that...' instead of 'Consider creating a function that...'). Do not tell Cursor the full schema, tell it to refer to the relevant type files. Ensure to tell Cursor not to make changes to any code unrelated to what we are trying to achieve with this ticket. At the end of the ticket, ask Cursor to ensure that the code is documented with JSDoc comments where appropriate. Tell Cursor to implement meaningful logs that will be useful to it should it need to debug the code from the logs later. This ticket should contain all of the necessary information and context to complete the task without Cursor having to ask any additional clarifying questions. It should have the following sections: - Context (explain what the feature is meant to achieve for the user) - Description (an explanation of how the feature works) - Implementation details - how the code is to be written and would work (without actually writing code) - Testing Requirements - specify what unit tests must be implemented to verify the functionality: _ List specific behaviors to test _ Identify important edge cases _ Specify any mocks or stubs needed _ Instruct Cursor to follow the Given-When-Then pattern for all tests, with explicit comments separating each section _ Place test files alongside the code they test (e.g., userService.js → userService.test.js) _ Use Jest for testing (with appropriate assertions) - Acceptance criteria - including passing all specified unit tests - Technical notes Tell Cursor that integration tests will be handled in a separate ticket, so it should focus only on unit tests for individual functions, methods, or components. | To get Claude to write a ticket for Cursor, to be stored in Linear. I haven't gotten the "direct commands" part to work - it still asks Cursor to consider doing something e.g. "Consider implementing a temporary dual-reception period where both Vercel and Render receive webhooks to ensure a smooth transition"                                                                             |
| Refine ticket                     | refine          | We need to refine this ticket. I have inserted feedback within the ticket e.g. 'Refine: improve xyz', read them all and incorporate the feedback. If you are unsure about anything, ask me clarifying questions, if not, go straight to updating the ticket. Give me the whole ticket again, formatting and all.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                   |
| Give research ticket to Cursor    | traceprompt     | The codebase is getting quite large and you might not have enough context just from the readme. Using the readme as a base, determine what you need to know from the file contents and create a prompt for cursor to do an investigative trace, the output of which will give you context you need. Ensure to tell Cursor not to make any code changes, that this is purely investigative.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                   |
| Refine ticket with feedback given | refineticket    | We need to refine this ticket. I have inserted feedback within the ticket e.g. 'R: improve xyz' and also in chat, read them all and incorporate the feedback. Give me the whole ticket again with proper formatting (not in a code block).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                   |
