---
layout: post
title: "Adding microposts to Middleman"
date: 2016-11-20 22:10
comments: true
categories: 
---

I came across the idea of microblogs from Manton Reece's [blog](http://www.manton.org/tag/microblogs).
The idea is that instead of just using the blog for long content, you can post short Tweet like items there also.
Needless to say you can just use Twitter for this but it's more fun to do something unneccessarily complicated!
I have form here - like using Octopress for a blog instead of WordPress.  

Basically, instead of using Twitter to publish short notifications, you can post them to your own site and if necessary to the likes of Twitter from there.
Having control of your own content gives you independence from third party sites - if Twitter shut down tomorrow then my posts would still be available here.
If I'm putting content on the web then ideally I would like it to be on my site under my own domain name.
I've decided to add them first to my [chess](http://www.gerardcondon.com/chess) site and then if they work out there, try to port them to Octopress.

That blog is written using [Middleman](http://www.middlemanapp.com), which has the ability to use data files.
Any [yaml](https://en.wikipedia.org/wiki/YAML) file in the data folder is available to the Middleman app using a `data.filename.foo` like syntax.
This means that I can create a yaml file containing the microblog posts and then use that to generate the appropriate html pages.

I have a microposts.yml file with each entry having text and date entries. This is sufficient for the moment but also yaml is flexible enough that I can add more attributes later.
``` yml microposts.yml
- text: here is another post
  date: 2016-11-19
- text: here is a post
  date: 2016-11-20
```

I use the Middleman [blog extension](https://middlemanapp.com/basics/blogging/) to generate the blog portion of the site.
I want to keep using this as it handles rss, tags, archive pages, pagination etc.
It works by generating posts from files in a source folder. In order to integrate with this, I want to generate a file per micropost in the correct location.
Under the source folder I split out my posts into two folders `manual` and `automated`.
I updated the regex in config.rb to handle this `blog.sources = "posts/{type}/{year}-{month}-{day}-{title}.html"`.
Middleman will process these equally but now I can check what type the post is by using `data["type"]`.

In the config.rb I delete the contents of the automated folder on each build.
I tried using `before_build` hooks but they were being called too late - the Middleman data structures were already created with the previous contents of the automated folder.
This lead to build errors and issues with sitemaps etc.
So I just perform the File operations directly in the config.rb. This ensurs that they are executed before the blog extension processes the posts folder.
Then I iterate through the micropost data from the yaml file and create the appropriate file in the automated folder.

``` ruby config.rb# Clear out automated posts folder
automated_posts_folder = "#{config[:source]}/posts/automated"
FileUtils.rmtree(automated_posts_folder)
FileUtils.mkdir(automated_posts_folder)

post_counter = 1
data.microposts.each do |micropost|
  post_content = "---\n"
  post_content += "title: mp#{post_counter}\n"
  post_content += "date: #{micropost.date}\n"
  post_content += "tags: microposts\n"
  post_content += "micropost: true\n"
  post_content += "---\n"
  post_content += "#{micropost.text}"

  File.write("#{automated_posts_folder}/#{micropost.date}-mp#{post_counter}.html.md", post_content)
  post_counter += 1
end
```

I've added `micropost: true` metadata to each of my generated files so that I can check if the post is a micropost using `if data["micropost"]`.
This allows me to customize how I display these posts - in particular I don't show a title for these posts.

Speaking of titles, some special handling is required for these as the Middleman blog extension uses these for the generated html file names.
I added an attribute `display_title` to the `BlogArticle` class which will return an empty string if the post is a micropost.
This allows me to specify a fake title for each micropost using an incrementing ID counter.
The blog extension can use this fake title as normal but it will never be seen by the viewer of the blog.
Instead anywhere that a title is required in html markup I can change it to use the `display_title`  attribute.
The fact that Ruby has open classes makes adding this very easy - I can do it in the config.rb file and don't need to go mucking about in the Blog extension code.

``` ruby config.rb
module Middleman
  module Blog
    module BlogArticle
      def display_title
        data["micropost"] ? "" : data["title"]
      end
    end
  end
end
```

This means that I can display these microposts just using the main text and not displaying a title. They interleave nicely with longer form blog posts.

Using the Middleman Blog Extension means that I also get RSS integration for these posts.
I edited the xml templates to remove the title if the post is a micropost.
Now in my RSS reader, they show up with the body text as the title instead of the fake title needed to make the extension work.

This current implementation is basic enough and I have a few ideas of how to improve it in future.

* I like the idea of the static site but the editing part is not particularly nice and is a bit of a barrier to posting. I would like to use something like [Contentful](https://www.contentful.com/) so that I can add posts without having to open a dev environment.
* Once you have something like Contentful, then the next step would be to use that to trigger automatic builds once a new post is added. This would mean that the site is always updated and assuming Contentful works well from Android, would allow me to make these microposts from my phone.
* I could use [IFTTT](https://ifttt.com/) to cross post the microposts to Twitter - however I would have to deal with the 140 character limit there. I would also like to automatically post a notification to Twitter when I put up a normal blog post.
* Currently everything is going into the same RSS feed. I would like to add separate RSS feeds e.g. micropost only, normal post only. I think this would also help with using IFTTT to post to Twitter.
* One nice consequence of using the Middleman data files is that there is a much better separation of content from markup. I would like to expand this to also apply to full blog posts. Ideally everything content related would come from the data files. Then I have more options as to how to generate those data files e.g. Contentful for the tweets or a Rails app for the pgn files.