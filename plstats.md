<h1>Premier League Statistics</h1>
<br>
<p id="result" style="font-color:red;display:none;" ></p>

<script>
//https://tri3dev.duckdns.org/api/premierleagueplayer
const resultContainer = document.getElementById("result");

// prepare URL's to allow easy switch from deployment and localhost
const url = "http://localhost:8086/api/premierleagueplayer"
const create_fetch = url + '/create';
const read_fetch = url + '/';

const requestOptions = {
    method: 'GET',
    redirect: 'follow'
};
fetch(read_fetch, requestOptions)
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
    .catch(error => {
        console.log('error', error);
        resultContainer.innerHTML = error;
        resultContainer.style.display="block";

    });
    try{
        //let mainImage = document.getElementById("lebronpic");
        let mainImage = document.getElementByTag("img");
        mainImage.src="barclays-premier-league-logo.jpg";
        mainImage.style.width = "200px";
        mainImage.style.height = "200px";
        mainImage.alt = "Premier League";
    } catch (e){
        console.log(e);
    }
</script>

  <!-- defines the tableID that is going to be referred to later in this segment -->
  <table id="sportsTable">
    <thead>
        <tr>
            <!-- these are the sortTable functions for each of the columns. the onclick triggers a sorting response on selecting the column header. -->
            <th onclick="sortTable('Player Name')">Player Name</th>
            <th onclick="sortTable('Team')">Team</th>
            <th onclick="sortTable('Position')">Position</th>
            <th onclick="sortTable('Jersey Number')">Jersey Number</th>
            <th onclick="sortTable('Age')">Age</th>
            <th onclick="sortTable('Height')">Height</th>
            <th onclick="sortTable('Weight')">Weight</th>
            <th onclick="sortTable('Goals')">Goals</th>
            <th onclick="sortTable('Assits')">Assists</th>
            <th onclick="sortTable('Yellow Cards')">Yellow Cards</th>
            <th onclick="sortTable('Red Cards')">Red Cards</th>
            <th onclick="sortTable('Passes Completed')">Passes Completed</th>
            <th onclick="sortTable('Tackles')">Tackles</th>
            <th onclick="sortTable('Clean Sheets')">Clean Sheets</th>
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