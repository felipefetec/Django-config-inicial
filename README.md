Configurações iniciais

- Ambiente Virtual
    
    Criar a venv
    
    ```bash
    ## Windows
    python -m venv .venv
    source .venv/Scripts/activate # Ativar ambiente
    
    ## Linux 
    ## Caso não tenha virtualenv. "pip install virtualenv"
    virtualenv .venv
    source .venv/bin/activate # Ativar ambiente
    ```
    
    Instalar os Pacotes
    
    ```bash
    pip install django
    pip install pillow
    ```
    
    Criar o arquivo requirements.txt
    
    ```bash
    pip freeze > requirements.txt
    ```
    
- Criando o Projeto
    
    “core” é nome do seu projeto e quando colocamos um “.” depois do nome do projeto significa que é para criar os arquivos na raiz da pasta. Assim não cria subpasta do projeto.
    
    ```bash
    django-admin startproject core .
    ```
    
    Testar se tudo ocorreu bem
    
    ```bash
    python manage.py runserver
    ```
    
- Configurar settings e arquivos static
    
    Arquivo ***settings.py***
    
    ```python
    import os
    
    # base_dir config
    BASE_DIR = Path(__file__).resolve().parent.parent
    TEMPLATE_DIR = os.path.join(BASE_DIR,'templates')
    STATIC_DIR=os.path.join(BASE_DIR,'static')
    
    STATIC_ROOT = os.path.join(BASE_DIR,'static')
    STATIC_URL = '/static/' 
    
    MEDIA_ROOT=os.path.join(BASE_DIR,'media')
    MEDIA_URL = '/media/'
    
    # Internationalization
    # Se quiser deixar em PT BR
    LANGUAGE_CODE = 'pt-br'
    TIME_ZONE = 'America/Sao_Paulo'
    USE_I18N = True
    USE_L10N = True 
    USE_TZ = True
    ```
    
    Arquivo ***core/urls.py***
    
    ```python
    from django.contrib import admin
    from django.conf import settings
    from django.conf.urls.static import static
    from django.urls import path
    
    urlpatterns = [
        path('admin/', admin.site.urls),
    ]
    
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) # Adicionar Isto
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # Adicionar Isto
    ```
    
- Criando o aplicativo
    
    O Django divide o aplicativo em micro apps para melhor gerenciar 
    
    Crie os modulos da aplicação substituindo myapp pelo nome do aplicativo
    
    Agora precisamos registrar nossa aplicação no *INSTALLED_APPS* localizado em *settings.py*.
    
    ```python
    python manage.py startapp myapp
    ```
    
- Criando template base
    
    ### Configuração do Bootstrap
    
    Doc: https://getbootstrap.com/docs/5.2/getting-started/introduction/
    
    Com Base na documentação para utilizar os recursos Boostrap basta adicionar as tags de CSS e JS. No HTML da Pagina Base.
    
    ```html
    <!-- CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
    
    <!-- JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
    ```
    
    ### Template Base
    
    criar um arquivo base ***base.html*** onde vamos renderizar nosso conteúdo.
    
    ```html
    {% load static %}
    <!DOCTYPE html>
    <html lang="pt-br">
    <head>
    	<meta charset="UTF-8">
    	<meta http-equiv="X-UA-Compatible" content="IE=edge">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>{% block title %}{% endblock %}</title>
    
    	<!-- CSS -->
    	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65" crossorigin="anonymous">
    
    </head>
    <body>  
    	
    	{% block content %}
    	
    	{% endblock %} 
     
    	<!-- JS-->
    	<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-kenU1KFdBIe4zVF0s0G1M5b4hcpxyD9F7jL+jjXkk+Q2h455rYXK/7HAuoJl+0I4" crossorigin="anonymous"></script>
    </body>
    </html>
    ```
    
- Criar View
    
    ***index1.html***
    
    ```html
    {% extends 'base.html' %}
    {% block title %}Pagina Inicial{% endblock %}
    {% block content %}
    	<h1>Pagina Inicial</h1>
    {% endblock %}
    ```
    
    ***myapp/views.py***
    
    ```python
    from django.shortcuts import render
    
    # Create your views here.
    def mysite(request):
        return render(request, 'index1.html')
    ```
    
    criar o arquivo ***myapp/urls.py***
    
    ```python
    from django.urls import path 
    from myapp import views
    
    urlpatterns = [
        path('', views.mysite, name='mysite'), 
    ]
    ```
    
    Mudando o arquivo urls.py do projeto. ***core/urls.py***
    
    ```python
    from django.contrib import admin
    from django.urls import path, include # adicionar include
    from django.conf import settings
    from django.conf.urls.static import static 
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('myapp.urls')), # url do app
    ]
    urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT) # Adicionar Isto
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT) # Adicionar Isto
    ```
    
    Rode para ver se foi tudo corretamente
    
    ```python
    python manage.py makemigrations && python manage.py migrate
    python manage.py runserver
    ```
    
    .gitignore
    
    ```
    /tmp
    passenger_wsgi.py 
    .venv
    db.sqlite3
    /static_media
    static_media
    /static_files
    static_files
    /media
    mydatabase
    file_name.sql
    __pycache__ 
    __pycache__/
    ```
