# Static 

: 개발자가 서버를 개발할 때 미리 넣어놓은 정적파일(img,js,css)

실습)

STATIC_URL = '/static/'

STATIFILES_DIRS = [

  os.path.join(BASE_DIR, 'blog', 'static')]

\# 현재 static 파일들이 어디에 있는지

STATIC_ROOT = os.path.join(BASE_DIR, 'static')

\# static 파일을 어디에 모을건지



# MEDIA

: 사용자가 업로드 할 수 있는 파일

실습)

- settings.py

MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

\# 이용자가 업로드한 파일을 모으는 곳

MEDIA_URL = '/media/'

- urls.py

urlpatterns = [

] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

+ models.py

class Blog(models.Model):

  image = models.ImageField(upload_to = "blog/", blank=True, null=True) 

*upload_to 는 업로드할 폴더를 지정하는 것. 

  settings. py에 MEDIA_URL로 지정해둔 media 폴더 안에 blog 폴더를 만들어서 관리하겠다는 설정임. 



