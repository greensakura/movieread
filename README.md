<!doctype html>
<html lang="en">

<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css"
          integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js"
            integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN"
            crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js"
            integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q"
            crossorigin="anonymous"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js"
            integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl"
            crossorigin="anonymous"></script>

    <title>이명종 영화 메모장 </title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Do+Hyeon&display=swap" rel="stylesheet">

    <style>
        * {
            font-family: 'Do Hyeon', sans-serif;
        }

        .jumbotron {
            color: black;
            background-color: lightgreen;

        }

        .wrap {
            width: 900px;
            margin: auto;
        }

        .comment {
            color: lightseagreen;
            font-weight: bold;
        }

        .posting-box {
            width: 500px;
            margin: 0px auto 30px auto;
            border: 3px solid green;
            border-radius: 10px;
            padding: 30px;

            display: none;
        }

        .card-title {
            font-size: xx-large;
        }

        .card-text {
            font-size: large;
        }

        .card-text2 {
            font-size: x-large;
        }
    </style>
    <script>
            $(document).ready(function () {
                showArticles();
            });

            function openClose() {
                if ($("#post-box").css("display") == "block") {
                    $("#post-box").hide();
                    $("#btn-post-box").text("포스팅 박스 열기");
                } else {
                    $("#post-box").show();
                    $("#btn-post-box").text("포스팅 박스 닫기");
                }
            }

            function postArticle() {
                let url = $('#post-url').val()
                let comment = $('#post-comment').val()
                let star = $('#post-star').val()
                $.ajax({
                    type: "POST",
                    url: "/memo",
                    data: {url_give: url, comment_give: comment, star_give:star},
                    success: function (response) {
                        alert(response["msg"]);
                        window.location.reload()
                    }
                })
            }

            function showArticles() {
                $.ajax({
                    type: "GET",
                    url: "/memo",
                    data: {},
                    success: function (response) {
                        let articles = response['all_articles']
                        for (let i=0; i<articles.length; i++){
                            let title = articles[i]['title']
                            let image = articles[i]['image']
                            let url = articles[i]['url']
                            let comment = articles[i]['comment']
                            let star = articles[i]['star']

                            let temp_html = `<div class="card">
                                                <img class="card-img-top"
                                                     src="${image}"
                                                     alt="Card image cap">
                                                <div class="card-body">
                                                    <a target="_blank" href="${url}" class="card-title">${title}</a>
                                                    <p class="card-text">${comment}</p>
                                                    <p class="card-text2">${star}</p>
                                                </div>
                                            </div>`
                            $('#card-box').append(temp_html)
                        }
                    }
                })
            }
    </script>
</head>

<body>
<div class="wrap">
    <div class="jumbotron">
        <h1 class="display-4">영화기록소, by 초록색벚꽃</h1>
        <p class="lead">간단하게 평점 적는 메모장</p>
        <hr class="my-4">
        <p>0.5점부터 5점까지의 영화들</p>
        <a href="https://pedia.watcha.com/ko-KR" target="_blank" title="영화조아">여기서 가져옵니다.</a>
        <p class="lead">
            <a onclick="openClose()" id="btn-postingbox" class="btn btn-primary btn-lg" href="#" role="button">포스팅박스
                열기</a>
        </p>
    </div>

    <div class="posting-box" id="post-box">
        <div class="form-group">
            <label>아티클 URL</label>
            <input type="email" class="form-control" id="post-url" aria-describedby="emailHelp"
                   placeholder="어떤 영화를 보셨나요?">
            <small id="emailHelp" class="form-text text-muted">걱정마세요! 절대 상업적 용도로 사용하지 않습니다!</small>
        </div>
        <div class="form-group">
            <label for="post-comment">감상평</label>
            <textarea class="form-control" id="post-comment" rows="3"></textarea>
            <label for="post-star">별점</label>
            <textarea class="form-control" id="post-star" rows="3"></textarea>
        </div>
        <div class="form-check">
            <input type="checkbox" class="form-check-input" id="exampleCheck1">
            <label class="form-check-label" for="exampleCheck1">마지막 확인!</label>
        </div>
        <button onclick="postArticle()" type="submit" class="btn btn-primary">제출</button>
    </div>

    <div class="card-columns" id="card-box">
    </div>
</body>

</html>
