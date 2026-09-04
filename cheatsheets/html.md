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
<header>     <!-- Site/section header -->
<nav>        <!-- Navigation links -->
<main>       <!-- Main content -->
<section>    <!-- Thematic grouping -->
<article>    <!-- Independent content -->
<aside>      <!-- Sidebar content -->
<footer>     <!-- Footer -->
<figure>     <!-- Image with caption -->
<figcaption> <!-- Caption for figure -->
```

## Text Elements
```html
<h1> to <h6>   <!-- Headings -->
<p>             <!-- Paragraph -->
<strong>        <!-- Bold (important) -->
<em>            <!-- Italic (emphasis) -->
<span>          <!-- Inline container -->
<br>            <!-- Line break -->
<hr>            <!-- Horizontal rule -->
<blockquote>    <!-- Quotation -->
<code>          <!-- Inline code -->
<pre>           <!-- Preformatted text -->
<mark>          <!-- Highlighted text -->
<small>         <!-- Smaller text -->
<del>           <!-- Deleted text -->
<ins>           <!-- Inserted text -->
<sub>           <!-- Subscript -->
<sup>           <!-- Superscript -->
```

## Links & Media
```html
<!-- Link -->
<a href="url" target="_blank" rel="noopener noreferrer">Link Text</a>

<!-- Image -->
<img src="image.jpg" alt="Description" width="300" height="200" loading="lazy">

<!-- Video -->
<video controls width="600">
  <source src="video.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

<!-- Audio -->
<audio controls>
  <source src="audio.mp3" type="audio/mpeg">
</audio>

<!-- iFrame -->
<iframe src="url" width="600" height="400" frameborder="0" allowfullscreen></iframe>
```

## Forms
```html
<form action="/submit" method="POST" novalidate>
  <!-- Text inputs -->
  <input type="text"     name="name"     placeholder="Name"     required>
  <input type="email"    name="email"    placeholder="Email"    required>
  <input type="password" name="pass"     placeholder="Password" required>
  <input type="number"   name="age"      min="1" max="100">
  <input type="date"     name="dob">
  <input type="url"      name="website"  placeholder="https://">
  <input type="tel"      name="phone"    placeholder="+92...">
  <input type="search"   name="q"        placeholder="Search...">
  <input type="file"     name="file"     accept=".pdf,.jpg,.png">
  <input type="hidden"   name="token"    value="abc123">

  <!-- Choices -->
  <input type="checkbox" name="agree"  value="yes"> I agree
  <input type="radio"    name="gender" value="male">   Male
  <input type="radio"    name="gender" value="female"> Female

  <!-- Range & Color -->
  <input type="range"  name="volume" min="0" max="100" step="5">
  <input type="color"  name="theme"  value="#007bff">

  <!-- Multiline & Select -->
  <textarea name="message" rows="5" placeholder="Your message"></textarea>

  <select name="country">
    <optgroup label="South Asia">
      <option value="pk" selected>Pakistan</option>
      <option value="in">India</option>
    </optgroup>
    <option value="us">USA</option>
  </select>

  <button type="submit">Submit</button>
  <button type="reset">Reset</button>
</form>
```

## Tables
```html
<table>
  <caption>Student Results</caption>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Grade</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ahmad</td>
      <td>A+</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td colspan="2">End of results</td>
    </tr>
  </tfoot>
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
<ol type="1" start="1">  <!-- type: 1 A a I i -->
  <li>Step 1</li>
  <li>Step 2</li>
</ol>

<!-- Description -->
<dl>
  <dt>HTML</dt>
  <dd>HyperText Markup Language</dd>
</dl>
```

## SEO & Meta Tags
```html
<!-- Basic -->
<meta name="description" content="Under 160 chars">
<meta name="keywords"    content="html, css, js">
<meta name="author"      content="Rana Ahmad">
<meta name="robots"      content="index, follow">

<!-- Open Graph (social sharing) -->
<meta property="og:type"        content="website">
<meta property="og:title"       content="Page Title">
<meta property="og:description" content="Description">
<meta property="og:image"       content="https://site.com/img.jpg">
<meta property="og:url"         content="https://site.com">

<!-- Twitter Card -->
<meta name="twitter:card"        content="summary_large_image">
<meta name="twitter:title"       content="Page Title">
<meta name="twitter:description" content="Description">
<meta name="twitter:image"       content="https://site.com/img.jpg">

<!-- Canonical -->
<link rel="canonical" href="https://site.com/page">

<!-- Favicon -->
<link rel="icon" href="/favicon.ico" type="image/x-icon">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">
```

## Accessibility
```html
<!-- ARIA roles -->
<div role="alert">Error message</div>
<nav role="navigation" aria-label="Main navigation">
<button aria-expanded="false" aria-controls="menu">Menu</button>

<!-- Alt text -->
<img src="logo.png" alt="Company Logo">         <!-- descriptive -->
<img src="decorative.png" alt="">               <!-- decorative: empty alt -->

<!-- Labels -->
<label for="email">Email address</label>
<input id="email" type="email">

<!-- Skip link -->
<a href="#main-content" class="skip-link">Skip to content</a>
```
