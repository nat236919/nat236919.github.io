---
title: "GitHub Copilot"
date: 2021-08-02T17:12:07+08:00
draft: false
categories: ["blog"]
tags: ["blog", "github-copilot", "AI"]
---

{{< image src="https://i.ibb.co/hyVzCs3/1-I6-Ps-YZkl-U09-Hn-DLn-Yph-Oiw.jpg" alt="github-copilot-logo" position="center">}}

## What is GitHub Copilot?

**[GitHub-Copilot](https://copilot.github.com/)** is basically a smart code autocomplete powered by Codex, the new AI system created by OpenAI. GitHub Copilot understands significantly more context than most code assistants. So, whether it's in a docstring, comment, function name, or the code itself, GitHub Copilot uses the context you've provided and synthesizes code to match.

I received an invitation to test out GitHub Copilot during its technical preview a couple of weeks ago, and I have been using it on Visual Studio Code ever since. I would say that I am quite impressed with it. It is smart enough to predict what you are going to type next, and it is able to suggest the code completion for that with high accuracy.

Let me give you some concrete examples.

---

### Example 1: JavaScript

Let's say I want to write a function that return a factorial of a number. I just type **factorial** in order to create its function.

{{< image src="https://i.ibb.co/6ZNxTdp/Dingtalk-20210802173749.jpg" alt="factorial" position="center">}}

Suddenly, GitHub Copilot suggests me a whole function that can use to calculate the factorial of a number on a fly. All I need to do is to accept that sugeestion and move on. Yes, it just simply works.

```javascript
// Factorial
// Description:
// Calculates the factorial of a number

function factorial(n) {
  if (n == 0) {
    return 1;
  } else {
    return n * factorial(n - 1);
  }
}
```

---

### Example 2: Python

Let's write a function that sort a list by the first character of each element. I just write a comment explaining what I want this function to do and Voila! It is done.

{{< image src="https://i.ibb.co/9GS36Gs/Dingtalk-20210802180052.jpg" alt="python" position="center">}}

```python
# Sort a list based on the first character of each string in ascending order.
def sort_by_first_character(arr):
    return sorted(arr, key=lambda x: x[0])
```

---

#### Example 3: Markdown

Even with a Markdown file, GitHub Copilot can suggest me a whole bunch of things. As I am writing this post, it helps me write a post faster with predicted sentences that I intend to put on markdown.

{{< image src="https://i.ibb.co/YLvVYnJ/Dingtalk-20210802174517.jpg" alt="markdown_1" position="center">}}

You can even chat with us using only markdown.

{{< image src="https://i.ibb.co/8748Ykq/Dingtalk-20210802175112.jpg" alt="markdown_2" position="center">}}

---

## Final Thoughts

Even though GitHub Copilot is still in its technical preview, I am quite happy with it. It saves so much time for development. However, of course, it cannot predict everything perfectly, it still has problems to be resolved. I am looking forward to see how it will improve in the future. If youre are interested in testing this project, please go to [GitHub Copilot](https://copilot.github.com/) and sign up.
