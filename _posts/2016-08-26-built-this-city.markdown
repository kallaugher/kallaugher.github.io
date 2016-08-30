---
layout: post
title:  "Rails Magic: Nested Forms and the Build Method"
date:   2016-08-26 09:00:00
categories: code
---

In this post, I'm going to take a look at nested forms in Rails, and in particular a closer look at a small, but useful method provided by ActiveRecord: the <code>build</code> method.

Imagine we're building a basic blogging app in Rails, with users, posts and tags. We need a form to allow users to submit a new post. Rails has a number of useful helpers for creating forms. In this case, because <code>Post</code> is a model, the logical form helper to use is <code>form_for</code> -- it binds a form to a model object, instead of having to repeat the object multiple times.

{% highlight ruby %}
// app/views/posts/new.html.erb

<%= form_for @post do |f| %>

	<%= f.hidden_field :user_id, value: current_user.id %>

	<%= f.label :title %>
	<%= f.text_field :title %><br><br>

	<%= f.label :content %>
	<%= f.text_field :content %><br><br>

	<%= f.submit %>

<% end %>

{% endhighlight %}

This will create a simple form with two fields - one for title, and one for content, as well as a hidden field that associated the current user with their post. So far, so good. But we still want to add tags for each post. Let's have a look at the associations for each of the three models:

* a User has many Posts
* a Post belongs to a User
* a Post has many Tags
* a Tag has many Posts

With the has_many relationship between Posts and Tags going both ways, we'll need a join table. We'll therefore express the relationship between them as follows:

* a Post has many Post_Tags
* a Post has many Tags, through Post_Tags
* a Tag has many Post_Tags
* a Tag has many Posts, throught Post_Tags

This association between tags and posts makes adding tags to the form slightly more complicated. When we add tags, they are by definition associated with the post that is being created with the form above. Therefore, instead of creating a separate <code>form_for</code> for tags, we want to nest an additional form inside the post <code>form_for</code>, one that will maintain the relationship between posts and tags.

{% highlight ruby %}
<%= form_for @post do |f| %>
	<%= f.hidden_field :user_id, value: current_user.id %>

	<%= f.label :title %>
	<%= f.text_field :title %><br><br>

	<%= f.label :content %>
	<%= f.text_field :content %><br><br>

	<%= f.collection_check_boxes :tag_ids, Tag.all, :id, :name %><br>

    <%= f.fields_for :tags, @post.tags.build do |tags_attributes| %>
      <%= tags_attributes.label :name  %>
      <%= tags_attributes.text_field :name %>
    <% end %>

	<%= f.submit %>
<% end %>
{% endhighlight %}

<code>collection_check_boxes</code> will iterate through the array <code>Tag.all</code>, and create a checkbox for each existing tag. These tags already exist, and if their checkbox is selected, will be associated with the post being created as well. For more details, see the [APIdock documentation](http://apidock.com/rails/v4.0.2/ActionView/Helpers/FormOptionsHelper/collection_check_boxes).

The <code>fields_for</code> form helper provides a field for creating a new tag. This is where the elusive <code>build</code> method comes into play. If this were a separate <code>form_for</code> for just the tags, you could define <code>@tag = Tag.new in the Tags controller, and the values from the form would populate that object. However, if we used <code>Tag.new</code> in this case, there would be no association between the tag and the newly created post.


The <code>build</code> method provides the same functionality as <code>.new</code> would, but instantiates with the defined association. For example, <code>@post.tags.build</code> instantiates a <code>Tag</code> object that is already associated with <code>@post</code>. Just like <code>.new</code>, the object has not been saved to the database.


Cementing the similarities with <code>.new</code>, <code>build</code> is an alias for <code>ActiveRecord::Relation#new</code> -- it is simply <code>.new</code> with relations baked in.

Back to my example -- if I wanted multiple blank form fields for adding multiple tags at once, I can simply run <code>.build</code> multiple times, and for each instantiated object, a new field will appear. Once you're using .build more than once, it is probably easier and simpler to move this to the post controller.

The <code>new.html.erb</code> will have the following snippet of code:

{% highlight ruby %}
<%= f.fields_for :tags do |tags_attributes| %>
	<%= tags_attributes.label :name  %>
   <%= tags_attributes.text_field :name %>
<% end %>
{% endhighlight %}

The controller will have:

{% highlight ruby %}
def new
	@post = Post.new
   	3.times {@post.tags.build}
end
{% endhighlight %}

This will create blank form fields for our additional tags, and three correctly associated (but unsaved) tag objects, ready to be populated with the form answers.

Has_one relationships
-------

One additional note: if you use <code>build</code> with a has_one association, the syntax is slightly different. For example, if each post had by definition only a single tag:

{% highlight ruby %}
	# we would expect to use:
@post.tag.build
	#spoiler: this won't work

	#instead, use:
@post.build_tag

{% endhighlight %}


-----

Thanks for reading! If you have any additional information on anything touched on above, leave a comment below.
