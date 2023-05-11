
---
layout: post
title:  "Moving (again) to Jekyll platform"
date:   2023-05-11 20:30:00 +0700
categories: blogging
---
In my previous post, I shared my journey from WordPress to Ghost, navigating the ever-changing landscape of blogging platforms. WordPress seemed to have lost its way, like a compass that only pointed to confusion. Writing on it made me feel as comfortable as a penguin in the desert.

In search of simplicity, I turned to Ghost—a platform that promised a clean and straightforward experience. While it mostly delivered, it still had a few bells and whistles that felt a bit excessive for my needs.

What are my needs?

- Just simple writing
- Code highlighting
- A straightforward blogging layout
- Simple RSS integration

But Ghost is a bit too generous for me. It offers subscription features (which didn't work for me because I needed to set up an email server), themes (some of which are paid options), and a statistical dashboard.

Maybe I'll need those extra features someday, or maybe I'll be perfectly content living in the simplicity of the present. The point is, I had to put on my explorer's hat and venture into the depths of Ghost's settings to unlock and exorcise the annoying clutter.

*Déjà vu*. I'm afraid Ghost might be following WordPress's direction.

## Enter Jekyll on GitHub Pages

However, there is a shining alternative. Jekyll on GitHub Pages.

Jekyll is a static site generator that supports blogging, offering a straightforward and peaceful experience. Imagine yourself in a calm meadow, writing your content in Markdown, inspired by the gentle breeze. It's a serene and refreshing way to create your blog.

What's even better? Jekyll doesn't need an admin panel to hold your hand. It trusts you to unleash your writing prowess in a plain old text editor. No hand-holding necessary!

And guess what? The code highlighting is superb without needing any additional plugins.

### Ruby
```rb
(1..5).each do |counter|
  puts "iteration #{counter}"
end
```

### Typescript
```ts
class PointPerson implements Person {
    name: string
    move() {}
}
```

### Clojure
```clj
(defn hello3
  ([] "Hello World")
  ([name] (str "Hello " name)))
```

### Kotlin
```kt
fun not(f: (Int) -> Boolean): (Int) -> Boolean {
        return {n -> !f.invoke(n)}
    }
``
`
It doesn't stop there. Jekyll's static nature means you can bid farewell to backend maintenance headaches.

*Happy coding, happy blogging!*