---
layout: page
title: buildup Twitter with ( nodejs, express, mongodb, passport, jQeury )
tagline: jQuery, ajax, express, jekyll
---

# 간단한 Twitter 만들어보기.  

### - 주요 기능 : 사용모듈 (외부 라이브러리)  

#####  - 회원가입 : **mongoDB   
#####  - 로그인 : **passport  
#####  - 트윗하기 : **jquery, ajax  
** 이외 기타 모듈 : nodejs, express, mongoJs, ejs  

::: 단순한 트위터의 '트윗하기', '타임라인보기', '로그인', '회원가입'밖에 없는 프로젝트지만 여러 모듈을 사용해볼수 있는 예제이다.


### mongoDB
> - 우선 몽고디비의 구성은, db로 'tweet'을 만들고, collection을 구성한다.
> - collection[ 'userslist', 'twitslist' ] //회원정보와 트윗정보 관리 DB
> - **회원정보[2] : username, password
> - **트윗정보[3] : 작성자, 작성시각, 트윗내용
> - 로그인시에, pass를 구분하고,  나중에 트윗 타임라인에서 작성시각을 기준으로 최근 트윗만 볼수 있도록 구성할것이다.
> - 몽고 디비에 대한 설명은 밑에서 다시하자.


### passport
> - 로그인정보 관리 모듈로 패스포트 사용 (loacl의 규약대로 진행 (아이디, 비번)
> - 로그인 인증
> - 로그인이 안된 페이지 요청 거부 (redirect)


### jQuery.ajax
> - 타임라인 페이지가 새로고침되지않고, 동적으로 페이지를 구성할수 있다.
> - 최근 트윗이 가장 위에 올라오게 되므로, 방금 작성한 트윗이 윗줄에 있도록한다.
> - tweetForm에 내용을 입력하고, **POST**방식으로 **submit**하면,  
> - 받아주는 페이지인 /twit/ajax로 보내지게 된다.
> - jQuery를 통해서 문서(document)에서  **tweetForm**의 내용을 뽑아낸다음  
그 값을 **url : "/twit/ajax"**로  data를 보내게 된다. 
 **서버쪽에선 twit/ajax에서 처리할 코드가 필요하게됨.
 ##### 이부분은 밑에서 다룬다.


 - - -
개발하면서 느낀것.
 - 웹서버 작업을 하다보니, application을 계속 켰다 껐다 하게됐는데..
 **supervisor app.js로 하면, app.js에 관련된 항목들이 업데이트 될때마다  
 수퍼바이저가 서버를 재시작해주면서 굉장히 편히 작업을 할수 있게 도와준다.  

 - ejs의 사용법에 대해서도 다시한번 확인하는 기회였다.
<list>에 <li>를 추가하면서, 반복적으로 작업을 해야 하는 부분이 있어, 

	<ul id="tweetsList">
	      <%if(twitFlag){ 
		//console.log(twitInfo);
		for (var i = 0, max = twitInfo.length; i < max; i++) {
		  var username = twitInfo[i].username
		  , date = twitInfo[i].date
		  , twit = twitInfo[i].twit;
		  //console.log(username);
		%>
		<li>
		  <textarea rows="5" cols="50" name="contents"  >Writer : <%= username %>
		  	Date : <%= date %>
		  	Twit : <%= twit %>
		 	 </textarea>
		</li>
		<% }} %>
	   </ul>

  
위와 같이 <%= 값 %>, <% 처리내용 %> 이라는 규칙을 이용하여 작업을 하였다.
매 줄마다 <% %>을 하는것이 아니고, 코드처리가 돼야하는 부분을 잘 감싸고,사용 할수 있도록 해주는 것이 필요하다 생각이 들었다.    



 - 이번 예제를 만들면서,  jQuery부분이 가장 이해가 안됐었는데- 이부분에 대해서 좀더 자세한 공부와 정리가 필요할것 같다.  

'jQuery' 이해하는 점이 가장 어려웠는데, ($ == qjuery)  
'$'는 페이지의 리로딩 없이 브라우저 내에서 html의 element(요소)들의 attribute, value를 변경(대치) 할수도 있게 해준다.  
특히  jquery의 ajax를 사용하면, 서버와의 통신결과를 바로 html에 적용함으로써, 동적으로 HTML을 제어할수 있게 된다.  
ajax의 사용문법은 다음과같다 **링크[http://api.jquery.com/jQuery.ajax/](http://api.jquery.com/jQuery.ajax/)  
처음엔 나도 이사이트를 아무리 봐도 뭔소린지 이해가 안됐는데, 나의 이해력으로는 이걸 어따 어떻게 써야할지 몰랐어서, 나중에 이글을 보는 나같은 개뉴비도 써먹을수 있도록 간단히 설명해보려고 한다.  


### jQuery.ajax( url [, settings] )
 * jQuery객체가 ajax라는 함수를 쓰려고 하는것이다. *난 왜 그렇게 못읽었을까-_-;;...
**(ex : 


	script type="text/javascript">  
	  $(document).ready( function() {  
	    $('#tweetForm').submit( function() {  
	      var tweetText = $('#tweetText').val();  
	
		$.ajax({
		  type: "POST",
		  url: "/twit/ajax",
		  data: {
		    tweetText: tweetText
		  }
		}).done( function(data) {
		  var html = '<li><textarea rows="5" cols="50" 
		    name="contents">
		    Writer : ' + data.data.username + '\
		    Date : ' + data.data.date + '\
		    Twit : ' + data.data.twit + '</textarea></li>';
		    $('#tweetsList').prepend(html);
		});

	      return false;
	    });

	  });
	</script>

이 코드를 HTML의 HEAD에 둔다.
첫줄부터 보자면, jquery객체인 '$'가 **document**(현재 보이는 브라우저상의 내용)를 읽어온다. 그리고 다 읽어왔을때 (HTML이 모두 로드 됐을때부터 다음 함수 내용을 실행가능하다 라는 뜻)

그래서, document의 정보를 $가 갖고있는데,  
	$('#tweetForm').submit( function() {
input이 submit될때, 이벤트 처리를 위해 textarea객체가 submit눌렸을때 처리할 내용을 만들어 둔것이다. 만약 성공하지않으면 하는 처리를 만들어준다.

jQuery의 .ajax() 방식과 .post() 방식으로 form의 submit 을 관리한다고 한다.
예를들어 ref[http://www.jensbits.com/2009/10/23/jquery-ajax-and-jquery-post-form-submit-examples-with-coldfusion/#viewSource](http://www.jensbits.com/2009/10/23/jquery-ajax-and-jquery-post-form-submit-examples-with-coldfusion/#viewSource) 다음 사이트를 참고해보면 좋을수도^ㅡ^

	$('#tweetForm').submit( function() {
      	 var tweetText = $('#tweetText').val();

$에서 tweetForm의 정보를 .val 객체화하여 tweetText로 저장하도록 한다.
.val()의 관련정보[http://api.jquery.com/val/](http://api.jquery.com/val/) Description: Get the current value of the first element in the set of matched elements. // 하지만 동일한 요소가있는 경우엔, 첫번째 요소의 값만을 취한다라고 한다.


	$.ajax({
		type: "POST",
		url: "/twit/ajax",
		data: {
		 tweetText: tweetText
		}
	}).done( function(data) {
		var html = '<li><textarea rows="5" cols="50" 		 	name="contents">Writer : ' + data.data.username + '\
		Date : ' + data.data.date + '\
		Twit : ' + data.data.twit + '</textarea></li>';
		$('#tweetsList').prepend(html);
	});

### jQuery.ajax  [http://api.jquery.com/jQuery.ajax/](http://api.jquery.com/jQuery.ajax/)

아놔.. **Description: Perform an asynchronous HTTP (Ajax) request.**  
이 주옥같은 한마디가..이제 눈에 들어온다..ㅠㅠ  
**ajax는 비동기적 HTTP request를 수행한다..라는 말ㅠㅠㅠ**   
이제 이해가 된다.. 좀 작동원리를 좀더 잘 이해했다면.. 쉽게 구현했을탠데 그러지 못하였다.    

여튼 **.ajax()**를 통해 필요한 type, url, data...여러 정보를 담아 보낼수 있고,  
그 request가 잘~완료( **done** )됐다면, 다음의 내용을 수행하라라는 것들을 볼수있따..    
	

	$('#tweetsList').prepend(html);

마지막으로, **< textarea>tweetsList** 에 받아온 html이란 녀석을 추가하면서,   
우리가 바라던, 동적 페이지랜더링이 가능해지는것 같다.  

으허..ㅠㅠㅠㅠ 지금 보니 별거아닌데..거의 일주일을 이걸로 고생한것같다..빠가...ㅠㅠ




  
### passport  [ http://passportjs.org/](http://passportjs.org/)  

 passport는 nodejs 를 지원해주는 인증절차 미들웨어 이다.  
 **Passport is authentication middleware for Node.js.**

이건 그나마 좀 그렇게 존나 어렵진않다..  
passport가 좋은건 40개 이상의 여러 auth방식을 지원해준다라는 사실인데,  
이점을 참고해서 나중에 Facebook, twitter같은것들의 인증을 붙일때 써먹으면 더욱 좋을것같다.    

인증을 할뿐만 아니라, 인증된정보를 관리해줌으로써, 로그인상태를 확인한다거나,  
사용자의 정보를 열람할수 있도록 지원해준다.   

사용법 [ http://passportjs.org/guide/authenticate.html](http://passportjs.org/guide/authenticate.html)참고^ㅡ^




### mongoDB
몽고디비를 설치하면, 이미 돌아라고 있는것이다..
그래서

	> mongo 

를 하면 db서버에 접속해서 작업이 가능하다.  
mongodb 는 크게, DB, collection을 만들어서 하게된다.
