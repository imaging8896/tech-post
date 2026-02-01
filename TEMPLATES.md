# Example Issue Templates

This document provides templates to help you create well-structured issues that will generate high-quality technical blog posts.

## Template 1: Problem-Solution Format

```markdown
**Problem Context:**
Describe the situation where you encountered the problem.

**The Problem:**
Clearly state what the issue was.

**Investigation:**
Explain how you investigated or debugged the problem.

**Solution:**
Describe how you solved it.

**Code/Configuration:**
Include relevant code snippets or configuration changes.

**Lessons Learned:**
What did you learn from this experience?
```

### Example:

**Title:** "Fixing Memory Leak in Node.js Application"

**Problem Context:**
Our production Node.js API server was experiencing memory leaks, causing it to crash every few hours under high load.

**The Problem:**
The heap memory usage kept growing linearly over time, eventually exceeding the allocated memory and causing the process to crash with an out-of-memory error.

**Investigation:**
1. Used `node --inspect` to enable remote debugging
2. Took heap snapshots at different intervals using Chrome DevTools
3. Analyzed the heap snapshots to identify retained objects
4. Found that event listeners were not being properly cleaned up

**Solution:**
Implemented proper cleanup of event listeners:

```javascript
class DataProcessor {
  constructor() {
    this.handler = this.handleData.bind(this);
  }
  
  start() {
    eventEmitter.on('data', this.handler);
  }
  
  stop() {
    // This was missing - causing the leak!
    eventEmitter.removeListener('data', this.handler);
  }
}
```

**Lessons Learned:**
- Always remove event listeners when they're no longer needed
- Use heap snapshots for effective memory leak debugging
- Consider using weak references for cache implementations

---

## Template 2: Tutorial/How-To Format

```markdown
**Overview:**
Brief description of what you'll teach.

**Prerequisites:**
What the reader needs to know or have installed.

**Step-by-Step Guide:**
1. First step
2. Second step
3. etc.

**Common Pitfalls:**
What to watch out for.

**Conclusion:**
Summary and next steps.
```

### Example:

**Title:** "Setting Up GitHub Actions for Continuous Deployment"

**Overview:**
This guide walks through setting up a GitHub Actions workflow for automatic deployment to AWS when code is pushed to the main branch.

**Prerequisites:**
- GitHub repository with your application code
- AWS account with appropriate credentials
- Basic understanding of CI/CD concepts

**Step-by-Step Guide:**

1. Create `.github/workflows/deploy.yml` in your repository
2. Configure AWS credentials as GitHub secrets
3. Define workflow triggers (on push to main)
4. Add build and test steps
5. Add deployment step using AWS CLI

```yaml
name: Deploy to AWS
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        run: |
          aws s3 sync ./dist s3://my-bucket
```

**Common Pitfalls:**
- Forgetting to add AWS credentials to GitHub secrets
- Not setting correct IAM permissions
- Skipping the build step before deployment

**Conclusion:**
You now have automatic deployment on every push to main. Consider adding manual approval for production deployments.

---

## Template 3: Technology Comparison

```markdown
**Technologies Being Compared:**
List the options you're comparing.

**Use Case:**
What problem are you trying to solve?

**Comparison Criteria:**
What factors matter for your decision?

**Analysis:**
Compare each option across your criteria.

**Recommendation:**
Which one did you choose and why?
```

### Example:

**Title:** "REST vs GraphQL for Our Microservices API"

**Technologies Being Compared:**
- REST (RESTful APIs)
- GraphQL

**Use Case:**
Building an API gateway for our mobile and web applications to access 15+ microservices.

**Comparison Criteria:**
1. Ease of integration
2. Performance (network efficiency)
3. Developer experience
4. Learning curve
5. Tooling and ecosystem

**Analysis:**

*REST:*
- ✅ Well-understood, mature ecosystem
- ✅ Simple caching with HTTP
- ❌ Over-fetching data
- ❌ Multiple round trips needed

*GraphQL:*
- ✅ Single endpoint, precise data fetching
- ✅ Strong typing and introspection
- ❌ Caching is more complex
- ❌ Steeper learning curve

**Recommendation:**
We chose GraphQL because:
- Our mobile clients need to minimize data transfer
- The strong typing improves developer productivity
- Apollo Client/Server provide excellent tooling

---

## Template 4: Architecture/Design Pattern

```markdown
**Context:**
What system or feature were you building?

**Requirements:**
What did it need to do?

**Design Decisions:**
What patterns or architectures did you consider?

**Implementation:**
How did you build it?

**Trade-offs:**
What compromises did you make and why?

**Results:**
How did it turn out?
```

### Example:

**Title:** "Implementing Event Sourcing for Order Management"

**Context:**
Building an e-commerce order management system that needs to maintain complete audit history and support complex order state changes.

**Requirements:**
- Complete audit trail of all order changes
- Ability to rebuild order state at any point in time
- Support for concurrent order modifications
- Easy debugging of order issues

**Design Decisions:**
Chose Event Sourcing over traditional CRUD:
- Store events rather than current state
- Rebuild state by replaying events
- Use CQRS for read/write separation

**Implementation:**
```javascript
class OrderAggregate {
  constructor() {
    this.events = [];
    this.state = null;
  }
  
  apply(event) {
    this.events.push(event);
    this.state = this.reducer(this.state, event);
  }
  
  reducer(state, event) {
    switch(event.type) {
      case 'OrderCreated':
        return { id: event.orderId, status: 'created', items: event.items };
      case 'OrderShipped':
        return { ...state, status: 'shipped', shippedAt: event.timestamp };
      default:
        return state;
    }
  }
}
```

**Trade-offs:**
- ✅ Perfect audit trail
- ✅ Easy to replay and debug
- ❌ More complex than CRUD
- ❌ Need snapshot strategy for performance

**Results:**
- Reduced debugging time by 60%
- Easy compliance with audit requirements
- Team needed 2 weeks to adapt to new pattern

---

## Tips for Writing Great Issues

1. **Be Specific:** Include actual code, error messages, and commands
2. **Provide Context:** Explain why this problem or topic matters
3. **Include Details:** Version numbers, environment info, relevant logs
4. **Tell a Story:** Walk through your thought process
5. **Share Insights:** What did you learn? What surprised you?
6. **Add Visuals:** Screenshots, diagrams, or architecture drawings (if applicable)
7. **Use Code Blocks:** Format code properly with syntax highlighting

## What Makes a Good Technical Post?

The AI will generate better posts when your issue includes:
- Clear problem statement
- Step-by-step explanation
- Code examples
- Outcomes and results
- Lessons learned or insights
- Practical applicability

## Quick Start

1. Choose a template that fits your content
2. Fill in the sections with your technical content
3. Create a new issue in this repository
4. Wait for the daily workflow to generate and publish your post
5. Check your LinkedIn profile for the published post

## Need Help?

If you're unsure which template to use:
- Bug fixes/problems → Problem-Solution
- Teaching something → Tutorial/How-To
- Evaluating options → Technology Comparison
- System design → Architecture/Design Pattern

Happy writing! 🚀