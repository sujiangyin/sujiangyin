<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<style>
    .mainDiv{
		width:100px;
		height:100px;
		margin:100px auto;
		text-align: center;
		line-height: 100px;
		font-weight: bold;
		color:#ddd;
		background:#ddd;
		border:1px solid #ddd;
		-webkit-transform:rotate(0deg) scale(1);
		-moz-transform:rotate(0deg) scale(1);
		transform:rotate(0deg) scale(1);
        transition: width 2s, height 2s,color 2s,transform 2s;
-moz-transition: width 2s, height 2s,color 2s,transform 2s;    /* Firefox 4 */
-webkit-transition: width 2s, height 2s,color 2s,transform 2s;	/* Safari 和 Chrome */
	}
	.mainDiv:hover{      
	     background-color:#EED2EE;
         color:#000;
         transform: rotate(720deg) scale(2);
-webkit-transform: rotate(720deg) scale(2);	/* Safari and Chrome */
-moz-transform: rotate(720deg) scale(2);
	}
</style>
<title>css3特效</title>
</head>
<body>
<div class="mainDiv">您好</div>
</body>
</html>
