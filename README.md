# truck---shift---dashboard
truck shift dashboard
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Truck Shift Dashboard - Mahlopi Metals</title>
<style>
    * { box-sizing: border-box; font-family: Arial, sans-serif; }
  body { background: #f2f2f2; margin: 0; padding: 20px; }
  h1 { color: #1a1a1a; }
  .cards { display: flex; gap: 12px; flex-wrap: wrap; margin-bottom: 20px; }
  .card { background: white; padding: 16px; border-radius: 10px; flex: 1; min-width: 140px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); }
  .card b { font-size: 28px; display: block; }
  .card span { color: #666; font-size: 13px; }
  form, table { background: white; padding: 16px; border-radius: 10px; margin-bottom: 20px; box-shadow: 0 2px 6px rgba(0,0,0,0.1); }
  input, select, button { padding: 10px; margin: 5px 0; width: 100%; border-radius: 6px; border: 1px solid #ccc; }
  button { background: #f5a800; border: none; font-weight: bold; cursor: pointer; }
  button:hover { background: #e09500; }
  table { width: 100%; border-collapse: collapse; }
  th, td { padding: 10px; text-align: left; border-bottom: 1px solid #eee; }
  .Running { color: green; font-weight: bold; }
  .Breakdown { color: red; font-weight: bold; }
  .Standing { color: orange; font-weight: bold; }
  .del { background: #ffdddd; width: auto; padding: 6px 10px; }
</style>
</head>
<body>

<h1>🚛 Truck Shift Dashboard</h1>
<p>Site: Mahlopi Metals Group | Shift: Day Shift | Supervisor: Sithole</p>

<div class="cards">
  <div class="card"><b id="totalTrucks">0</b><span>Total Trucks</span></div>
  <div class="card"><b id="totalLoads">0</b><span>Total Loads Today</span></div>
  <div class="card"><b id="running">0</b><span>Running</span></div>
  <div class="card"><b id="breakdown">0</b><span>Breakdown</span></div>
</div>

<form id="truckForm">
  <h3>Add / Update Truck</h3>
  <input id="truckNo" placeholder="Truck No - e.g. ADT 04" required>
  <input id="driver" placeholder="Driver Name - e.g. Ofentse Diatshwana" required>
  <select id="status">
    <option value="Running">Running</option>
    <option value="Breakdown">Breakdown</option>
    <option value="Standing">Standing</option>
  </select>
  <input id="loads" type="number" placeholder="Loads Done" min="0" value="0" required>
  <button type="submit">Save Truck</button>
</form>

<table>
  <thead>
    <tr><th>Truck</th><th>Driver</th><th>Status</th><th>Loads</th><th></th></tr>
  </thead>
  <tbody id="truckTable"></tbody>
</table>

<script>
let trucks = [
  { truckNo: "ADT 01", driver: "Ofentse Diatshwana", status: "Running", loads: 12 },
  { truckNo: "ADT 02", driver: "T. Shoko", status: "Running", loads: 10 },
  { truckNo: "RDT 05", driver: "K. Sithole", status: "Breakdown", loads: 4 },
];

function render() {
  const tbody = document.getElementById("truckTable");
  tbody.innerHTML = "";
  let totalLoads = 0, running = 0, breakdown = 0;
  trucks.forEach((t, i) => {
    totalLoads += Number(t.loads);
    if (t.status === "Running") running++;
    if (t.status === "Breakdown") breakdown++;
    tbody.innerHTML += `<tr>
      <td>${t.truckNo}</td><td>${t.driver}</td>
      <td class="${t.status}">${t.status}</td><td>${t.loads}</td>
      <td><button class="del" onclick="removeTruck(${i})">Delete</button></td>
    </tr>`;
  });
  document.getElementById("totalTrucks").textContent = trucks.length;
  document.getElementById("totalLoads").textContent = totalLoads;
  document.getElementById("running").textContent = running;
  document.getElementById("breakdown").textContent = breakdown;
}

function removeTruck(i) { trucks.splice(i, 1); render(); }

document.getElementById("truckForm").addEventListener("submit", function(e) {
  e.preventDefault();
  const truckNo = document.getElementById("truckNo").value;
  const driver = document.getElementById("driver").value;
  const status = document.getElementById("status").value;
  const loads = document.getElementById("loads").value;
  // If truck exists, update it, else add new
  const found = trucks.find(t => t.truckNo.toLowerCase() === truckNo.toLowerCase());
  if (found) { found.driver = driver; found.status = status; found.loads = loads; }
  else { trucks.push({ truckNo, driver, status, loads }); }
  this.reset();
  render();
});
render();
</script>
</body>
</html>
</body>
</html>
