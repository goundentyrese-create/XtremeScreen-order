<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Class Movie Day - Seat & Order Selection</title>
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

    button.btn-submit, button.btn-reset {
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

    button.btn-submit:hover, button.btn-reset:hover {
      background-color: var(--accent-hover);
    }

    button.btn-reset {
      margin-top: 1.5rem;
    }
