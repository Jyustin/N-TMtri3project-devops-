<h1>NBA Statistics for the NBA 75 Team</h1>
<br>

<html>
<body>

<script>
  var requestOptions = {
    method: 'GET',
    redirect: 'follow'
  };

  fetch("https://tri3dev.duckdns.org/api/nbastats", requestOptions)
    .then(response => response.json())
    .then(data => {
      const table = document.getElementById("musicTable");
      const tbody = document.createElement("tbody");

      data.forEach(player => {
        const row = document.createElement("tr");

        // Iterate over each property and create a table cell (td) for it
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

  // Rest of the code...
</script>


  <table id="sportsTable">
    <thead>
      <tr>
        <th onclick="sortTable('assists per game')">Assists Per Game</th>
        <th onclick="sortTable('minutes per game')">Minutes Per Game</th>
        <th onclick="sortTable('blocks per game')">Blocks Per Game</th>
        <th onclick="sortTable('defensive rebounds')">Defensive Rebounds</th>
        <th onclick="sortTable('fg percent')">FG Percent</th>
        <th onclick="sortTable('ft percent')">FT Percent</th>
        <th onclick="sortTable('games played')">Games Played</th>
        <th onclick="sortTable('height (inches)')">Height (inches)</th>
        <th onclick="sortTable('name')">Name</th>
        <th onclick="sortTable('offensive rebounds')">Offensive Rebounds</th>
        <th onclick="sortTable('points per game')">Points Per Game</th>
        <th onclick="sortTable('steals per game')">Steals Per Game</th>
        <th onclick="sortTable('team')">Team</th>
        <th onclick="sortTable('three percent')">Three Percent</th>
        <th onclick="sortTable('weight (pounds)')">Weight (pounds)</th>


      </tr>
    </thead>
  </table>
  
  <script>
    // Function to sort the table based on the selected column
function sortTable(columnName) {
  const table = document.getElementById('sportsTable');
  const rows = Array.from(table.tBodies[0].getElementsByTagName('tr'));
  const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
  const isAscending = !headerRow.classList.contains('asc');
  
  rows.sort((rowA, rowB) => {
    let cellA = rowA.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;
    let cellB = rowB.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;

    if (columnName.toLowerCase() === 'name' || columnName.toLowerCase() === 'team') {
      return isAscending ? cellA.localeCompare(cellB, undefined, { sensitivity: 'base' }) : cellB.localeCompare(cellA, undefined, { sensitivity: 'base' });
    }

    // Convert the cell values to numbers for the "Games Played" column
    if (columnName.toLowerCase() === 'games played') {
      cellA = parseInt(cellA);
      cellB = parseInt(cellB);
    }
    
    return isAscending ? cellA - cellB : cellB - cellA;
  });
  
  rows.forEach(row => table.tBodies[0].appendChild(row));
  headerRow.classList.toggle('asc');
}


  
    // Helper function to get the index of the selected column
    function getColumnIndex(columnName) {
      const table = document.getElementById('sportsTable');
      const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
      const headers = Array.from(headerRow.getElementsByTagName('th'));
      
      return headers.findIndex(header => header.innerText.toLowerCase() === columnName.toLowerCase()) + 1;
    }
  </script>

<script>

const resultContainer = document.getElementById("result");
  // prepare URL's to allow easy switch from deployment and localhost
const url = "https://tri3dev.duckdns.org/api/nbastats"
const create_fetch = url + '/create';
const read_fetch = url + '/';
read_players();

function read_players() {
    // prepare fetch options
    const read_options = {
      method: 'GET', // *GET, POST, PUT, DELETE, etc.
      mode: 'cors', // no-cors, *cors, same-origin
      cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
      credentials: 'omit', // include, *same-origin, omit
      headers: {
        'Content-Type': 'application/json'
      },
    };     // fetch the data from API
    fetch(read_fetch, read_options)
      // response is a RESTful "promise" on any successful fetch
      .then(response => {
        // check for response errors
        if (response.status !== 200) {
            const errorMsg = 'Database read error: ' + response.status;
            console.log(errorMsg);
            const tr = document.createElement("tr");
            const td = document.createElement("td");
            td.innerHTML = errorMsg;
            tr.appendChild(td);
            return;
        }
        // valid response will have json data
        response.json().then(data => {
            console.log(data);
            for (let row in data) {
              console.log(data[row]);
              add_row(data[row]);
            }
        })
    })
      // catch fetch errors (ie ACCESS to server blocked)
    .catch(err => {
      console.error(err);
      const tr = document.createElement("tr");
      const td = document.createElement("td");
      td.innerHTML = err;
      tr.appendChild(td);
      resultContainer.appendChild(tr);
    });
  }

</script>

</body>
</html>