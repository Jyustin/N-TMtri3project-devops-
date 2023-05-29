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
      const table = document.getElementById("sportsTable");
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

  <!-- defines the tableID that is going to be referred to later in this segment -->
  <table id="sportsTable">
    <thead>
        <tr>
            <!-- these are the sortTable functions for each of the columns. the onclick triggers a sorting response on selecting the column header. -->
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
    // sortTable function meant to sort based on each column header
    function sortTable(columnName) {
        const table = document.getElementById('sportsTable');
        // constant calls the tableID previously defined
        const rows = Array.from(table.tBodies[0].getElementsByTagName('tr'));
        const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
        const isAscending = !headerRow.classList.contains('asc');
        // very important line. the asc class helps the function decide whether or not the column is going to be sorted in an ascending or descending order. 

        rows.sort((rowA, rowB) => {
            // Get the cell values of the selected column for comparison
            let cellA = rowA.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;
            let cellB = rowB.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;
            // the nth-child selector is different from normal JS arrays, these have an index starting at 1 rather than 0
            // the rows are sorted based on the column. getColumnIndex is used to get the index of the sorting stats in each column. 

            if (columnName.toLowerCase() === 'name' || columnName.toLowerCase() === 'team') {
                // Sort alphabetically if the column is "Name" or "Team"
                return isAscending ? cellA.localeCompare(cellB, undefined, { sensitivity: 'base' }) : cellB.localeCompare(cellA, undefined, { sensitivity: 'base' });
            }

            // this if segment is for special situations with the name and team columns. since they process strings and need to be sorted alphabetically, so localeCompare is used to sort the rows by comparing cellA and cellB's values.

            // convert the cell values to numbers for the "Games Played" column
            if (columnName.toLowerCase() === 'games played') {
                cellA = parseInt(cellA);
                cellB = parseInt(cellB);
            }

            // games played was not sorting for some odd reason, which is why this if statement is necessary
            // the purpose is to parse the values in the games played column as integers, and then sorting them numerically.
            return isAscending ? cellA - cellB : cellB - cellA;
        });

        rows.forEach(row => table.tBodies[0].appendChild(row));
        headerRow.classList.toggle('asc');
        // this is meant for after sorting. after the sorting is done, the appendChild is used to format and append the SORTED data to the table.
    }

    
    function getColumnIndex(columnName) {
        // each column name is taken in columnName to get the index of the column values
        const table = document.getElementById('sportsTable');
        const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
        // contains the column headers, index is 0
        const headers = Array.from(headerRow.getElementsByTagName('th'));

        return headers.findIndex(header => header.innerText.toLowerCase() === columnName.toLowerCase()) + 1;
        // column header names are converted to lowercase. the sortTable is formatted like that as seen above - the IDs are all lowercase, but the formatted frontend headers are all uppercase, so they need to be converted to lowercase. makes process a whole lot easier and more efficient rather than having to deal with manually matching the ID names and column header names.
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