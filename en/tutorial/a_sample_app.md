# A Sample App

Sometimes easiest way to learn a new piece of technology is to start building.  Our first example project will be a simple todo app like the ones on [todomvc.com](http://todomvc.com/)  Our app will have many simple features:

--TODO: Add screenshot --

- A field where users type in a todo and press enter to add.
- A list of todos
    - A checkbox to complete the todo
    - An x button to remove the todo
- The number of remaining todos
- A button to complete all tasks (that is disabled if all tasks are already complete)

## Install Volt

To start, lets generate a new Volt app, and go into the generated directory, by running the commands outlined below:

```
gem install volt
volt new todo_example
cd todo_example
```

## Volt vs. Rails

Since many of you probably have some experience in Rails, it is worth noting the structural similarities and differences that dinstinguish Volt as a framework.

You are going to notice that the structure of your new Volt app is very similar to a Rails app. The biggest different you'll encounter is in the ```todo_example/app``` folder. In a Rails app, you have one "app". In Volt, the subdirectories within this folder act as "components", which can be re-used throughout the other components of your system.

We start with the default "main" component.

## Boot up

```
bundle exec volt s
```

In your browser, navigate to http://localhost:3000, and you'll see the default Volt app.

One cool thing to note about Volt is that your app will automatically reload if you change a file. To test this out yourself, open up the file ```todos_example/app/main/views/main/main.html``` and edit the name inside ```html <h3 class="text-muted">``` within ```html <div class="header">```. Save the file, and keep an eye on your browser.

Now, we can get down to some real code!

## Add a todos view

Let's get started by adding a ```todos.html``` file. Go into the ```todos_example/app/main/views/main``` folder and add the file ```todos.html```.

Hey, don't copy and paste. Copy using your eyes and your hands, so that it sticks in your head.

```html
<:Title>
  Todos
  
<:Body>
  <h1>Todo List</h1>
```

We need a way to get *to* our new view, so we pop open the ```todos_example/app/main/views/main/main.html``` file and, below the line ```html <:nav href="/" text="Home">``` we'll add:

```html
  <:nav href="/todos" text="Todos">
```

We also need to get the route setup for our new view, so we're going to open up ```todos_example/app/main/config/routes.rb```. Add the line

```ruby
get "/todos", _action: "todos"
```

You should now have a link to your todos page.

## Adding todo list items

Open up ```todos_example/app/main/views/main/todos.html``` again, and let's start fleshing out the code for todo additions.

First, let's set up our form. Note the ```e-submit``` attribute, informing Volt to pay attention and handle this form:

```html
<form e-submit="add_todo" role="form">
</form>
```

This is an important bit. The value of the ```e-submit``` attribute is the name of the controller method which we will call. That means you need to open ```todos_example/app/main/controllers/main_controller.rb``` and add the method itself. For now, since we're focusing on the HTML content, we're just making a quick aside, and we'll come back to this method soon:

```ruby
def add_todo
end
```

We'll flesh that method out in just a bit. Head back over to the ```todos.html``` file and let's get cracking setting up our form. Inside the ```html <form e-submit="add_todo" role="form">``` block, we're going to add a bit of code:

```html
<div class="form-group">
  <label>Todo</label>
  <input class="form-control" type="text" value="{{ page._new_todo }}">
</div>
```

There it is, our first real taste of Volt magic. Notice the ```value``` attribute, with its fancy curly braces. Here, we're asking the ```page``` scope to give us the value of ```new_todo``` (**NOTE:** This is not a typo. If the value is named ```new_todo``` in Ruby, it is referred to as ```_new_todo``` in the template.)

Now, the value of that text field is bound. As a result, any time that that value changes, any fields bound to it will change synchronously. Cool beans.

### Adding todo items

What we want is:

  1. User enters text into the "new todo" field
  2. User hits enter
  3. The new todo is added to the list of current todos

As a starting point, let's go and flesh out the #add_todo method. Now, it should look like this:

```ruby
def add_todo
  page._todos << { name: page._new_todo }
end 
```

Wait, though. When the user hits enter, we obviously will need to clear out the ```new_todo```, so add this line just above the ```end``` line of the ```def add_todo``` block:

```ruby
page._new_todo = ''
```

We've got that bit set up just fine. It's time to get some visual results from this whole ordeal!

## Showing the todo list

Let's navigate back to ```todos_example/app/main/views/main/todos.html```. Between the ```<h1>``` tag and the ```<form>``` that we added earlier, you need to place the following:

```html
<table class="table">
  {{ page._todos.each do |todo| }}
    <tr>
      <td>{{ todo._name }}</td>
    </tr>
  {{ end }}
</table>
```

Go back to your browser and play with your new app. You should be able to add items to the list, now.

## Completing and removing todo items

Obviously, our users may make a typo or some other mistake, and they want the ability to remove items they've added to their todo list.

Let's start by giving them a button. Below the ```<td>``` we just added for the list item name, let's add another:

```html
<td>
  <button e-click="remove_todo(todo)">X</button>
</td>
```

Just as obviously, our users may want to mark an item as "completed". Let's use a checkbox for this one. *Above* the ```<td>``` we added first, for the name of the list item, add a new ```<td>```. At this point, this bit of code should look something like

```html
<tr>
  <td>
    <input type="checkbox" checked="{{ todo._completed }}">
  </td>
  <td>{{ todo._name }}</td>
  <td>
    <button e-click="remove_todo(todo)">X</button>
  </td>
</tr>
```

## Getting stylish

Functionality is key. But stylishness is another beast entirely. A checkbox is a boring indicator, to our users, that their task is done. Let's give 'em some styling.

Change your line ```<td>{{ todo._name }}</td>``` to this:

```html
<td class="{{ if todo._completed }}complete{{ end }}">{{ todo._name }}</td>
```
