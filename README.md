# 才华横溢 muse 案例教学 12
```
cd workspace
rails new muse
cd muse
git init
git commit -m "initial commit"
git remote add origin https://github.com/shenzhoudance/muse.git
git push -u origin master
```
```
atom .
rails server
http://localhost:3000/
```
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpnw0fa0rej30uy0rsh3o.jpg)

```
git checkout -b post
rails g model post title:string link:string description:text
rake db:migrate
git add .
git commit -m "add model post"
git push origin post
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpnw57nd6vj31bk0h0wi2.jpg)
```
rails g controller Posts
---
config/routes.rb
---
Rails.application.routes.draw do
  resources :posts
  root 'posts#index'
end
---
```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpnwxsqojij311u09wjs6.jpg)
```
app/controllers/posts_controller.rb
---
class PostsController < ApplicationController
  def index
  end
end
---
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpnx04vaesj31kw0hp44e.jpg)
```
app/views/posts/index.html.erb
---
<h1>欢迎来到才华横溢的世界</h1>
```
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpnx26bh58j30r808kdgf.jpg)

```
git checkout -b add_gem
https://rubygems.org/
---
gem 'haml', '~> 5.0', '>= 5.0.4'
gem 'simple_form', '~> 3.5', '>= 3.5.1'
gem 'paperclip', '~> 6.0'
---
bundle install
git status
git add .
git commit -m "add application gems and post view files"
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpp3ktwqsij31c20h8adl.jpg)

```
app/controllers/posts_controller.rb
---
def index
end
def show
end
def new
@post = Post.new
end
def create
@post = Post.new(post_params)
if @post.save
 redirect_to @post
else
 render 'new'
end

def edit
end

def update
end

def destroy
end

private

def find_params
end

def post_params
params.require(:post).permit(:title, :link, :description)
end
end
---
app/views/posts/_form.html.haml
---
= simple_form_for @post do |f|
	= f.input :title
	= f.input :link
	= f.input :description

	= f.button :submit
---
app/views/posts/new.html.haml
---
%h1 Add Inspiration

= render 'form'
---
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpp423dip1j30ou0dkgmg.jpg)

# 添加内容不被 show 页面显示

![image](https://ws3.sinaimg.cn/large/006tKfTcly1fpp49hi0faj30oi0dmgmx.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcly1fpp49nbglnj31kw0gd7ab.jpg)

# 显示 show 的内容
```
app/views/posts/show.html.haml
--
%h1= @post.title
%h1= @post.link
%h1= @post.description
---
app/controllers/posts_controller.rb
---
class PostsController < ApplicationController
  before_action :find_post, only: [:show, :edit, :update, :destroy]

  def index

  end

  def show

  end

  def new
   @post = Post.new
  end

  def create
    @post = Post.new(post_params)
    if @post.save
    redirect_to @post
    else
    render 'new'
  end
end

  def edit
  end

  def update
  end

  def destroy
  end

  private

  def find_post
    @post = Post.find(params[:id])
  end

  def post_params
    params.require(:post).permit(:title, :link, :description)
  end
end
---
```
![image](https://ws2.sinaimg.cn/large/006tKfTcly1fpp4mwslgzj31080d8q4r.jpg)

# 首页页面呈现效果
```
app/views/posts/index.html.haml
---
- @posts.each do |post|
  %h2= link_to post.title, post

= link_to "add posts", new_post_path
---
```
![image](https://ws3.sinaimg.cn/large/006tKfTcly1fpp51g7tasj31bc0cwac8.jpg)
```
app/controllers/posts_controller.rb
---
def index
		@posts = Post.all.order("created_at DESC")
	end
---

app/views/posts/show.html.haml
---
%h1= @post.title
%h1= @post.link
%h1= @post.description

= link_to "home", root_path
= link_to "edit", edit_post_path(@post)
= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }

```

![image](https://ws4.sinaimg.cn/large/006tKfTcly1fpp5h200bqj30oi0dmgmx.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcly1fpp5h3k7kjj30m40emabi.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcly1fpp5h9eoj9j30mw08oaan.jpg)

# 以上的代码的梳理，完成了基本功能的 CRUD 的构建；
```
git status
git add .
git commit -m "add post CRUD"
git push origin add_gem
```
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpp61v0931j31kw0otdl6.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpp61wmb2sj317s0q6tcg.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpp61xzkxmj318a0s20yj.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpp61zezq0j318k0ligq6.jpg)

```
git checkout -b devise
gem 'devise', '~> 4.4', '>= 4.4.3'
bundle install
rails g devise:views
rails g devise User
rake db:migrate
rails g migration add_user_id_to_post user_id:integer
rake db:migrate
---
app/models/post.rb
---
class Post < ApplicationRecord
  belongs_to :user
end
---
app/models/user.rb
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
  has_many :posts
end
---
```
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpq0b9q4mvj31kw11048o.jpg)
```
app/controllers/posts_controller.rb
---
class PostsController < ApplicationController
  before_action :find_post, only: [:show, :edit, :update, :destroy]
  before_action :authenticate_user!, except: [:index, :show]
  def index
    @posts = Post.all.order("created_at DESC")
  end

  def show

  end

  def new
   @post = current_user.post.build
  end

  def create
    @post = current_user.post.build(post_params)
    if @post.save
    redirect_to @post
    else
    render 'new'
  end
end

  def edit
  end

  def update
    if @post.update(post_params)
      redirect_to @post
    else
      render 'edit'
  end
end

  def destroy
    @post.destroy
    redirect_to root_path
  end

  private

  def find_post
    @post = Post.find(params[:id])
  end

  def post_params
    params.require(:post).permit(:title, :link, :description)
  end
end
```
```
rails server
http://localhost:3000/users/sign_in
```
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpq0bjazoxj31by0egq4c.jpg)
![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpq0fxw4y6j31js0bugn9.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpq0fz2y4hj30ua09igmf.jpg)

```
rails c
2.3.1 :001 > @post = Post.last
2.3.1 :002 > @post = Post.last
2.3.1 :003 > @post = Post.first
2.3.1 :004 > @post.user_id = 1
2.3.1 :005 > @post.save
2.3.1 :006 > exit
```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpq0p6kwwij31kw0ig10b.jpg)
```
git status
git add .
git commit -m "add user_id & setuo devise"
git push origin devise
```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpq0sj134mj318a112qcm.jpg)

```
rails g migration add_name_to_user name:string
rake db:migrate
rails g devise views
```
```
app/views/devise/registrations/edit.html.erb
---
<div class="form-inputs">
  <%= f.input :name, required: true, autofocus: true %>
  <%= f.input :email, required: true %>
---
app/views/devise/registrations/new.html.erb
---
<div class="form-inputs">
  <%= f.input :name, required: true, autofocus: true %>
  <%= f.input :email, required: true %>
  <%= f.input :password, required: true, hint: ("#{@minimum_password_length} characters minimum" if @minimum_password_length) %>
  <%= f.input :password_confirmation, required: true %>
</div>
---
app/controllers/application_controller.rb
---
https://stackoverflow.com/questions/37341967/rails-5-undefined-method-for-for-devise-on-line-devise-parameter-sanitizer
---
class ApplicationController < ActionController::Base
  protect_from_forgery with: :exception

   before_action :configure_permitted_parameters, if: :devise_controller?

 	protected

 	def configure_permitted_parameters
 	  devise_parameter_sanitizer.permit(:sign_up, keys: [:username])
 	  devise_parameter_sanitizer.permit(:account_update, keys: [:username])
 	end
 end
---
```
![image](https://ws2.sinaimg.cn/large/006tKfTcly1fpq1wj68axj312e0fiwg1.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcly1fpq1wizvgrj312a0hwgnj.jpg)

```
git checkout -b paperclip
app/models/post.rb
---
class Post < ApplicationRecord
  belongs_to :user
  has_attached_file :image, styles: { medium: "700x500>", thumb: "350x250>" }
  validates_attachment_content_type :image, content_type: /\Aimage\/.*\z/
end
---
rails generate paperclip post image
rake db:migrate
```
![image](https://ws3.sinaimg.cn/large/006tNc79gy1fpq9bf0klij31kw0g7dko.jpg)

```
app/views/posts/show.html.haml
---
%h1= @post.title
%p= @post.link
%p= @post.description
%p= @post.user.name
%p= @post.image


= link_to "home", root_path
= link_to "edit", edit_post_path(@post)
= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }
---
app/controllers/posts_controller.rb
---
private

def find_post
  @post = Post.find(params[:id])
end

def post_params
  params.require(:post).permit(:title, :link, :description, :image)
end
end
---
```

# 本案例的关键的操作在于：
- （1）完成基本 CRUD 的基本功能；（model + controller + views + routes）
- （2）完成 devise + name 的使用；（这个注册页面的修改上面，还需要强化认知；）
http://xbearx1987-blog.logdown.com/posts/1707961-how-to-devise-new-name-field-and-administrator-rights
```
Devise 4的参数Sanitaizer API已更改

class ApplicationController < ActionController::Base
  before_action :configure_permitted_parameters, if: :devise_controller?

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:username])
  end
end
```
- （3）完成 图片 栏位的使用；gem paperclip（这个图片上传的 gem 需要使用最新的上传功能）
```
app/views/posts/_form.html.haml
---
= simple_form_for @post do |f|
	= f.input :image
	= f.input :title
	= f.input :link
	= f.input :description

	= f.button :submit
---
app/views/posts/show.html.haml
---
= image_tag @post.image.url(:medium)
%h1= @post.title
%p= @post.link
%p= @post.description
%p= @post.user.name


= link_to "home", root_path
= link_to "edit", edit_post_path(@post)
= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }
---
app/models/post.rb
---
class Post < ApplicationRecord
  belongs_to :user
  has_attached_file :image, styles: { medium: "700x500>", thumb: "350x250>" }
  validates_attachment_content_type :image, content_type: /\Aimage\/.*\z/
end
---
app/views/posts/index.html.haml
---
- @posts.each do |post|
  = link_to (image_tag post.image.url), post
  %h2= link_to post.title, post

  = link_to "add posts", new_post_path
---
```
![image](https://ws4.sinaimg.cn/large/006tKfTcly1fpqalvwfuij30ng0dmab0.jpg)
![image](https://ws4.sinaimg.cn/large/006tKfTcly1fpqam397hjj30uq0rmtlo.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcly1fpqaucz05ij30xo0wadsz.jpg)

```
git status
git add .
git commit -m "add paperclip for image uploadomg"
git push origin paperclip


- （4）完成 用户 评论的对接；
- （5）完成 页面 美化的调试；

```
# 完成 用户 评论的对接
```
git checkout -b comment
rails g model comment content:text post:references user:references
rake db:migrate
---
app/models/post.rb
---
class Post < ApplicationRecord
  belongs_to :user
  has_many :comments
  has_attached_file :image, styles: { medium: "700x500>", thumb: "350x250>" }
  validates_attachment_content_type :image, content_type: /\Aimage\/.*\z/
end
---
app/models/user.rb
---
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable
  has_many :posts
  has_many :comments
end
---
config/routes.rb
---
Rails.application.routes.draw do
  devise_for :views
  devise_for :users
  resources :posts do
    resources :comments
  end
  root 'posts#index'
end
---
```
![image](https://ws3.sinaimg.cn/large/006tKfTcly1fpqbd1rwsnj31kw0uiqf1.jpg)

```
rails g controller comments
---
app/controllers/comments_controller.rb
---
class CommentsController < ApplicationController
 before_action :authenticate_user!

 def create
   @post = Post.find(params[:post_id])
   @comment = Comment.create(params[:comment].permit(:content)
   @comment.user_id = current_user.id
   @comment.post_id = @post.id

   if @comment.save
     redirect_to post_path(@post)
   else
     render 'new'
  end
 end
end
---
app/views/comments/_form.html.haml
---
= simple_form_for([@post, @post.comments.build]) do |f|
	= f.input :content, label: "Reply to thread"
	= f.button :submit, class: "button"
---
app/views/posts/show.html.haml
---
= image_tag @post.image.url(:medium)
%h1= @post.title
%p= @post.link
%p= @post.description
%p= @post.user.name

#comments
	%h2.comment_count= pluralize(@post.comments.count, "Comment")
	- @comments.each do |comment|
		.comment
			%p.username= comment.user.name
			%p.content= comment.content

	= render "comments/form"

= link_to "home", root_path
= link_to "edit", edit_post_path(@post)
= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }
---
```
![image](https://ws2.sinaimg.cn/large/006tKfTcly1fpqc2rmwbgj30uq0v6wrj.jpg)

```
app/views/posts/index.html.haml
---
- @posts.each do |post|
  = link_to (image_tag post.image.url), post
  %h2= link_to post.title, post
  %p
  = post.comments.count
  Comments

%h2= link_to "add posts", new_post_path, class: "button"
---
```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpqckld8rtj30to0xitlv.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpqck30kksj30um0ymank.jpg)
```
git status
git add .
git commit -m "add comment to posts"
git push origin comment
```
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpqcngnzikj31a80uudnb.jpg)
```
git checkout -b acts_as_votable
https://rubygems.org/gems/acts_as_votable
gem 'acts_as_votable', '~> 0.11.1'
bundle install
rails generate acts_as_votable:migration
rake db:migrate
---
app/models/post.rb
---
class Post < ApplicationRecord
  acts_as_votable
  belongs_to :user
  has_many :comments
  has_attached_file :image, styles: { medium: "700x500>", thumb: "350x250>" }
  validates_attachment_content_type :image, content_type: /\Aimage\/.*\z/
end
---
config/routes.rb
---
Rails.application.routes.draw do
  devise_for :views
  devise_for :users
  resources :posts do
    member do
    put 'like', to: "posts#upvote"
    put 'dislike', to: "posts#downvote"
    end
    resources :comments
  end
  root 'posts#index'
end
---
app/controllers/posts_controller.rb
---
class PostsController < ApplicationController
  before_action :find_post, only: [:show, :edit, :update, :destroy, :upvote, :downvote]
  before_action :authenticate_user!, except: [:index, :show]
  def index
    @posts = Post.all.order("created_at DESC")
  end

  def show
    @comments = Comment.where(post_id: @post)
  end

  def new
   @post = current_user.posts.build
  end

  def create
    @post = current_user.posts.build(post_params)
    if @post.save
    redirect_to @post
    else
    render 'new'
  end
end

  def edit
  end

  def update
    if @post.update(post_params)
      redirect_to @post
    else
      render 'edit'
  end
end

  def destroy
    @post.destroy
    redirect_to root_path
  end

  def upvote
  @post.upvote_by current_user
  redirect_back fallback_location: root_path
end

def downvote
  @post.downvote_from current_user
  redirect_back fallback_location: root_path
end

  private

  def find_post
    @post = Post.find(params[:id])
  end

  def post_params
    params.require(:post).permit(:title, :link, :description, :image)
  end
end
---
app/views/posts/index.html.haml
---
- @posts.each do |post|
  = link_to (image_tag post.image.url), post
  %h2= link_to post.title, post
  %p
  = post.comments.count
  Comments
  %p
    = post.get_likes.size
    Likes

%h2= link_to "add posts", new_post_path, class: "button"
---
app/views/posts/show.html.haml
---
= image_tag @post.image.url(:medium)
%h1= @post.title
%p= @post.link
%p= @post.description
%p= @post.user.name
%p= @post.get_upvotes.size
%p= @post.get_downvotes.size
= link_to "like", like_post_path(@post), method: :get
= link_to "dislike", dislike_post_path(@post), method: :get



#comments
	%h2.comment_count= pluralize(@post.comments.count, "Comment")
	- @comments.each do |comment|
		.comment
			%p.username= comment.user.name
			%p.content= comment.content

	= render "comments/form"

= link_to "home", root_path
= link_to "edit", edit_post_path(@post)
= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }
---
```
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpqe5rvujcj30tm0pan94.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpqe5gbqddj30z00w410n.jpg)

```
git status
git add .
git commit -m "add index show & setup voting"
git push origin acts_as_votable
```
![image](https://ws4.sinaimg.cn/large/006tKfTcgy1fpqe9yxgraj31c20usdni.jpg)
```
git checkout -b layouts-scss
app/views/layouts/application.html.haml
---
!!!
%html
%head
	%title Muse
	= stylesheet_link_tag    'application', media: 'all'
	= javascript_include_tag 'application'
	%link{:rel => "stylesheet", :href => "http://cdnjs.cloudflare.com/ajax/libs/normalize/3.0.1/normalize.min.css"}
	%link{:rel => "stylesheet", :href => "http://maxcdn.bootstrapcdn.com/font-awesome/4.2.0/css/font-awesome.min.css"}
	= csrf_meta_tags
%body
	%header
		.wrapper.clearfix
			#logo= link_to "Muse", root_path
			%nav
				- if user_signed_in?
					= link_to "current_user.name", edit_user_registration_path, class: "button"
					= link_to "Add New Inspiration", new_post_path, class: "button"
				- else
					= link_to "Sign in", new_user_session_path
					= link_to "Sign Up", new_user_registration_path, class: "button"
	%p.notice= notice
	%p.alert= alert
	.wrapper
		= yield
---
app/assets/stylesheets/application.css.scss
---
/*
 *= require_self
 */

body {
	font-family: "Proxima Nova";
	font-weight: 100;
}

h1, h2, h3, h4, h5, h6 {
	font-weight: 100;
}

.wrapper {
	width: 80%;
	margin: 0 auto;
}

.clearfix:before, .clearfix:after {
	content: " ";
	display: table;
}

.clearfix:after {
	clear: both;
}

.button {
	border: 1px solid #9B9B9B;
	padding: .7rem 1.25rem;
	border-radius: .3rem;
	outline: none;
	background: white;
	color: #9B9B9B;
}

header {
	width: 100%;
	padding: 2rem 0;
	border-bottom: 1px solid #E4E4E4;
	#logo {
		float: left;
		font-size: 1.75rem;
		a {
			color: #333233;
			text-decoration: none;
			&:hover {
				color: #50A7C7;
			}
		}
	}
	nav {
		float: right;
		a {
			margin-left: 1.5rem;
			line-height: 2;
			color: #9B9B9B;
			text-decoration: none;
			&:hover {
				color: #50A7C7;
			}
		}
	}
}

@import 'home.css.scss';
@import 'post.css.scss';
app/assets/stylesheets/home.css.scss
---

#intro {
	margin: 2.75rem 0 1.75rem 0;
	text-align: center;
	font-size: 1.75rem;
	line-height: 1;
	span {
		font-size: 1.25rem;
		color: #9B9B9B;
	}
}
#posts {
	.post {
		background: #F8F9FA;
		width: 30%;
		float: left;
		margin: 1rem 1.5%;
		border: 1px solid #E4E4E4;
		border-radius: 0.3rem;
		.post_image {
			height: 200px;
			overflow: hidden;
			img {
				width: 100%;
				border-radius: .3rem .3rem 0 0;
			}
		}
		.post_content {
			h2 {
				margin: 0;
				font-weight: 100;
				padding: 1rem 5%;
				border-bottom: 1px solid #E4E4E4;
				font-size: 1.25rem;
				a {
					text-decoration: none;
					color: #333233;
					&:hover {
						color: #50A7C7;
					}
				}
			}
			.data {
				padding: .75rem 5%;
				color: #969696;
				.username, .buttons {
					margin: 0;
					font-size: .8rem;
				}
				.username {
					float: left;
				}
				.buttons {
					float: right;
					span {
						margin-left: .5rem;
					}
				}
			}
		}
	}
}
---
app/assets/stylesheets/post.css.scss
---
#post_show {
	padding-top: 2rem;
	.post_image_description {
		width: 50%;
		float: left;
		margin-right: 5%;
	}
	.post_data {
		width: 15%;
		float: left;
		.button {
			width: 100%;
			display: inline-block;
			padding: .75rem 0;
			margin-bottom: 1rem;
			text-align: center;
			text-decoration: none;
			color: #969696;
			&:hover {
				color: #50A7C7;
				border: 1px solid #50A7C7;
			}
		}
		.data {
			width: 100%;
			display: block;
			padding: .75rem 0;
			border-bottom: 1px solid #E9E9E9;
			text-decoration: none;
			color: #969696;
			margin: 0;
		}
	}
	#random_post {
		width: 25%;
		margin-left: 5%;
		margin-top: -3.2rem;
		float: left;
		h3 {
			font-size: 1rem;
		}
		.post {
			background: #F8F9FA;
			border: 1px solid #E4E4E4;
			border-radius: 0.3rem;
			.post_image {
				height: 200px;
				overflow: hidden;
				img {
					width: 100%;
					border-radius: .3rem .3rem 0 0;
				}
			}
			.post_content {
				h2 {
					margin: 0;
					font-weight: 100;
					padding: 1rem 5%;
					border-bottom: 1px solid #E4E4E4;
					font-size: 1.25rem;
					a {
						text-decoration: none;
						color: #333233;
						&:hover {
							color: #50A7C7;
						}
					}
				}
				.data {
					padding: .75rem 5%;
					color: #969696;
					.username, .buttons {
						margin: 0;
						font-size: .8rem;
					}
					.username {
						float: left;
					}
					.buttons {
						float: right;
						span {
							margin-left: .5rem;
						}
					}
				}
			}
		}
	}
	h1 {
		margin: 0;
		color: #333233;
	}
	img {
		width: 100%;
		border-radius: .3rem;
	}
	.username {
		color: #969696;
		margin: 0 0 1.5rem 0;
	}
	.description {
		margin: 2rem 0;
		font-size: 1.1rem;
		line-height: 1.5;
		color: #969696;
	}
}

#comments {
	padding-bottom: 4rem;
	width: 50%;
	.comment_count {
		border-bottom: 1px solid #E9E9E9;
		padding-bottom: .5rem;
		margin-bottom: 0;
	}
	.comment {
		padding: 2rem 0;
		border-bottom: 1px solid #E9E9E9;
		padding-left: 5%;
		.username {
			font-size: 1.3rem;
			color: #333233;
			margin: 0 0 .5rem 0;
		}
		.content {
			margin: 0;
			color: #969696;
		}
	}
	.comment_content {
		margin-top: 2.5rem;
		textarea {
			width: 100%;
			margin: 1rem 0;
			min-height: 150px;
			border: 1px solid #E4E4E4;
			background: #F8F9FA;
			border-radius: 0.3rem;
		}
	}
}
```
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpqezmzawjj31k40zqtmm.jpg)
![image](https://ws1.sinaimg.cn/large/006tKfTcgy1fpqeyu51bjj31kw0xadop.jpg)
```
app/views/posts/index.html.haml
---
- if user_signed_in?
	%p#intro
		Hello
		= current_user.name
		%br/
		%span
			Share your inspiration and see what's inspiring others.
- else
	%p#intro
		What's your muse?
		%br/
		%span
			Share your inspiration and see what's inspiring others.
#posts
	- @posts.each do |post|
		.post
			.post_image
				= link_to (image_tag post.image.url), post
			.post_content
				.title
					%h2= link_to post.title, post
				.data.clearfix
					%p.username
						Shared by
						= post.user.name
					%p.buttons
						%span
							%i.fa.fa-comments-o
							= post.comments.count
						%span
							%i.fa.fa-thumbs-o-up
							= post.get_likes.size
---
app/views/posts/show.html.haml
---
= image_tag @post.image.url(:medium)
%h1= @post.title
%p= @post.link
%p= @post.description
%p= @post.user.name
%p= @post.get_upvotes.size
%p= @post.get_downvotes.size
= link_to "like", like_post_path(@post), method: :get
= link_to "dislike", dislike_post_path(@post), method: :get



#comments
	%h2.comment_count= pluralize(@post.comments.count, "Comment")
	- @comments.each do |comment|
		.comment
			%p.username= comment.user.name
			%p.content= comment.content

	= render "comments/form"

= link_to "home", root_path
= link_to "edit", edit_post_path(@post)
= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }
---
# 改变
---
#post_show
	%h1= @post.title
	%p.username
		Shared by
		= @post.user.name
		about
		= time_ago_in_words(@post.created_at)
	.clearfix
		.post_image_description
			= image_tag @post.image.url(:medium)
			.description= simple_format(@post.description)
		.post_data
			= link_to "Visit Link", @post.link, class: "button"
			= link_to like_post_path(@post), method: :get, class: "data" do
				%i.fa.fa-thumbs-o-up
				= pluralize(@post.get_upvotes.size, "Like")
			= link_to dislike_post_path(@post), method: :get, class: "data" do
				%i.fa.fa-thumbs-o-down
				= pluralize(@post.get_downvotes.size, "Dislike")
			%p.data
				%i.fa.fa-comments-o
				= pluralize(@post.comments.count, "Comment")
			- if @post.user == current_user
				= link_to "Edit", edit_post_path(@post), class: "data"
				= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }, class: "data"
		#random_post
			%h3 Random Inspiration
			.post
				.post_image
					= link_to (image_tag @random_post.image.url(:medium)), post_path(@random_post)
				.post_content
					.title
						%h2= link_to @random_post.title, post_path(@random_post)
					.data.clearfix
						%p.username
							Shared by
							= @random_post.user.name
						%p.buttons
							%span
								%i.fa.fa-comments-o
								= @random_post.comments.count
							%span
								%i.fa.fa-thumbs-o-up
								= @random_post.get_likes.size

#comments
	%h2.comment_count= pluralize(@post.comments.count, "Comment")
	- @comments.each do |comment|
		.comment
			%p.username= comment.user.name
			%p.content= comment.content

	= render "comments/form"
---
app/controllers/posts_controller.rb
---
def show
  @comments = Comment.where(post_id: @post)
  @random_post = Post.where.not(id: @post).order("RANDOM()").first
end
---
app/views/posts/show.html.haml
---
#post_show
	%h1= @post.title
	%p.username
		Shared by
		= @post.user.name
		about
		= time_ago_in_words(@post.created_at)
	.clearfix
		.post_image_description
			= image_tag @post.image.url(:medium)
			.description= simple_format(@post.description)
		.post_data
			= link_to "Visit Link", @post.link, class: "button"
			= link_to like_post_path(@post), method: :get, class: "data" do
				%i.fa.fa-thumbs-o-up
				= pluralize(@post.get_upvotes.size, "Like")
			= link_to dislike_post_path(@post), method: :get, class: "data" do
				%i.fa.fa-thumbs-o-down
				= pluralize(@post.get_downvotes.size, "Dislike")
			%p.data
				%i.fa.fa-comments-o
				= pluralize(@post.comments.count, "Comment")
			- if @post.user == current_user
				= link_to "Edit", edit_post_path(@post), class: "data"
				= link_to "Delete", post_path(@post), method: :delete, data: { confirm: "Are you sure?" }, class: "data"
		#random_post
			%h3 Random Inspiration
			.post
				.post_image
					= link_to (image_tag @random_post.image.url(:medium)), post_path(@random_post)
				.post_content
					.title
						%h2= link_to @random_post.title, post_path(@random_post)
					.data.clearfix
						%p.username
							Shared by
							= @random_post.user.name
						%p.buttons
							%span
								%i.fa.fa-comments-o
								= @random_post.comments.count
							%span
								%i.fa.fa-thumbs-o-up
								= @random_post.get_likes.size

#comments
	%h2.comment_count= pluralize(@post.comments.count, "Comment")
	- @comments.each do |comment|
		.comment
			%p.username= comment.user.name
			%p.content= comment.content

	= render "comments/form"
---
```

```
git status
git add .
git commit -m "edit show & index post"
git push origin layouts-scss
```

# 最后效果图

![image](https://ws2.sinaimg.cn/large/006tKfTcgy1fpqfmy3n6qj31kw0krn6w.jpg)
![image](https://ws3.sinaimg.cn/large/006tKfTcgy1fpqfmi63jnj31kw0undxx.jpg)
