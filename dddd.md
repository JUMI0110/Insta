게시물 기능 -> 로그인 기능
Model
ImageField() -> Pillow 라이브러리 필요
upload_to=업로드할 파일
MEDIA_ROOT/uploads  설정하고싶은 url
# 업로드한 사진을 저장할 위치 (실제 폴더 경로)
MEDIA_ROOT = BASE_DIR / 'media'
~/Desktop/DMF/insta/media
# media url 인터넷을 통해 사진을 보기위해 접근할 수 있는 경로설정
MEDIA_URL = '/media/'
media 파일 gitignore 장고에서 기록하지 말라고하는 파일에 들어있음




상단 templates 안의 파일 기능단위로 나눠서 저장
_기능.html '_' 기능을 하는 파일
{% include '_nav.html' %}



이미지에 접근
장고 파일 관리하는 방법 
from django.conf.urls.static import static
static('/media/', 'BASE_DIR/ media') 사용자가 업로드한 파일 제공
static(경로요청, 사진의 실제위치)





{% block body %}
    <form action="" method="POST" enctype="multipart/form-data"> 
    encoding 타입 정보 바꿈 파일을 따로 FILES로 전송 
        {% csrf_token %}
        {{form}}
        <input type="submit">
    </form>
{% endblock %}
요청을 2가지 넣어줘야함 POST,FILES
def create(request):
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('posts:index')

    else:
        form = PostForm()

    context = {
        'form': form,
    }

    return render(request, 'form.html', context)


사진크기 일괄처리
Model
    image = ResizedImageField(
    size=[500, 500],
    crop=['middle', 'center'],
    upload_to='image/%Y/%m'
)

require_post()
from django.views.decorators.http import require_POST
@require_POST
http 405 Method Not Allowed 
GET요청인지 POST요청인지 확인 
request.method = 'POST' 와 같은 역할을 함
GET요청이 들어왔을 때 어떤 문제인지 확인 할 수 없기에 에러코드 보여주는 것이 좋음