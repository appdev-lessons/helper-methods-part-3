# Helper Methods Part 3: Bootstrap, partials, and devise

## Getting Started

There is no target for this app, but the project does include a few basic automated tests, so click on this button to get started:

LTI{Load Helper Methods Part 3 assignment}(https://grades.firstdraft.com/launch)[S9ymPy6WCsn18gLbByVbZQ7k]{vfdtzJb5bLYqYwuqgeRKpc5d}(10)[Helper Methods Part 3 Project]

The staring point code for `helper-methods-part-3` is where you should have arrived by the end of the previous `helper-methods-part-1-and-2` project. The current project `helper-methods-part-3` covers everything in this lesson.

Let's keep going with our helper methods and add some bootstrap to make our app _look_ professional, learn about rendering partials, and then add user accounts with Devise.

## Walkthrough video

For the rest of this lesson, there is a walkthrough video available.

<div class="bg-red-100 py-1 px-5" markdown="1">
**Please note**, the video is from a previous iteration of the project, so there are some differences:

- I am using Gitpod as my cloud editor, so the interface looks a bit different.
- Anything contained in the project "README" or in the "chapters" is now contained in this Lesson
- I use `bin/server` to start my live app preview, _you_ should use `bin/dev`
</div>

Did you read the differences above? Good! Then [here is a walkthrough video for this project.](https://share.descript.com/view/mQOqBmvJmSo)

**As you watch the video, pause it frequently, read the associated text, and type out the code.**

## Bootstrap CSS navbar

I want to start getting in the habit of making our apps look better. That means pulling in Bootstrap CSS and Font Awesome, using the [quick link to them here](https://learn.firstdraft.com/lessons/143-design-resources#quick-links-to-assets):

```erb
<!-- Expand the number of characters we can use in the document beyond basic ASCII 🎉 -->
<meta charset="utf-8">

<!-- Make it responsive -->
<meta name="viewport" content="width=device-width, initial-scale=1">

<!-- Connect Bootstrap CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/css/bootstrap.min.css">

<!-- Connect Bootstrap JavaScript and its dependencies -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.1/dist/js/bootstrap.min.js"></script>

<!-- Connect Font Awesome -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.1.1/js/all.min.js"></script>
```

Take those HTML elements and copy-paste into the `<head>` of your `app/views/layouts/application.html.erb`.

<aside markdown="1">

When copy-pasting code into HTML documents, you can auto-format the indentation so that the elements are properly nested using <kbd>Cmd</kbd> + <kbd>shift</kbd> + <kbd>p</kbd> (Mac) or <kbd>Cntrl</kbd> + <kbd>shift</kbd> + <kbd>p</kbd> (Windows) to open the **command pallet** in VSCode. 

With the `>` command pallet prompt open, you can search "Format" and find the command. (You can search for any command in this prompt.) You will also see a direct keyboard shortcut to that formatting command that might be useful to you. Become a keyboard ninja!
</aside> 

Now refresh a page in your app (`/`, the root should be set to the movies `index` page) and see the change. If the fonts look a little better, you know you you connected the assets correctly.

Let's start putting it to use. Visit the [Bootstrap docs](https://getbootstrap.com/docs/5.3/getting-started/introduction/), specifically the docs for implementing a [navbar](https://getbootstrap.com/docs/5.3/components/navbar/).

<div class="bg-red-100 py-1 px-5" markdown="1">

In the video, I am using the Bootstrap documentation for version 5.1, but the links contained in this lesson are for version 5.3. Use the newer version.
</div>

Near the top of the page you can find the example with everything in it, and use the top right "Copy" button to grab it all. There's a lot here, but we just want to take it all and then remove what we don't need:

```erb
<!-- app/views/layouts/application.html.erb -->

<!-- ... -->
  <body>

    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">Navbar</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        
        <!-- ... etc., etc. -->

    </nav>
<!-- ... -->
```

This is a good example of something that would go in the application layout file in the `<body>` above our `<%= yield %>` statement, because we want the navbar to appear on every page.

After this copy-paste, check on your live app that the navbar showed up.

Let's start modifying the navbar to suit our purposes, starting with the homepage link:

```erb{4,6}
<!-- ... -->
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <!-- <a class="navbar-brand" href="#">Navbar</a> -->

        <a class="navbar-brand" href="/">Helper Methods 3</a>

       <!-- ... -->
```

Okay, we have a link to our `/` root page now. But actually, we should use our new helper methods! Remember, we have `link_to` now, which will draw `<a>` elements for us:

```erb{6,8}
<!-- ... -->
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <!-- <a class="navbar-brand" href="#">Navbar</a> -->

        <!-- <a class="navbar-brand" href="/">Helper Methods 3</a> -->

        <%= link_to "Helper Methods 3", root_path, class: "navbar-brand" %>

       <!-- ... -->
```

We added another argument to `link_to` besides the copy (first position), and the path (second position). We can also pass additional arguments as a hash on the end of the method, noting that we dropped the curly braces since this hash is the last argument to the method. And in that hash, we are just stating that the `class` of the link should be `"navbar-brand"`, as it was in the original `<a>` element.

Confirm that the new `link_to` method works exactly as expected (and looks good with bootstrap). Then feel free to delete the old HTML element.

Just below the `"navbar-brand"` root link, there is code for the "hamburger" menu for smaller screens (`<button class="navbar-toggler"...`). We don't want to modify that at all. But, a bit farther down are the actual nav links:

```erb{5,8}
<!-- ... -->
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">Home</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Link</a>
          </li>
<!-- ... -->
```

We have a list beginning with `<ul>`, then each `<li>` with a `class="nav-item"` followed by an `<a>` link of `class="nav-link"`. We can replace those with `link_to`s and put in the paths we want links to, e.g. from this:

```erb
<a class="nav-link active" aria-current="page" href="#">Home</a>
```

to this:

```erb
<%= link_to "Movies", movies_path, class: "nav-link active" %>
```

We could add more links to things like `/movies/new`, but we'll just keep it simple for now. 

Lastly, we can delete the entire `<li class="nav-item dropdown">` nested element that defines the dropdown menu, since we only have the single resource in this app.

## Git command-line interface

Now is a good time for another git commit. However, rather than your familiar VSCode built in workflow, it's time to see how most developers interact with git in the real world: via the command line.

Pause for a moment and spend just 5 to 10 minutes [glancing over the next lesson on the Git Command-Line Interface (CLI)](https://learn.firstdraft.com/lessons/196-git-cli). **Do not spend time worrying about everything, most importantly, just get a sense of how you can commit and push from the command line.** You will get much more practice with git at the command line soon; including branching and merging.

Have you looked over the Git CLI reading? If you feel comfortable with it, follow along with how I made my commit in the video by opening a bash prompt and running the commands:

```
% git add -A
% git commit -m "added bootstrap navbar"
% git push
```

(Or, as I do with shortcuts in the video:

`% g acm "added bootstrap navbar"` 

and then: 

`% g p`.)

## Bootstrap alerts

Let's add some more bootstrap. Right now, when we add a movie at `/movies/new` there is a green sentence that comes up, which is the `:notice` message we set in our `create` action in the controller being rendered in the application layout.

We can improve that simple green text with [Bootstrap alerts](https://getbootstrap.com/docs/5.3/components/alerts/). Again, we can just copy and paste the examples we want (perhaps the green "success" and red "danger" boxes):

```erb{10-12,14-16}
<!-- app/views/layouts/application.html.erb -->

<!-- ... -->
  <body>

    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <!-- ... -->
    </nav>
    
    <div class="alert alert-success" role="alert">
      <%= notice %>
    </div>

    <div class="alert alert-danger" role="alert">
      <%= alert %>
    </div>
    
    <%= yield %>
  </body>
</html>
```

And now try to refresh the `/movies` page. 

We have an issue. If there is no notice or alert, a red and a green box still appear. So what should we do? How about some `if` control flow? There's a nice method for this, which will check if the message is undefined or `nil` and return true or false: `.present?`

```erb
<!-- ... -->
    <% if notice.present? %>
      <div class="alert alert-success" role="alert">
        <%= notice %>
      </div>
    <% end %>

    <% if alert.present? %>
      <div class="alert alert-danger" role="alert">
        <%= alert %>
      </div>
    <% end %>
<!-- ... -->
```

Do the messages appear and disappear as expected? Good! Time for another commit.

## Bootstrap containers for padding

Wouldn't it be nice if everything in the app wasn't pushed all the way to the edges? We would like some padding around the content. This is the perfect use case for the [Bootstrap `container` class](https://getbootstrap.com/docs/5.3/layout/containers/). 

Since we want padding on every page, why not put it in the application layout again!

```erb{10-24}
<!-- app/views/layouts/application.html.erb -->

<!-- ... -->
  <body>

    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <!-- ... -->
    </nav>
    
    <div class="container">
      <% if notice.present? %>
        <div class="alert alert-success" role="alert">
          <%= notice %>
        </div>
      <% end %>

      <% if alert.present? %>
        <div class="alert alert-danger" role="alert">
          <%= alert %>
        </div>
      <% end %>

      <%= yield %>
    </div>
  </body>
</html>
```

<aside markdown="1">

When I'm actually writing code, to speed things up, I use the "Emmet" abbreviation to generate tags for a given class in HTML. For instance, you can just type `div.container` in the code editor, and you should see an option come up to press <kbd>tab</kbd>, which will generate `<div class="container"></div>`.
</aside>

We put the container around our alerts and our `yield` (where all the rest of each page goes), but under the navbar, so the navbar will still stretch across the top of the screen.

If we test this, it looks pretty good. But there's not a lot of space between flash messages and the navbar. We can shift everything down a little by adding to the `class` attribute in the container and making it: `class="container mt-4"`. MT for margin top and "4" for the spacing. 

Looks good? Great! Commit again.

I want to note: there's an open [pull request for this project](https://github.com/appdev-projects/helper-methods-part-3/pull/2/files) on Github, that contains all of the changes we will be incrementally making in this lesson. That's a nice resource to occasionally glance at as we go through the project to see a summary of changes on each file.

## Reviewing form builder

Sometimes we have code that we want to reuse. Not in every single template, but in _some_ templates. And it would be nice to avoid duplication. Also, look how long our application layout has become with all of the Bootstrap additions we made. Wouldn't it be great if we could put some of that code in separate files to make it all more readable?

We're headed towards partial view templates. Let's get our first feel for things with our `new.html.erb` and `edit.html.erb` forms. Open those files. What to do you notice? Where are the differences?

It turns out, because we switched to `form_with` helpers, the two forms are identical besides the copy in the `<h1>` tag at the top of each page! ("New" vs. "Edit".)

`form_with` does a lot for us. It knows whether the object is persisted in the database or not, and how to fill in placeholder values for partially filled out forms or objects that already have column values. So everything is basically the same! 

Now, what if we add a new column? Let's see how we would change our code to allow that. How about an image URL for the movie?

Start by running the bash prompt command:

```
rails g migration AddImageUrlToMovies image_url:string
```

Then migrate the change to the database:

```
rake db:migrate
```

And you can check the `db/schema.rb` file to see that the table `movies` now has this additional column.

What if I want to add it to my form so people can actually use that column? 

We can do that with the `form_with` construction easily:

```erb{20-23}
<!-- app/views/movies/new.html.erb -->

<h1>New movie</h1>

<% @movie.errors.full_messages.each do |message| %>
  <p style="color: red;"><%= message %></p>
<% end %>

<%= form_with(model: @movie, data: { turbo: :false }) do |form| %>
  <div>
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :description %>
    <%= form.text_area :description %>
  </div>

  <div>
    <%= form.label :image_url %>
    <%= form.text_field :image_url %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```

But what if I try that out in the `/movies/new` page? It won't actually work yet. Why?

Because we also need to remember to whitelist this new column to allow for mass assignment to take place!

```ruby{5:(70-81),12:(70-81)}
# app/controllers/movies_controller.rb

  # ...
  def create
    movie_params = params.require(:movie).permit(:title, :description, :image_url)
  
  # ...
  
  def update
    @movie = Movie.find(params.fetch(:id))

    movie_params = params.require(:movie).permit(:title, :description, :image_url)
  
  # ...
```

Back to the point. We have these two forms `edit` and `new` and we would like to reuse the code in them and not need to make changes in two places ever.

First of all, we can DRY up where we are creating that variable `movie_params` twice in our controller. We can do that by defining a new method in the bottom of the controller:

<aside markdown="1">
DRY: Don't Repeat Yourself.
</aside>

```ruby{5,7-9}
# app/controllers/movies_controller.rb

# ...

  private

  def movie_params
    params.require(:movie).permit(:title, :description, :image_url)
  end
end
```

We put a new keyword, `private`, and below this we defined our helper method. We put any methods that are just helpers for the actions below `private` in the controller.

Then passing that _method_ `movie_params` to the `create` and `update` actions allows us to simplify them:

```ruby{5:(24-35),11:(22-33)}
# app/controllers/movies_controller.rb

  # ...
  def create
    @movie = Movie.new(movie_params)
  # ...
  
  def update
    @movie = Movie.find(params.fetch(:id))

    if @movie.update(movie_params)
    # ...
```

When these actions are triggered, Rails will reach that `movie_params` argument, which we didn't define in the action, and instead of throwing an error, it will search the controller for a method that matches that argument and call that method. We could write `self.movie_params`, as the longhand for what this step is doing!

So now all of our whitelisted columns are in one place and we don't need to repeat our code when we make changes to the database.

## Partial view templates

Now that we can add to the forms, how can we get the forms in one place so we can reuse them for different purposes (`create` and `update`) on different pages? The answer is **partial view templates**.

Partial view templates (or just "partials", for short) are an extremely powerful tool to help us modularize and organize our view templates. Especially once we start adding in styling with Bootstrap, our view files will grow to be hundreds or thousands of lines long, so it becomes increasingly helpful to break them up into partials.

Here is the [official article in the Rails API reference](https://edgeapi.rubyonrails.org/classes/ActionView/PartialRenderer.html) describing all the ways you can use partials. There are lots of powerful options available, but for now we're going to focus on the most frequently used ones.

Let's get our first feel for partials, then see about our `new.html.erb` and `edit.html.erb` forms. 

### Static HTML partials

We create a partial view template in the same way that you create a regular view template, except that the first letter in the file name must be an underscore. This is how we (and Rails) distinguish partial view templates from full view templates.

For example, create a folder (`app/views/zebra/`) and file within it called `_giraffe.html.erb`. Within the file, write the following:

```erb
<!-- app/views/zebra/_giraffe.html.erb -->

<h1>Hello from the giraffe partial!</h1>
```

Then, in any of your other view templates, e.g. `movies/index.html.erb`, add:

```erb{7}
<!-- app/views/movies/index.html.erb -->

<h1>
  List of all movies
</h1>

<%= render partial: "zebra/giraffe" %>
<!-- ... -->
```

Notice that we don't include the underscore when referencing the `partial:` in the `render` method, even though the underscore _must_ be present in the actual filename.

You can render the partial as many times as you want:

```erb{9-11}
<!-- app/views/movies/index.html.erb -->

<h1>
  List of all movies
</h1>

<%= render partial: "zebra/giraffe" %>

<hr>

<%= render partial: "zebra/giraffe" %>
<!-- ... -->
```

A more realistic example of putting some static HTML into a partial is extracting the long Bootstrap navbar into `app/views/shared/_navbar.html.erb` and then `render`ing it from within the application layout. This will make our very long view templates much easier to navigate and make sense of. Try doing that now!

First create the folder `shared` in `app/views/`, then create the `_navbar.html.erb` file within that. Now, cut and paste the entire `<nav></nav>` element from the `app/views/layouts/application.html.erb` file to the new `_navbar.html.erb` file. And back in the layout:

```erb{6}
<!-- app/views/layouts/application.html.erb -->

<!-- ... -->
  <body>

    <%= render partial: "shared/navbar" %>
    
    <div class="container">
      <% if notice.present? %>
        <!-- ... -->
  </body>
</html>
```

Much more organized! And why not do the same with the alert messages? Put the whole content of the `if ...present?` statements into a file `app/views/shared/_flash_messages.html.erb` and render that in the layout file. And while we're at it: the Bootstrap and Font Awesome stuff can also go in another static view template called `app/views/shared/_cdn_assets.html.erb`!

Now our application layout file should look much, much cleaner:

```erb{12,18,21}
<!-- app/views/layouts/application.html.erb -->

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Helper Methods Part 3</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= render partial: "shared/cdn_assets" %>

    <%= stylesheet_link_tag "application", "data-turbo-track": "reload" %>
    <%= javascript_importmap_tags %>
  <body>

    <%= render partial: "shared/navbar" %>

    <div class="container mt-4">
      <%= render partial: "shared/flash_messages" %>
      <%= yield %>
    </div>
  </body>
</html>
```

Partials make our code more modular and easier to read.

### Partials with Inputs 01:02:30 to

Partials get even more powerful when you can send data into them to be automatically used in the output of the partial. 

Create another file called `app/views/zebra/_elephant.html.erb`, and add the following code there:

```erb
<h1>Hello, <%= person %>!</h1>
```

Then, in `movies/index`, try:

```erb
<%= render partial: "zebra/elephant" %>
```

When you test it, it will break and complain about an undefined local variable person. To fix it, try:

```erb
<%= render partial: "zebra/elephant", locals: { person: "Alice" } %>
```

Now it becomes more clear why it can be useful to render the same partial multiple times:

```erb
<%= render partial: "zebra/elephant", locals: { person: "Alice" } %>

<hr>

<%= render partial: "zebra/elephant", locals: { person: "Bob" } %>
```

If we think of rendering partials as _calling methods that return HTML_, then the `:locals` option is how we pass in arguments to those methods. This allows us to create powerful, reusable HTML components.

The arguments that we pass with the option `:locals` take the form of a hash with key/value pairs. The keys correspond to the name of the variables in the the partial, and the value is whatever we want the variable to be.

We're finally at the point where we can move our form into a partial to reuse on the `new` and `edit` pages!

Make a new file in `app/views/movies/` called `_form.html.erb`, and cut-paste the content of our form there (changing all of the `@movie` instance variables to just local variables called `movie`):

```erb
<!-- app/views/movies/_form.html.erb -->

<% movie.errors.full_messages.each do |message| %>
  <p style="color: red;"><%= message %></p>
<% end %>

<%= form_with(model: movie, data: { turbo: :false }) do |form| %>
  <div>
    <%= form.label :title %>
    <%= form.text_field :title %>
  </div>

  <div>
    <%= form.label :description %>
    <%= form.text_area :description %>
  </div>

  <div>
    <%= form.label :image_url %>
    <%= form.text_field :image_url %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```
{: mark_lines="3 7"}

<aside markdown="1">
Note that we could have still used `@movie` in this partial, because instance variables that are defined in the actions will be available in the partial, and the instance variable is the same in both actions using this form. But if the name was different in the two actions (e.g., `@the_movie` vs. `@new_movie`), then we would want to use `:locals` as we do in the example.
</aside>

And now, we can just render this partial on both of our view templates, passing the `@movie` instance variable from the `create` and `update` actions to the partial as the local variable `movie`:

```erb
<!-- app/views/movies/new.html.erb -->

<h1>New movie</h1>

<%= render partial: "movies/form", locals: { movie: @movie } %>
```

```erb
<!-- app/views/movies/edit.html.erb -->

<h1>Edit movie</h1>

<%= render partial: "movies/form", locals: { movie: @movie } %>
```

And in our partial, everywhere that we put `movie` will be replaced with the instance variable and everything will work like before.

Actually, we can shorten things a bit. If you are rendering a partial with some local variables, we could just drop the `partial:` and `locals:` keyword, and Rails will figure it all out:

```erb
<%= render "movies/form", movie: @movie %>
```

That's optional though, and I prefer typing out the long way with `partials:` and `locals:` so you can tell exactly what's going on.

### Adding Another Column 01:17:00 to 01:19:18

Let's see the power of our new form partials by adding another column to the database and then to our forms.

The first step is generating the migration and running it:

```bash
rails g migration AddReleasedOnToMovies released_on:date
```

```bash
rails db:migrate
```

Then we need to whitelist the new column in our strong parameters to allow mass assignment:

```ruby
# app/controllers/movies_controller.rb

...
  private

  def movie_params
    params.require(:movie).permit(:title, :description, :image_url, :released_on)
  end
end
```
{: mark_lines="7"}

And finally, we can just pop that into the `movies/_form.html.erb` file (using the `.date_select` method to create a nice selection menu on the form):

```erb
...
  <div>
    <%= form.label :image_url %>
    <%= form.text_field :image_url %>
  </div>

  <div>
    <%= form.label :released_on %>
    <%= form.date_select :released_on %>
  </div>

  <div>
    <%= form.submit %>
  </div>
<% end %>
```
{: mark_lines="7-10"}

And it will appear (and work) in both of our new and edit forms!

## Bootstrap Cards for Show Page 01:25:00 to 01:32:00

To explore the next topic, let's try and make the `movies#show` page look better. We don't want a list of information with the plain `<dl>` "description list" construction, we want [Bootstrap cards](https://getbootstrap.com/docs/5.3/components/card/)! (And maybe even some [free Font Awesome icons](https://fontawesome.com/search?o=r&m=free).)

Let's cut to the chase with the code we want on the `app/view/movies/show.html.erb` page and see how it looks. Open that file and replace the entire contents with this code:

```erb
<!-- app/view/movies/show.html.erb -->

<div class="card">
  <div class="card-header">
    <%= link_to "Movie ##{@movie.id}", @movie %>
  </div>

  <div class="card-body">
    <dl>
      <dt>
        Title
      </dt>
      <dd>
        <%= @movie.title %>
      </dd>

      <dt>
        Description
      </dt>
      <dd>
        <%= @movie.description %>
      </dd>
    </dl>

    <div class="row">
      <div class="col">
        <div class="d-grid">
          <%= link_to edit_movie_path(@movie), class: "btn btn-outline-secondary" do %>
            <i class="fa-regular fa-pen-to-square"></i>
          <% end %>
        </div>
      </div>
      <div class="col">
        <div class="d-grid">
          <%= link_to @movie, data: { turbo_method: :delete }, class: "btn btn-outline-secondary" do %>
            <i class="fa-regular fa-trash-can"></i>
          <% end %>
        </div>
      </div>
    </div>
  </div>

  <div class="card-footer">
    Last updated <%= time_ago_in_words(@movie.updated_at) %> ago
  </div>
</div>
```

Refresh the `/movies/1` (or whatever movie ID you want, just click a "Show details" link from the index). 

You should see a bootstrap card with a card header, card footer, and a card body. In the card body, we have our description list with the attributes, and then I have buttons with Font Awesome icons for editing and deleting.

Read the code we copy-pasted and make some sense of things.

We begin with an outer container `<div class="card">` that everything goes inside of. Then you have three direct `<div>` children of this element, for the `class="card-header"`, `class="card-body"`, `class="card-footer"`.

The `"card-header"` contains a `link_to` element for the current movie we are viewing, the `"card-body"` has our `<dl>` list (and some additional `<div>` children we'll discuss in a moment), and the `"card-footer"` just has some additional copy with the `updated_at` information for the movie.

Now, in the `"card-body"` class, there are three `<div>`s:

```erb
    <div class="row">
      <div class="col">
        <div class="d-grid">
```

That wrap the Font Awesome icons, which are being drawn with:

```erb
          <%= link_to edit_movie_path(@movie), class: "btn btn-outline-secondary" do %>
            <i class="fa-regular fa-pen-to-square"></i>
          <% end %>
```

and:

```erb
          <%= link_to @movie, data: { turbo_method: :delete }, class: "btn btn-outline-secondary" do %>
            <i class="fa-regular fa-trash-can"></i>
          <% end %>
```

(That's a `link_to` being used with the edit and delete methods in a block that draws an icon `fa-pen-to-square` (edit) and `fa-trash-can` (delete). Also note the `data: { turbo_method: :delete }` argument for the delete link.)

What are the three `"row"`, `"col"`, and `"d-grid"` classes doing? It turns out, these are what we need to achieve two side-by-side buttons that take up the full width of their Bootstrap row! The syntax might look familiar to creating an HTML table with rows and columns.

Let's edit the code to center the card in the middle of the screen, rather than stretching across the full width. (Minus the padding that we provided in the application layout with `<div class="container mt-4">`.)

To do this, we can wrap our entire card in a Bootstrap row and column, right? Because we already have the `"container"` from the application layout that is wrapping every page in our app, and now we can put rows and columns inside of _that_. 

```erb
<!-- app/view/movies/show.html.erb -->

<div class="row">
  <div class="col-md-4">
      <div class="card">
        ...
      </div>
  </div>
</div>
```
{: mark_lines="3-4 8-9"}

The `md-4` refers to "medium" and the "4" means that we are taking one third of the page (with the page divided width-wise into 12 equal breakpoints). You can find information on this grid layout that we are creating in the [Bootstrap grid system documents](https://getbootstrap.com/docs/5.3/layout/grid/).

If you check the movie details page, you will see the card has shrunk to a third of the page width, but it's all the way on the left side.

We need to center the card by adding another option to the `"col"` class:

```erb
<!-- app/view/movies/show.html.erb -->

<div class="row">
  <div class="col-md-4 offset-md-4">
      <div class="card">
        ...
      </div>
  </div>
</div>
```
{: mark_lines="4"}

This `offset-md-4` will add another 4-width "invisible" column on the left side, shifting the card to the center. Have a look at the show details page. Much better!

We could even make the card a bit wider by changing the width of the columns:

```erb
<!-- app/view/movies/show.html.erb -->

<div class="row">
  <div class="col-md-6 offset-md-3">
      <div class="card">
        ...
      </div>
  </div>
</div>
```
{: mark_lines="4"}

Very nice!

## Bootstrap Cards for Index Page 01:32:00 to 01:37:00

Now that we have cards on the details page for a movie, let's put cards on the index page as well. In another place (view template) we want the bit of HTML with embedded Ruby that we just wrote. How can we solve this? 

Partials!

Let's make a file `app/views/movies/_movie_card.html.erb`, and fill it with the card code, and, because we want the option of passing `:locals` to the partial, we replace and `@movie` model instance variables with just `movie`:

```erb
<!-- app/views/movies/_movie_card.html.erb -->

<div class="card">
  <div class="card-header">
    <%= link_to "Movie ##{movie.id}", movie %>
  </div>

  <div class="card-body">
    <dl>
      <dt>
        Title
      </dt>
      <dd>
        <%= movie.title %>
      </dd>

      <dt>
        Description
      </dt>
      <dd>
        <%= movie.description %>
      </dd>
    </dl>

    <div class="row">
      <div class="col">
        <div class="d-grid">
          <%= link_to edit_movie_path(movie), class: "btn btn-outline-secondary" do %>
            <i class="fa-regular fa-pen-to-square"></i>
          <% end %>
        </div>
      </div>
      <div class="col">
        <div class="d-grid">
          <%= link_to movie, data: { turbo_method: :delete }, class: "btn btn-outline-secondary" do %>
            <i class="fa-regular fa-trash-can"></i>
          <% end %>
        </div>
      </div>
    </div>
  </div>

  <div class="card-footer">
    Last updated <%= time_ago_in_words(movie.updated_at) %> ago
  </div>
</div>
```

Right away, we can remove all the card HTML from the `show` page and replace it with a clean partial (including our `:locals` with the current `@movie` object), reducing the whole view template to just five lines of code:

```erb
<!-- app/views/movies/show.html.erb -->

<div class="row">
  <div class="col-md-6 offset-md-3">
    <%= render partial: "movies/movie_card", locals: { movie: @movie } %>
  </div>
</div>
```

And we have our partial, so we can plug that into the 

We can take the code in the `index` and get rid of our entire table setup, rather rendering every card in the `.each` loop onto its own card! Again, reducing the amount of code in the view template by a huge amount:

```erb
<!-- app/views/movies/index.html.erb -->

<h1>
  List of all movies
</h1>

<hr>

<div>
  <%= link_to "Add a new movie", new_movie_path %>
</div>

<hr>

<% @movies.each do |movie| %>
  <%= render partial: "movies/movie_card", locals: { movie: movie } %>
<% end %>
</div>
```

Note that in the `.each` loop our block variable was called `movie`, and the local variable in the partial also happens to be called `movie`. Hence the `movie: movie` in the `:locals`. That might seem confusing, but these are two different objects. The first `movie:` is a symbol, representing the name of the variable in the partial, and the second `movie` is the block variable coming from the `@movies.each` iteration.

If you refresh the `index` page with that code, you will see all the cards, but the spacing is not great. That's because we're just rendering the partial over and over in the loop, without putting any spacing around them. We can quickly add some spacing within our loop like so:

```erb
...
<hr>

<div class="row">
  <% @movies.each do |movie| %>
    <div class="col-md-3">
      <%= render partial: "movies/movie_card", locals: { movie: movie } %>
    </div>
  <% end %>
</div>
```

We put the entire `.each` loop in a `"row"` of the container (again, the `"container"` is coming from the application layout), and then put the card partials in columns with a width of 3.

How does the page look now? Pretty good, right? Time to commit!

## Partials shine along with Jump To File 01:37:00 to 01:40:00

With all of these partials, we're really starting to have a lot of files to jump around between! And this is only for one `movies` resource. Imagine if we had four or five or more.

In addition to all the other benefits, partials really help you get around your codebase efficiently. For example, if you need to make a change to the navbar, rather than going to the application layout file and digging around, you can use the Jump To File keyboard shortcut

 - Windows: <kbd>ctrl</kbd>+<kbd>p</kbd>
 - Mac: <kbd>cmd</kbd>+<kbd>p</kbd>

Start typing the name of the file — VSCode fuzzily matches what you type and usually finds the right file within a few characters. Hit return and boom you're transported to just where you want to be.

If you're still manually clicking files and folders in the sidebar, start trying to get used to navigating with Jump To File instead.

Most of what we do as web developers is getting data from our CRUD database and then putting HTML around it for our users. Aren't partials just wonderful for that!

## `ActiveRecord` Object Partials 01:40:00 to 01:45:00

In the real world, a lot of professional developers would have just named the partial for the movie card `_movie.html.erb`. In fact, it's so common that we can open a `rails console` and see a specific method for it:

```ruby
pry(main)> m = Movie.first
pry(main)> m.to_partial_path
=> "movies/movie"
```

The method `.to_partial_path` was inherited from the `ApplicationRecord` class. We could even overwrite this method to follow _our_ naming convention:

```ruby
# app/models/movie.rb

class Movie < ApplicationRecord
  validates :title, presence: true

  def to_partial_path
    "movies/movie_card"
  end
end
```
{: mark_lines="6-8"}

But, let's just keep things conventional and rename our `_movie_card.html.erb` file to the traditional `_movie.html.erb` name (without the `_card`, which is not convention).

All of this means that there is a shortcut for rendering the partial. For instance, in the show page, we could now just change the render statement from:

```erb
    <%= render partial: "movies/movie_card", locals: { movie: @movie } %>
```

to:

```erb
    <%= render @movie %>
```

That's pretty crazy! The render method, given an `ActiveRecord` object, will figure out the path to the partial and the name of the locals in the partial. 

With the conventional naming and shortcut, the `render` method will even do the entire `.each` looping for us in the index if you give it an `ActiveRecord:Relation` (list of movie records) on the `index` page, so we could change:

```
<div class="row">
  <% @movies.each do |movie| %>
    <div class="col-md-3">
      <%= render partial: "movies/movie_card", locals: { movie: movie } %>
    </div>
  <% end %>
</div>
```

to:

```
<div class="row">
  <%= render @movies %>
</div>
```

(In this case we did lose our `class="col-md-3"` to get some nicer spacing.)

You will see these `render` shortcuts for partials on some online resources, so it's good to be aware of. But, in general, I like writing things out the more long-hand way to make it clear.

You can see all of the cool ways to use partials like we've done here in [the official Rails documentation](https://edgeapi.rubyonrails.org/classes/ActionView/PartialRenderer.html).

## `before_action` 01:45:00 to 01:53:30

Now that we're comfortable with partials and using Bootstrap to style our app, let's switch gears.

In my controller, if I had some method or logic that was happening in several actions, how could I replace all of that to DRY (Don't Repeat Yourself) up my controller?

For instance, say we had a random method `foo` that just printed something:

```ruby
# app/controllers/movies_controller.rb

class MoviesController < ApplicationController
  def foo
    p "hiya" * 100
  end

  def new
    @new_movie = Movie.new
  end
...
```
{: mark_lines="4-6"}

And we wanted to have that method run in a few different actions (say `new` and `index`):

```ruby
# app/controllers/movies_controller.rb

...
  def new
    self.foo
    @new_movie = Movie.new
  end

  def index
    self.foo
    @movies = Movie.order(created_at: :desc)

    respond_to do |format|
      format.json { render json: @movies }

      format.html
    end
  end
...
```
{: mark_lines="5 10"}

<aside markdown="1">
Did we need the `self.` before `foo`? No! Rails will look for a `self.` defined method before crashing if we left it off.
</aside>

Now, we could create a new movie from our index page in our live app, and visit the server log in our terminal where `bin/dev` is running (`p` statements in the controller will be rendered in the server log). We should see the printed statement run for both the `new` and `index` action.

There's actually a method inherited into the `MoviesController` that we can use to run this `foo` action before specific actions in our controller:

```ruby
# app/controllers/movies_controller.rb

class MoviesController < ApplicationController
  before_action :foo, only: [:new, :index]

  def foo
    p "hiya" * 100
  end

  def new
    # self.foo
    @new_movie = Movie.new
  end

  def index
    # self.foo
    @movies = Movie.order(created_at: :desc)

    respond_to do |format|
      format.json { render json: @movies }

      format.html
    end
  end
...
```
{: mark_lines="4 11 16"}

The method `before_action` takes a first argumet with the method name (as a symbol: `:foo`) and the second argument we give it is `only:`, which means "only run the action before this list of actions", and the array of actions in the controller to run `foo` _before_ (here, we run our dummy code before `:new` and `:index`). We could also use `except:` in place of `only:`, to pass a list of actions that we _don't_ want to run the method before.

We saw `before_action` in AD1! When we wanted to force a user to sign in to the app before they did anything else, we added: `before_action :force_user_sign_in` to the top of our `ApplicationController`, which is inherited by every other conroller (`MoviesController < ApplicationController`). Refresh your memory on that [here](https://chapters.firstdraft.com/chapters/888#current_user).

That's nice. Now let's delete that `foo` code and see where we can use this in our controller file to DRY up code.

How about all of the places in actions where we are writing `@movie = Movie.find(params.fetch(:id))`? 

Let's first make a method for this (and put it under `private` since it's just a helper for the actions in our CRUD app):

```ruby
# app/controllers/movies_controller.rb

...
  private

  def movie_params
    params.require(:movie).permit(:title, :description, :image_url, :released_on)
  end

  def set_movie
    @movie = Movie.find(params.fetch(:id))
  end
end
```
{: mark_lines="10-12"}

And now, we can call this action before the methods that require it (and delete the code line with `@movie = ...` from those actions):

```ruby
# app/controllers/movies_controller.rb

class MoviesController < ApplicationController
  before_action :set_movie, only: [:show, :edit, :update, :destroy]
...
```
{: mark_lines="4"}

With that, two of those actions will actually end up being completely empty now!

```ruby
# app/controllers/movies_controller.rb

...
  def show
  end
...
  def edit
  end
...
```

One could argue that this use of `before_action` makes our codebase more difficult to read, so it's not strictly necessary. But, again, it's something you will see a lot and you should get used to.

You can read more about this [in the official Rails documentation](https://guides.rubyonrails.org/action_controller_overview.html#filters).

## Directors Scaffold 01:53:30 to 02:07:00

With all of this new knowledge we've gained, let's try something. Open terminal tab and generate a new scaffold for a `directors` resource:

```bash
rails g scaffold director name dob:date bio:text
```

Carefully read through all of the code that was generated by this built in Rails action. Do you understand much more of it now? Hopefully yes! 

One thing: `scaffold` generates a basic CSS stylesheet in `app/assets/stylesheets/scaffold.scss`, which interferes with any Bootstrap we added, so you can delete this file.

Understanding _most_ of what `scaffold` does is the baseline minimum knowledge you should have as a developer for looking at online resources.

Most developers learn Rails with `scaffold` as a _starting_ point. However, through AD1 and AD2, you know how to drop back to basics with no helper methods or partials or anything else fancy, and just write an RCAV from scratch. That's something many developers never really learn how do to. Remember, when things are going wrong, drop back to basics and Route, Controller, Action, View!

## User accounts with Devise 02:07:00 to 02:17:30

You have already used the devise gem to add user accounts in the _Bulletin Board 2_, and we will do so again in this project.

The next reading lesson, _User Authentication with Devise_, has a few more details about the Devise gem and will be a handy cheat sheet; but this section is all you need to finish this current project.

 1. Add `gem "devise"` to your `Gemfile` and `bundle install`.

 1. Then run another command at the terminal to finish installing it: 
 
    ```bash
    rails g devise:install
    ```

 1. Now create a `users` resource with: 
 
    ```bash
    rails g devise user first_name:string last_name:string
    ```

  This is just like our previous resource generator, where we have the table name `user` and the name:datatype of the columns.
 
 1. Migrate the table to our database: 
    
    ```bash
    rails db:migrate
    ```
 
 1. Check out the `User` model (`app/models/user.rb`). You'll see that Devise automatically adds columns for email and password, among other things.
 
 1. Devise provides the following route helper methods and corresponding RCAVs for free (all with the addition of `devise_for :users` in `config/routes.rb`):
    - (GET) `new_user_registration_path`: displays a sign up form
    - (GET) `new_user_session_path`: displays a sign in form
    - (DELETE) `destroy_user_session_path`: signs the user out
    - (GET) `edit_user_registration_path`: displays an edit profile form
    - even more for cancelling accounts, forgotten passwords, etc.
 
To see the new code in action, restart the server with <kdb>ctrl</kbd>+<kdb>c</kbd> in the `bin/dev` terminal and restart the server. 

Now visit `/users/sign_in`. The whole login RCAV is up and running without us doing anything! 

Try to signup for an account. You can just use `alice@example.com` and `password`, no need to use your actual email. When you signup, you should be redirected to the root homepage `/`.

Let's add some links to our navbar in the `_navbar.html.erb` partial to reach the sign up, sign in, sign out pages. E.g.:

```erb
<!-- ... -->
    <li class="nav-item">
      <%= link_to "Movies", movies_path, class: "nav-link" %>
    </li>
    <li class="nav-item">
      <%= link_to "Sign up", new_user_registration_path, class: "nav-link" %>
    </li>
    <li class="nav-item">
      <%= link_to "Log in", new_user_session_path, class: "nav-link" %>
    </li>
    <li class="nav-item">
      <%= link_to "Sign out", destroy_user_session_path, class: "nav-link", data: { turbo_method: :delete } %>
    </li>
<!-- ... -->
```

(Note we needed a `data: { turbo_method: :delete }` for the sign out action, because this is a DELETE request in the route that Devise defines.)

Now: 

 1. If someone is signed in, conditionally display sign out/edit profile links in the navbar. You can check if someone is signed in with the `user_signed_in?` method, which is defined by Devise and available in all view templates.
 
 1. For the edit profile link, make the content of the `<a>` tag the user's first name + last name. You can access the signed in user with the `current_user` helper method, which is defined by Devise and available in all actions and all view templates.
 
 1. Sign out. (If your sign-out link didn't work, you probably forgot to add `data: { turbo_method: :delete }` to it.)
 
 1. Force someone to be signed in by adding the `before_action :authenticate_user!` method to the `ApplicationController`. The `:authenticate_user!` method is defined by Devise.

## Solutions

You can see my solutions for this project in [this pull request](https://github.com/appdev-projects/helper-methods-part-3/pull/2/files).

---
