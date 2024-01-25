# HTML-public
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https://code.jquery.com/jquery-3.7.1.js"> </script>
    <title>xml</title>
    <script>
        let api_url = 'http://apis.data.go.kr/6300000/pis/parkinglotIF';
        let key = '01/u7SxCxMdAKSpKhhWj/x4Qh9TmVtiTH+IWuVKC1G55zIZzc/J61x4mSkmjAoWE7G7YJgmM8tQzmQs+6dGdng=='
        $(document).ready(function(){
            $("#btn").click(function(){
                let rows = $("#rows").val();
                let pages = $("#pages").val();
                $.ajax({
                    url : api_url
                    ,type : "GET"
                    , dataType : "xml"
                    , data : {serviceKey : key
                             , numOfRows:rows
                            , pageNo:pages}
                    ,success : function(res){
                        console.log(res);
                        // page 수 (xml안에서 totalCount 태그를 찾아서 안에 있는 text 가져오기)
                        let cnt = $(res).find('totlaCount').text();
                        console.log(cnt);
                        $(res).find("item").each(function(){
                            let item = $(this);
                            let nm = item.find('name').text();
                            let lat = item.find('lat').text(); // 위도
                            let lon = item.find('lon').text(); // 경도
                            let addr = item.find('address').text(); // 주소
                            console.log(nm, lat, lon, addr);
                            let str = `<p> 주차장명 : ${nm} , 주소: ${addr}, 기본요금: ${baseRate} </p>`;
                            $("#content").append(str);
                        })
                    }, error :function(e){
                        console.log(e);
                    }
                });
            });
        });
    </script>
    <script>
    </script>
</head>
<body>
    <input type="text" id="rows" value="50">
    <input type="text" id="pages" value="1">
    <button type="button" id="btn">요청</button>
    <div id="content"></div>
    </body>
</html>
