	2- Installed django with pip and other stuff
		a. Django-adming start djangonautic
		b. Python manage.py runserver
		
	3- Urls and views
		a. Add from django urls import path I also added from django.conf.urls import urls in that path('about/', views.about),
		b. In view.py from django.http import HttpResponse
		c. Def for hompage and about
	4- For html templates create a folder in the djangonautic app saying the templates create the html files needed there add those files in the views in their separate definitions by adding the from django.shortcuts import render
	And then in their definition return with render(request, 'homepage.html')
	
	Also in settings.py in Templates lists add the DIRS = ['templates']
	5- Modularized the whole project created an article project and had a namespaced in it's own templates, but still need to register that app in the djangonautic->settings.py
		a. Added the articles app's urls to main djangonautic app's urls.py by having from django.conf.urls import url, include------url(r'^articles/', include('articles.urls)), 
		b. Don't forget the coma and also to return the render(request,….) in the views.py of the articles app.
	6- Django Models 
		a. Class Article(models.Model) from django.db import models
		b. Title, slug, body, date -django documentation for the following -  CharField(max_length=100), SlugField(), TextField, DateTimeField(auto_now_add=True)
	7- Making migrations two commands
		a. Python manage.py makemigrations
		b. Python manage.py migrate
	8- Django ORM
		a. Python manage.py shell
			i. From articles.models import Article
			ii. Article.objects.all() shows all the available article as Article: object so to see all their titles we have to write a function in the models.py 
			iii. Def __str__(self): return self.title
			iv. Create another article2 = ArticleA()
			v. article2.title = "Django Rules"
			vi. article2.save()
			vii. Article.objects.all() will show all the articles saved in the database with their titles
	9- Django admin
		a. Created adming account by python manage.py createsuperuser then python manage.py runserver
		b. In admin.py write admin.site.register(Article) to see the article section in the admin portal
	10- Template Tags
		a. Views.py from . models import Article 
		b. articles = Article.object.all().order_by('date') - grabbing all the records from this Article db table
			i. We can order them by anything which is defined in the models.py file(we have done by date)
			ii. Adding the dictionary to the return render({ 'articles': article}), we can call it whatever we want and add that to the template that is going to be rendered
			iii. Add all of them like {% for article in articles %}{{article.title}} {{article.body}} {{article.date}}{% end for %}
			
		
	11- Model methods
		a. Added new Lorem Ipsum article
		b. And to show only 50 character of the body add function snippet with self.body[0:50] and to have … in the end just concatenate that string with the "…."
	12- Static Files & Images
		a. In urls.py,
			i. From django.contrib.staticfiles.urls import staticfiles_urlpatterns
			ii. Urlpatterns += staticfiles_urlpatterns()
		b. Settings.py
			i. STATICFILES_DIRS = (
				Os.path.join(BASE_DIR, 'assets'),
			)
		c. In the templates {% load static from static files %}
	13- Extending Templates
		a. {% block content %}{% endblock %}
		b. In different templates just write {% extends 'base_layout.html' %} or any other template that we want to extend from and to and add the html in it.
	14- URL Parameters and RegEx
		a. Urls.py add url(r'^(?P<slug>[\w-]+)/$', views.article_detail)
		b. Adding things in views.py the function to run it
			i. Article_detail(request, slug): return HttpResponse(slug)
	15- URL tags and names
		a. Name the urls in urls.py file with name= "list" "detail" also add the app_name = 'articles' its called name spacing so when we have more urls it doesn't conflict with other names
		b. In the html templates of the respective base_layout and article_list add in 
		<a href="{% url 'articles:detail' slug=article.slug %}">
		<a href="{% url 'articles:list' %}"><img src="{% static 'logo.jpg' %}"
	16- Article Detail Template
		a. In views.py
			def article_detail(request, slug):
			# return HttpResponse(slug)
			article = Article.objects.get(slug=slug)
			return render(request, 'articles/article_detail.html', {'article': article})
			
		b. And added article_detail.html to showcase each particular articles and adding all the article.detail, article.body, article.date with the block content that we are supposed to use.
	17- Uploading Media
		a. Should be able to find media at localhost:8000/media
		b. Settings.py
			i. MEDIA_URL = '/media/'
			ii. MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
		c. Settings.py 
	18- Accounts App
		a. To control signing up and thing "python manage.py startapp account" settings.py in apps add 'account'
		b. Urls.py "from django.conf.urls import url" and "from . Import views"
		c. App_name = 'account'
		d. Also add urlpatterns = [add the signup with RegEx] in django 2.0 it 
		would be paths
		e. Mainly add the account url in the djangonautics/urls.py
		f. Create the function for signup_view in the views.py
		g. Have the signup.html template which extends the base_layout.html
	19- User Creation Form
		a. In views.py from django.contrib.auth.forms import UserCreationForm
		b. And call its instance in the signup_view function
		c. And the render's 3rd property would be a dictionary of that instance 'form' I wrote 'arji' I guess
		d. Call that {{ form }} to output data in the template of signup.html
		e. It’s a post method request so we are sending get requeest and post request at the same url
		f. So in template add the <form action=" url 'account:signup'"> or one can hard code it by <form action="account/signup">
	20- Saving Users
		a. Add the in signup_view function
			i. If request.method = 'POST', if its post request else it’s a Get request do the need ful
			ii. Form= UserCreationForm(request.POST)
			iii. If form.is_valid To save it form.save() would suffice
		b. Next to step to log the user in #25 I guess
		c. And it returns the redirect to the main 'articles:list'
		d. Else give the UserCreationForm() again.
		e. All 3 scenario s
		f. Will give a csrf_token error so add it to the template {% csrf_token %}
	21- Login Form
		a. Create a login urls with views.login_view, name="login"
		b. A function for login_view(request), it would be a "get" request
		c. Same 3 scenarios as above
		d. Don't forget to import AuthenticationForm() from the django.contrib.auth.forms
		e. In the template of it <form action="{% url 'account:login' %}"> with the post method and csrf_token and output the form 
		f. If request.method == 'POST' form = AuthenticatedForm(data = request.POST) 'cause this is not naturally the first parameters of this function
	22- Logging Users In
		a. In the views.py from django.contrib.auth import login
		b. User = form.get_user <- if form.is_valid() <- login_view
		c. Login(request, user)
		d. In the signup_view user=form.save() so to login(request, user)
	23- Logging Users Out
		a. Create the Urls for it views.logout_view, name="logout"
		b. Create that template  and a function for it in the views file
		c. Best practice is to use "POST" request to log the user out and not a "GET" request such that you don't visit the url as /account/logout
		d. If reuqest.method == 'POST' simply logout(request) and redirect to articles:list
		e. Base_layout.html -> nav, ul, li tag created the Logout button <form calss = "logout-link" action="{% url 'account:logout' %}" method = 'post'> -> csrf_token
		f. And then the button for Logout
	24- Requiring Login
		a. Put this before the views.article_detail Articles->urls.py -> url(r'^create', views.artivles_create, name="create")
		b. Create the view for it article_create(request)
		c. And render(request, 'articles/article_create.html')
		d. Template extends base_layout.html 
		e. Views.py -> from django.contrib.auth.decorators import login_required 
			i. Add the decorator on top of the function '@login_required()'
				1) So it is protecting the view 
				2) So if they aren't logged in redirect them to login page {@login_required(login url="/accounts/login/")}
	25- Redirect after Login
		a. When logged out the url gets {?next=/articles/create} that's query string
		b. Template -> {% if request.GET.next %} <input type="hidden" name="next" value="{{request.GET.next}}"> {% endif %}
			i. Checking if the query parameters exist, when we then make a post req, this info request.GET.next info with come along with it, then we can access the info inside the post request of the login_view
		c. Login_view -> if 'next' in request.POST -> return redirect(request.POST.get('next')) to get the particular parameter from the post request 
		d. Else: redirect('articles:list')
		
	26- Model Forms
		a. Article-> form.py(newly created) from django import form also models
		b. Class CreateArticle(forms.ModelForm) inside that another class
			i. Class Meta:
				model = models.Article
				fields = ['title, 'body', 'slug', 'thumb']
		c. In article_create views we can use it diretly don't forget to import it 
		d. New instance of this form and send that to the browser so we can render it
			i. Form = forms.CreateArticle()
			ii. Return render add the dictionary { 'form': form }
		e. Article_create.html form method post output the csrf_token {{ form }}, create button too
			i. For thumbnails add enctype="multipart/form-data"
				1) This is for encoding the formdata that is uploaded 
		f. If request.method == 'POST'
			i. Add the data that we retrive , form = forms.CreateArticle(request.POST, request.FILES) it is validating the data against the createArticle form for the thumbnail files as they don't come along with the POST object they need their own objet 
		g. If form.is_valid() then redirect to 'articles:list'
		
		
		
	27- To associate articles with user/authors by Foreign Key
		a. Models.py-> models.ForeignKey(User, default=None) to use that we need to from django.contrib.auth.models import User
			i. So as we changed the models.py we have to migrate the changes.
			ii. Delete all the articles and and then migrate, so that you can create new ones with the authors related to it.
		b. So now to POST the article to database and save it 
			i. Views.py instance = form.save(commit=False)
				1) Only form.save will return with saving it into db but commit=False means hang on a bit we are gonna save this just yet and save to the variable
			ii. Instance.author = request.user
			iii. Instance.save()
	28- Checking Login status
		a. Base_layout.html
			i. {% if user.is_authenticated%} then show logout
			ii. Else show {% url 'account:login' %} & {% url 'account:signup' %}
			iii. Also add the create new article while logged in
	29- Redirecting the Homepage
		a. we want to show the articles_list page whenever we just hit the server 
			i. urls.py we would add from articles import views as article_views
			ii. and add in the url as articles_views.articles_list, name="home"
			iii. so now when clicking on the logo let's not go to /articles/ and instead head over to home.
				1) /templates/base_layout.html, {% url 'home' %}
	30- Styling the app
		a. as said manipulating the styles.css
		b. also it needs to show the author on the main page too
			./articles/templates/articles/article_list.html
				add author with {{ article.author.username }}
		c. also added the cool css to show author "I really liked that design"

	31- Editing CSS
		a. edited it for "Login" & "SignUp" site-form is the class
		b. styled the error messages and seen them pop up
			liked: Traversy media using other live templates is much better
			not: still need to scroll down for the whole page to see 
	32- Adding JS on front for live
		a. created ./assets/slugify.js 
		b. add it to the article_create.html
		c. js is hitting some errors now and not working
		d. used a particular set of RegEx so that our slug and URL set with dashes.
		
	Deployment on pythonanywhere.com following this tutorials https://www.youtube.com/watch?v=Y4c4ickks2A also do check out the wsgi file application, and try to adjust the working directory to the djangonautic itself.
		another main thing to add the static files with in the assets folder also changed thet settings.py for it.
