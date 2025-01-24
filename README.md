# [Rails 8 Import Maps Tutorial](https://medium.com/jungletronics/mastering-import-maps-in-rails-8-5f6c32991aa8)

This repository demonstrates how to set up and use Import Maps in a Rails 8 application. Import Maps allow you to manage JavaScript dependencies natively in the browser without relying on package managers like Webpack or npm. This tutorial is based on the [How To Use Import Maps with Rails](https://gorails.com) guide.

---

## Introduction

Rails 8 introduces Import Maps as a modern way to manage JavaScript dependencies. This feature eliminates the need for complex bundlers, offering a simpler way to serve and manage JavaScript files directly from your Rails app.

This tutorial walks you through:

        1. Setting up a Rails app with Import Maps.
        2. Integrating Import Maps with the Rails Asset Pipeline.
        3. Adding and managing JavaScript files using Import Maps.

---

## System Requirements

- **Operating System**: Ubuntu 24.04 LTS
- **Processor**: Intel® Core™ i7–9750H × 12
- **RAM**: 8.0 GiB
- **Rails Version**: 8.0.1

---

## Steps to Follow

### 1. Create a New Rails Application

Run the following commands to set up your app:

```bash
  rails new importmap

  cd importmap
```
    rails g controller pages index

Set the root route in config/routes.rb:

    root "pages#index"

Add a placeholder to app/views/pages/index.html.erb:

    <h1>Import Map</h1>

Modify your layout in app/views/layouts/application.html.erb:

Comment out the default javascript_importmap_tags:

    <%#= javascript_importmap_tags %>

2. Understand Import Maps

An Import Map is a browser-native feature that maps JavaScript module paths directly in the browser. It eliminates the need for bundlers by serving JavaScript directly from the app.

Example:
```HTML
<script type="importmap">
{
  "imports": {
    "example": "/example.js",
    "hello": "/hello.js"
  }
}
</script>
<script type="module">
  import "example";
  console.log("Import Map Example Loaded");
</script>
```
3. Add JavaScript Files

Create JavaScript files under the public directory:

    touch public/example.js public/hello.js

Add the following content:

example.js:

    import "hello";
    console.log("Hello from example.js");

hello.js:

    console.log("Hello from hello.js");

4. Enable Import Maps in Layout

Uncomment the javascript_importmap_tags line in 

app/views/layouts/application.html.erb:

    <%= javascript_importmap_tags %>

5. Move JavaScript Files to app/javascript

Transfer your files to the app/javascript directory:

    mv public/example.js app/javascript/example.js
    mv public/hello.js app/javascript/hello.js

6. Pin JavaScript Files

Update config/importmap.rb:

    pin "example", to: "example.js"
    pin "hello", to: "hello.js"

7. Import Modules in application.js

Update app/javascript/application.js:

    import "example";
    import "hello";

8. Update index.html.erb for Production

Replace the placeholder in app/views/pages/index.html.erb:
```HTML
<h1>Import Map</h1>
<script type="importmap">
{
  "imports": {
    "example": "<%= path_to_asset 'example.js' %>",
    "hello": "<%= path_to_asset 'hello.js' %>"
  }
}
</script>
<script type="module">
  import "example";
  console.log("Import Map Example Loaded");
</script>
```
9. Run the Application

Start the Rails server:

    rails s

Open the app in your browser, and inspect the scripts in the <head> section using the developer tools (Ctrl+Alt+I).

How It Works

    Import Maps: Map JavaScript modules to their respective URLs.
    Preloading: Optimize performance by preloading JavaScript modules.
    Rails Asset Pipeline: Use asset fingerprinting for cache invalidation.

Key Features

    Lightweight Dependency Management: Manage JavaScript files natively in the browser.
    Optimized Performance: Preload assets for faster loading.
    Integrated Workflow: Simplify frontend management with Rails 8.

Additional Notes

In production, Rails uses the Asset Pipeline to fingerprint JavaScript files. This ensures updated assets are loaded when the content changes.

Example:
```HTML
<script type="importmap">
{
  "imports": {
    "example": "/assets/example-1293c9f1.js",
    "hello": "/assets/hello-af42800b.js"
  }
}
</script>
<link rel="modulepreload" href="/assets/example-1293c9f1.js">
<link rel="modulepreload" href="/assets/hello-af42800b.js">
<script type="module">
  import "application";
</script>
```
### References
[How To Use Import Maps with Rails](https://gorails.com)

### License

[MIT](https://choosealicense.com/licenses/mit/)

Enjoy building your Rails app with Import Maps! 😍️
