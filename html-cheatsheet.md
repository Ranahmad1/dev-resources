# HTML5 Cheatsheet

## Document Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Page description">
  <title>Page Title</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <!-- Content here -->
  <script src="script.js"></script>
</body>
</html>
```

## Semantic Elements
```html
<header>   <!-- Site/section header -->
<nav>      <!-- Navigation links -->
<main>     <!-- Main content -->
<section>  <!-- Thematic grouping -->
<article>  <!-- Independent content -->
<aside>    <!-- Sidebar content -->
<footer>   <!-- Footer -->
<figure>   <!-- Image with caption -->
<figcaption> <!-- Caption for figure -->
```

## Text Elements
```html
<h1> to <h6>  <!-- Headings -->
<p>            <!-- Paragraph -->
<strong>       <!-- Bold (important) -->
<em>           <!-- Italic (emphasis) -->
<span>         <!-- Inline container -->
<br>           <!-- Line break -->
<hr>           <!-- Horizontal rule -->
<blockquote>   <!-- Quotation -->
<code>         <!-- Inline code -->
<pre>          <!-- Preformatted text -->
<mark>         <!-- Highlighted text -->
<small>        <!-- Smaller text -->
<del>          <!-- Deleted text -->
<ins>          <!-- Inserted text -->
<sub>          <!-- Subscript -->
<sup>          <!-- Superscript -->
```

## Links & Media
```html
<!-- Link -->
<a href="url" target="_blank" rel="noopener">Link Text</a>

<!-- Image -->
<img src="image.jpg" alt="Description" width="300" height="200">

<!-- Video -->
<video controls width="600">
  <source src="video.mp4" type="video/mp4">
</video>

<!-- Audio -->
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
</audio>

<!-- iFrame -->
<iframe src="url" width="600" height="400" frameborder="0"></iframe>
```

## Forms
```html
<form action="/submit" method="POST">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" placeholder="Enter name" required>

  <input type="email" name="email" placeholder="Email">
  <input type="password" name="pass" placeholder="Password">
  <input type="number" name="age" min="1" max="100">
  <input type="date" name="dob">
  <input type="file" name="file" accept=".pdf,.jpg">
  <input type="checkbox" name="agree" value="yes"> I agree
  <input type="radio" name="gender" value="male"> Male

  <textarea name="message" rows="5" cols="40"></textarea>

  <select name="country">
    <option value="pk">Pakistan</option>
    <option value="us">USA</option>
  </select>

  <button type="submit">Submit</button>
</form>
```

## Tables
```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ahmad</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
```

## Lists
```html
<!-- Unordered -->
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
</ul>

<!-- Ordered -->
<ol>
  <li>Step 1</li>
  <li>Step 2</li>
</ol>

<!-- Description -->
<dl>
  <dt>Term</dt>
  <dd>Definition</dd>
</dl>
```

## Meta Tags (SEO)
```html
<meta name="description" content="Page description under 160 chars">
<meta name="keywords" content="html, css, javascript">
<meta name="author" content="Rana Ahmad">
<meta property="og:title" content="Page Title">
<meta property="og:description" content="Description">
<meta property="og:image" content="image.jpg">
```
