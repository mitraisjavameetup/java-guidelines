# Description #
[Thymeleaf](http://www.thymeleaf.org/) is a java template engine that support for natural templating. Natural templating means that web designer can design the web application pages using HTML, CSS and JS naturally without need to know about the back end at all. So the designer can can focus in developing UI and UX using front-end technology (HTML, CSS and JS).  

# Sample code project #
Sample code project can be found [here](https://github.com/erikfarhanmalik/thymeleaf-natural-templating)

# Study Case #
Create a simple employee application that consist only 2 page, first page will be a home page and the second page will contain employee list and also contain ajax interaction.

# Disclaimer #
The main focus of this guide is the natural templating with thymeleaf in Spring. So the discussion of how to set up thymeleaf in spring won't be covered in this guide, refer to [this post](https://www.mkyong.com/spring-boot/spring-boot-hello-world-example-thymeleaf/) if needed.  

# Terms #
These term is created to minimize confusion in this guide:
- `Natural template` term in this guide is referring to an HTML file that is natural HTML file created by web designer, it's located in `\src\main\resources\templates\natural` 
- `Thymeleaf template` term in this guide is referring to an HTML file that will be used by Thymeleaf Template Engine, it's located in `\src\main\resources\templates`


# Code structure #
The thymeleaf resource location and structure in this guide sample code are defined like this:    
\src\main\resources:  
![alt text](https://raw.githubusercontent.com/erikfarhanmalik/thymeleaf-natural-templating/master/screenshot/file-structure.JPG)

- `static` folder will be used for resources such as css and js files.
- `templates` folder will be used for thymeleaf template root location folder.
- `templates/natural` folder will be used for natural template root location folder.
- `templates/natural/ajax-contract` folder will be used to store json file that contain data as that can be used as mock response for ajax request in natural templating, and will become reference for back-end to give response at particular end-point that involve in ajax process.

## Front end part ##
- The front-end developer (or web designer) should develop the application UI and UX in form of HTML, CSS and JS. And place the developed code files in the location described in the code structure part. 

- The designer should develop the UI and UX naturally using web front-end technology (HTML, CSS and JS), sample for this guide natural template are: [index.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/fisrt-stage/src/main/resources/templates/natural/index.html), [list-page.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/fisrt-stage/src/main/resources/templates/natural/list-page.html) (both files are plain HTML with theirs needed resources. The designer can open those file by `right click and open with a browser`, and developed the needed pages that way)

- To simulate page navigation, designer should use relative path as shown in [index.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/fisrt-stage/src/main/resources/templates/natural/index.html)

```html
<a class="waves-effect waves-light btn-large" href="./list-page.html">
	...
</a>
```
 
- To access self defined resources, use a relative link to `resources/static` folder as shown in [index.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/fisrt-stage/src/main/resources/templates/natural/index.html)

```html
<link rel="stylesheet" href="../../static/css/self-defined.css" />
<script type="text/javascript" src="../../static/js/self-defined.js" />
```

- If designer need to use vendor provided resources, use the online version of those resources for easiness, like in [index.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/fisrt-stage/src/main/resources/templates/natural/index.html)

```html
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/0.100.2/css/materialize.min.css" />
<script type="text/javascript" src="https://code.jquery.com/jquery-3.2.1.js"></script>
```

- For integration with back-end easiness, designer should create a consistent layout (a page is divided into several sections and these section division should be consistent).   

- For features that depend on ajax request response, like for example in [list-page.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/master/src/main/resources/templates/natural/list-page.html):  

```javascript
<script type="text/javascript">
	function doSomeAjaxCall() {
			$.ajax({method : "GET",
					url : "http://localhost:9000/api/employee"})
			.done(function(response) {
					$("#ajax-response").html(JSON.stringify(response));
				});
	}
</script>
```
for this kind of case designer should create a mock server that can handle the ajax request and gives the needed response. The needed response in form of json files should be placed in `templates/natural/ajax-contract`, and designer can use [this tool](https://github.com/erikfarhanmalik/rest-api-dummy) (or similar) to expose those json files as dummy response for certain API. Json files in this folder will be use as reference for back-end to create end-points responses.

## Back end part ##
- The back-end developer should create Thymeleaf template in `templates` folder. It's better to give the template same name with the natural template, for example create `templates\index.html` for `templates\natural\index.html`.

- For the created thymeleaf template in the first step.

- Print the data that is needed by the page just to make sure the needed data is already provided when the page is loaded, like in [index.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/fisrt-stage/src/main/resources/templates/index.html), or [list-page.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/fisrt-stage/src/main/resources/templates/list-page.html) (this step only needed if the back-end developer have created the thymeleaf template but the designer haven't created the natural template yet, ex. back-end developer have created `templates\index.html` but the designer haven't created `templates\natural\index.html` yet).

- The back-end developer then should read the natural template (ie. `templates\natural\index.html`) and modify it by adding needed thymeleaf attributes or elements. For example, like in [list-page.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/master/src/main/resources/templates/natural/list-page.html):

```html
change from: <title>RMS - Table</title>
to: <title th:fragment="title">RMS - Table</title>
...
change from: <span>Friski Y Pratama</span>
to: <span th:text="${user.name}">Friski Y Pratama</span>
...
change from: <span class="title">(Admin)</span>
to: <span class="title" th:if="${user.role eq 'Admin'}">(Admin)</span>
```

- The back-end developer should also add thymeleaf fragment attribute to mark a component to be included in the thymeleaf template, for example in [list-page.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/master/src/main/resources/templates/natural/list-page.html):

```html
change from: <div class="navbar-fixed">
to: <div class="navbar-fixed" th:fragment="navigation-bar">
...
change from: <div class="content">
to: <div class="content" th:fragment="content">
```

- The back-end developer then open the thymeleaf template (ie. `templates\index.html`) then bundle all resources needed by the page using dandelion then get and arrange all the elements in the template. See [index.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/master/src/main/resources/templates/index.html) for example.

- For static section such as header, navigation bar, side menu, footer, etc. Always use the one from `templates/natural/index.html` and load only for the dynamic content from other pages. For example in [list-page.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/master/src/main/resources/templates/list-page.html) the navigation bar still load from `templates/natural/index.html` (`<div th:replace="natural/index::navigation-bar"></div>`) but load from `templates/natural/list-page.html` for the content, title, and in line script.

- For features that involve ajax interaction, add real end point like in [list-page.html](https://github.com/erikfarhanmalik/thymeleaf-natural-templating/blob/master/src/main/resources/templates/natural/list-page.html):  

```javascript
function doSomeAjaxCall() {
	$.ajax({method : "GET",
			url : /*[[@{/api/employee/2}]]*/"http://localhost:9000/api/employee"})
	.done(function(response) {
		$("#ajax-response").html(JSON.stringify(response));
	});
}
```

and the real end point should give the response in json form, and the json structure should follow the response which specified by the dummy api in `templates/natural/ajax-contract`.
