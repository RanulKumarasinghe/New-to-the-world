# New-to-the-world
Web Development code
<!DOCTYPE html>

<html>
<head>

<meta charset="UTF-8">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.12.1/jquery-ui.min.js"></script>


<script>
 //This function creates the contents for each table
  function createTable(link) {

    $("#restaurantTable tbody tr").remove();
    $("#restaurantTable").append("<tr><th>" + "BusinessName" + "</th><th>" + "BusinessAddress" + "</th><th>" + "HygeneRating" + "</th><th>" + "Date" + "</th><th>" + "Ratings" + "</th></tr>");
    $.get(link, function(operation){
      $.each(operation, function(key, value){
          var CuRating = `<button class="cusrating" type="button" onclick="rating('${value.name}')">Customer Rating</button>`;


          $("#restaurantTable").append("<tr><td>" + value.name + "</td><td>" + value.addressLine + "</td><td>" + value.hygieneRating + "</td><td>" + value.ratingDate + "</td><td>" + CuRating + "</td>");
      });
    });
  }
  //This function fetches the rating.php data and alerts whether there is data or no data.
      function rating(name) {

          var link = `https://www.cs.kent.ac.uk/people/staff/sm951/co539/assessment2/rating.php?name=${name}`;
            console.log(link);
             $.get(link, function(data) {
             if (data == "") {
               alert("No data");
             }
             $.each(data, function(key,value){
              alert("The rating for:" + " " + value.name + " " + "at" + " " + value.locationId  + " " + "is" + " " + value.rating);
           });

       });
      }


    $(document).ready(function(){

          //Creates the first page
            var link = "https://www.cs.kent.ac.uk/people/staff/sm951/co539/assessment2/hygiene.php?operation=get&page=1";
            createTable(link)



          //Creates the buttons for the pages
          $.get("https://www.cs.kent.ac.uk/people/staff/sm951/co539/assessment2/hygiene.php?operation=pages", function(data){
            var p = data.pages;
            var i;
            for(i=1; i <= p; i++){
              $("#buttons").append("<button class='button1' type='button' value='" + [i] + "'>" + [i] + "</button>");
            }
          });

          //Generates the page when clicked on the button for the page
          $(document).on("click",".button1", function() {
            var link = "https://www.cs.kent.ac.uk/people/staff/sm951/co539/assessment2/hygiene.php?operation=get&page=" + $(this).val()

            createTable(link)

          });


          //Searches the input in the text box and generate words that
        $(document).on("click","#searchlist", function() {
          input = $("#search").val();
          var link = "https://www.cs.kent.ac.uk/people/staff/sm951/co539/assessment2/hygiene.php?operation=search&name=" + input;

          createTable(link)

          if (!($.inArray(input, businesses)>-1)) {
            businesses.push(input);
          }
        });




        //Default auto complete terms
        var businesses = [
          "subway",
          "Alisha",
          "Chi",
          "My Cafe"
        ];

        //Gets the array for the autocomplete and shows your choice before pressing the button at the side of the screen
        $("#search").autocomplete({
         source: businesses,
         autoFill: true,
         selectFirst: true
        });

  });


</script>
<style>
table, tbody {
    font-family: "Times New Roman", Times, serif;
    color: #000000;
    border-collapse: collapse;
    width: 90%;
}

td, th {
    border: 2px solid #262626;
    text-align: middle;
    padding: 2px;
}

th{
   font-size: 18px;
}

tr:nth-child(even) {
    background-color: #ff6666;
}

tr:nth-child(odd) {
    background-color: #f2f2f2;
}
tr{
  font-weight: bold;
  text-align: middle;
}

.button1{
  border-radius: 50px;
  background-color: #f2f2f2;
}

body {
  background-color: #ffffff;
}

.div2{
  font-size: 20px;
}

#div1 {
  position: absolute;
  left: 40%;
}

#searchlist{
  padding: 1px 20px;
}

#search{
  width: 200px;
}

</style>

<title>Hygiene ratings</title>

</head>
<body>
  <center><h1> Hygene Ratings </h1></center>
<div class = "div2"><center> Pages:  <div id="buttons"> </div> </center></div>

<center><table id ="restaurantTable">

    <tbody>
    </tbody>
  </table>
<br>
<div class = "div2"><div id="div1">Business Name: </div2><input type="text" id="search" /> <button id="searchlist">Search</button></div></center>

</body>

</html>
