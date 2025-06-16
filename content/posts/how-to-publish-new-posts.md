+++
title = "How to Publish New Posts to Your Zola Site"
date = 2025-06-16
description = "A step-by-step guide to creating and publishing new blog posts on your Zola static site"
[taxonomies]
tags = ["zola", "static site", "tutorial", "blogging"]
categories = ["technical"]
+++

# How to Publish New Posts to Your Zola Site

Creating and publishing new content on your Zola static site is straightforward. This guide will walk you through the entire process from creating a new post to seeing it live on your website.

## 1. Create a New Markdown File

Start by creating a new Markdown file in the `content` directory. The standard organization is:

```
content/
  └── posts/
      └── your-new-post.md
```

You can create this file using any text editor or IDE of your choice.

## 2. Add Front Matter Metadata

Every post needs front matter metadata at the beginning of the file. This is enclosed between `+++` markers:

```markdown
+++
title = "Your Post Title"
date = 2025-06-16
description = "A brief description of your post"
[taxonomies]
tags = ["tag1", "tag2"]
categories = ["category"]
+++
```

Essential front matter fields include:
- `title`: The title of your post
- `date`: Publication date (format: YYYY-MM-DD)
- `description`: A brief summary of your post (optional but recommended)
- `taxonomies`: Optional section for categories and tags

## 3. Write Your Content

After the front matter, write your post content using Markdown syntax. You can:
- Use `#` through `######` for different heading levels
- Create code blocks with triple backticks and specify the language
- Add images with `![alt text](image-path-or-url)`
- Create links with `[link text](url)`

## 4. Add Images and Other Resources

To include images or other resources in your post:

1. Create a resource folder in the `static` directory:
   ```
   static/
     └── images/
         └── your-post-name/
             └── image.jpg
   ```

2. Reference them in your post:
   ```markdown
   ![Image description](/images/your-post-name/image.jpg)
   ```

## 5. Preview Your Post

Preview your post locally before publishing:

```bash
zola serve
```

Then visit `http://127.0.0.1:1111` in your browser to see how your post will look.

## 6. Publish to GitHub Pages

When you're satisfied with your post, commit and push your changes to GitHub:

```bash
git add content/posts/your-new-post.md
git add static/images/your-post-name/  # If you added images
git commit -m "Add new post: Your Post Title"
git push origin main
```

GitHub Actions will automatically build your site and deploy it to GitHub Pages. Within a few minutes, your new post will be available at your site URL.

## Example Post

Here's a simple example post:

```markdown
+++
title = "Introduction to Zola Static Site Generator"
date = 2025-06-16
[taxonomies]
tags = ["zola", "static site", "tutorial"]
categories = ["technical"]
+++

# Introduction to Zola Static Site Generator

Zola is a fast, simple static site generator written in Rust. This post will introduce how to create and maintain a personal blog website using Zola.

## Why Choose Zola?

- **Speed**: Zola is written in Rust and builds sites incredibly fast
- **Single Executable**: No complex dependencies required
- **Powerful Templates**: Uses the Tera templating engine

## Example Code

```rust
fn main() {
    println!("Hello, Zola!");
}
```

## Summary

Zola is an excellent choice, especially for developers who prefer simple, efficient tools.
```
```

By following these steps, you'll be able to consistently create and publish new content to your Zola-powered website!
