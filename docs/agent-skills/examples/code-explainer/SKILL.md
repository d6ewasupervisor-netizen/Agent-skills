---
name: code-explainer
description: Explains code with visual diagrams and analogies. Use when explaining how code works, teaching about a codebase, or when the user asks "how does this work?"
---

# Code Explainer

When explaining code, make technical concepts accessible and engaging through visual aids and real-world comparisons.

## Explanation Framework

### 1. Start with an Analogy

Compare the code to something from everyday life that shares similar patterns or behaviors.

**Examples**:
- A function → A recipe
- A class → A blueprint
- A loop → An assembly line
- A callback → A restaurant pager
- An API → A waiter at a restaurant

### 2. Draw a Diagram

Use ASCII art to visualize:
- Control flow
- Data flow
- System architecture
- State transitions
- Relationships between components

**Example Flow Diagram**:
```
User Request
    |
    v
[Controller] ---> [Service Layer]
    ^                  |
    |                  v
    |            [Database]
    |                  |
    +------------------+
         Response
```

### 3. Walk Through Step-by-Step

Explain what happens in sequence:

```
Step 1: User submits form
Step 2: Controller validates input
Step 3: Service layer processes data
Step 4: Database stores information
Step 5: Response sent back to user
```

### 4. Highlight a Gotcha

Point out:
- Common mistakes
- Misconceptions
- Edge cases
- Performance pitfalls
- Security concerns

## Examples

### Example 1: Explaining a React Hook

```javascript
const [count, setCount] = useState(0);
```

**Analogy**: Think of `useState` like a labeled box in a warehouse:
- `count` is the label you use to check what's in the box
- `setCount` is the function to update what's in the box
- `0` is what's initially in the box

**Gotcha**: Calling `setCount` doesn't update `count` immediately - React schedules the update for the next render, like putting in a request to update the warehouse inventory.

### Example 2: Explaining Async/Await

```javascript
const data = await fetchData();
console.log(data);
```

**Analogy**: Like ordering food at a restaurant:
- `await` means "wait for the food to arrive before continuing"
- Without `await`, you'd try to eat before the food arrives
- The kitchen (promise) prepares your food while you wait

**Diagram**:
```
Time ---->
[Request sent] -> [Waiting...] -> [Data received] -> [Continue]
                      ^
                   await here
```

## Guidelines

1. **Keep it conversational**: Write like you're explaining to a friend
2. **Use multiple analogies**: Different perspectives help understanding
3. **Avoid jargon**: If you must use technical terms, explain them
4. **Build progressively**: Start simple, then add complexity
5. **Relate to experience**: Connect to things the user likely knows

## Explanation Template

Use this structure for comprehensive explanations:

```markdown
## [Code Concept]

### What it is
[Brief description]

### Real-world analogy
[Everyday comparison]

### How it works
[Step-by-step breakdown]

### Visual representation
[ASCII diagram]

### Common gotcha
[Pitfall to avoid]

### When to use it
[Practical applications]
```

## Tips

- **For complex code**: Break into smaller pieces and explain each
- **For algorithms**: Show example inputs and outputs
- **For architecture**: Draw system diagrams
- **For data structures**: Visualize the structure
- **For patterns**: Show before/after examples

## Interactive Approach

When explaining:
1. Ask if certain concepts need deeper explanation
2. Provide multiple levels of detail
3. Check understanding with examples
4. Relate to user's experience level

## Anti-Patterns to Avoid

- ❌ Using technical jargon without explanation
- ❌ Skipping the "why" to only explain the "what"
- ❌ Not providing visual aids for complex concepts
- ❌ Assuming too much prior knowledge
- ❌ Making analogies too complex or obscure
