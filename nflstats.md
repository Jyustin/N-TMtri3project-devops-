<h1>NFL Statistics</h1>
<br>

<script>
  var requestOptions = {
    method: 'GET',
    redirect: 'follow'
  };

  fetch("https://tri3dev.duckdns.org/api/nfl", requestOptions)
    .then(response => response.json())
    .then(data => {
      const table = document.getElementById("sportsTable");
      const tbody = document.createElement("tbody");

      data.forEach(player => {
        const row = document.createElement("tr");

        for (const key in player) {
          const cell = document.createElement("td");
          cell.innerText = player[key];
          row.appendChild(cell);
        }

        tbody.appendChild(row);
      });

      table.appendChild(tbody);
    })
    .catch(error => console.log('error', error));

</script>


<table id="nflStats">
    <thead>
        <tr>
          <th onclick="sortTable('player name')">Player Name</th>
          <th onclick="sortTable('player id')">Player ID</th>
          <th onclick="sortTable('team id')">Team ID</th>
          <th onclick="sortTable('weight')">Weight</th>
          <th onclick="sortTable('position')">Position</th>
          <th onclick="sortTable('jersey number')">Jersey Number</th>
          <th onclick="sortTable('height')">Height</th>
          <th onclick="sortTable('espn player link')">ESPN Player Link</th>
    </thead>
 </table>
 
 <script>
    
    function sortTable(columnName) {
        const table = document.getElementById('nflStats');
        const rows = Array.from(table.tBodies[0].getElementsByTagName('tr'));
        const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
        const isAscending = !headerRow.classList.contains('asc');

        rows.sort((rowA, rowB) => {
            let cellA = rowA.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;
            let cellB = rowB.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;
            return isAscending ? cellA - cellB : cellB - cellA;
        });

        rows.forEach(row => table.tBodies[0].appendChild(row));
        headerRow.classList.toggle('asc');
       
    }

    
    function getColumnIndex(columnName) {
        const table = document.getElementById('nflStats');
        const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
        const headers = Array.from(headerRow.getElementsByTagName('th'));

        return headers.findIndex(header => header.innerText.toLowerCase() === columnName.toLowerCase()) + 1;
    }
</script>



            
            

