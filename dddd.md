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