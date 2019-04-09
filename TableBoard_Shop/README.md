# TableBoard_Shop
게시판-Shop 의 TODO 완성하기!

## 기존 파일
```
 .
├── css - board_form.php와 index.php 에서 사용하는 stylesheet
│   └── ...
├── fonts - 폰트
│   └── ...
├── images - 아이콘 이미지
│   └── ...
├── vender - 외부 라이브러리
│   └── ...
├── js - board_form.php와 index.php 에서 사용하는 javascript
│   └── ...
├── function
│   └── insert.php - 게시글 작성 기능 구현
│   └── update.php - 게시글 수정 기능 구현
│   └── delete.php - 게시글 삭제 기능 구현
├── board_form.php - 게시글 작성/수정 시 사용하는 form이 포함된 php 파일
├── index.php - 게시글 조회 기능 구현
```

## MySQL 테이블 생성!

[여기에 테이블 생성 시, 사용한 Query 를 작성하세요.]
```
Note: 
- table 이름은 tableboard_shop 으로 생성
- 기본키는 num 으로, 그 외의 속성은 board_form.php 의 input 태그 name 에 표시된 속성 이름으로 생성
- 각 속성의 type 은 자유롭게 설정 (단, 입력되는 값의 타입과 일치해야 함)
    - ex) price -> int
    - ex) name -> char or varchar
    create table tableboard_shop(
     num int not null auto_increment,
     date datetime,
     orderid char(200),
     name char(200),
     price int not null,
     quantity int not null,
     primary key(num)
    );
    으로 table 작성 추가,
 ```
## index.php 수정
```
[여기에 index.php 를 어떻게 수정했는지, 설명을 작성하세요.]
  $connect = mysql_connect("localhost", "kjh", "1234");
    // DB 선택
    mysql_select_db("kjh_db", $connect);
    // sql 쿼리 string 생성
    $sql = "select * from tableboard_shop order by num asc";
    // sql 쿼리 실행
    $result = mysql_query($sql);
    // 선택된 sql 갯수
    /** @var TYPE_NAME $numofrow */
    $numsofrow = mysql_num_rows($result);
    
    for($i=0;$i<$numsofrow;$i++) {
                            /** @var TYPE_NAME $row */
                            $row = mysql_fetch_row($result);
    echo '<tr onclick="location.href = (\'board_form.php?num='; echo $i+1;echo '\')">';
    echo '<td class="column1">'; echo $row[1]; echo '</td>';
    echo '<td class="column2">'; echo $row[2]; echo '</td>';
    echo '<td class="column3">'; echo $row[3]; echo '</td>';
    echo '<td class="column4">'; echo '$'; echo $row[4]; echo '</td>';
    echo '<td class="column5">'; echo $row[5]; echo '</td>';
    echo '<td class="column6">'; echo '$'; echo $row[4]*$row[5]; echo '</td>';
    echo '</tr>';
    }
    numsofrow에 result를 결과값을 받아 우리가 메인 홈페이지에 추가할,
    내역들을 for문을 통해 받으므
    마지막 echo는 row4,5의 곱이기 때문에 row배열을 새로 안받음. 
```
## board_form.php 수정
```
[여기에 board_form.php 를 어떻게 수정했는지, 설명을 작성하세요.]
#TODO: MySQL 테이블에서, num에 해당하는 레코드 가져오기
    $connect = mysql_connect("localhost", "kjh", "1234");
    // DB 선택
    mysql_select_db("kjh_db", $connect);
    // sql 쿼리 string 생성
    $sql = "select * from tableboard_shop where num = $_GET[num]";
    // sql 쿼리 실행
    $result = mysql_query($sql);
    $row = mysql_fetch_row($result);
    
     if(isset($_GET[num])) { //update 의 경우!
      ?>
      <td class="column1"> <input name="date" type="text" value="<? echo $row[1]; ?>" /> </td>
       <td class="column2"> <input name="order_id" type="number" value="<? echo $row[2]; ?>" /> </td>
       <td class="column3"> <input name="name" type="text" value="<?  echo $row[3]; ?>" /> </td>
      <td class="column4"> <input name="price" type="number" placeholder="$" style="text-align: right;" value="<? echo $row[4]; ?>" /> </td>
       <td class="column5"> <input name="quantity" type="number" value="<? echo $row[5]; ?>" style="text-align: right;" /> </td>
       <td class="column6"> $<span id="total"> <? echo $row[4]*$row[5]; ?> </span> </td>
      <?
      index와 마찬가지로 그 결과값을 GET[num]으로 받아서 똑같이 출력한다.
      이경우 아무것도 입력이 되있지 않을때, update하여 출력하는 경우로 GET[num]으 받는 값을 이용한다. 
      
```
## function
### insert.php 수정
```
[여기에 insert.php 를 어떻게 수정했는지, 설명을 작성하세요.]
$connect = mysql_connect("localhost", "kjh", "1234");
// DB 선택
mysql_select_db("kjh_db", $connect);
// sql 쿼리 string 생성
$sql = "insert into tableboard_shop(date,orderid, name, price, quantity) 
             values('$_POST[date]','$_POST[order_id]','$_POST[name]','$_POST[price]','$_POST[quantity]')";
// sql 쿼리 실행
$result = mysql_query($sql);
# 참고 : 에러 메시지 출력 방법
if(!$result) {
    echo "<script> alert('ERROR!!'); </script>";
}
$sql=insert into를 통해 valuse을 입력받아 추가.
```
### update.php 수정
```
[여기에 update.php 를 어떻게 수정했는지, 설명을 작성하세요.]
$connect = mysql_connect("localhost", "kjh", "1234");
// DB 선택
mysql_select_db("kjh_db", $connect);
// sql 쿼리 string 생성
$sql = "update tableboard_shop set date ='$_POST[date]',orderid ='$_POST[order_id]', name='$_POST[name]',price='$_POST[price]',quantity='$_POST[quantity]' where num = $_GET[num]";
// sql 쿼리 실행
$result = mysql_query($sql);
# 참고 : 에러 메시지 출력 방법
if(!$result) {
    echo "<script> alert('update - error message') </script>";
}
update를 시키면서 새 값을 입력받고 GET[num]을 통해 자리를 배정.
```
### delete.php 수정
```
[여기에 delete.php 를 어떻게 수정했는지, 설명을 작성하세요.]
$connect = mysql_connect("localhost", "kjh", "1234");
// DB 선택
mysql_select_db("kjh_db", $connect);
// sql 쿼리 string 생성
$sql = "delete from tableboard_shop where num = $_GET[num]";
// sql 쿼리 실행
$result = mysql_query($sql);

$sql2 = "set @cnt =0";
$sql3 =  "update tableboard_shop set tableboard_shop.num = @cnt:=@cnt+1";
$sql4 =  "alter table tableboard_shop auto_increment=1";

$result2 = mysql_query($sql2);
$result3 = mysql_query($sql3);
$result4 = mysql_query($sql4);
# 참고 : 에러 메시지 출력 방법

if(!$result || !$result2 || !$result3 || !$result4) {
    echo "<script> alert('delete - error message') </script>";
}
delete를 시킬 때 중간의 항목을 삭제시키면, 중간 숫자가 그대로 남아있는 경우가 생길 수 있음.
그래서 auto_incrememt를 통해 중간의 값이 delete된 경우 계속에서 번호를 순차적으로 조정.
```