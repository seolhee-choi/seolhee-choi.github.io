---
title : "GET 방식으로 할인 쿠폰 발급하는 방법"
categories:
 - Php
tags:
 - [ php ] 

toc: true
toc_sticky: true

date: 2022-02-22
last_modified_at: 2022-02-22

---

##### 매일 수강인증한 회원에 한해 특정 쿠폰이 발급되어야 한다는 내용의 요청을 받았다.
##### 운영팀에서 인증한 회원의 ID를 게시판에 등록하면, 나는 해당 게시글의 idx값만 확인한 후 수동으로 호출해주면 되게끔 하였다.
##### 추가기능(카카오 알림톡 - 현재 회사에서는 별도로 사용하는 api가 있음) + 쿠폰 발급 로그


```php
@session_start();

MySQLConnectDB();

$send_cupon = "Y"; //쿠폰발급여부
$event_idx = 0; //event_idx 현재 없음
$send_sms = "N"; //sms 발송 여부
$event_date = date("Y-m-d"); //이벤트확인날짜 오늘

$cuponname = "수강인증 할인쿠폰"; //쿠폰이름
$persent = 50; //할인율
$startdate = $event_date; //시작일
$timestamp = strtotime("+7 days"); //쿠폰유효기간
$enddate = date("Y-m-d", $timestamp); //쿠폰사용 종료일
$cupon_state = 0; //쿠폰사용여부 0-사용안함, 1-사용함
$cuponNO= 0;

$condition_value = "조건 없음";

$idx = $_GET['idx'];
//$idx = 7;

$event_type = "0124_event_promotion";
$today = date("Ymd");

$promotion_select_sql = "SELECT context FROM 2022_event_promotion WHERE idx=".$idx;
$promotion_select_res = MySQLQuery($promotion_select_sql);
$promotion_select_list = MySQLFetchArray($promotion_select_res);

$useridarray = $promotion_select_list['context'];
$useridarray = explode(",",$useridarray);

for($i=0; $i<count($useridarray); $i++) {
    $userid = trim($useridarray[$i]);
    //중복발급 확인
    $dup_sql = "SELECT count(*) as cnt from 2022_condition_coupon_log";
    $dup_sql .= " WHERE send_cupon= 'Y' AND event_type='$event_type' and userid='$userid'";
    $dup_res = MySQLQuery($dup_sql);
    $dup_list = MySQLFetchArray($dup_res);

    if ($dup_list['cnt'] > 0) //중복발급인 경우
    {
        $msg = "중복 발급입니다. ID:" . $userid . "<br>";;
        $send_cupon = "N";
        $send_sms = "N";
        echo $msg;
        $msg = iconv("EUC-KR", "UTF-8", $msg);
        insert_condition_log($userid, $event_idx, $event_type, $condition_value, $send_cupon, $cuponNO, $send_sms, $event_date, $msg);
    } else {
        $cuponNO = cuponNO();

        //조건없음
        $send_cupon = "Y";
        $condition_value = "조건없음";

        //쿠폰의 사용 여부 확인 ( 사용했으면 1, 사용하지 않았으면 0)
        $cupon_insert_sql = "insert into my_cupon (userid,cuponname,couponNO,persent,startdate,enddate,cupon_state) ";
        $cupon_insert_sql .= "values ('$userid','$cuponname','$cuponNO.',$persent, '$startdate', '$enddate', '$cupon_state')";
        $copon_insert_result = MySQLQuery($cupon_insert_sql);

        $msg = "쿠폰이 발급되었습니다. : ". $userid. "<br>";
        echo $msg;
        $msg = iconv("EUC-KR", "UTF-8", $msg);


        $member_select_sql = "SELECT * FROM tbl_member WHERE user_id='$userid'";
        $member_select_res = MySQLQuery($member_select_sql);
            while ($member_select_list = MySQLFetchArray($member_select_res)) {
                $sendanswer = $member_select_list['sendanswer'];
                $member_id = $member_select_list['user_id'];
                $member_name = $member_select_list['user_name'];
                $member_phone = $member_select_list['home_phone'];//휴대폰 번호

                if($sendanswer == "Y" || $sendanswer == "예") {
                    $send_sms = "Y";

                    //알림톡 전송
                    $post_data = array(
                        "id" => iconv('EUC-KR', 'UTF-8', $member_id),
                        "name" => iconv('EUC-KR', 'UTF-8', $member_name),
                        "hp_num" => iconv('EUC-KR', 'UTF-8', $member_phone),
                    );

                    $ch = curl_init();
                    curl_setopt($ch, CURLOPT_URL, "https://url");//해당 링크를 통해 카카오톡 발송 api를 사용
                    curl_setopt($ch, CURLOPT_POST, 1);
                   

                    //SSL 필수 시작 - HTTPS일때 필수
                    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, FALSE);
                    curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 0);
                    //SSL 필수 끝
                    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
                    $output = curl_exec($ch);

                    $curl_errno = curl_errno($ch);
                    $curl_error = curl_error($ch);

                    $info = curl_getinfo($ch);

                    curl_close($ch);

                } else {
                    $send_sms = "N";
                }
            }
            //로그 남기기
            insert_condition_log($userid, $event_idx, $event_type, $condition_value, $send_cupon, $cuponNO, $send_sms, $event_date, $msg);

    }
}
```

밑에는 insert_condition_log 함수 내용(공통 파일에 넣어져있다)

```php
/*
  ┌──────────────────────────────────────────────────────┐
  │▒ 함수명 : insert_condition_log()                    ▒│
  │▒ 설  명 : 조건 발급 쿠폰 로그                        ▒│
  │▒                                                    ▒│
  │▒ 입력값 : $userid : userid, $event_idx :이벤트 idx
  │▒ 입력값 : $event_type : 이벤트type, $condition_value :조건
  │▒ 입력값 : $send_cupon : 쿠폰발급여부, $cuponNO :쿠폰번호
  │▒ 입력값 : $send_sms : sms발신여부, $event_date : 이벤트시작일
  │▒ 입력값 : $message : 사용자 메시지
  │▒ 리턴값 : 없음                                      ▒│
  └──────────────────────────────────────────────────────┘*/
function insert_condition_log($userid, $event_idx, $event_type, $condition_value, $send_cupon, $cuponNO, $send_sms, $event_date, $message)
{
  //id, event_idx(이벤트 idx)-지금은 없음, 
  //event_type(이벤트타입)-지금은 0124_event 고정, 
  //condition_value(조건:값) 내용-결제금액>10만원:$결제금액
  //send_cupon(발급여부)-Y,N couponNO, 
  //send_sms(sms 송신여부)-지금은N, 
  //event_date(이벤트날짜,오늘), reg_date(등록일시) condition_coupon_log에 내용 저장
	//조건에 안맞아도 무조건 log에는 기입
	$message = iconv("UTF-8", "EUC-KR", $message);
	$cupon_log_insert_sql = " INSERT INTO 테이블명(userid, event_idx, event_type, condition_value, send_cupon, couponNO, send_sms, event_date, message, reg_date)";
	$cupon_log_insert_sql .= " VALUES ('$userid','$event_idx','$event_type', '$condition_value', '$send_cupon', '$cuponNO','$send_sms', '$event_date', '$message', NOW())";
	$cupon_log_result = MySQLQuery($cupon_log_insert_sql);
}
```

##### 이렇게 소스코드로 짜여진 coupon.php 파일을 도메인에 idx값과 함께 수동으로 입력하면
##### 해당하는 사람들에게 해당 쿠폰(수강인증 할인쿠폰)이 자동으로 발급되고 카카오 안내톡도 발송된다.
##### EX) https://www.test.kr/coupon.php?idx=7