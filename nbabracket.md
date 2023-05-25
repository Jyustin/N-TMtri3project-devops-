<html>
<table summary="Tournament Bracket" class="bracket">
<style>
   table.bracket {
    border-collapse: collapse;
    border: none;
}

.bracket td {
    vertical-align: middle;
    width: 40em;
    margin: 0;
    padding: 10px 0px 10px 0px;
}

.bracket td p {
    border-bottom: solid 1px black;
    border-top: solid 1px black;
    border-right: solid 1px black;
    margin: 0;
    padding: 5px 5px 5px 5px;
}

.bracket th{
    text-align:center;
}
</style>
<tr>
    <th>game 1<br>date</th>
    <th>game 2<br>date</th>
    <th>game 3<br>date</th>
</tr>
<tr>
    <td>
    <select name="teams1" id="selection 1"> 
        <option value="team 1">1</option> 
        <option value="team 2">2</option> 
        <option value="team 3">3</option> 
        <option value="team 4">4</option> 
    </select></td>
    <td rowspan="2" id="selection 2"><select name="teams2"> 
        <option value="team 1">1</option> 
        <option value="team 2">2</option> 
        <option value="team 3 ">3</option> 
        <option value="team 4">4</option> </td>
    <td rowspan="4"><p>team1</p></td>
</tr>
<tr>
    <td><p>team 2</p></td>
</tr>
<tr>
    <td><p>team 3</p></td>
    <td rowspan="2"><p>team4</p></td>
</tr>
<tr>
    <td><p>team 4</p></td>
</tr>


</table>

<button onclick="tester()">test</button>


<script>
function tester() {
    alert("hi");
    test1 = "cool"
    var selection2 = document.getElementById("selection2").getElementsByTagName("select")[0].value;

    document.getElementById("selection 2").innerHTML = selection2;
    alert(test1);

}
</script>
</html>