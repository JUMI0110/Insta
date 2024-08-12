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

새로운 컬럼 추가 했을 때
migrations 에러 
It is impossible to add a non-nullable field 'post' to comment without specifying a default. This is because the database needs something to populate existing rows.
Please select a fix:
 1) Provide a one-off default now (will be set on all existing rows with a null value for this column) 새로운 컬럼 기본값
 2) Quit and manually define a default value in models.py. models.py 수정
Select an option:1 (1번 선택)

Type 'exit' to exit this prompt
>>>1 (1을 넣음)



좋아요 기능 추가

M : N 

ManyToManyField django의 편의기능 
게시물을 좋아하는 모든 사람들의 목록을 리스트로 조회가능
M : N 관계 설정 후 새로운 컬럼을 만들 때는 새로운 모델 생성해야함

Post모델
like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_posts')

ERRORS:
posts.Post.like_users: (fields.E304) Reverse accessor 'User.post_set' for 'posts.Post.like_users' clashes with reverse accessor for 'posts.Post.user'.
        HINT: Add or change a related_name argument to the definition for 'posts.Post.like_users' or 'posts.Post.user'.
posts.Post.user: (fields.E304) Reverse accessor 'User.post_set' for 'posts.Post.user' clashes with reverse accessor for 'posts.Post.like_users'.
        HINT: Add or change a related_name argument to the definition for 'posts.Post.user' or 'posts.Post.like_users'.

        Post모델과 User모델이 연결되어 User에 미리 만들어놓은 post_set이 있는데 또다른 post_set을 만들려고 시도 -> 충돌 -> related_name 수정


builtin filter tag
html 코드에서 파이썬 함수 사용하는 방법
{{value|파이썬 함수}}


팔로워 팔로우
좋아요와 다르게 to, from 관계
모델링
User 모델:
followings = models.ManyToMany('self', related_name='followers', symmetrical=False)
followings 내가 팔로우하는 사람
followers 나를 팔로우하는 사람
symetrical=False 대칭값을 주지않음(단방향 from-to)
symmertrical 값을 주지 않으면 related_name 필요없고 양방향임