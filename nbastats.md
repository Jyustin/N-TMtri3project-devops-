<h1>NBA Statistics</h1>

<html>
<body>

<table style="width:100%" id="table">
  <tr>
    <th>Real Time NBA Player Statistics</th>
  </tr>
</table>




<script>

var requestOptions = {
  method: 'GET',
  redirect: 'follow'
};


fetch("", requestOptions)
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


<table>
  <thead>
  <tr>
    <th>Player Name</th>
    <th>Team</th>
    <th>Height</th>
    <th>Weight</th>
    <th>Career Games Played</th>
    <th>Average Minutes Played</th>
    <th>Career PPG</th>
    <th>Career FG Percent</th>
    <th>Career 3 Percent</th>
    <th>Career FT Percent</th>
    <th>Career Offensive Rebounds</th>
    <th>Career Defensive Rebounds</th>
    <th>Career Assists</th>
    <th>Career Steals</th>
    <th>Career Blocks</th>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>


<script>

function read_users() {
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
const url = "https://fnvs.duckdns.org/api/fact"
const create_fetch = url + '/create';
const read_fetch = url + '/';
read_users();

</script>


<form action="javascript:create_user()">
 <p><label>
        Tell Us Something that Happened on Your Favorite Day!
        <input type="text" name="fact" id="fact" placeholder="fact" required>
        <input type="text" name="fact" id="date" placeholder="day/month" required>
        <input type="number" name="fact" id="year" placeholder="year" required>


    </label></p>
    <p><button>Add</button></p>
</form>


<script>
  function create_user() {
    fetch("https://fnvs.duckdns.org/api/fact/create", {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({fact:document.getElementById("fact").value,date:document.getElementById("date").value,year:document.getElementById("year").valueAsNumber})
    }).then(e => console.log(
     
      "yay"
    ));
  }
</script>

<form action="javascript:create_user()">
 <p><label>
        Tell Us Something that Happened on Your Favorite Day!
        <input type="text" name="fact" id="fact" placeholder="fact" required>
        <input type="text" name="fact" id="date" placeholder="day/month" required>
        <input type="number" name="fact" id="year" placeholder="year" required>


    </label></p>
    <p><button>Add</button></p>
</form>

</body>


</html>