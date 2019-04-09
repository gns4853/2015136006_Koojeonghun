# boardz
게시판 검색 기능 완성하기

## 기존 파일
```
 .
├── css
│   └── style.css
├── src
│   └── boardz.css
├── board.html
```

## 추가 및 수정된 파일
```
 .
├── css
│   └── style.css
├── src
│   └── boardz.css
├── board.php (수정)
[만약 추가한 파일이 있으면, 내용 추가! 없으면 이 문구 삭제!]
```

## board.php (수정)
```
[내용 추가!!]
<?
// MySQL 데이터베이스 연결
$connect = mysql_connect("localhost", "kjh", "1234");
// DB 선택
mysql_select_db("kjh_db", $connect);
// sql 쿼리 string 생성
$sql = "select * from boardz where title like '%$_POST[search]%'";
// sql 쿼리 실행
$result = mysql_query($sql);
$num = mysql_num_rows($result);
$row = mysql_fetch_array($result);
?>
```
## board.php (main 수정)
```
먼저 그림의 전체 쿼리 갯수를 7이라고 한다. 그 때 num으로 7을 받았을 때,
아무것도 입력이 안되어 있는 경우를 뜻한다. 그 경우 전체 사진을 출력할 수 있도록 설정.
이와 관련된 코드
if($num==7) {
                echo '
            <!-- Example Boardz element. -->
            <div class="boardz centered-block beautiful">
                <ul>
                    <li>
                        <h1></h1>
                        <img src="http://2.bp.blogspot.com/-pINYV0WlFyA/VUK-QcGbU5I/AAAAAAAABcU/fNy2pd2cFRk/s1600/WEB-Jack-White-Poster-Creative.png" alt="demo image"/>
                    </li>

                    <li>
                        <img src="http://payload140.cargocollective.com/1/10/349041/5110553/Florrie.jpg" alt="demo image"/>
                    </li>
                </ul>
                <ul>
                    <li>
                        <h1></h1>
                        <img src="http://wpmedia.ottawacitizen.com/2015/11/01.jpg?quality=55&strip=all&w=840&h=630&crop=1" alt="demo image"/>
                    </li>
                    <li>
                        <h1></h1>
                        <img src="https://s-media-cache-ak0.pinimg.com/736x/8c/ee/ff/8ceeff967c03c7cf4f86391dd6366544.jpg" alt="demo image"/>
                    </li>

                </ul>
                <ul>
                    <li>
                        <h1></h1>
                        <img src="https://s-media-cache-ak0.pinimg.com/originals/87/16/8c/87168cbbf07cb54a9793bebaa20b1bde.jpg" alt="demo image"/>
                    </li>
                    <li>
                        <h1></h1>
            Ex nostrud verterem mea, duo no delicata neglegentur. Audire integre rationibus ut pri, ex cibo oblique euismod sit, cibo iracundia vix at. Legimus torquatos definiebas an nec, mazim postulant at sit. Ne qui quando vocent accusata, nam tritani fierent no. Ea per vocent voluptatibus.

                        <br />

                        <img src="https://s-media-cache-ak0.pinimg.com/736x/22/95/48/229548086245c332443109ca9f2e0890.jpg" alt="demo image"/>

                    </li>
                    <li>
                        <h1></h1>

                        <br />

                        <img src="https://inspirationfeeed.files.wordpress.com/2014/01/ca402f7410884454ec5c303336e8591d1.jpg" alt="demo image"/>
                    </li>
                </ul>
그리고 title의 값에 따라 사진을 출력할 때, 
 (num > 0) 1번째 자리 사진 활성화
 (num > 1) 1번째 2번째 자리 사진 활성화
 (num > 2) 1번째 2번째 3번째 자리 사진 활성화
 
 그리고 그 자리에 3으로 나눈 경우 
 나머지가 1일때 1,4,7...사진
 2일때 2,5,8....사진
 0일때 3,6,9....사진이 첨부된다.
 
 이와 관련된 코드
    if($num>0)
                 {
                     echo '<ul>';
                     for($i=0;$i<$num;$i++) {
                         if($i%3==0) {
                             echo '<li>';
                             echo '<h1>'; $data = mysql_result($result, $i, title); echo $data; echo '</h1>';
                             echo '<img src="';$data = mysql_result($result, $i, image_url); echo $data; echo '" alt="demo image"/>';
                             echo '</li>';
                         }
                     }
                     echo '</ul>';
                 }
 
                 if($num>1)
                 {
                     echo '<ul>';
                     for($i=0;$i<$num;$i++) {
                         if($i%3==1) {
                             echo '<li>';
                             echo '<h1>'; $data = mysql_result($result, $i, title); echo $data; echo '</h1>';
                             echo '<img src="';$data = mysql_result($result, $i, image_url); echo $data; echo '" alt="demo image"/>';
                             echo '</li>';
                         }
                     }
                     echo '</ul>';
                 }
 
                 if($num>2)
                 {
                     echo '<ul>';
                     for($i=0;$i<$num;$i++) {
                         if($i%3==2) {
                             echo '<li>';
                             echo '<h1>'; $data = mysql_result($result, $i, title); echo $data; echo '</h1>';
                             echo '<img src="';$data = mysql_result($result, $i, image_url); echo $data; echo '" alt="demo image"/>';
                             echo '</li>';
                         }
                     }
                     echo '</ul>';
                 }
             }
```

