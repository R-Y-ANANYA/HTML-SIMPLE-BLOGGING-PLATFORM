# HTML-SIMPLE-BLOGGING-PLATFORM
Sign in:
       	<!DOCTYPE html>
<html>
<head>
  <title>Sign In</title>
  <style>
    /* Custom CSS styles */
    body {
      font-family: Arial, sans-serif;
      background-color: #f2f2f2;
      margin: 0;
      padding: 0;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    form {
      max-width: 300px;
      margin: 0 auto;
      background-color: #fff;
      padding: 20px;
      border: 1px solid #ccc;
      border-radius: 5px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    label {
      font-weight: bold;
      margin-bottom: 5px;
    }
    
    input[type="email"],
    input[type="password"] {
      width: 100%;
      padding: 10px;
      margin-bottom: 10px;
      border: 1px solid #ccc;
      border-radius: 3px;
    }
    input[type="submit"] {
      background-color: #333;
      color: #fff;
      border: none;
      padding: 10px 20px;
      cursor: pointer;
      border-radius: 3px;
    }
    input[type="submit"]:hover {
      background-color: #555;
    }
  </style>
</head>
<body>
  <h1>Sign In</h1>
  <form ACTION="<?php echo $loginFormAction; ?>" name="frmlogin" id = "frmlogin" method="POST">
    <label for="email">Email:</label><br>
    <input type="email" id="uname" name="uname" required><br><br>
    
    <label for="password">Password:</label><br>
    <input type="password" id="pwd" name="pwd" required><br><br>
    
    <input type="submit" value="Sign In">
  <input type="hidden" name="MM_insert" value="post_form">
  </form>

</body>
</html>

<?php require_once('connection.php'); 

if (!function_exists("GetSQLValueString")) {
function GetSQLValueString($theValue, $theType, $theDefinedValue = "", $theNotDefinedValue = "") 
{
  if (PHP_VERSION < 6) {
    $theValue = get_magic_quotes_gpc() ? stripslashes($theValue) : $theValue;
  }

  $theValue = function_exists("mysql_real_escape_string") ? mysql_real_escape_string($theValue) : mysql_escape_string($theValue);

  switch ($theType) {
    case "text":
      $theValue = ($theValue != "") ? "'" . $theValue . "'" : "NULL";
      break;    
    case "long":
    case "int":
      $theValue = ($theValue != "") ? intval($theValue) : "NULL";
      break;
    case "double":
      $theValue = ($theValue != "") ? doubleval($theValue) : "NULL";
      break;
    case "date":
      $theValue = ($theValue != "") ? "'" . $theValue . "'" : "NULL";
      break;
    case "defined":
      $theValue = ($theValue != "") ? $theDefinedValue : $theNotDefinedValue;
      break;
  }
  return $theValue;
}
}

$loginFormAction = $_SERVER['PHP_SELF'];
if (isset($_SERVER['QUERY_STRING'])) {
  $loginFormAction .= "?" . htmlentities($_SERVER['QUERY_STRING']);
}

if ((isset($_POST["MM_insert"])) && ($_POST["MM_insert"] == "post_form")) {
  $LoginRs_Query = "select * from regtable where uname = '" . $_POST['uname'] . "' and pwd = '" . $_POST['pwd'] . "'" ;
 
   mysqli_select_db($con,$database);
  $LoginRS = mysqli_query($con, $LoginRs_Query) or die(mysqli_error($con));
  $loginFoundUser = mysqli_num_rows($LoginRS);
  echo  "<p align='right'>Incorrect Login";
  if($loginFoundUser)
  {
    $insertGoTo = "blogdet.php";
    header(sprintf("Location: %s", $insertGoTo));
  }
}

?
Blog display :
		<!DOCTYPE html>
<html>
<head>
    <title>Simple Blogging Platform</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }
        header {
            background-color: #333;
            color: #fff;
            padding: 20px;
            text-align: center;
        }
        h1 {
            margin: 0;
        }
        main {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
        }
        #blog-posts article {
            margin-bottom: 30px;
            border: 1px solid #ccc;
            padding: 20px;
            background-color: #f7f7f7;
        }
        #blog-posts h2 {
            margin: 0;
        }
        #blog-posts p {
            margin-bottom: 10px;
        }
        #create-post form {
            margin-top: 20px;
        }
        #create-post label {
            display: block;
            margin-bottom: 5px;
        }
        #create-post input[type="text"],
        #create-post textarea {
            width: 100%;
            padding: 5px;
            margin-bottom: 10px;
        }
        #create-post button[type="submit"] {
            background-color: #333;
            color: #fff;
            padding: 8px 15px;
            border: none;
            cursor: pointer;
        }
        #create-post button[type="submit"]:hover {
            background-color: #555;
        }
    </style>
     <style>
        /* Add your custom CSS styles here */
        .comment-section {
            margin-top: 20px;
        }
        .comment {
            margin-bottom: 10px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 3px;
            background-color: #f7f7f7;
        }

        .comment p {
            margin: 0;
        }
        .comment-form label {
            font-weight: bold;
            margin-bottom: 5px;
            display: block;
        }
        .comment-form input[type="text"],
        .comment-form textarea {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }
        .comment-form button[type="submit"] {
            background-color: #333;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 3px;
        }

        .comment-form button[type="submit"]:hover {
            background-color: #555;
        }
    </style>
</head>
<body>
    <header>
        <h1>Simple Blogging Platform</h1>
    </header>
    <main>
        
        <?php do { ?
        <section id="blog-posts">
            <article>
                <h2><?php echo $row_rsblog['title']; ?></h2>
                <p>Posted by <?php echo $row_rsblog['author']; ?></p>
                <p><?php echo $row_rsblog['content'];?></p>
                <section class="comment-section">
                    <h3>Comments</h3>
                    <div class="comments">
                        <?php
                                $query1= "SELECT * FROM comment where blogid = " . $row_rsblog['bid'];
                                $rscomment = mysqli_query($con,$query1) or die(mysqli_error($con));
                                $row_rscomment = mysqli_fetch_assoc($rscomment);
                                $totalRows_rscomment = mysqli_num_rows($rscomment);
                        do { ?>
                        <p><?php echo $row_rscomment['comm']; ?></p>
                        <p>Posted By :<?php echo $row_rscomment['name']; ?></p>
                        <?php } while ($row_rscomment = mysqli_fetch_assoc($rscomment)); ?>
                        <!-- Existing comments will be displayed here -->
                    </div>
                    <form class="comment-form" action="<?php echo $editFormAction; ?>" method="POST" id="post_form" name="post_form"  enctype="multipart/form-data">
                        <label for="name">Name:</label>
                        <input type="text" id="cmdid" name="cmdid" value="<?php echo $row_rsblog['bid'];?>" hidden>
                        <input type="text" id="name" name="name" required>
                        
                        <label for="comment">Comment:</label>
                        <textarea id="comment" name="comment" required></textarea>

                        <button type="submit" id="btn_add" name="btn_add">Submit Comment</button>
                        <input type="hidden" name="MM_insert" value="post_form">                        
                        
                    </form>
                   
                </section>
            </article>
        <?php } while ($row_rsblog = mysqli_fetch_assoc($rsblog)); ?>
    </main> 
</body>
</html>
</body>
</html>
<?php require_once('connection.php'); ?>
<?php
if (!function_exists("GetSQLValueString")) {
function GetSQLValueString($theValue, $theType, $theDefinedValue = "", $theNotDefinedValue = "") 
{
  if (PHP_VERSION < 6) {
    $theValue = get_magic_quotes_gpc() ? stripslashes($theValue) : $theValue;
  }

  $theValue = function_exists("mysql_real_escape_string") ? mysql_real_escape_string($theValue) : mysql_escape_string($theValue);

  switch ($theType) {
    case "text":
      $theValue = ($theValue != "") ? "'" . $theValue . "'" : "NULL";
      break;    
    case "long":
    case "int":
      $theValue = ($theValue != "") ? intval($theValue) : "NULL";
      break;
    case "double":
      $theValue = ($theValue != "") ? doubleval($theValue) : "NULL";
      break;
    case "date":
      $theValue = ($theValue != "") ? "'" . $theValue . "'" : "NULL";
      break;
    case "defined":
      $theValue = ($theValue != "") ? $theDefinedValue : $theNotDefinedValue;
      break;
  }
  return $theValue;
}
}
$editFormAction = $_SERVER['PHP_SELF'];
if (isset($_SERVER['QUERY_STRING'])) {
  $editFormAction .= "?" . htmlentities($_SERVER['QUERY_STRING']);
}

if ((isset($_POST["MM_insert"])) && ($_POST["MM_insert"] == "post_form")) {
  $insertSQL = "INSERT INTO comment (blogid, name, comm) VALUES ('" .  $_POST['cmdid'] . "','" . $_POST['name'] . "','" . 
      $_POST['comment'] . "')";
      
      /*sprintf("INSERT INTO blog (title, author, content) VALUES (%s, %s, %s)",
                       GetSQLValueString($_POST['tit'], "text"),
                       GetSQLValueString($_POST['aut'], "text"),
                       GetSQLValueString($_POST['cont'], "text"));*/

  mysqli_select_db($con,$database);
  $Result1 = mysqli_query($con,$insertSQL) or die(mysqli_error($con));

  $insertGoTo = "blogdisp.php";
  if (isset($_SERVER['QUERY_STRING'])) {
    $insertGoTo .= (strpos($insertGoTo, '?')) ? "&" : "?";
    $insertGoTo .= $_SERVER['QUERY_STRING'];
  }
  header(sprintf("Location: %s", $insertGoTo));
echo $insertSQL;    
}

mysqli_select_db($con,$database);
$query= "SELECT * FROM blog";
$rsblog = mysqli_query($con,$query) or die(mysqli_error($con));
$row_rsblog = mysqli_fetch_assoc($rsblog);
$totalRows_rsblog = mysqli_num_rows($rsblog);

?>
Blog detail:
 	<!DOCTYPE html>
<html>
<head>
    <title>Simple Blogging Platform</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }

        header {
            background-color: #333;
            color: #fff;
            padding: 20px;
            text-align: center;
        }

        h1 {
            margin: 0;
        }

        main {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
        }

        #blog-posts article {
            margin-bottom: 30px;
            border: 1px solid #ccc;
            padding: 20px;
            background-color: #f7f7f7;
        }

        #blog-posts h2 {
            margin: 0;
        }

        #blog-posts p {
            margin-bottom: 10px;
        }

        #create-post form {
            margin-top: 20px;
        }

        #create-post label {
            display: block;
            margin-bottom: 5px;
        }

        #create-post input[type="text"],
        #create-post textarea {
            width: 100%;
            padding: 5px;
            margin-bottom: 10px;
        }

        #create-post button[type="submit"] {
            background-color: #333;
            color: #fff;
            padding: 8px 15px;
            border: none;
            cursor: pointer;
        }

        #create-post button[type="submit"]:hover {
            background-color: #555;
        }
    </style>
     <style>
        /* Add your custom CSS styles here */
        .comment-section {
            margin-top: 20px;
        }

        .comment {
            margin-bottom: 10px;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 3px;
            background-color: #f7f7f7;
        }

        .comment p {
            margin: 0;
        }

        .comment-form label {
            font-weight: bold;
            margin-bottom: 5px;
            display: block;
        }

        .comment-form input[type="text"],
        .comment-form textarea {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }

        .comment-form button[type="submit"] {
            background-color: #333;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 3px;
        }

        .comment-form button[type="submit"]:hover {
            background-color: #555;
        }
    </style>
</head>
<body>
    <header>
        <h1>Simple Blogging Platform</h1>
    </header>

    <main>
        <section id="create-post">
            <h2>Create a New Blog Post</h2>
             <form action="<?php echo $editFormAction; ?>" method="POST" id="post_form" name="post_form"  enctype="multipart/form-data">
                <label for="title">Title:</label>
                <input type="text" id="tit" name="tit"  required>

                <label for="author">Author:</label>
                <input type="text" id="aut" name="aut" required>

                <label for="content">Content:</label>
                <textarea id="cont" name="cont" required></textarea>

                <button type="submit" id="btn_add" name="btn_add" Value="Submit">Submit</button>
                <input type="hidden" name="MM_insert" value="post_form">
            </form>
        </section>
            
    </main>

    
</body>
</html>

</body>
</html>

<?php require_once('connection.php'); ?>
<?php
if (!function_exists("GetSQLValueString")) {
function GetSQLValueString($theValue, $theType, $theDefinedValue = "", $theNotDefinedValue = "") 
{
  if (PHP_VERSION < 6) {
    $theValue = get_magic_quotes_gpc() ? stripslashes($theValue) : $theValue;
  }

  $theValue = function_exists("mysql_real_escape_string") ? mysql_real_escape_string($theValue) : mysql_escape_string($theValue);

  switch ($theType) {
    case "text":
      $theValue = ($theValue != "") ? "'" . $theValue . "'" : "NULL";
      break;    
    case "long":
    case "int":
      $theValue = ($theValue != "") ? intval($theValue) : "NULL";
      break;
    case "double":
      $theValue = ($theValue != "") ? doubleval($theValue) : "NULL";
      break;
    case "date":
      $theValue = ($theValue != "") ? "'" . $theValue . "'" : "NULL";
      break;
    case "defined":
      $theValue = ($theValue != "") ? $theDefinedValue : $theNotDefinedValue;
      break;
  }
  return $theValue;
}
}

$editFormAction = $_SERVER['PHP_SELF'];
if (isset($_SERVER['QUERY_STRING'])) {
  $editFormAction .= "?" . htmlentities($_SERVER['QUERY_STRING']);
}

if ((isset($_POST["MM_insert"])) && ($_POST["MM_insert"] == "post_form")) {
  $insertSQL = "INSERT INTO blog (title, author, content) VALUES ('" .  $_POST['tit'] . "','" . $_POST['aut'] . "','" . 
      $_POST['cont'] . "')";
      
      /*sprintf("INSERT INTO blog (title, author, content) VALUES (%s, %s, %s)",
                       GetSQLValueString($_POST['tit'], "text"),
                       GetSQLValueString($_POST['aut'], "text"),
                       GetSQLValueString($_POST['cont'], "text"));*/

  mysqli_select_db($con,$database);
  $Result1 = mysqli_query($con,$insertSQL) or die(mysqli_error($con));

  $insertGoTo = "blogdisp.php";
  if (isset($_SERVER['QUERY_STRING'])) {
    $insertGoTo .= (strpos($insertGoTo, '?')) ? "&" : "?";
    $insertGoTo .= $_SERVER['QUERY_STRING'];
  }
  header(sprintf("Location: %s", $insertGoTo));
echo $insertSQL;    
}


/*mysql_select_db($database, $con);
$query_rsproduct = "SELECT * FROM product";
$rsproduct = mysql_query($query_rsproduct, $jeepcon) or die(mysql_error());
$row_rsproduct = mysql_fetch_assoc($rsproduct);
$totalRows_rsproduct = mysql_num_rows($rsproduct);*/
?>
Database:
	<?php 
$hostname = "localhost";
$database = "myblog";
$username = "root";
$password = "";

$con = mysqli_connect($hostname, $username, $password,$database);
if (!$con) {
    die('Could not connect: ' . mysql_error());
}

//echo 'Connected successfully';
//mysqli_close($con);
?>
