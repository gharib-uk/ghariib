---
title: "<div>Don't use TypeScript types like this. Use Map Pattern instead</div>"
date: 2025-01-29
---

### Introduction

While working on a real-life project, I came across a particular TypeScript implementation that was _functional_ but lacked flexibility. In this blog, I'll walk you through the problem I encountered, and how I improved the design by making a more dynamic approach using the **Map pattern**.

## Table of Contents

1. The problem
2. The issue with this approach
3. Solution
4. Clean code
5. More secure solution
6. Visual representation
7. Conslusion

## The problem

I came across this TypeScript type:  

```
// FinalResponse.ts
import { Reaction } from './Reaction'

export type FinalResponse = {
  totalScore: number
  headingsPenalty: number
  sentencesPenalty: number
  charactersPenalty: number
  wordsPenalty: number
  headings: string[]
  sentences: string[]
  words: string[]
  links: { href: string; text: string }[]
  exceeded: {
    exceededSentences: string[]
    repeatedWords: { word: string; count: number }[]
  }
  reactions: {
    likes: Reaction
    unicorns: Reaction
    explodingHeads: Reaction
    raisedHands: Reaction
    fire: Reaction
  }
}
```

Additionally, this `Reaction` type was defined:  

```
// Reaction.ts
export type Reaction = {
  count: number
  percentage: number
}
```

And this was being used in a function like so:  

```
// calculator.ts
export const calculateScore = (
  headings: string[],
  sentences: string[],
  words: string[],
  totalPostCharactersCount: number,
  links: { href: string; text: string }[],
  reactions: {
    likes: Reaction
    unicorns: Reaction
    explodingHeads: Reaction
    raisedHands: Reaction
    fire: Reaction
  },
): FinalResponse => {
  // Score calculation logic...
}

```

### The Issue with This Approach

Now, imagine the scenario where the developer needs to add a new reaction (e.g., hearts, claps, etc.).  
Given the current setup, they would have to:

- Modify the `FinalResponse.ts` file to add the new reaction type.
- Update the `Reaction.ts` type if necessary.
- Modify the `calculateScore` function to include the new reaction.
- Possibly update other parts of the application that rely on this structure.

So instead of just adding the new reaction in one place, they end up making changes in **three or more files**, which increases the potential for errors and redundancy. This approach is **tightly coupled**.

### Solution

I came up with a cleaner solution by introducing a more flexible and reusable structure.  

```
// FinalResponse.ts
import { Reaction } from './Reaction'

export type ReactionMap = Record<string, Reaction>

export type FinalResponse = {
  totalScore: number
  headingsPenalty: number
  sentencesPenalty: number
  charactersPenalty: number
  wordsPenalty: number
  headings: string[]
  sentences: string[]
  words: string[]
  links: { href: string; text: string }[]
  exceeded: {
    exceededSentences: string[]
    repeatedWords: { word: string; count: number }[]
  }
  reactions: ReactionMap
}
```

Explanation:

- `ReactionMap`: This type uses `Record<string, Reaction>`, which means any string can be a key, and the value will always be of type `Reaction`.
- `FinalResponse`: Now, the reactions field in `FinalResponse` is of type `ReactionMap`, allowing you to add any reaction dynamically without having to modify multiple files.

### Clean code

In the `calculator.ts` file, the function now looks like this:  

```
// calculator.ts
export const calculateScore = (
  headings: string[],
  sentences: string[],
  words: string[],
  totalPostCharactersCount: number,
  links: { href: string; text: string }[],
  reactions: ReactionMap,
): FinalResponse => {
  // Score calculation logic...
}
```

### But Wait! We Need Some Control

Although the new solution provides flexibility, it also introduces the risk of adding unchecked reactions, meaning anyone could potentially add any string as a reaction. We definitely don't want that.

To fix this, we can enforce stricter control over the allowed reactions.

### More secure solution

Here’s the updated version where we restrict the reactions to a predefined set of allowed values:  

```
// FinalResponse.ts
import { Reaction } from './Reaction'

type AllowedReactions =
  | 'likes'
  | 'unicorns'
  | 'explodingHeads'
  | 'raisedHands'
  | 'fire'

export type ReactionMap = {
  [key in AllowedReactions]: Reaction
}

export type FinalResponse = {
  totalScore: number
  headingsPenalty: number
  sentencesPenalty: number
  charactersPenalty: number
  wordsPenalty: number
  headings: string[]
  sentences: string[]
  words: string[]
  links: { href: string; text: string }[]
  exceeded: {
    exceededSentences: string[]
    repeatedWords: { word: string; count: number }[]
  }
  reactions: ReactionMap
}
```

### Visual representation

![TypeScript Types](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fv7fnvfkvsbpf8q07qhmd.png)

![TypeScript Types](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fa2ow6meqh4gcfalyitme.png)

### Conclusion

This approach strikes a balance between flexibility and control:

- **Flexibility**: You can easily add new reactions by modifying just the `AllowedReactions` type.
- **Control**: The use of a union type ensures that only the allowed reactions can be used, preventing the risk of invalid or unwanted reactions being added.

With this pattern, we can easily extend the list of reactions without modifying too many files, while still maintaining strict control over what can be added.

Hope you found this solution helpful! Thanks for reading. 😊

Go to Source
