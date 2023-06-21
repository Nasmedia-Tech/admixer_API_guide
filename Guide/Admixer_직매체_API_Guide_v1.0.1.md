# Admixer 직매체 API Guide (for publisher)

### Udate History
Version|Date|Note
:---|:---|:---
1.0.0|2023.05.19|최초릴리즈
1.0.1| 2023.06.20|인앱 video 포맷 업데이트


## 목차

[1. 소개](#1-소개) <br>
    &emsp;[1.1. 설명](#11-설명) <br>
    &emsp;[1.2. 지원 가능한 광고](#12-지원-가능한-광고)

[2. Request](#2-request) <br>
    &emsp;[2.1. 설명](#21-설명) <br>
    &emsp;[2.2. 도메인 정보](#22-도메인-정보) <br>
    &emsp;[2.3. Request URL 예시](#23-request-url-예시) <br>
    &emsp;[2.4. Request 기본 객체](#24-request-기본-객체) <br>
    &emsp;[2.5. 디바이스 객체](#25-디바이스-객체) <br>
   &emsp; [2.6. 비디오 객체](#26-비디오-객체)

[3. Response](#3-response) <br>
    &emsp;[3.1. 설명](#31-설명) <br>
    &emsp;[3.2. Response 기본 객체](#32-response-기본-객체) <br>
    &emsp;[3.3. body 객체](#33-body-객체) <br>
    &emsp;[3.4. Macro 지원](#34-macro-지원) <br>
    &emsp;&emsp;[3.4.1. ${CLICK_TRACK_URL}](#341-click_track_rul)

[4. 코드 정의](#4-코드-정의) <br>
    &emsp;[4.1. 에러코드, 에러메세지](#41-에러코드-에러메세지) <br>
    &emsp;[4.2. 광고 코드](#42-광고-코드) <br>
    &emsp;&emsp;[4.2.1. adformat](#421-adformat) <br>
    &emsp;&emsp;[4.2.2. platform](#422-platform) <br>
    &emsp;&emsp;[4.2.3. video_type](#423-video_type) <br>
    &emsp;&emsp;[4.2.4. video_placement](#424-video_placement) <br>
    &emsp;&emsp;[4.2.5. vast_ver](#425-vast_ver) <br>
    &emsp;&emsp;[4.2.6. native asset id](#426-native-asset-id) <br>
    &emsp;&emsp;[4.2.7. native tracker](#427-native-tracker) <br>

[5. Samples](#5-sample) <br>
    &emsp;[5.1. Response Sample](#51-response-sample) <br>
    &emsp;&emsp;[5.1.1. banner Response Sample](#511-banner-response-sample) <br>
    &emsp;&emsp;[5.1.2. video Response Sample](#512-video-response-sample) <br>
    &emsp;&emsp;[5.1.3. native Response Sample](#513-native-response-sample)

<hr>

## 1. 소개
### 1.1. 설명
- 본 문서는 Admixer Publisher에게 제공되는 광고 송출 API 가이드 문서입니다.

### 1.2. 지원 가능한 광고
<table>
    <th align="left">Platform</th>
    <th align="left">Adformat</th>
    <th align="left">설명</th>
    <tr>
        <td rowspan="3">Android</td>
        <td>Banner</td>
        <td>320X480* / 300x250* / 320x100 / 320x50</td>
    </tr>
    <tr>
        <td>Native</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Video</td>
        <td>reward** / instream / outstream*</td>
    </tr>
    <tr>
        <td rowspan="3">iOS</td>
        <td>Banner</td>
        <td>320X480* / 300x250* / 320x100 / 320x50</td>
    </tr>
    <tr>
        <td>Native</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Video</td>
        <td>reward** / instream / outstream*</td>
    </tr>
    <tr>
        <td rowspan="2">m.web</td>
        <td>Banner</td>
        <td>320X480* / 300x250* / 320x100 / 320x50 / 100x100</td>
    </tr>
    <tr>
        <td>Native</td>
        <td>320X480* / 300x250* / 320x100* / 320x50</td>
    </tr>
    <tr>
        <td rowspan="2">pc.web</td>
        <td>Banner</td>
        <td>300x250* / 200x200 / 728x90 / 970x90 / 160x600 / 300x600 / 120x600 / 250x250</td>
    </tr>
    <tr>
        <td>Native</td>
        <td>300x250* / 728x90* / 970x90* / 160x600* / 300x600* / 120x600* / 250x250*</td>
    </tr>
</table>
<div align="left"><i>*interstitial 가능 / **interstitial만 가능</i></div>
<div align="left"><i>Android / iOS - Native Adformat은 asset 구성에 따라 자유롭게 리사이징 가능</i></div>
<hr>

## 2. Request
### 2.1. 설명
- Admixer의 광고 API로써, 자체 광고 및 제휴 DSP(외부) 광고를 제공합니다.
- HTTP GET 방식으로 정의된 객체를 작성해 ADMIXER 인터페이스를 호출합니다.

### 2.2. 도메인 정보
구분|URL
:---|:---
상용|https://<nohyper>adn.admixer.co.kr

### 2.3. Request URL 예시
`https://adn.admixer.co.kr/api/adrequest?media_key={media_id}&adunit_id={adunit_id}&platform={platform_code}&os={device_os_code}&os_ver={device_os_ver}&adid={device_ifa}&adformat={adformat}&fullscreen={fullscreen_yn}&width={width}&height={height}&adid_use={adid_use_yn}&pack_name={package_name} `

### 2.4. Request 기본 객체
필드|유형|필수|설명|값
:---|:---|:---|:---|:---
media_key|string|**필수**|매체 키|애드믹서 파트너사이트에서 발급*
adunit_id|string|**필수**|애드유닛 아이디|애드믹서 파트너사이트에서 발급*
adformat|string|권장|광고형태, [4.2.1](#421-adformat) 참고|banner : 배너<br>video : 비디오<br>native : 네이티브
fullscreen|integer|**필수**|전면 여부|0 : no<br>1 : yes
platform|string|**필수**|매체 플랫폼, [4.2.2](#422-platform) 참고|android : Android 플랫폼<br>ios : IOS 플랫폼<br>m_web : 모바일 웹 플랫폼<br>pc_web : PC 웹 플랫폼
pack_name|string|(앱의 경우)<br>**필수**|앱 패키지명<br>(platform이 Android or IOS인 경우)|ex) Android : kr.co.nasmedia.biorhythm<br>iOS : 916041783
site_url|string|(웹의 경우)<br>**필수**|웹사이트 url<br>(platform이 m_web, pc_web인 경우)|ex) pc_web : admixer.co.kr<br>m_web : m.admixer.co.kr
clicktrack|integer|권장|클릭 측정 URL 사용 여부, [3.4.1](#341-click_track_rul) 참고|0 : no<br>1 : yes
coppa|integer|권장|어린이 온라인 사생활 보호법 준수 여부<br>(Children's Online Privacy Protection Act)|0 : no<br>1 : yes
<div align="left"><i>*test 광고는 별도 문의</i></div>

### 2.5. 디바이스 객체
필드|유형|필수|설명|값
:---|:---|:---|:---|:---
os|string|권장|디바이스 OS명|ex) android, ios, window
os_ver|string|권장|디바이스 OS 버전|ex) 10, 12, 16.1.1
network|string|권장|디바이스 네트워크|ex) wifi, 3G, LTE
lang|string|권장|디바이스 언어|ex) ko, ja, en
adid|string|(앱의 경우)<br>**필수**|Android : Google Advertise ID<br>iOS : IDFA|
adid_use|integer|권장|브라우저가 헤더에 설정한 ADID 표준 추적 플래그,<br>미기입시 1|0 : no<br>1 : yes
model|string|권장|디바이스 모델|
carrier|string|권장|디바이스 통신사|

### 2.6. 비디오 객체
- adformat이 video인 경우에 사용합니다.

필드|유형|필수|설명|값
:---|:---|:---|:---|:---
video_type|integer|권장|video의 형태, [4.2.3](#423-video_type) 참고|1 : instream<br>2 : outstream<br>3 : rewarded
video_placement|integer|권장|비디오의 배치, [4.2.4](#424-video_placement) 참고|1 : in-stream<br>2 : in-banner<br>3 : in-article<br>4 : in-feed<br>5 : floating<br>10 : hybrid
vast_ver|integer|권장|응답 받고자 하는 VAST 버전(미기입시 3 : VAST 3.0),<br>[4.2.5](#425-vast_ver) 참고|2 : VAST 2.0<br>3 : VAST 3.0<br>7 : VAST 4.0
<hr>

## 3. Response
### 3.1. 설명
- Admixer는 아래 정의된 객체를 json 형태로 응답합니다.

### 3.2. Response 기본 객체
필드|유형|필수|설명
:---|:---|:---|:---
error_code|integer|**필수**|에러 코드, [4.1](#41-에러코드-에러메세지) 참고
error_msg|string|**필수**|에러 메세지, [4.1](#41-에러코드-에러메세지) 참고
body|object|**필수**|광고 응답문이 삽입된 body, [3.3](#33-body-객체) 참고

### 3.3. body 객체
필드|유형|필수|설명
:---|:---|:---|:---
adformat|string|**필수**|광고 형태, [4.2.1](#421-adformat) 참고
interstitial|integer|**필수**|전면 여부, 0:no, 1:yes
media_key|string|**필수**|매체 키
adunit_id|integer|**필수**|애드유닛 아이디
width|integer|**필수**|가로 사이즈
height|integer|**필수**|세로 사이즈
adm|string|**필수**|광고 응답 전문<br>banner - html 형태의 응답<br>video - VAST 형태의 응답*<br>native - native 1.2 규격의 응답, [4.2.6](#426-native-asset-id), [4.2.7](#427-native-tracker) 참고
<div align="left"><i>*2.6. vast_ver 필드의 값을 따름</i></div>

### 3.4. Macro 지원
Macro|지원|설명
:---|:---|:---
${CLICK_TRACK_URL}|O|클릭 측정 URL

#### 3.4.1. ${CLICK_TRACK_RUL}
- 아래와 같은 순서로 클릭 측정 URL을 사용할 시 클릭이 발생한 후 클릭 측정 URL이 호출되어 매체가 클릭 발생 여부를 알 수 있습니다.
1. Request 기본 객체([2.4](#24-request-기본-객체)) 내 clicktrack의 value를 1로 설정
2. Response의 body.adm 내부에 ${CLICK_TRACK_URL} 매크로 검색
3. 매크로를 클릭 측정 URL로 변경 (인코딩 X)
4. 교체된 클릭 측정 URL은 이미지 1x1 픽셀로 응답
<hr>

## 4. 코드 정의
### 4.1. 에러코드, 에러메세지
error_code|error_msg|설명
:---|:---|:---
0|OK|정상 응답
3003|Parameter_Platform_Invalid|매체에서 지원하는 플랫폼이 아님
3004|Parameter_Platform_Missing|유효한 platform 값이 아님
3022|Parameter_Adunitid_Invalid|유효한 adunit_id가 아님
3023|Parameter_Mediakey_Missing|media_key 필수값을 기입하지 않음
4003|No_Ads_Available|광고 없음
4006|Invalid_Media|유효한 media_key가 아님
4007|Invalid_Request|필수 파라미터 누락

### 4.2. 광고 코드
#### 4.2.1. adformat
값|설명
:---|:---
banner|배너
video|비디오
native|네이티브

#### 4.2.2. platform
값|설명
:---|:---
android|Android 플랫폼 (대소문자 구분X)
ios|IOS 플랫폼 (대소문자 구분X)
m_web|모바일 웹 플랫폼 (대소문자 구분X)
pc_web|PC 웹 플랫폼 (대소문자 구분X)

#### 4.2.3. video_type
값|설명
:---|:---
1|instream
2|outstream
3|rewarded

#### 4.2.4. video_placement
값|설명
:---|:---
1|in-stream
2|in-banner
3|in-article
4|in-feed
5|floating
10|hybrid

#### 4.2.5. vast_ver
값|설명
:---|:---
2|VAST 2.0
3|VAST 3.0
7|VAST 4.0

#### 4.2.6. native asset id
asset id|value|설명
:---|:---|:---
100|title|제목
101|advertiser|광고주명
102|cta|CTA
103|description|Description
200|img|메인이미지
201|icon|아이콘이미지
202|video|비디오

#### 4.2.7. native tracker
value|설명
:---|:---
clicktracker|- 광고 클릭 집계를 위한 트래킹 url<br>- 다수의 url이 포함될 수 있음<br>- 광고 클릭시 해당 url을 호출하도록 처리 필요
eventtracker|- 광고 노출 집계를 위한 트래킹 url<br>- 다수의 url이 포함될 수 있음<br>- 광고 이미지 노출(show)시 해당 url을 호출하도록 처리 필요
<hr>

## 5. Sample
### 5.1. Response Sample
#### 5.1.1. banner Response Sample
```JSON
{
    "error_code":0, 
    "error_msg":"OK",
    "body":{
        "adformat":"banner",
        "interstitial":1,
        "media_key":"12345678",
        "adunit_id":87654321,
        "width":320,
        "height":480,
        "adm":"<div style=\"position:absolute; width:320px;height:480px;z-index:999;overflow:hidden;\" onclick=\"new Image().src='[${CLICK_TRACKING}]';this.removeAttribute('onclick');\"><meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no\"> <style type=\"text/css\"> body{margin:0; padding:0;}</style> <a href=\"https://partner.admixer.co.kr/\" title=\"advertise\" target=\"_blank\" style=\"display:block;\"><span style=\"display:none;\"></span> <img src=\"https://cdnet.nasmob.com/dev/admixer/image/admixer_dsp_ad/banner_320x480.png\" width=\"320\" height=\"480\" alt=\"ad\" title=\"\" border=\"0\"></a> <div id=\"beacon_8e9f829ea7\" style=\"position:absolute; left:0px; top:0px; visibility:hidden;\"></div></div>"
        }
}
```
#### 5.1.2. video Response Sample
```JSON
{
    "error_code":0,
    "error_msg":"OK",
    "body":{
        "adformat":"video",
        "interstitial":0,
        "media_key":"12345678",
        "adunit_id":87654321,
        "width":640,
        "height":360,
        "adm":"<VAST version=\"3.0\"><Ad id=\"1234\"><InLine><Impression><![CDATA[https://sampletracker.co.kr/...]]></Impression><AdSystem version=\"2.0\"></AdSystem><AdTitle><![CDATA[1234]]></AdTitle><Impression id=\"1234\"><![CDATA[https://sampletracker1.co.kr/...]]></Impression><Impression id=\"1234\"><![CDATA[https://sampletracker2.co.kr/...]]></Impression><Creatives><Creative><Linear><Duration>00:00:15</Duration><TrackingEvents><Tracking event=\"firstQuartile\"><![CDATA[http://sampletest.com/firstquartile_tracking]]></Tracking><Tracking event=\"midpoint\"><![CDATA[http://sampletest.com/midpoint_tracking]]></Tracking><Tracking event=\"thirdQuartile\"><![CDATA[http://sampletest.com/thirdquartile_tracking]]></Tracking><Tracking event=\"complete\"><![CDATA[http://sampletest.com/complete_tracking]]></Tracking></TrackingEvents><VideoClicks><ClickTracking><![CDATA[http://sampletest.com/click_tracking?click]]></ClickTracking><ClickThrough><![CDATA[https://sampletest.com/click?click]]></ClickThrough><ClickTracking><![CDATA[http://sampletest.com/click_tracking?click]]></ClickTracking></VideoClicks><MediaFiles><MediaFile id=\"1234\" delivery=\"progressive\" type=\"video/mp4\" width=\"640\" height=\"360\"><![CDATA[https://sampletest.com/video.mp4]]></MediaFile></MediaFiles></Linear><CreativeExtensions></CreativeExtensions></Creative><Creative><CompanionAds><Companion id=\"1234\" width=\"1280\" height=\"720\" assetWidth=\"0\" assetHeight=\"0\" expandedWidth=\"0\" expandedHeight=\"0\"><CompanionClickThrough><![CDATA[https://sampletest.com/click?click]]></CompanionClickThrough><CompanionClickTracking><![CDATA[http://sampletest.com/click_tracking?click]]></CompanionClickTracking><TrackingEvents><Tracking event=\"creativeView\"><![CDATA[http://sampletest.com/endcard_tracking]]></Tracking></TrackingEvents><StaticResource creativeType=\"image/png\"><![CDATA[https://sampletest.com/endcard.png]]></StaticResource></Companion></CompanionAds><CreativeExtensions></CreativeExtensions></Creative></Creatives><Description></Description><Survey></Survey><Extensions></Extensions></InLine></Ad></VAST>"
        }
}
```

#### 5.1.3. native Response Sample
```JSON
{
    "error_code":0,
    "error_msg":"OK",
    "body":{
        "adformat":"native",
        "interstitial":0,
        "media_key":"12345678",
        "adunit_id":87654321,
        "width":0,
        "height":0,
        "adm":"{\"native\":{\"assets\":[{\"id\":100,\"required\": 1,\"title\":{\"text\":\"Admixer\"}},{\"id\":201,\"required\":1,\"img\":{\"url\":\"https://cdnet.nasmob.com/admixer/image/icon.png\",\"h\":80,\"w\":80,\"type\":0}},{\"id\":200,\"required\":1,\"img\":{\"url\":\"https://cdnet.nasmob.com/admixer/image/main.png\",\"h\":627,\"w\":1200,\"type\":0}},{\"id\":103,\"required\":1,\"data\":{\"value\":\"Admixer\"}},{\"id\":102,\"required\":1,\"data\":{\"value\":\"Start now\"}},{\"id\":202,\"video\":{\"vasttag\":\"<VAST version='3.0'>......</VAST>\"}}],\"link\":{\"url\":\"https://dspserver/clickurl\",\"clicktrackers\":[\"http://testserver/click_tracking?clickurl\"]},\"eventtrackers\":[{\"event\":1,\"method\":1,\"url\":\"http://testserver/beacon?beaconurl\"},{\"event\":1,\"method\":1,\"url\":\"https://dspserver/impurl\"}]}}"
        }
}
```
