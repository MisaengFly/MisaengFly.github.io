---
layout: post
title:  "Implementing FaceBook sVoice on Google Colab - Part 1"
tags:
  - Implementation
  - Speech Separation
  - sVoice
hero: https://source.unsplash.com/collection/430471/
overlay: red
published: true

---
sVoice is a recently introduced state-of-the-art speech separation technique by FaceBook. The technique, introduced in
the paper "Voice Separation with an Unkwown Number of Multiple Speakers", successfully separates voices (or speeches)
of multiple speakers in a single audio sequence.

This post will breifly run through training the open source sVoice model on Google Colab, and introduce some of the
challenges that faced along the way. The sVoice github repository is available at 
https://github.com/facebookresearch/svoice.

Before anything else, let's create a new file in Google Colab and mount the file with Google Drive.
~~~ruby 
from google.colab import drive
drive.mount('/content/drive')
~~~ 

Then clone the sVoice repository to your Google Drive. Cloning the repository will create a new folder named 'svoice'.
~~~ruby 
!git clone https://github.com/facebookresearch/svoice.git
!cd svoice
~~~

Now, let's install the dependencies following the instructions available on the sVoice repository.
~~~ruby
!pip install torch==1.6.0+cu101 torchvision==0.7.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html
!pip install -r requirements.
~~~


You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`.
{: .lead}
<!–-break-–>
This launches a web server and auto-regenerates your site when a file is updated.  
To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

~~~ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
~~~

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
