<h1>NBA Statistics</h1>

<html>
<body>

<script>

var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};


fetch("http://172.21.244.147:8086/api/nbastats/", requestOptions)
  .then(response => response.json())
  .then(r => {
  r.forEach(ev => {
    const row = document.createElement("tr")
    const data = document.createElement("td")
    data.innerHTML = `${ev.name}, ${ev.team}, ${ev.height}, ${ev.weight}, ${ev.gamesplayed}, ${ev.avgminutes}, ${ev.ppg}, ${ev.fgpercent}, ${ev.threepercent}, ${ev.ftpercent}, ${ev.orebounds}, ${ev.drebounds}, ${ev.assists}, ${ev.steals}, ${ev.blocks}`
    row.appendChild(data)
    document.getElementById("table").appendChild(row)
  })
  })
  .catch(error => console.log('error', error))




function reset() {
  window.location.reload();
}




</script>


<table id="musicTable">
    <thead>
      <tr>
        <th onclick="sortTable('name')">Player</th>
        <th onclick="sortTable('team')">Team</th>
        <th onclick="sortTable('height')">Height (inches)</th>
        <th onclick="sortTable('weight')">Weight (pounds)</th>
        <th onclick="sortTable('gamesplayed')">Games Played</th>
        <th onclick="sortTable('avgminutes')">Minutes Per Game</th>
        <th onclick="sortTable('ppg')">Points Per Game</th>
        <th onclick="sortTable('fgpercent')">FG Percent</th>
        <th onclick="sortTable('orebounds')">Offensive Rebounds Per Game</th>
        <th onclick="sortTable('drebounds')">Defensive Rebounds Per Game</th>
        <th onclick="sortTable('assists')">Assists Per Game</th>
        <th onclick="sortTable('steals')">Steals Per Game</th>
        <th onclick="sortTable('blocks')">Blocks Per Game</th>

      </tr>
    </thead>
    <tbody>
      <tr>
        <td>LeBron James</td>
        <td>Los Angeles Lakers</td>
        <td>81</td>
        <td>225</td>
        <td>1421</td>
        <td>32</td>
        <td>28</td>
        <td>52</td>
        <td>4</td>
        <td>4</td>
        <td>7</td>
        <td>1</td>
        <td>1</td>
      </tr>
      <tr>
        <td>Stephen Curry</td>
        <td>Golden State Warriors</td>
        <td>75</td>
        <td>188</td>
        <td>1259</td>
        <td>34</td>
        <td>21</td>
        <td>58</td>
        <td>3</td>
        <td>2</td>
        <td>6</td>
        <td>1</td>
        <td>1</td>
      </tr>
    </tbody>
  </table>

<script>
    // Function to sort the table based on the selected column
    function sortTable(columnName) {
      const table = document.getElementById('musicTable');
      const rows = Array.from(table.tBodies[0].getElementsByTagName('tr'));
      const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
      const isAscending = !headerRow.classList.contains('asc');
      
      rows.sort((rowA, rowB) => {
        const cellA = rowA.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;
        const cellB = rowB.querySelector(`td:nth-child(${getColumnIndex(columnName)})`).innerText;
        
        return isAscending ? cellA.localeCompare(cellB) : cellB.localeCompare(cellA);
      });
      
      rows.forEach(row => table.tBodies[0].appendChild(row));
      headerRow.classList.toggle('asc');
    }
  
    // Helper function to get the index of the selected column
    function getColumnIndex(columnName) {
      const table = document.getElementById('musicTable');
      const headerRow = table.getElementsByTagName('thead')[0].getElementsByTagName('tr')[0];
      const headers = Array.from(headerRow.getElementsByTagName('th'));
      
      return headers.findIndex(header => header.innerText.toLowerCase() === columnName.toLowerCase()) + 1;
    }
  </script>

<script>

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

const resultContainer = document.getElementById("result");
  // prepare URL's to allow easy switch from deployment and localhost
const url = "http://172.21.244.147:8086/api/nbastats/"
const create_fetch = url + '/create';
const read_fetch = url + '/';
read_players();

</script>

</body>
</html>