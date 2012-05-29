bkoojs
======

Ben Kinsey's Object Oriented Javascript Library

<pre>
oo.js (name spaced as $oo) is a javascript library which allows you to build simple object oriented html, js, and css.

In your html document you have to include the library.
&lt;script src="oo.js">&lt;/script>
(For now, each object has to be listed in a config file, so the $oo library knows what objects we have. Eventually, we can make this auto scannable from filesystem the server side.)

The oo.js code will scan your html for instances of objects that it knows about, such as:

&lt;div class="_oo_instance_of_my_obj">&lt;/div>

It will then swap out that div with the html/js/css "object" bundle associated with that instance.

Or, alternatively you can instantiate a named instance and display it of that object using javascript:

&lt;script>
  var instance = $oo.create_instance('my_obj');
  $oo.display(instance);
&lt;/script>

html/js/css object bundles are stored in a directory, and at least the html file muse be present:

your_project/objects/my_obj/my_obj.html

my_obj.html is simply html with some javascript script tags if desired.

&lt;div>Hello World&lt;/div>

We also have the concept of variables (more like placeholders inside templates):

&lt;div>Hello &lt;div class="_user">&lt;/div>&lt;/div>

(the underscore convention means that this class is not actually inserted into the rendered DOM.)

Now, if we want to instantiate the object, and set the variable, we can do it with html:

&lt;div class="_oo_instance_of_my_obj">
  &lt;div class="_name">World&lt;/div>
&lt;/div>

or javascript:

&lt;script>
  var instance = $oo.create_instance('my_obj', {user: 'World'});
  $oo.display(instance);
&lt;/script>

Or, in javascript with three steps:

&lt;script>
  var instance = $oo.create_instance('my_obj');
  $oo.init(instance {user: 'World');
  $oo.display(instance);
&lt;/script>


Either way, the fully rendered DOM will look like this:

&lt;div>Hello World&lt;/div>

Subclassing is done like as follows. Create a new file:

your_project/objects/my_sub_obj/my_sub_obj.html

In this case we simply want to append content:

&lt;div class="_oo_extends_my_obj">&lt;/div>
  &lt;div class="_oo_prepend_content">
  ALOHA!&nbsp;
  &lt;/div>
  &lt;div class="_oo_append_content">
  . I hope you have a wonderful day!
  &lt;/div>
&lt;/div>

Now, when we instantiate:

&lt;div class="_oo_instance_of_my_sub_obj">
  &lt;div class="_name">World&lt;/div>
&lt;/div>

Renders as this:

&lt;div>ALOHA! Hello World. I hope you have a wonderful day!&lt;/div>

(note that whitespace is trimmed)

Another example that demonstrates subclassing replacing sections:

your_project/objects/parent/parent.html:

&lt;div>
  &lt;div class="_section_1">This is section 1 from the parent&lt;/div>
  &lt;div class="_section_2">This is section 2 from the parent&lt;/div>
&lt;/div>

your_project/objects/parent/child.html:

&lt;div class="_oo_extends_parent">
  &lt;div class="_section_2">This is section 2 from the child!&lt;/div>
&lt;/div>

Instantiated like this:

&lt;div class="_oo_instance_of_child">&lt;/div>

Renders like this:

&lt;div>
  &lt;div class="_section_1">This is section 1 from the parent&lt;/div>
  &lt;div class="_section_2">This is section 2 from the child!&lt;/div>
&lt;/div>

Including an instance of one class inside another:

your_project/objects/parent/outer.html:
&lt;div>
  &lt;div class="_oo_instance_of_inner">
    &lt;div class="_user">Joe&lt;/div>
  &lt;/div>
  &lt;div class="_oo_instance_of_inner">
    &lt;div class="_user">Fred&lt;/div>
  &lt;/div>
&lt;/div>

your_project/objects/parent/inner.html:
&lt;div>The name is &lt;div class="_user">&lt;/div>&lt;/div> 

Looping. Note that we are free to use jquery, since this will be done on the client side.

your_project/objects/parent/outer.html:
&lt;div>
  &lt;script>
    $.each(['Joe', 'Fred'], function(index, value) {
      var instance[index] = $oo.create_instance('inner');
      $oo.init(instance, {user: value});
      $oo.display(instance[index]);
    }
  &lt;/script>
&lt;/div>

Populating from the server via html:

&lt;div class="_oo_instance_of_user">
  &lt;div class="_oo_init">/users/user.json&lt;/div>
&lt;/div>

Populating from the server via js:

&lt;script>
  $.getJSON('/users/user.json', function(data) {
    var instance = $oo.create_instance('my_obj', data);
    $oo.display(instance);  
  }
&lt;/script>

</pre>