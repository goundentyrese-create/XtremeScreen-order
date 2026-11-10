<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>XtremeScreen-order</title>
  <style>
    :root {
      --bg-color: #0f172a;
      --card-bg: #1e293b;
      --accent: #38bdf8;
      --accent-hover: #0284c7;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --seat-available: #334155;
      --seat-selected: #22c55e;
      --seat-taken: #ef4444;
      --seat-hover: #475569;
      --border: #334155;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-main);
      padding: 2rem 1rem;
      display: flex;
      justify-content: center;
      min-height: 100dvh;
    }

    .container {
      max-width: 800px;
      width: 100%;
      display: flex;
      flex-direction: column;
      gap: 2rem;
    }

    header {
      text-align: center;
    }

    header h1 {
      font-size: 2rem;
      color: var(--accent);
      margin-bottom: 0.5rem;
    }

    header p {
      color: var(--text-muted);
    }

    .card {
      background-color: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 1.5rem;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.3);
    }

    .card h2 {
      font-size: 1.25rem;
      margin-bottom: 1rem;
      color: var(--text-main);
      border-bottom: 1px solid var(--border);
      padding-bottom: 0.5rem;
    }

    .screen-container {
      perspective: 400px;
      margin-bottom: 2rem;
      text-align: center;
    }

    .screen {
      background: linear-gradient(to bottom, #e2e8f0, #94a3b8);
      height: 12px;
      width: 80%;
      margin: 0 auto 0.5rem auto;
      border-radius: 4px;
      box-shadow: 0 3px 10px rgba(255, 255, 255, 0.2);
    }

    .screen-text {
      font-size: 0.75rem;
      letter-spacing: 2px;
      color: var(--text-muted);
      text-transform: uppercase;
    }

    .seating-grid {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      align-items: center;
    }

    .row {
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .row-label {
      font-weight: bold;
      width: 20px;
      color: var(--accent);
    }

    .seats-group {
      display: flex;
      gap: 0.4rem;
    }

    .aisle {
      width: 24px;
    }

    .seat {
      width: 32px;
      height: 32px;
      background-color: var(--seat-available);
      border: 1px solid #475569;
      border-radius: 6px 6px 2px 2px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.75rem;
      font-weight: 600;
      color: var(--text-main);
      transition: all 0.2s ease;
    }

    .seat:hover:not(.taken) {
      background-color: var(--seat-hover);
      border-color: var(--accent);
    }

    .seat.selected {
      background-color: var(--seat-selected);
      border-color: #16a34a;
      color: #000;
    }

    .seat.taken {
      background-color: var(--seat-taken);
      border-color: #dc2626;
      cursor: not-allowed;
      opacity: 0.6;
    }

    .legend {
      display: flex;
      justify-content: center;
      gap: 1.5rem;
      margin-top: 1.5rem;
      font-size: 0.85rem;
      color: var(--text-muted);
    }

    .legend-item {
      display: flex;
      align-items: center;
      gap: 0.5rem;
    }

    .legend-box {
      width: 16px;
      height: 16px;
      border-radius: 3px;
    }

    .form-group {
      margin-bottom: 1.25rem;
    }

    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
      font-weight: 500;
    }

    input[type="text"], select {
      width: 100%;
      padding: 0.75rem;
      background-color: #0f172a;
      border: 1px solid var(--border);
      border-radius: 6px;
      color: var(--text-main);
      font-size: 1rem;
    }

    input[type="text"]:focus, select:focus {
      outline: none;
      border-color: var(--accent);
    }

    .checkbox-group {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
    }

    .checkbox-item {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      background: #0f172a;
      padding: 0.6rem;
      border-radius: 6px;
      border: 1px solid var(--border);
      cursor: pointer;
    }

    .checkbox-item input {
      accent-color: var(--accent);
    }

    button.btn-submit {
      width: 100%;
      padding: 0.85rem;
      background-color: var(--accent);
      color: #0f172a;
      border: none;
      border-radius: 6px;
      font-size: 1rem;
      font-weight: bold;
      cursor: pointer;
      transition: background-color 0.2s;
    }

    button.btn-submit:hover {
      background-color: var(--accent-hover);
    }

    .summary-card {
      display: none;
      background: #064e3b;
      border: 1px solid #059669;
    }

    .summary-card.active {
      display: block;
    }
  </style>
</head>
<body>

  <div class="container">
    <header>
      <h1>🎬 Class Movie Day</h1>
      <p>Reserve your seat and choose your snacks & drinks!</p>
    </header>

    <form id="orderForm">
      <div class="card">
        <h2>1. Student Details</h2>
        <div class="form-group">
          <label for="studentName">Your Name:</label>
          <input type="text" id="studentName" placeholder="Enter your full name" required>
        </div>
      </div>

      <div class="card">
        <h2>2. Choose Your Seat</h2>
        <div class="screen-container">
          <div class="screen"></div>
          <div class="screen-text">MOVIE SCREEN</div>
        </div>

        <div class="seating-grid" id="seatingGrid"></div>

        <div class="legend">
          <div class="legend-item"><div class="legend-box" style="background-color: var(--seat-available);"></div> Available</div>
          <div class="legend-item"><div class="legend-box" style="background-color: var(--seat-selected);"></div> Selected</div>
          <div class="legend-item"><div class="legend-box" style="background-color: var(--seat-taken);"></div> Taken</div>
        </div>
        <input type="hidden" id="selectedSeatInput" required>
      </div>

      <div class="card">
        <h2>3. Select Your Drinks & Snacks</h2>

        <div class="form-group">
          <label>Milkshake:</label>
          <select id="milkshakeSelect">
            <option value="None">None</option>
            <option value="Vanilla Milkshake">Vanilla</option>
            <option value="Chocolate Milkshake">Chocolate</option>
          </select>
        </div>

        <div class="form-group">
          <label>Soft Drinks (Starz 24PK):</label>
          <select id="softDrinkSelect">
            <option value="None">None</option>
            <option value="Starz Feijoa">Feijoa</option>
            <option value="Starz Lime">Lime</option>
            <option value="Starz Orange">Orange</option>
            <option value="Starz Passionfruit">Passionfruit</option>
            <option value="Starz Raspberry">Raspberry</option>
            <option value="Starz Cola">Cola</option>
          </select>
        </div>

        <div class="form-group">
          <label>Tea Selection:</label>
          <select id="teaSelect">
            <option value="None">None</option>
            <option value="Normal Hot Tea">Normal Hot Tea</option>
            <option value="Peach Cold Tea">Fruit Cold Tea - Peach</option>
            <option value="Lemon Cold Tea">Fruit Cold Tea - Lemon</option>
            <option value="Berry Cold Tea">Fruit Cold Tea - Berry</option>
          </select>
        </div>

        <div class="form-group">
          <label>Movie Snacks:</label>
          <div class="checkbox-group">
            <label class="checkbox-item">
              <input type="checkbox" value="Popcorn" class="snack-check"> Popcorn
            </label>
          </div>
        </div>

        <button type="submit" id="submitBtn" class="btn-submit">Submit Reservation</button>
      </div>
    </form>

    <div class="card summary-card" id="summaryCard">
      <h2>🎉 Order Confirmed!</h2>
      <p style="margin-bottom: 0.5rem;" id="summaryName"></p>
      <p style="margin-bottom: 0.5rem;" id="summarySeat"></p>
      <p style="margin-bottom: 0.5rem;" id="summaryDrinks"></p>
      <p id="summarySnacks"></p>
    </div>
  </div>

  <script>
    // Corrected Apps Script Web App backend URL
    const APPS_SCRIPT_URL = "https://script.google.com/macros/s/AKfycbwwWRO2pZGz8eX1fS7_Ft8oo0haGt_iyf1YcQnhuRysAVpOntUVTTp7MterLsATLtTffA/exec";

    const rows = ['A', 'B', 'C'];
    const seatingGrid = document.getElementById('seatingGrid');
    let selectedSeat = null;
    let takenSeats = [];

    function buildGrid() {
      seatingGrid.innerHTML = '';
      rows.forEach(rowLabel => {
        const rowDiv = document.createElement('div');
        rowDiv.className = 'row';

        const label = document.createElement('div');
        label.className = 'row-label';
        label.innerText = rowLabel;
        rowDiv.appendChild(label);

        // Left 5 seats
        const leftGroup = document.createElement('div');
        leftGroup.className = 'seats-group';
        for (let i = 1; i <= 5; i++) {
          leftGroup.appendChild(createSeatElement(`${rowLabel}${i}`));
        }
        rowDiv.appendChild(leftGroup);

        // Middle Aisle
        const aisle = document.createElement('div');
        aisle.className = 'aisle';
        rowDiv.appendChild(aisle);

        // Right 5 seats
        const rightGroup = document.createElement('div');
        rightGroup.className = 'seats-group';
        for (let i = 6; i <= 10; i++) {
          rightGroup.appendChild(createSeatElement(`${rowLabel}${i}`));
        }
        rowDiv.appendChild(rightGroup);

        seatingGrid.appendChild(rowDiv);
      });
    }

    function createSeatElement(seatId) {
      const seat = document.createElement('div');
      seat.className = 'seat';
      seat.innerText = seatId;
      seat.dataset.id = seatId;

      if (takenSeats.includes(seatId)) {
        seat.classList.add('taken');
      } else {
        seat.addEventListener('click', () => {
          if (seat.classList.contains('taken')) return;
          document.querySelectorAll('.seat:not(.taken)').forEach(s => s.classList.remove('selected'));
          seat.classList.add('selected');
          selectedSeat = seatId;
          document.getElementById('selectedSeatInput').value = seatId;
        });
      }
      return seat;
    }

    async function fetchTakenSeats() {
      try {
        const response = await fetch(APPS_SCRIPT_URL);
        const data = await response.json();
        takenSeats = data.takenSeats || [];
        buildGrid();
      } catch (err) {
        console.error("Could not fetch seat data", err);
      }
    }

    buildGrid();
    fetchTakenSeats();
    setInterval(fetchTakenSeats, 5000);

    document.getElementById('orderForm').addEventListener('submit', async (e) => {
      e.preventDefault();

      const name = document.getElementById('studentName').value;
      const seat = document.getElementById('selectedSeatInput').value;

      if (!seat) {
        alert('Please select an available seat from the seating chart.');
        return;
      }

      const milkshake = document.getElementById('milkshakeSelect').value;
      const softDrink = document.getElementById('softDrinkSelect').value;
      const tea = document.getElementById('teaSelect').value;
      const snacks = Array.from(document.querySelectorAll('.snack-check:checked')).map(cb => cb.value);

      const submitBtn = document.getElementById('submitBtn');
      submitBtn.innerText = "Submitting...";
      submitBtn.disabled = true;

      const payload = {
        name,
        seat,
        milkshake,
        softDrink,
        tea,
        snacks: snacks.join(', ')
      };

      try {
        const response = await fetch(APPS_SCRIPT_URL, {
          method: 'POST',
          body: JSON.stringify(payload)
        });
        const result = await response.json();

        if (result.status === 'success') {
          document.getElementById('summaryName').innerHTML = `<strong>Student:</strong> ${name}`;
          document.getElementById('summarySeat').innerHTML = `<strong>Selected Seat:</strong> ${seat}`;
          
          const drinksList = [milkshake, softDrink, tea].filter(d => d !== 'None').join(', ');
          document.getElementById('summaryDrinks').innerHTML = `<strong>Drinks:</strong> ${drinksList || 'None'}`;
          document.getElementById('summarySnacks').innerHTML = `<strong>Snacks:</strong> ${snacks.join(', ') || 'None'}`;

          document.getElementById('orderForm').style.display = 'none';
          document.getElementById('summaryCard').classList.add('active');
          document.getElementById('summaryCard').scrollIntoView({ behavior: 'smooth' });
        } else {
          alert('Error: ' + result.message);
          submitBtn.innerText = "Submit Reservation";
          submitBtn.disabled = false;
          fetchTakenSeats();
        }
      } catch (err) {
        alert('Network error. Please try again.');
        submitBtn.innerText = "Submit Reservation";
        submitBtn.disabled = false;
      }
    });
  </script>
</body>
</html>
