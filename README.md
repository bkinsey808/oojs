oojs
====

Object Oriented Javascript Library

oo.js (name spaced as $oo) is a javascript library which allows you to build simple object oriented html, js, and css.

(For now, each object has to be listed in a config json file, so the $oo library knows what objects we have. Eventually, we can make this auto scannable from filesystem the server side.)

bkoojs.objects.json:
<pre>
{
  'my_obj': {'version': 1, 'path':'your_project/objects/my_obj'},
  'my_sub_obj': {'version': 1, 'path':'your_project/objects/my_sub_obj'}
}
</pre>

In your html document you include the library.<pre>
&lt;script src="oo.js">&lt;/script>
</pre>

The oo.js code will scan your html for instances of objects that it knows about, such as:
<pre>
&lt;div class="_oo_instance_of _my_obj">&lt;/div>
</pre>

It will then swap out that div with the html/js/css "object" bundle associated with that instance.

Or, alternatively you can instantiate a named instance and display it using javascript:

<pre>
&lt;script>
  var instance = $oo.create_instance('my_obj');
  $oo.display(instance);
&lt;/script>
</pre>

html/js/css object bundles are stored in a directory, and at least the html file muse be present:

your_project/objects/my_obj/my_obj.html

my_obj.html is simply html with some javascript script tags if desired.

<pre>
&lt;div>Hello World&lt;/div>
</pre>

We also have the concept of variables (more like placeholders inside templates):

<pre>
&lt;div>Hello &lt;div class="_user">&lt;/div>&lt;/div>
</pre>

(the underscore convention means that this class is not actually inserted into the rendered DOM.)

Now, if we want to instantiate the object, and set the variable, we can do it with html:

<pre>
&lt;div class="_oo_instance_of _my_obj">
  &lt;div class="_user">World&lt;/div>
&lt;/div>
</pre>

or javascript:

<pre>
&lt;script>
  var instance = $oo.create_instance('my_obj', {user: 'World'});
  $oo.display(instance);
&lt;/script>
</pre>

Or, in javascript with three steps:

<pre>
&lt;script>
  var instance = $oo.create_instance('my_obj');
  $oo.init(instance {user: 'World');
  $oo.display(instance);
&lt;/script>
</pre>

Either way, the fully rendered DOM will look like this:

<pre>
&lt;div>Hello World&lt;/div>
</pre>

Subclassing is done like as follows. Create a new file:

your_project/objects/my_sub_obj/my_sub_obj.html

In this case we simply want to append content:

<pre>
&lt;div class="_oo_extends _my_obj">&lt;/div>
  &lt;div class="_oo_prepend_content">
  ALOHA!&nbsp;
  &lt;/div>
  &lt;div class="_oo_append_content">
  . I hope you have a wonderful day!
  &lt;/div>
&lt;/div>
</pre>

Now, when we instantiate:

<pre>
&lt;div class="_oo_instance_of _my_sub_obj">
  &lt;div class="_name">World&lt;/div>
&lt;/div>
</pre>

Renders as this:

<pre>
&lt;div>ALOHA! Hello World. I hope you have a wonderful day!&lt;/div>
</pre>

(note that whitespace is trimmed)

Another example that demonstrates subclassing replacing sections:

your_project/objects/parent/parent.html:

<pre>
&lt;div>
  &lt;div class="_section_1">This is section 1 from the parent&lt;/div>
  &lt;div class="_section_2">This is section 2 from the parent&lt;/div>
&lt;/div>
</pre>

your_project/objects/child/child.html:

<pre>
&lt;div class="_oo_extends _parent">
  &lt;div class="_section_2">This is section 2 from the child!&lt;/div>
&lt;/div>
</pre>

Instantiated like this:

<pre>
&lt;div class="_oo_instance_of _child">&lt;/div>
</pre>

Renders like this:

<pre>
&lt;div>
  &lt;div class="_section_1">This is section 1 from the parent&lt;/div>
  &lt;div class="_section_2">This is section 2 from the child!&lt;/div>
&lt;/div>
</pre>

Including an instance of one class inside another:

your_project/objects/parent/outer.html:

<pre>
&lt;div>
  &lt;div class="_oo_instance_of _inner">
    &lt;div class="_user">Joe&lt;/div>
  &lt;/div>
  &lt;div class="_oo_instance_of _inner">
    &lt;div class="_user">Fred&lt;/div>
  &lt;/div>
&lt;/div>
</pre>

your_project/objects/parent/inner.html:

<pre>
&lt;div>The name is &lt;div class="_user">&lt;/div>&lt;/div> 
</pre>

Looping. Note that we are free to use jquery, since this will be done on the client side.

your_project/objects/parent/outer.html:

<pre>
&lt;div>
  &lt;script>
    $.each(['Joe', 'Fred'], function(index, value) {
      var instance[index] = $oo.create_instance('inner');
      $oo.init(instance, {user: value});
      $oo.display(instance[index]);
    }
  &lt;/script>
&lt;/div>
</pre>

Populating from the server via html:

<pre>
&lt;div class="_oo_instance_of _user">
  &lt;div class="_oo_init">/users/user.json&lt;/div>
&lt;/div>
</pre>

Populating from the server via js:

<pre>
&lt;script>
  $.getJSON('/users/user.json', function(data) {
    var instance = $oo.create_instance('my_obj', data);
    $oo.display(instance);  
  }
&lt;/script>
</pre>

