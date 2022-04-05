<?php
$arResult = array();
$arPagination = array();

require_once('databases.php');
header('Content-Type: text/html; charset=utf-8');


if(!isset($_GET['page'])) $page = 1; else $page = htmlspecialchars($_GET['page']);
if(ctype_digit($page) === false) $page = 1;
$count_query = $connection -> query("SELECT COUNT(*) FROM news");
$count_array = $count_query->fetch_array(MYSQLI_NUM);
$count = $count_array[0];
$limit = 5;
$start = ($page*$limit)-$limit; //с какого места выводим запись
$length = ceil($count/$limit); //длинна записи

if ((int)$page > $length) $start = 0;
$query = mysqli_query($connection,"SELECT * FROM news ORDER BY idate DESC LIMIT $start, $limit");

function Pagination($length, $page){

//foreach (range(1, $length) as $p) echo '<a href="news.php?page='.$p.'"> '.$p.' </a>';

              /* if($length < 5) foreach (range(1, $length) as $p) echo  '<a href="index.php?page='.$p.'"> '.$p.' </a>';
            if($length > 4 && $page < 5) foreach (range(1, 5) as $p) echo  '<a href="index.php?page='.$p.'"> '.$p.' </a>';
            if($length-5 < 5 && $page > 5) foreach (range($length-4, $length) as $p) echo  '<a href="index.php?page='.$p.'"> '.$p.' </a>';
            if($length > 4 && $length-5 < 5 && $page == 5) foreach (range($page-2, $length) as $p) echo  '<a href="index.php?page='.$p.'"> '.$p.' </a>';
            if($length > 4 && $length-5 > 5 && $page >= 5 && $page <= $length-4) foreach (range($page-2, $page+2) as $p) echo  '<a href="index.php?page='.$p.'"> '.$p.' </a>';
            if($length > 4 && $length-5 > 5 && $page > $length-4) foreach (range($length-4, $length) as $p) echo  '<a href="index.php?page='.$p.'"> '.$p.' </a>';*/
}

while ($article = mysqli_fetch_assoc($query)){
    $arResult[] = array(
        'idate' => date('d.m.Y ', $article['idate']),
        'title' => $article['title'],
        'id' => $article['id'],
        'announce' => $article['announce']
    );
}
if(isset($_GET['id'])) {
    $newsId = $_GET['id'];
}
$query = mysqli_query($connection,"SELECT * FROM `news`WHERE id='$newsId'");
while ($article = mysqli_fetch_assoc($query)){
    $arNews[] = array(
        'title' => $article['title'],
        'id' => $article['id'],
        'content' => $article['content']
    );
}
?>
